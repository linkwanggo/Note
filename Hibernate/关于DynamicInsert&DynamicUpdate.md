# 关于DynamicInsert&DynamicUpdate 

@DynamicInsert属性:设置为true,设置为true,表示insert对象的时候,生成动态的insert语句,如果这个字段的值是null就不会加入到insert语句当中.默认false。
比如希望数据库插入日期或时间戳字段时，在对象字段为空的情况下，表字段能自动填写当前的sysdate。

@DynamicUpdate属性:设置为true,设置为true,表示update对象的时候,生成动态的update语句,如果这个字段的值是null就不会被加入到update语句中,默认false。
比如只想更新某个属性，但是却把整个对象的属性都更新了，这并不是我们希望的结果，我们希望的结果是：我更改了哪些字段，只要更新我修改的字段就够了。

举例说明

看下面打印的sql语句就会立刻明白,使用这两个注解的效果

@DynamicInsert注解下Hibernate日志打印SQL：

```
Hibernate: insert into Cat (cat_name, id) values (?, ?)  
```

反之

```
Hibernate: insert into Cat (create_time, update_time, cat_name, id) values (?, ?, ?, ?)  
```

@DynamicUpdate注解下Hibernate日志打印SQL：

说明：如果字段有更新，Hibernate才会对该字段进行更新

```
Hibernate: update Cat set update_time=? where id=?
```

例如

```java
import lombok.Data;
import org.hibernate.annotations.DynamicInsert;
import org.hibernate.annotations.DynamicUpdate;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;
import java.math.BigDecimal;
import java.sql.Timestamp;

/**

@author itguang
@create 2017-11-25 14:02
**/
@Entity
@DynamicInsert
@DynamicUpdate
@Data
@Table(name = "product_info")
public class ProductInfoEntity {

@Id
@Column(name = "product_id")
private String productId;
private String productName;
private BigDecimal productPrice;
private int productStock;
private String productDescription;
private String productIcon;
private Integer productStatus;
private int categoryType;
private Timestamp createTime;
private Timestamp updateTime;
```

总结:

如果我们在更新表时,只想更新某个字段,就不要加 @DynamicUpdate,通常为了更新表时的效率,都是不加的.

反之,如果我们更新某个字段时,更新所有的字段,就可以加上 @DynamicUpdate.

@DynamicInsert 同理.