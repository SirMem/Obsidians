---
Project: "[[MybatisPlus]]"
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-11-05
Connected:
---

#review

Redis 是一个键值对数据库，具有非常灵活和高效的操作方式，支持多种数据类型。下面我将详细解释你提供的 Redis 3供相关代码示例。

### 1. 选择数据库（`select`）
Redis 默认包含16个数据库，数据库是由一个整数索引标识，索引从 0 开始。可以通过 `select` 命令选择某个数据库。

**命令说明：**
```bash
select <数据库索引>
```

**示例：**
```bash
select 1  # 选择数据库1
```

### 2. 数据添加（`set`）
Redis 中的数据默认是以键值对的形式进行存储。你可以通过 `set` 命令向 Redis 中添加数据。键和值都以字符串的形式存储。

**命令说明：**
```bash
set <key> <value>
```

**示例：**
```bash
set user:info:12345:name "lbw"  # 向数据库添加键为 user:info:12345:name 的值 "lbw"
```

### 3. 批量添加数据（`mset`）
Redis 还支持批量设置多个键值对，使用 `mset` 命令。

**命令说明：**
```bash
mset <key1> <value1> <key2> <value2> ...
```

**示例：**
```bash
mset user:info:12345:name "lbw" user:info:12345:age 25  # 批量设置多个键值对
```

### 4. 获取数据（`get`）
你可以通过 `get` 命令获取指定键的值。

**命令说明：**
```bash
get <key>
```

**示例：**
```bash
get user:info:12345:name  # 获取键为 user:info:12345:name 的值
```

### 5. 设置数据过期时间（`set` 和 `expire`）
Redis 允许你为键值对设置过期时间。可以通过 `EX` 或 `PX` 来设置过期时间，分别表示秒和毫秒。

**命令说明：**
```bash
set <key> <value> EX <秒>       # 设置过期时间（秒）
set <key> <value> PX <毫秒>     # 设置过期时间（毫秒）
expire <key> <秒>               # 设置过期时间（秒）
ttl <key>                       # 获取键的剩余过期时间（秒）
pttl <key>                      # 获取键的剩余过期时间（毫秒）
persist <key>                   # 取消键的过期时间，使其变为永久
```

**示例：**
```bash
set user:info:12345:name "lbw" EX 3600  # 设置 user:info:12345:name 的过期时间为1小时（3600秒）
expire user:info:12345:name 3600  # 设置键的过期时间为1小时（3600秒）
ttl user:info:12345:name   # 获取该键的剩余过期时间
persist user:info:12345:name  # 取消该键的过期时间，使其变为永久
```

### 6. 删除数据（`del`）
如果你希望删除某个键及其对应的值，可以使用 `del` 命令。

**命令说明：**
```bash
del <key> ...
```

**示例：**
```bash
del user:info:12345:name  # 删除 user:info:12345:name 键
```

### 7. 查看数据库中所有键（`keys`）
你可以通过 `keys *` 来列出当前数据库中所有的键。

**命令说明：**
```bash
keys *
```

**示例：**
```bash
keys *  # 获取当前数据库中所有的键
```

### 8. 判断键是否存在（`exists`）
`exists` 命令用于检查某个键是否存在。

**命令说明：**
```bash
exists <key>
```

**示例：**
```bash
exists user:info:12345:name  # 检查键 user:info:12345:name 是否存在
```

### 9. 获取随机键（`randomkey`）
Redis 允许你获取数据库中一个随机的键。

**命令说明：**
```bash
randomkey
```

**示例：**
```bash
randomkey  # 获取数据库中的一个随机键
```

### 10. 移动键到另一个数据库（`move`）
可以将某个键从当前数据库移动到另一个数据库。

**命令说明：**
```bash
move <key> <数据库索引>
```

**示例：**
```bash
move user:info:12345:name 1  # 将 user:info:12345:name 键移动到数据库 1
```

### 11. 修改键名（`rename` 和 `renamex`）
你可以使用 `rename` 命令修改键名。`renamex` 命令会检查目标键是否已经存在。

**命令说明：**
```bash
rename <key> <新的名称>   # 修改键名
renamex <key> <新的名称>  # 如果目标键已存在，则不会修改
```

**示例：**
```bash
rename user:info:12345:name user:info:12345:username  # 修改键名
renamex user:info:12345:name user:info:12345:username  # 如果目标键已存在，则不执行
```

### 12. 数字自增/自减（`incr`, `incrby`, `decr`）
Redis 支持对存储在键中的数字进行自增或自减操作。

