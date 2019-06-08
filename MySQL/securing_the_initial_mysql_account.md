## mysql_secure_installation

https://dev.mysql.com/doc/refman/5.6/ja/mysql-secure-installation.html
https://dev.mysql.com/doc/refman/8.0/en/mysql-secure-installation.html
https://mariadb.com/kb/ja/mysql_secure_installation/

※ 5.6までは引数を取らないコマンドだったが、5.7以降は多くの引数が用意されている。  
※ `/usr/bin/mysql_secure_installation` がスクリプトの実体。  

インストール直後に実行することでセキュリティを向上させることができる。  
具体的には以下のような内容を実施する。  

- rootにパスワードを設定する
- リモートホストからのrootでのログインを禁止する
- 匿名ユーザーを削除する
- どのユーザーでもアクセスできるtestデータベースを削除する

これらは対話形式で実行されていくため、例えばAnsibleなどで自動化しようと思うと不便。  
`mysql_secure_installation` 実行時と同じような状態にするにはどうすれば良いか。  

## まずはユーザーの確認

```
$ mysql -uroot
mysql> SELECT Host, User, Password FROM mysql.user;
+------------------------------------------------+------+----------+
| Host                                           | User | Password |
+------------------------------------------------+------+----------+
| localhost                                      | root |          |
| host_name                                      | root |          |
| 127.0.0.1                                      | root |          |
| ::1                                            | root |          |
| localhost                                      |      |          |
| host_name                                      |      |          |
+------------------------------------------------+------+----------+
6 rows in set (0.000 sec)
```

結果から、rootユーザーと匿名ユーザーが存在し、いずれもパスワードが設定されていない、ということがわかる。  

## 匿名ユーザーの削除

匿名ユーザーが存在すると、以下のコマンドでMySQLに接続できる。  

```
$ mysql
```

匿名ユーザーを削除するには、例えば次のようにすれば良い。  

```
mysql> DROP USER ''@'localhost';
mysql> DROP USER ''@'host_name';
```

ただ、ホストは環境によって異なるし、一回のコマンドですべての匿名ユーザーを削除してしまいたい。  
`DROP USER` ではホストも指定する必要があるため、一回のコマンドですべての匿名ユーザーを削除することはできない。  
よって `DELETE` を使って削除する。

```
mysql> DELETE FROM mysql.user WHERE User='';
mysql> FLUSH PRIVILEGES;
```

`FLUSH PRIVILEGES;` を忘れずに。  
https://dev.mysql.com/doc/refman/5.6/ja/privilege-changes.html

以下、削除できた場合。

```
MariaDB [(none)]> select Host, User, Password from mysql.user;
+------------------------------------------------+------+----------+
| Host                                           | User | Password |
+------------------------------------------------+------+----------+
| localhost                                      | root |          |
| host_name                                      | root |          |
| 127.0.0.1                                      | root |          |
| ::1                                            | root |          |
+------------------------------------------------+------+----------+
4 rows in set (0.000 sec)
```

`mysql` だけでは接続できなくなる。

```
$ mysql
ERROR 1045 (28000): Access denied for user 'host_name'@'localhost' (using password: NO)
```