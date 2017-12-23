# 五种数据结构

> 参考文档`http://redisdoc.com/`



## 1, STRING(字符串)

- SET (设置存储在给定键中值)

  > set hello word

- GET (获取存储在给定键中的值)

  > get hello

- DEL (删除存储在给定键中的值, 可用于所有类型)

  > del hello

## 2, LIST (列表)

- RPUSH(LPUSH) (将给定值推入列表的右端或左端)

  > rpush list-key item

- LRANGE (获取列表在给定范围上的所有值)

  > lrange list-key 0 -1

- LINDEX (获取列表中给定位置的单个元素)

  > lindex list-key 1

- LPOP (RPOP) (从列表的左端或右端弹出一个值, 并返回被弹出的值)

  > lpop list-key

## 3,SET (集合)

- SADD (将给定的元素添加到集合)

  > sadd set-key item

- SMEMBERS (返回集合包含的所有元素)

  > smembers set-key

- SISMEMBER (检查给定元素是否存在与集合中)

  > sismember set-key item

- SREM (如果给定的元素存在于集合中, 那么一处这个元素)

  > srem set-key item

## 4,HASH (散列)

- HSET (在散列中关联起给定的键值)

  > hset hash-key sub-key1 value

- HGET (获取指定散列键的值)

  > hget hash-key sub-key1

- HGETALL (获取散列中包含的所有键值对)

  > hgetall hash-key

- HDEL (如果给定的键存在于散列中,那么移除这个键)

  > hdel hash-key sub-key1

## 5,ZSET (有序集合)

- ZADD (将一个带有给定分值的成员添加到有序集合中)

  > zadd zset-key 728 member1

- ZRANGE (根据元素在有序排列中所处的位置,从有序集合里面获取多个元素)

  > zrange zset-key 0 -1

- ZRANGEBYSCORE (获取有序集合在给定分值范围内的所有元素)

  > zrangewithscore zset-key 0 800

- ZREM (如果给定成员存在于有序集合中,那么移除这个元素)

  > zrem zset-key member1

# 基本配置

## 主要配置文件内容

- `INCLUDES` ：用于添加其他配置文件

- `NETWORK`：主要用于设置IP、端口等一些网络配置

- `GENERAL`： 通用配置

- `SNAPSHOTTING`： 快照

- `REPLICATION`：主从复制

- `SECURITY`

- `LIMITS`：配置连接限制，如连接数

- `APPEND ONLY MODE`： 追加

- `LUA SCRIPTING`： 配置Lua脚本的最大执行时间

  ​

## 1、 服务后台运行

> 在redis配置文件中的通用配置(GENERAL)下将`daemonize` 修改为`yes`, 启动服务时制定该配置文件



# 基本操作

- 切换数据库：`select <dbid>` , 如切换为7号数据库`select 6`
- 查询当前数据库中key的数量：`dbsize` 
- 查看当前数据库中的所有键：`keys *` 
- 清空当前数据库：`flushdb` 
- ​