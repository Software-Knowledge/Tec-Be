Spring Redis
---
1. 尝试使用Spring和Redis结合进行使用

<!-- TOC -->

- [1. Redis的对应Maven依赖](#1-redis的对应maven依赖)
  - [1.1. 在对应的配置文件中的Redis配置](#11-在对应的配置文件中的redis配置)
- [2. Java结合Rredis](#2-java结合rredis)
  - [2.1. 连接到redis服务](#21-连接到redis服务)
  - [2.2. String(字符串)实例](#22-string字符串实例)
  - [2.3. List实例](#23-list实例)
  - [2.4. Java Keys实例](#24-java-keys实例)
- [3. Spring Boot 合并 redis](#3-spring-boot-合并-redis)
  - [3.1. config文件](#31-config文件)
  - [3.2. 实现Redis的Service](#32-实现redis的service)
  - [3.3. 参考](#33-参考)

<!-- /TOC -->

# 1. Redis的对应Maven依赖
```xml
<!-- 引入 redis 依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-redis</artifactId>
    <version>1.4.5.RELEASE</version>
</dependency>
```

## 1.1. 在对应的配置文件中的Redis配置
```yml
redis:
    # Redis数据库索引(默认为0)
    database: 10
    # Redis服务器地址(默认为localhost)
    host: localhost
    # Redis服务器连接端口
    port: 6349
    # Redis服务器连接密码(默认为空)
    password: root
    # Redis连接超时时间(单位毫秒)
    timeout: 0
    pool:
      # 连接池最大连接数(使用负值表示没有限制)
      max-active: 8
      # 连接池最大阻塞等待时间(使用负值表示没有限制)
      max-wait: -1
      # 连接池中的最大空闲连接
      max-idle: 8
      # 连接池中的最小空闲连接
      min-idle: 0
      # 连接超时时间(毫秒)
      timeout: 0
```

# 2. Java结合Rredis
1. 首先需要配置完成对应的redis文件包

## 2.1. 连接到redis服务
```java
import redis.clients.jedis.Jedis;
 
public class RedisJava {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: "+jedis.ping());
    }
}
```

## 2.2. String(字符串)实例
```java
import redis.clients.jedis.Jedis;
 
public class RedisStringJava {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
        //设置 redis 字符串数据
        jedis.set("runoobkey", "www.runoob.com");
        // 获取存储的数据并输出
        System.out.println("redis 存储的字符串为: "+ jedis.get("runoobkey"));
    }
}
```

## 2.3. List实例
```java
import java.util.List;
import redis.clients.jedis.Jedis;
 
public class RedisListJava {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
        //存储数据到列表中
        jedis.lpush("site-list", "Runoob");
        jedis.lpush("site-list", "Google");
        jedis.lpush("site-list", "Taobao");
        // 获取存储的数据并输出
        List<String> list = jedis.lrange("site-list", 0 ,2);
        for(int i=0; i<list.size(); i++) {
            System.out.println("列表项为: "+list.get(i));
        }
    }
}
```

## 2.4. Java Keys实例
```java
import java.util.Iterator;
import java.util.Set;
import redis.clients.jedis.Jedis;
 
public class RedisKeyJava {
    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
 
        // 获取数据并输出
        Set<String> keys = jedis.keys("*"); 
        Iterator<String> it=keys.iterator() ;   
        while(it.hasNext()){   
            String key = it.next();   
            System.out.println(key);   
        }
    }
}
```

# 3. Spring Boot 合并 redis

## 3.1. config文件
```java
@Configuration  
@EnableCaching//开启注解  
public class RedisConfig extends CachingConfigurerSupport {
    @Bean
    public CacheManager cacheManager(RedisTemplate<?,?> redisTemplate) {
       CacheManager cacheManager = new RedisCacheManager(redisTemplate);
       return cacheManager;
    }

    @Bean
    public RedisTemplate<String, String> redisTemplate(RedisConnectionFactory factory) {
       RedisTemplate<String, String> redisTemplate = new RedisTemplate<String, String>();
       redisTemplate.setConnectionFactory(factory);
       return redisTemplate;
    }
}
```

## 3.2. 实现Redis的Service
```java
@Service
public class RedisService {
    @Resource
    private RedisTemplate<String,Object> redisTemplate;

    private volatile static ValueOperations<String,Object> vo;

    private void initRedis(){
        if(vo == null)
        {
            synchronized (RedisService.class)
            {
                if(vo == null)
                {
                    vo = redisTemplate.opsForValue();
                }
            }
        }
    }

    public void set(String key, Object value) {
        initRedis();
        vo.set(key, value);
    }

    public String get(String key) {
        initRedis();
        return (String)vo.get(key);
    }

    /**
     * 放置有时效性的键
     * @param key
     * @param value
     * @param seconds 秒
     */
    public void setTime(String key, Object value, long seconds){
        initRedis();
        Duration duration = Duration.between(LocalDateTime.now(),
                LocalDateTime.now().plusSeconds(seconds));
        vo.set(key, value, duration);
    }
}

```

## 3.3. 参考
1. <a href = "https://blog.csdn.net/change_on/article/details/62059833?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task">SpringBoot 整合 Redis 的简单案例</a>