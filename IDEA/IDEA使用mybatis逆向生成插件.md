```
#### 1. 引入mybatis、mybatis-generator-core依赖

2.x版本会出现不兼容的情况，建议使用1.x

​```
<!--mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>

<!-- mybatis-generator-core   mapper + pojo生成器-->
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.3.7</version>
</dependency>
​```

#### 2. 配置generatorConfig.xml

在pom.xml的同级目录下创建generatorConfig.xml

​```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <context id="MysqlContext" targetRuntime="MyBatis3Simple" defaultModelType="flat">
        <property name="beginningDelimiter" value="`">
        <property name="endingDelimiter" value="`"/>

        <plugin type="tk.mybatis.mapper.generator.MapperPlugin">
            <property name="mappers" value="com.linkwanggo.springbootdemo.util.MyMapper"/>
        </plugin>

        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/tumo?useUnicode=true&amp;characterEncoding=utf-8"
                        userId="root"
                        password="1383852WANgwe!">
        </jdbcConnection>

        <!-- 对于生成的pojo所在包 -- >
        <javaModelGenerator targetPackage="com.linkwanggo.springbootdemo.pojo" targetProject="src/main/java"/>

        <!-- 对于生成的mapper所在目录 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources"/>

        <!-- 配置mapper对应的java映射 -->
        <javaClientGenerator targetPackage="com.linkwanggo.springbootdemo.mapper" targetProject="src/main/java"
                             type="XMLMAPPER"/>


        <table tableName="tb_user"></table>

    </context>
</generatorConfiguration>
​```

#### 3. 在util(其他位置也可以)创建GeneratorDisplay.java类

​```
package com.linkwanggo.springbootdemo.util;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

public class GeneratorDisplay {
    public void generator() throws Exception{

        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        //指定 逆向工程配置文件
        File configFile = new File("generatorConfig.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                callback, warnings);
        myBatisGenerator.generate(null);

    }

    public static void main(String[] args) throws Exception {
        try {
            GeneratorDisplay generatorSqlmap = new GeneratorDisplay();
            generatorSqlmap.generator();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
​```
#### 4. 运行创建GeneratorDisplay.java类，得到相应的mapper, pojo, mapper.xml
生成结果如下所示：
​```
package com.linkwanggo.springbootdemo.pojo;

import lombok.Data;

import javax.persistence.*;

@Data
@Table(name = "tb_user")
public class TbUser {
    /**
     * 编号
     */
    @Id
    private Long id;

    /**
     * 用户名
     */
    private String username;

    /**
     * 昵称
     */
    private String nickname;

    /**
     * 密码
     */
    private String password;

    /**
     * 盐值
     */
    private String salt;

    /**
     * 邮箱
     */
    private String email;

    /**
     * 头像
     */
    private String avatar;
}
​```

##### --END--
```