**命令说明：**
```bash
incr <key>        # 键值增加1
incrby <key> <b>  # 键值增加 b
decr <key>        # 键值减少1
```

**示例：**
```bash
incr user:info:12345:age  # user:info:12345:age 增加1
incrby user:info:12345:age 5  # user:info:12345:age 增加5
decr user:info:12345:age  # user:info:12345:age 减少1
```

### 13. 查看数据类型（`type`）
Redis 提供了 `type` 命令，用来查看指定键的数据类型。

**命令说明：**
```bash
type <key>
```

**示例：**
```bash
type user:info:12345:name  # 查看键 user:info:12345:name 的数据类型
```

### 总结
Redis 以其高效、简单的键值存储方式，支持多种数据类型，并提供了丰富的命令来操作数据。你可以利用这些命令来实现各种缓存、存储和数据操作的需求。如果你是开发者或系统管理员，掌握 Redis 的基本操作将极大提升你的工作效率。

如果你对 Redis 的某个特性或者命令有更多疑问，欢迎随时询问！


在 Redis 中，除了最基本的 `String` 类型，还有其他几种数据结构：`Hash`、`List`、`Set` 和 `SortedSet`。这些数据结构类似于 Java 中的集合类型，每种结构都提供了相应的命令以便操作。下面详细介绍这些数据类型的操作和示例代码。

---

### 1. Hash 类型

`Hash` 类似于一个嵌套的 `HashMap`，非常适合用来存储对象。每个 `Hash` 类型的数据都包含一个主键（key）和若干字段（field）与值（value），类似于对象的属性和属性值。

#### 示例代码

假设我们需要存储一个用户的信息（例如用户 ID、用户名、年龄等），可以使用 `Hash` 类型来存储：

```sql
# 添加一个 Hash 类型的数据
hset user:1001 name "Alice"
hset user:1001 age 25

# 一次性添加多个字段
hmset user:1002 name "Bob" age 30 city "New York"

# 获取某个字段
hget user:1001 name  # 返回 "Alice"

# 获取所有字段和值
hgetall user:1002
# 返回结果：
# 1) "name"
# 2) "Bob"
# 3) "age"
# 4) "30"
# 5) "city"
# 6) "New York"

# 判断某个字段是否存在
hexists user:1001 age  # 返回 1 表示存在，0 表示不存在

# 删除 Hash 中的某个字段
hdel user:1002 city

# 获取 Hash 中字段的数量
hlen user:1001  # 返回 2
```

---

### 2. List 类型

`List` 是一个双端列表，可以在头部或尾部添加和删除元素，支持随机访问。`List` 常用于实现消息队列、任务列表等场景。

#### 示例代码

假设我们有一个任务队列：

```sql
# 向列表头部添加任务
lpush task_queue "task1"
lpush task_queue "task2"

# 向列表尾部添加任务
rpush task_queue "task3"

# 获取列表中的第一个任务（不会移除）
lindex task_queue 0  # 返回 "task2"

# 获取列表中所有任务
lrange task_queue 0 -1
# 返回 ["task2", "task1", "task3"]

# 获取并移除头部任务
lpop task_queue  # 返回 "task2"

# 获取并移除尾部任务
rpop task_queue  # 返回 "task3"
```

---

### 3. Set 类型

`Set` 是一个无序集合，不允许重复元素，类似于 Java 中的 `HashSet`。它可以用于存储一些唯一值，例如标签、类别等，还支持集合运算。

#### 示例代码

```sql
# 添加元素到集合
sadd tags "Redis" "Database" "Cache"

# 获取集合的所有元素
smembers tags
# 返回 ["Redis", "Database", "Cache"]

# 检查某个元素是否存在
sismember tags "Redis"  # 返回 1 表示存在

# 获取集合的元素个数
scard tags  # 返回 3

# 获取集合间的交集
sadd tags_2 "Database" "NoSQL"
sinter tags tags_2  # 返回 ["Database"]

# 删除集合中的指定元素
srem tags "Cache"
```

---

### 4. SortedSet 类型

`SortedSet` 是一个有序集合，每个元素都有一个 `score`（分数）决定排序位置，适用于排行榜、计分系统等场景。`SortedSet` 可以按照分数进行范围查找和排序。

#### 示例代码

假设我们要实现一个简单的排行榜系统：

