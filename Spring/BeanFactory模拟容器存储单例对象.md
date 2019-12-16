# BeanFactory模拟容器存储单例对象

## 1. BeanFactory代码实现

```java
package com.linkwanggo.factory;

import java.io.InputStream;
import java.util.Enumeration;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

/**
 * 工厂模式
 * BeanFactory作为一个容器，存储DAO及Service单例模式实例。
 * 目的：1.将编译期错误转换为运行时错误。
 *      2.通过配置的方式加载类，实现程序间的解耦 （将new实例转化为从工厂中获取实例）
 */
public class BeanFactory {

    // 存储Bean的容器
    private static Map<String, Object> beanMap;

    // 配置实例
    private static Properties properties;

    // 读取配置，加载类的实例，实现单例模式
    static {
        try {
            // 1.将配置文件载入到properties
            properties = new Properties();
            InputStream is = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            properties.load(is);
            // 2.将properties中的数据载入到beanMap中
            beanMap = new HashMap<>();
            Enumeration<Object> keys = properties.keys();
            while (keys.hasMoreElements()) {
                String key = keys.nextElement().toString();
                String className = properties.getProperty(key);
                Object classInstance = Class.forName(className).newInstance();
                beanMap.put(key, classInstance);
            }
            System.out.println(111);
        } catch (Exception e) {
            throw new ExceptionInInitializerError("读取配置文件失败！");
        }
    }

    public static Object getBean(String beanName) {
        return beanMap.get(beanName);
    }
}

```

## 2. bean.properties(注意：对象的加载顺序！！！)

```
accountService=com.linkwanggo.service.AccountServiceImpl
accountDao=com.linkwanggo.dao.AccountDaoImpl
```

## 3. Client启动类

```java
package com.linkwanggo;

import com.linkwanggo.dao.IAccountDao;
import com.linkwanggo.factory.BeanFactory;
import com.linkwanggo.service.IAccountService;

public class Client {

    public static void main(String[] args) {

        IAccountService accountService = (IAccountService) BeanFactory.getBean("accountService");
        for (int i = 0; i < 5; i++) {
            System.out.println(accountService);
            accountService.saveAccount();
        }
    }
}

```

## 4.结果------>单例对象容器化成功！

```java
com.linkwanggo.service.AccountServiceImpl@677327b6
保存用户
com.linkwanggo.service.AccountServiceImpl@677327b6
保存用户
com.linkwanggo.service.AccountServiceImpl@677327b6
保存用户
com.linkwanggo.service.AccountServiceImpl@677327b6
保存用户
com.linkwanggo.service.AccountServiceImpl@677327b6
保存用户
```



