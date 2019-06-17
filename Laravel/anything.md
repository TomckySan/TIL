## データベース・Eloquent

joinするとき同名のカラムが上書きされてしまうため、selectで指定する。

```php
$user = User::join('event_user', 'users.id', '=', 'event_user.user_id')
    ->select('users.*')
    ->where('is_tester', false)
    ->where('event_user.event_id', $eventId)
    ->first()
;
```

## ルート名を使ったリダイレクト

```php
return redirect()->route('posts.index', ['category' => $category]);
```

## アクション名を使ったリダイレクト

```php
return redirect()->action('PostsController@index', ['category' => $category]);
```

## ランダムトークン生成

```php
use Illuminate\Support\Str;

$token = Str::random(32);
```

## ハッシュ生成

```php
use Illuminate\Support\Facades\Hash;

Hash::make('foo');
```

## フォームに直前のリクエストデータを詰める

```php
<input type="text" name="username" value="{{ old('username') }}">
```

## 中間テーブルへのデータ保存

```php
App\User::find(1)->roles()->save($role);
// ↑これでできるけど

$user = new User();
$user->roles()->save($role);
// これはNG

$user = new User();
$user->save();
$user->roles()->save($role);
// 一度saveしないとダメ
```