```sql
# 添加带分数的元素
zadd leaderboard 100 "Alice"
zadd leaderboard 200 "Bob"
zadd leaderboard 150 "Charlie"

# 获取排行榜中的前两个
zrange leaderboard 0 1 withscores
# 返回 ["Alice", 100, "Charlie", 150]

# 获取分数在 100 到 200 之间的玩家
zrangebyscore leaderboard 100 200
# 返回 ["Alice", "Charlie", "Bob"]

# 获取某个玩家的排名
zrank leaderboard "Alice"  # 返回 0

# 移除玩家
zrem leaderboard "Charlie"
```

---

这四种类型都是 Redis 中常用的数据结构。合理利用这些数据结构，能帮助实现高效的存储与操作。

### Redis 持久化机制

Redis 是一个基于内存的数据库，这使得它非常快速，但同时也带来了一个问题：如果服务器突然崩溃或断电，内存中的数据将会丢失。为了避免这个问题，Redis 提供了两种持久化机制：RDB（快照存储）和 AOF（日志追加存储）。这两种机制可以单独使用，也可以结合使用，根据不同的应用场景选择合适的持久化方式。

---

### 1. RDB（Redis 数据库快照）

RDB 是 Redis 提供的一种数据持久化机制，它通过生成数据的快照（即将当前数据库的所有数据存储到磁盘上的一个文件 `dump.rdb`），来保证数据的持久化。在 RDB 模式下，Redis 会在一定的条件下自动生成数据的快照，这些条件可以配置在 Redis 的配置文件中。

#### 工作原理

1. Redis 在满足某些条件时，会创建一个数据快照，并将数据写入到磁盘上的 `dump.rdb` 文件中。
2. 可以通过命令 `save` 或者 `bgsave` 来手动触发数据持久化操作。
3. Redis 在服务重启时会自动加载 `dump.rdb` 文件，恢复之前保存的数据。

#### 优点

- **加载速度快**：由于数据是通过快照方式保存的，当 Redis 重启时，只需要加载这个文件即可，恢复数据的速度较快。
- **磁盘空间占用小**：RDB 文件是一个压缩过的快照，相对而言，文件体积较小。

#### 缺点

- **可能丢失数据**：RDB 只在特定的时间点创建快照，如果 Redis 在快照保存过程中崩溃，则可能会丢失快照后但未持久化的数据。
- **性能消耗**：如果数据量很大，生成快照时可能会占用较多的 CPU 和内存资源，影响系统的性能。

#### 配置示例

```bash
# 在 redis.conf 文件中配置 RDB 保存策略
save 900 1     # 900秒内至少进行1次写操作时进行持久化
save 300 10    # 300秒内至少进行10次写操作时进行持久化
save 60 10000  # 60秒内至少进行10000次写操作时进行持久化
```

手动执行 RDB 持久化：

```bash
save      # 阻塞式命令，立即保存数据
bgsave    # 非阻塞式命令，后台进程保存数据
```

---

### 2. AOF（Append Only File，追加文件）

AOF 是另一种 Redis 持久化方式，它会将 Redis 执行的每一条写命令记录到一个日志文件（`appendonly.aof`）中，所有的命令都会按顺序追加到文件中。当 Redis 重启时，它会按顺序重新执行这些命令，以恢复数据库的状态。

#### 工作原理

1. Redis 会记录每一条写操作命令，并将其追加到 `appendonly.aof` 文件中。
2. 在 Redis 重启时，会重新执行 AOF 文件中的命令来恢复数据库的状态。
3. AOF 允许配置不同的同步策略，如每次操作后同步、每秒同步或不同步。

#### 优点

- **实时性高**：AOF 可以记录每次写操作，因此相对于 RDB，可以最大限度地减少数据丢失的情况。
- **支持持久化精细化控制**：可以根据配置灵活控制数据同步的频率，优化性能和可靠性。

#### 缺点

- **AOF 文件较大**：由于每条命令都会被记录，因此文件体积通常比 RDB 文件要大得多。
- **加载速度较慢**：相比于 RDB 的快照恢复，AOF 需要按顺序重演每一条写命令，恢复过程比较慢。

#### 配置示例

```bash
# 启用 AOF 持久化
appendonly yes

# 配置 AOF 同步策略
appendfsync always   # 每次写操作后同步到硬盘
appendfsync everysec # 每秒同步一次（默认配置）
appendfsync no       # 不强制同步，交由操作系统管理

# 配置 AOF 文件的自动重写
auto-aof-rewrite-percentage 100  # 当 AOF 文件大小增加 100% 时触发自动重写
auto-aof-rewrite-min-size 64mb   # 当 AOF 文件大小超过 64MB 时触发自动重写
```

---

### 3. RDB 和 AOF 混合使用

