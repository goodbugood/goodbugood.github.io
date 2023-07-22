# RedisTemplate框架使用


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


