## SQLの実行結果をTSVに落とす

```
$ mysql -uuser -p -hhost database -e 'select * from post;' > post.tsv
```

ファイルに記述したSQLを実行する場合はこう。

```
$ mysql -uuser -p -hhost database < foo.sql > hoge.tsv
```

## サブクエリを利用したDELETE文

```sql
begin;
delete from post_history where post_id in (select id from post where blog_id = 13);

# 成功したらcommit, 失敗したらrollback.
```