Redis 支持同时启用 RDB 和 AOF，这样可以同时享受到两者的优点：RDB 提供快速恢复的能力，而 AOF 提供数据不丢失的保障。在这种模式下，Redis 会根据配置同时保存 RDB 快照和记录 AOF 日志文件。

#### 配置示例

```bash
# 启用 RDB 和 AOF 持久化
save 900 1       # RDB 快照配置
appendonly yes   # 启用 AOF 持久化
appendfsync everysec  # AOF 每秒同步一次
```

---

### 4. AOF 重写机制

由于 AOF 持久化方式会记录每一个写操作命令，随着时间推移，AOF 文件会越来越大，这时 Redis 提供了 AOF 重写机制，来压缩 AOF 文件，减少文件体积。AOF 重写并不会丢失任何数据，只会把命令合并成最简化的形式。

#### 手动触发 AOF 重写

```bash
bgrewriteaof
```

AOF 文件重写的过程是自动进行的。可以配置重写的触发条件：

```bash
# 当 AOF 文件大小增长到原大小的 100% 时触发自动重写
auto-aof-rewrite-percentage 100

# 当 AOF 文件大小超过 64MB 时触发自动重写
auto-aof-rewrite-min-size 64mb
```

---

### 总结

- **RDB**：适用于数据量较大、对实时性要求不高的场景，文件较小，加载速度快，但可能会丢失一些数据。
- **AOF**：适用于对数据一致性要求较高的场景，实时性好，但文件较大，恢复速度慢。
- **混合使用**：同时使用 RDB 和 AOF，可以兼顾数据安全性和恢复速度。

选择合适的持久化方案取决于业务的需求，如果数据量不大且对实时性要求不高，RDB 足矣；如果需要保证数据的实时持久化，AOF 是更好的选择。如果要兼顾两者，可以同时启用 RDB 和 AOF。

### Redis 事务和锁机制

#### 1. Redis 事务

Redis 的事务机制允许你将多个命令打包成一个事务，在事务提交之前，命令会被加入一个队列中，等到 `exec` 命令被执行时，这些命令才会一并执行。与 MySQL 不同，Redis 事务并不会保证命令间的原子性，也不提供隔离性，事务中的命令并不会被其他命令打断，然而事务执行前并不会检查命令是否成功。

##### Redis 事务的基本操作

- **开启事务**：使用 `multi` 命令来开启一个事务，开始将命令放入队列。

  ```bash
  multi
  ```

- **添加命令**：在事务开启后，可以执行多个 Redis 命令，这些命令不会立即执行，而是加入到队列中。

  ```bash
  set key1 value1
  incr key2
  ```

- **提交事务**：使用 `exec` 提交事务，这时所有的命令会被依次执行。

  ```bash
  exec
  ```

- **取消事务**：如果想放弃事务中的所有命令，可以使用 `discard` 来取消事务。

  ```bash
  discard
  ```

##### 示例

```bash
multi         # 开启事务
set foo bar   # 设置 foo 的值为 bar
incr counter  # 对 counter 进行自增操作
exec          # 提交事务
```

#### 2. Redis 锁机制

在多客户端操作同一数据时，为了避免数据冲突，Redis 提供了乐观锁机制。与 MySQL 中的悲观锁不同，乐观锁不会在访问资源之前加锁，而是在操作时进行验证，确保没有其他客户端修改了目标数据。

##### 乐观锁：使用 `WATCH` 命令

Redis 事务提供了一种乐观锁机制，通过 `WATCH` 命令来监视某个或某些键。如果在事务执行之前，任何被监视的键发生了变化，则事务会被取消，确保数据一致性。

- **监视某个键**：使用 `watch` 命令监视一个或多个键。

  ```bash
  watch key1 key2
  ```

- **取消监视**：如果决定不再监视，可以使用 `unwatch` 命令。

  ```bash
  unwatch
  ```

##### 示例：使用 `WATCH` 实现乐观锁

```bash
watch mykey       # 监视 mykey 键
multi             # 开启事务
set mykey newval  # 修改 mykey 的值
exec              # 执行事务，只有在 mykey 未被修改时才会成功
```

如果在 `exec` 命令执行时，`mykey` 被其他客户端修改了，事务将不会执行，Redis 会返回空值。

#### 乐观锁与悲观锁的对比

- **悲观锁**：认为资源会被其他人占用，因此在访问资源时会先加锁，直到操作完成，保证数据的安全性。适用于高冲突的场景，防止其他线程/进程访问共享资源。
  
