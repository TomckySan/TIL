（ファイルの区別が面倒なものをとりあえず書く用）

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