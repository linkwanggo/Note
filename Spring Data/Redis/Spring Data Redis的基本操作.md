# Spring Data Redis的基本操作

## 配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

```yml
spring:
  redis:
    host: 192.168.x.x
```

在spring boot环境下有个StringRedisTemplate对象，默认已经为我们配置好了，只需要自动注入过来就能用，但是使用它只能在Redis中存放字符串。具体操作如下：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@DirtiesContext
public class Test {
    @Autowired
    private StringRedisTemplate stringRedisTemplate;
 
    @Test
    public void test() throws Exception {
        stringRedisTemplate.opsForValue().set("aaa", "111");
        Assert.assertEquals("111", stringRedisTemplate.opsForValue().get("aaa"));
 
    }
}
```

因为在StringRedisTemplate的构造器中，设置的序列化器是字符串，所以它只能存取字符串。构造器：

```java
public StringRedisTemplate() {
    RedisSerializer<String> stringSerializer = new StringRedisSerializer();
    this.setKeySerializer(stringSerializer);
    this.setValueSerializer(stringSerializer);
    this.setHashKeySerializer(stringSerializer);
    this.setHashValueSerializer(stringSerializer);
}
```

现在，如果我们想使用RedisTemplate存取对象，那我们只需要设置相应的序列化器就行了。操作如下：

RedisConfig.java

```java
package com.tensquare.article.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
public class RedisConfig {

    /**
     * redisTemplate 序列化使用的jdkSerializeable, 存储二进制字节码, 所以自定义序列化类
     * @param redisConnectionFactory
     * @return
     */
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory);

        // 使用Jackson2JsonRedisSerialize 替换默认序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        objectMapper.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);

        jackson2JsonRedisSerializer.setObjectMapper(objectMapper);

        // 设置value的序列化规则和 key的序列化规则
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
```

查看结果

![](https://tva1.sinaimg.cn/large/006tNbRwly1gb0p033tukj30uw0oq0w8.jpg)

## 入门操作

Spring Data Redis接口封装

| ValueIoerations | 简单K-V操作            |
| --------------- | ---------------------- |
| SetIOperations  | set类型数据操作        |
| ZSetOperations  | zset类型数据操作       |
| HashOperations  | 针对map类型的数据操作  |
| ListOperations  | 针对list类型的数据操作 |

### KV操作

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:spring/applicationContext-redis.xml")
public class TestValue {
    @Autowired
    private RedisTemplate redisTemplate;    
    @Test
    public void setValue(){
        redisTemplate.boundValueOps("name").set("qingmu");        
    }    
    @Test
    public void getValue(){
        String str = (String) redisTemplate.boundValueOps("name").get();
        System.out.println(str);
    }    
    @Test
    public void deleteValue(){
        redisTemplate.delete("name");;
    }    
}
```

### Set操作

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:spring/applicationContext-redis.xml")
public class TestSet {
    
    @Autowired
    private RedisTemplate redisTemplate;
    
    /**
     * 存入值
     */
    @Test
    public void setValue(){
        redisTemplate.boundSetOps("nameset").add("曹操");        
        redisTemplate.boundSetOps("nameset").add("刘备");    
        redisTemplate.boundSetOps("nameset").add("孙权");
    }
    
    /**
     * 提取值
     */
    @Test
    public void getValue(){
        Set members = redisTemplate.boundSetOps("nameset").members();
        System.out.println(members);
    }
    
    /**
     * 删除集合中的某一个值
     */
    @Test
    public void deleteValue(){
        redisTemplate.boundSetOps("nameset").remove("孙权");
    }
    
    /**
     * 删除整个集合
     */
    @Test
    public void deleteAllValue(){
        redisTemplate.delete("nameset");
    }
}
```

### List操作

```java
		/**
     * 右压栈：后添加的对象排在后边
     */
    @Test
    public void testSetValue1(){        
        redisTemplate.boundListOps("namelist1").rightPush("刘备");
        redisTemplate.boundListOps("namelist1").rightPush("关羽");
        redisTemplate.boundListOps("namelist1").rightPush("张飞");        
    }
    
    /**
     * 显示右压栈集合
     */
    @Test
    public void testGetValue1(){
        List list = redisTemplate.boundListOps("namelist1").range(0, 10);
        System.out.println(list);
    }
```

```java
		/**
     * 左压栈：后添加的对象排在前边
     */
    @Test
    public void testSetValue2(){        
        redisTemplate.boundListOps("namelist2").leftPush("刘备");
        redisTemplate.boundListOps("namelist2").leftPush("关羽");
        redisTemplate.boundListOps("namelist2").leftPush("张飞");        
    }
    
    /**
     * 显示左压栈集合
     */
    @Test
    public void testGetValue2(){
        List list = redisTemplate.boundListOps("namelist2").range(0, 10);
        System.out.println(list);
    }
```

```java
		/**
     * 查询集合某个元素
     */
    @Test
    public void testSearchByIndex(){
        String s = (String) redisTemplate.boundListOps("namelist1").index(1);
        System.out.println(s);
    }
```

```java
		/**
     * 移除集合某个元素
     */
    @Test
    public void testRemoveByIndex(){
        redisTemplate.boundListOps("namelist1").remove(1, "关羽");
    }
```

### Hash操作

```java
    @Test
    public void testSetValue(){
        redisTemplate.boundHashOps("namehash").put("a", "唐僧");
        redisTemplate.boundHashOps("namehash").put("b", "悟空");
        redisTemplate.boundHashOps("namehash").put("c", "八戒");
        redisTemplate.boundHashOps("namehash").put("d", "沙僧");
    }
```

```java
    @Test
    public void testGetKeys(){
        Set s = redisTemplate.boundHashOps("namehash").keys();        
        System.out.println(s);        
    }
```

```java
    @Test
    public void testGetValues(){
        List values = redisTemplate.boundHashOps("namehash").values();
        System.out.println(values);        
    }
```

```java
    @Test
    public void testGetValueByKey(){
        Object object = redisTemplate.boundHashOps("namehash").get("b");
        System.out.println(object);
    }
```

```java
    @Test
    public void testRemoveValueByKey(){
        redisTemplate.boundHashOps("namehash").delete("c");
    }
```

