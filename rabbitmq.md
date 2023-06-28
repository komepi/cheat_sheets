# rabbitmq

## rabbitmqctl
RabbitMQサーバーに対してvhost, ユーザーの作成, 削除を行うためのコマンド

### vhost
vhostはユーザー、キュー、コネクションなどのリソースの集合
複数定義することができて名前を持つ
デフォルトで`/`という名前のvhostが存在する

#### 確認: list_vhosts
```
[root@server ~]# rabbitmqctl list_vhosts
Listing vhosts ...
/
...done.
```

#### 追加: add_vhost
```cmd
vhost(/test1)を追加する。
[root@server ~]# rabbitmqctl add_vhost /test1
Creating vhost "/test1" ...
...done.

vhost(/test2)を追加する。
[root@server ~]# rabbitmqctl add_vhost /test2
Creating vhost "/test2" ...
...done.

vhostを確認する。/test1と/test2が追加されたことがわかる。
/はデフォルトで存在するvhostです。
[root@server ~]# rabbitmqctl list_vhosts
Listing vhosts ...
/
/test1
/test2
...done.
```

#### 削除: delete_vhost
```cmd
vhost(/test1)を削除する。
[root@server ~]# rabbitmqctl delete_vhost /test1
Deleting vhost "/test1" ...
...done.

vhost(/test2)を削除する。
[root@server ~]# rabbitmqctl delete_vhost /test2
Deleting vhost "/test2" ...
...done.

vhostを確認する。/test1と/test2が削除されたことがわかる。
[root@server ~]# rabbitmqctl list_vhosts
Listing vhosts ...
/
...done.
```

### ユーザー
#### 確認: list_users
```
[root@server ~]# rabbitmqctl list_users
Listing users ...
admin   [administrator]
guest   [administrator]
...done.
```
#### 追加: add_user
書式は以下
```
rabbitmqctl add_user <user> <password>
```
以下例
```
登録されているユーザを確認する。adminとguestが登録されていることがわかる。
[root@server ~]# rabbitmqctl list_users
Listing users ...
admin   [administrator]
guest   [administrator]
...done.

ユーザ(user1)を追加する。
[root@server ~]# rabbitmqctl add_user user1 11111
Creating user "user1" ...
...done.

ユーザを確認する。user1が追加されたことがわかる。
[root@server ~]# rabbitmqctl list_users
Listing users ...
admin   [administrator]
guest   [administrator]
user1   []
...done.
```

#### 削除: delete_user
```
ユーザ(user1)を削除する。
[root@server ~]# rabbitmqctl delete_user user1
Deleting user "user1" ...
...done.

ユーザを確認する。user1が削除されたことがわかる。
[root@server ~]# rabbitmqctl list_users
Listing users ...
admin   [administrator]
guest   [administrator]
...done.
```

#### 権限追加： set_permissions
```
[root@server ~]# rabbitmqctl add_vhost /test1
Creating vhost "/test1" ...
...done.
[root@server ~]# rabbitmqctl add_user user1 11111
Creating user "user1" ...
...done.

[root@server ~]# rabbitmqctl set_permissions -p /test1 user1 ".*" ".*" ".*"
Setting permissions for user "user1" in vhost "/test1" ...
...done.

[root@server ~]# rabbitmqctl list_permissions -p /test1
Listing permissions in vhost "/test1" ...
user1   .*      .*      .*
...done.
```

#### 権限の削除: clear_permissions
```
[root@server ~]# rabbitmqctl clear_permissions -p /test1 user1
Clearing permissions for user "user1" in vhost "/test1" ...
...done.
[root@server ~]# rabbitmqctl list_permissions -p /test1
Listing permissions in vhost "/test1" ...
...done.
```

### キュー
#### リストの表示: list_queues
```
"/"に存在するキューを表示する。"hello"が存在することがわかる。
[root@server ~]# rabbitmqctl list_queues -p /
Listing queues ...
hello   0
...done.
```
