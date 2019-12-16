# Spring基于配置的事务控制银行转账Demo

## 1. bean.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置accountService -->
    <bean id="accountService" class="com.linkwanggo.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>

    <!-- 配置accountDao -->
    <bean id="accountDao" class="com.linkwanggo.dao.impl.AccountDaoImpl">
        <property name="jdbcTemplate" ref="jdbcTemplate"></property>
    </bean>

    <!-- 配置JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 配置 datasource -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/eesy03"></property>
        <property name="username" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>

    <!-- 配置transactionManager事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 配置tx通知 -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*"></tx:method>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <!-- 配置AOP-->
    <aop:config>
        <!-- 配置切点 -->
        <aop:pointcut id="pi1" expression="execution(* com.linkwanggo.service.impl.*.*(..))"></aop:pointcut>
        <!--建立切入点表达式和事务通知的对应关系 -->
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pi1"></aop:advisor>
    </aop:config>
</beans>
```

## 2. accountDao

* accountDao使用了Spring默认的jdbcTemlate作为数据库操作对象，采用set方式注入。Spring提供的DriverManagerDataSource作为数据源。

  ```java
  public class AccountDaoImpl implements IAccountDao {
  
      private JdbcTemplate jdbcTemplate;
  
      public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
          this.jdbcTemplate = jdbcTemplate;
      }
  
      public Account findAccountByName(String username) {
          List<Account> accountList = jdbcTemplate.query("select * from account where username = ?", new BeanPropertyRowMapper<Account>(Account.class), username);
          return accountList.isEmpty() ? null : accountList.get(0);
      }
  
      public Account findAccountById(Integer accountId) {
          List<Account> accountList = jdbcTemplate.query("select * from account where id = ?", new BeanPropertyRowMapper<Account>(Account.class), accountId);
          if (accountList.isEmpty()) {
              return null;
          }
          if (accountList.size() > 1) {
              throw new RuntimeException("结果集不唯一");
          }
          return accountList.get(0);
      }
  
      public void updateAccount(Account account) {
          jdbcTemplate.update("update account set username = ?, money = ? where id = ?", account.getUsername(), account.getMoney(), account.getId());
      }
  }
  ```

## 3. accountService模拟转账操作（方法中有多次数据库操作）

```java
public class AccountServiceImpl implements IAccountService {

    private IAccountDao accountDao;

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    /**
     * 转账操作
     * @param sourceName
     * @param targetName
     * @param money
     */
    public void transfer(String sourceName, String targetName, Float money) {
        Account source = accountDao.findAccountByName(sourceName);
        Account target = accountDao.findAccountByName(targetName);
        source.setMoney(source.getMoney() - money);
        target.setMoney(target.getMoney() + money);
        accountDao.updateAccount(source);
        int i = 1 / 0;
        accountDao.updateAccount(target);
    }
}
```

## 4. Junit单元测试

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:bean.xml")
public class AccountTest {

    @Autowired
    private IAccountService accountService;

    @Test
    public void testTransfer() {
        accountService.transfer("aaa", "bbb", 100f);
    }
}
```

## 5. 运行结果

在accountService中产生异常后，Spring仍然正常的将事务进行了回滚，Demo成功！

![期望的运行异常](https://i.loli.net/2019/10/28/sVgAMmY9GN8np3f.png)

**数据库结果：**数据并未发生改变！

![数据并未改变](https://i.loli.net/2019/10/28/Z6tyTWamcUO3ISr.png)

## ----END----

