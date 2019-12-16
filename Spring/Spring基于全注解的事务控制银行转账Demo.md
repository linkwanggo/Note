# Spring基于全注解的事务控制银行转账Demo

* 总览：
  * **SpringConfiguration**配置类，代替了bean.xml的作用
  * **JdbcConfig**配置类，提供DataSource和JdbcTemplate
  * **TransactionConfig**配置类，提供TransactionManager
  * **AccountDaoImpl**, 提供持久层服务
  * **AccountServiceImpl**, 提供业务层转账服务
  * **jdbc.properties**，提供数据库连接配置
  * **AccountTest**, 提供测试类

## **1. SpringConfiguration**

```java
/**
 * 代替bean.xml的作用，实现Spring的全注解配置
 */

@Configuration
@ComponentScan("com.linkwanggo")
@PropertySource("classpath:jdbc.properties")
@EnableTransactionManagement
@Import({JdbcConfig.class, TransactionConfig.class})
public class SpringConfiguration {

}
```

## 2. JdbcConfig

```java
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }

    @Bean
    public DataSource getDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

## 3. TransactionConfig

```java
public class TransactionConfig {

    /**
     * 用于创建事务管理器对象
     * @param dataSource
     * @return
     */
    @Bean("transactionManager")
    public PlatformTransactionManager getPlatformTransactionManager(DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }
}
```

## 4. AccountDaoImpl

```java
@Repository
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private JdbcTemplate jdbcTemplate;

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

## 5. AccountServiceImpl

```java
@Service
public class AccountServiceImpl implements IAccountService {

    @Autowired
    private IAccountDao accountDao;

    /**
     * 转账操作
     * @param sourceName
     * @param targetName
     * @param money
     */
    @Transactional(propagation = Propagation.REQUIRED)
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

## 6. jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/eesy03
jdbc.username=root
jdbc.password=123456
```

## 7. AccountTest

```java
@RunWith(SpringJUnit4ClassRunner.class)
// 全注解时需要使用classes指定主配置类
@ContextConfiguration(classes = SpringConfiguration.class) 
public class AccountTest {

    @Autowired
    private IAccountService accountService;

    @Test
    public void testTransfer() {
        accountService.transfer("aaa", "bbb", 100f);
    }
}
```

## ---END---