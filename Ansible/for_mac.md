## MacにAnsibleをインストール

```
$ brew doctor
$ brew update
$ brew search ansible
$ brew install ansible
$ ansible --version
```

Inventoryのhost指定では `~/.ssh/config` に記述したホスト名が使用できる。

.ssh/config

```sh
Host foo
    HostName xxx.xxx.xx.xx
    Port xx
    ...
```

inventory

```
[dev]
foo
```