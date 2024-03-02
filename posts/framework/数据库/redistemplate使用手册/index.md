# RedisTemplate框架使用


## 历史

> RedisTemplate 是 spring-data-redis 对 redis 的封装。

1. Jedis 是 Redis 官方开发的客户端
2. spring-data-redis 对 Jedis 进行了高度封装，对外提供 RedisTemplate 类。

### SpringBoot 整合 RedisTemplate

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
  <version>2.5.9</version>
</dependency>
```

#### RedisTemplate 整合 lettuce

```xml
// spring-boot-starter-data-redis-2.5.9.pom

<dependency>
  <groupId>org.springframework.data</groupId>
  <artifactId>spring-data-redis</artifactId>
  <version>2.5.8</version>
  <scope>compile</scope>
</dependency>
<dependency>
  <groupId>io.lettuce</groupId>
  <artifactId>lettuce-core</artifactId>
  <version>6.1.6.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

## 命令使用

### 查询哈希表

```java
import org.springframework.data.redis.core.RedisTemplate;

@Service
@RequiredArgsConstructor
public class UserService {
    private final RedisTemplate<String, String> redisTemplate;
    
    public getUserName(int userId) {
        // hashKey
        String hashKey = String.format("user_id_%d", userId);
        String fieldName = "name";
        Object userName = redisTemplate.opsForHash().get(hashKey, fieldName);
        return userName.toString();
    }
}
```

注意 `HV get(H key, java.lang.Object hashKey)` 很容易使用错误，第一个参数 key 是 Redis 的 key 的名字。而 hashKey 是哈希表中的字段的名字。用反了就拿不到数据了。

