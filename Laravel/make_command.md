### コマンドテンプレート生成

```
php artisan make:command SendEmails
```

### 出力

```php
$this->info('');
$this->error('');
```

### コマンドライン引数

```php
// 引数定義
protected $signature = 'email:send {user}';

// 引数取得
$userId = $this->argument('user');
```

### サンプルコード

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Illuminate\Support\Str;
use Illuminate\Support\Facades\DB;
use App\Models\User;

class RefreshUsersAccessToken extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'users:refresh-access-token';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Refresh users access token';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $this->info('=== Start refreshing access token ===');

        $users = User::all();

        DB::beginTransaction();
        try {
            foreach ($users as $user) {
                $this->refreshAccessToken($user);
            }
            DB::commit();
        } catch (\Exception $e) {
            DB::rollback();
            $this->error('=== Error: something went wrong ===');
            $this->error($e->getMessage());
        }

        $this->info('=== Finished refreshing all ===');
    }

    private function refreshAccessToken(User $user)
    {
        $this->info(sprintf('Refresh access token [user_id = %s]', $user->id));

        $user->access_token = Str::random(32);
        $user->save();

        $this->info(sprintf('Done [user_id = %s]', $user->id));
    }
}
```

```php
<?php

// ...

class GenerateRandomUsers extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'users:generate {count}';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Generate random users';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return mixed
     */
    public function handle()
    {
        $this->info('=== Start generating random users ===');

        $generateCount = $this->argument('count');
        foreach (range(1, $generateCount) as $i) {
            $this->info(sprintf('Generate user [count = %s]', $i));
            $this->generateUser();
            $this->info(sprintf('Done [count = %s]', $i));
        }

        $this->info('=== Finished generating all ===');
    }

    private function generateUser()
    {
        $user = new User();
        // ...
    }
}
```