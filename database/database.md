# nodejs连接数据库

## mysql

### sql语句
首先复习一下sql语句
增 insert into 表 (key) values (value)
删 delete from 表 where 条件
改 update 表 set key = newValue where 条件
查 select * from 表 where 条件 / select key from 表 where 条件

学习到一个技巧`where 1=1`

node 连接mqsql出错
```
{ Error: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL client
    at Handshake.Sequence._packetToError (/Users/zzh/test/my-node/test1/node_modules/_mysql@2.18.1@mysql/lib/protocol/sequences/Sequence.js:47:14)
```
[解决参考](https://stackoverflow.com/questions/50093144/mysql-8-0-client-does-not-support-authentication-protocol-requested-by-server/50131831#50131831)

## mongodb