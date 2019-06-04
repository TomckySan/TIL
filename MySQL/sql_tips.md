## SQLの実行結果をTSVに落とす

```
$ mysql -uuser -p -hhost database -e 'select * from post;' > post.tsv
```

ファイルに記述したSQLを実行する場合はこう。

```
$ mysql -uuser -p -hhost database < foo.sql > hoge.tsv
```