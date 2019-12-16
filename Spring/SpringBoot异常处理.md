# SpringBoot异常处理

## 1. 自定义Exception

**注意：**必须继承RuntimeException !!!

```java
public class CommonException extends RuntimeException {

    public CommonException(String message) {
        super(message);
    }
    
}
```

## 2. 编写异常处理通知（CommonExceptionAdvice）

```java
@RestControllerAdvice
public class CommonExceptionAdvice {

    @ExceptionHandler(value = CommonException.class)
    public CommonResponse<String> handleException(HttpServletRequest request,
                                                  CommonException ce) {
        CommonResponse<String> response = new CommonResponse<>(
                CommonResponseStatus.ERROR.getStatus(),
                CommonResponseStatus.ERROR.getDesc());
        response.setData(ce.getMessage());
        return response;
    }
}
```

## 代码请查看Mapleleaf项目

# ---END---

