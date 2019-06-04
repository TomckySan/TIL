# Heroku Postgres

https://elements.heroku.com/addons/heroku-postgresql

- 無料の`Hobby Dev`は10,000レコードまで保存可・最大接続数が20。
- 一つ上の`Hobby Basic`は10,000,000レコードまで保存できるが、最大接続数は20のまま。$9/mo。
- 最大接続数20はかなり厳しいので本番運用は`Standard 0`以上が望ましい。
- `Standard 0`は最大接続数120。$50/mo。

# Upgrading Heroku Postgres

## 1. 新しいデータベースを追加

管理画面からHeroku Postgresアドオンページを開いて、データベースを追加する。  
追加したら以下のコマンドで状態を確認してみる。  

```
$ heroku pg:info -a app-name
```

## 2. データベースの停止・サイトメンテナンスモードに切替

```
$ heroku pg:wait
$ heroku maintenance:on -a app-name
```

## 3. 旧データベースバックアップ

```
$ heroku pg:backups capture -a app-name
```

バックアップが取れているか確認。

```
$ heroku pg:backups -a app-name
```

必要があれば手元にダウンロード。

```
$ heroku pg:backups download {id} -a app-name
```

## 4. 旧データベースを新データベースへコピー

```
$ heroku pg:copy DATABASE_URL HEROKU_POSTGRESQL_BLACK_URL -a app-name
```

コピー後はPostgresの管理画面からデータ数などを見てコピーできていることを確認しておく。

## 5. 新データベースにプロモート

```
$ heroku pg:promote HEROKU_POSTGRESQL_BLACK_URL -a app-name
```

プロモート後はアプリの接続先も変更する必要がある。  
環境変数を修正して、接続先を変更する。  

## 6. 最終確認してメンテナンス解除

```
$ heroku pg:info -a app-name
$ heroku maintenance:off -a app-name
```

## 7. 無事移行が確認できたら旧データベースを削除

管理画面から削除すると良い。