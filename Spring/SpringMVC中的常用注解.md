# SpringMVC中的常用注解

## 1. RequestParam

```java
@GetMapping("/requestParam")
public String requestParam(@RequestParam("username") String username) {
    return "success";
}
```



## 2. RrquestBody

* 注意：requestBody只能接受application/json类型的post参数

```java
@PostMapping("requestBody")
public String requestBody(@RequestBody User user) {
    return "success";
}
```



## 3. PathVariable

* 注意：pathVariable只能获取get请求路径中的参数

```java
@GetMapping("/pathVariable/{username}")
public String pathVariable(@PathVariable("username") String username) {
    return "success";
}
```



## 4. RequestHeader

* 作用：用于获取请求消息头， 一般不用

  ```java
  @RequestMapping("/requestHeader")
  public String requestHeader(@RequestHeader("user-agent") String userAgent) {
      return "success";
  }
  ```

  

## 5. CookieValue

* 获取cookie中key对应的value

  ```java
  @RequestMapping("/cookieValue")
  public String cookieValue(@CookieValue("JSESSIONID") String JSESSIONID) {
      return "success";
  }
  ```

  

## 6. ModelAttribute

* 作用：当form表单参数提交不完整时，使用@ModelAttribute修饰的方法能够在请求方法执行之间执行，帮助补充参数的缺失，保证缺失的字段能够和数据库中的字段值保持一致，避免数据库数据丢失。
* **详情请参考案例：springmvc_day01_01_start案例**

## 7. SessionAttribute

* 作用：用于将request中的参数加载到session中，扩大参数的作用范围，实现在方法中进行参数共享

  ```java
  /**
   * 常用的注解
   */
  @Controller
  @RequestMapping("/anno")
  @SessionAttributes(value={"msg"})   // 把msg=美美存入到session域对中
  public class AnnoController {
  
      /**
       * 获取Cookie的值
       * @return
       */
      @RequestMapping(value="/testCookieValue")
      public String testCookieValue(@CookieValue(value="JSESSIONID") String cookieValue){
          System.out.println("执行了...");
          System.out.println(cookieValue);
          return "success";
      }
  
      /**
       * ModelAttribute注解
       * @return
       */
      @RequestMapping(value="/testModelAttribute")
      public String testModelAttribute(@ModelAttribute("abc") User user){
          System.out.println("testModelAttribute执行了...");
          System.out.println(user);
          return "success";
      }
  
      @ModelAttribute
      public void showUser(String uname, Map<String,User> map){
          System.out.println("showUser执行了...");
          // 通过用户查询数据库（模拟）
          User user = new User();
          user.setUname(uname);
          user.setAge(20);
          user.setDate(new Date());
          map.put("abc",user);
      }
  
      /**
       * 该方法会先执行
  
      @ModelAttribute
      public User showUser(String uname){
          System.out.println("showUser执行了...");
          // 通过用户查询数据库（模拟）
          User user = new User();
          user.setUname(uname);
          user.setAge(20);
          user.setDate(new Date());
          return user;
      }
       */
  
      /**
       * SessionAttributes的注解
       * @return
       */
      @RequestMapping(value="/testSessionAttributes")
      public String testSessionAttributes(Model model){
          System.out.println("testSessionAttributes...");
          // 底层会存储到request域对象中
          model.addAttribute("msg","美美");
          return "success";
      }
  
      /**
       * 获取值
       * @param modelMap
       * @return
       */
      @RequestMapping(value="/getSessionAttributes")
      public String getSessionAttributes(ModelMap modelMap){
          System.out.println("getSessionAttributes...");
          String msg = (String) modelMap.get("msg");
          System.out.println(msg);
          return "success";
      }
  
      /**
       * 清除
       * @param status
       * @return
       */
      @RequestMapping(value="/delSessionAttributes")
      public String delSessionAttributes(SessionStatus status){
          System.out.println("getSessionAttributes...");
          status.setComplete();
          return "success";
      }
  
  }
  
  ```

  

## 更详细请参考：spring5mvc第一天【大纲笔记】.pdf