- **乐观锁**：假设资源不会被其他人修改，先进行操作，再通过验证来判断是否发生冲突。适用于冲突较少的场景，能够提高并发性能。

#### 总结

- **Redis 事务**：允许将多个命令打包执行，但并不保证事务的原子性。可以通过 `multi`、`exec`、`discard` 等命令来控制事务的开启、提交和取消。
- **Redis 锁机制**：采用乐观锁机制，使用 `watch` 和 `unwatch` 来监视数据是否被修改。事务只有在监视的键没有被修改时才会执行，确保数据一致性。

如果希望在 Redis 中实现更复杂的并发控制，通常结合事务和乐观锁来确保数据的正确性。





### 使用Java与Redis交互

通过Java与Redis交互的常用方式是使用 **Jedis** 或 **Spring Data Redis**。下面分别介绍两者的使用方式。

#### 1. **Jedis操作Redis**

Jedis 是一个 Redis 客户端，提供了与 Redis 数据库交互的 API。

##### **Jedis依赖**（在 `pom.xml` 中添加）

```xml
<dependencies>
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>4.0.0</version>
    </dependency>
</dependencies>
```

##### **连接Redis**

使用 Jedis 连接到 Redis 服务器，并执行基本操作：

```java
public static void main(String[] args) {
    // 创建Jedis对象，连接到本地Redis服务器
    try (Jedis jedis = new Jedis("localhost", 6379)) {
        jedis.set("test", "lbwnb");  // 等同于 set test lbwnb 命令
        System.out.println(jedis.get("test"));  // 等同于 get test 命令
    }
}
```

##### **操作Hash类型**

```java
public static void main(String[] args) {
    try (Jedis jedis = new Jedis("192.168.10.3", 6379)) {
        jedis.hset("hhh", "name", "sxc");  // hset hhh name sxc
        jedis.hset("hhh", "sex", "19");    // hset hhh sex 19
        jedis.hgetAll("hhh").forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

##### **操作List类型**

```java
public static void main(String[] args) {
    try (Jedis jedis = new Jedis("192.168.10.3", 6379)) {
        jedis.lpush("mylist", "111", "222", "333");  // lpush mylist 111 222 333
        jedis.lrange("mylist", 0, -1)
             .forEach(System.out::println);  // lrange mylist 0 -1
    }
}
```

#### 2. **Spring Boot 整合 Redis**

在Spring Boot中，整合 Redis 操作框架需要使用 `spring-boot-starter-data-redis`，它底层使用的是 **Lettuce** 客户端。

##### **添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

##### **配置Redis连接**

在 `application.yml` 或 `application.properties` 中配置 Redis 连接信息。

```yaml
spring:
  redis:
    host: 192.168.10.3
    port: 6379
    database: 0
```

##### **使用 `StringRedisTemplate` 操作 Redis**

```java
@SpringBootTest
class RedisTestApplicationTests {

    @Autowired
    StringRedisTemplate template;

    @Test
    void contextLoads() {
        // 操作 Redis 值
        ValueOperations<String, String> operations = template.opsForValue();
        operations.set("c", "xxxxx");  // 设置键值
        System.out.println(operations.get("c"));  // 获取值

        // 删除键
        template.delete("c");
        System.out.println(template.hasKey("c"));  // 判断是否存在该键
    }
}
```

##### **Redis事务操作**

Spring Boot 中并没有专门的 Redis 事务管理器，但我们可以通过启用事务支持来实现：

```java
@Service
public class RedisService {

    @Resource
    StringRedisTemplate template;

    @PostConstruct
    public void init() {
        template.setEnableTransactionSupport(true);  // 启用事务支持
    }

    @Transactional  // 使用 Spring 的事务注解
    public void test() {
        template.multi();  // 开始事务
        template.opsForValue().set("d", "xxxxx");  // 设置键值
        template.exec();   // 提交事务
    }
}
```

##### **存储对象**

通过为 `RedisTemplate` 配置序列化器来支持存储 Java 对象（如 JSON）。

```java
@Test
void contextLoad2() {
    // 存储 Student 对象，注意该对象需实现 Serializable
    template.opsForValue().set("student", new Student());
    System.out.println(template.opsForValue().get("student"));
}
```

#### 总结

1. **Jedis**：适合直接通过 Java API 与 Redis 交互，操作简单直观。
2. **Spring Data Redis**：提供了更为优雅的方式来与 Redis 交互，特别是与 Spring 项目集成时，支持事务、对象存储等功能。

两者各有优劣，Jedis 更轻量，但对于 Spring Boot 项目，推荐使用 `Spring Data Redis`。