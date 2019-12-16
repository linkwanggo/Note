# SpringBoot-文件上传&文件下载&静态资源访问Demo

## 1.文件上传

### 1.配置上传文件属性（自定义） application.properties

**注意**：最后添加**"/"**！！！

```properties
# 文件上传配置
# windows下的文件上传路径  注意最后添加"/"！！！
file.winUploadFolder=I:/Data/SpringBootUploadData/files/
# mac下文件上传路径
file.macUploadFolder=/home/linkwanggo/Data/SpringBootUploadData/files/
```

### 2. 配置文件上传参数 （MultipartConfig）

```java
/**
 * 系统加载时配置文件上传属性
 * 1. 指定文件上传的BasePath
 */
@Configuration
public class MultipartConfig {

    @Value("${file.winUploadFolder}")
    private String winUploadFolder;

    @Value("${file.macUploadFolder}")
    private String macUploadFolder;

    @Bean
    public MultipartConfigElement multipartConfigElement() {
        MultipartConfigFactory factory = new MultipartConfigFactory();
        Map<String, String> envMap = System.getenv();
        String osName = envMap.get("OS");
        if (osName.toLowerCase().startsWith("win")) {
            // 创建文件存放目录
            File winUploadFolderFile = new File(winUploadFolder);
            if (!winUploadFolderFile.exists()) {
                winUploadFolderFile.mkdirs();
            }
            factory.setLocation(winUploadFolder);
        } else {
            // TODO: 2019/10/30 暂时未知mac及linux下的OS名称
        }
        return factory.createMultipartConfig();
    }
}
```

### 3. FileController -----> upload

```java
@RestController
public class FileController {

    @Value("${file.winUploadFolder}")
    private String winUploadFolder;

    @PostMapping("/upload")
    public String upload(@RequestParam("uploadFile") MultipartFile uploadFile) {
        if (uploadFile.isEmpty()) {
            return "isEmpty";
        }
        String fileName = uploadFile.getOriginalFilename();
        String uuid = UUID.randomUUID().toString().replaceAll("-", "");
        fileName = uuid + "_" + fileName;
        try {
            uploadFile.transferTo(new File(fileName));
            // TODO: 2019/10/31 存入数据库 
        } catch (IOException e) {
            e.printStackTrace();
        }
        return "success";
    }
}
```

## 2. 文件下载

### 1. 解决不同浏览器下载中文乱码的问题（DownloadUtils）

```java
public class DownloadUtils {

    public static String getFileName(String agent, String filename) throws UnsupportedEncodingException {
        if (agent.contains("MSIE")) {
            // IE浏览器
            filename = URLEncoder.encode(filename, "utf-8");
            filename = filename.replace("+", " ");
        } else if (agent.contains("FireFox")) {
            // 火狐浏览器
            BASE64Encoder base64Encoder = new BASE64Encoder();
            filename = "=?utf-8?B?" + base64Encoder.encode(filename.getBytes(StandardCharsets.UTF_8)) + "?=";
        } else {
            // 其他浏览器
            filename = URLEncoder.encode(filename, "utf-8");
        }
        return filename;
    }
}
```

### 2. FileController ------> download

```java
@RestController
public class FileController {

    @Value("${file.winUploadFolder}")
    private String winUploadFolder;

    @GetMapping("/download/{fileName}")
    public String downLoad(HttpServletRequest request,
                           HttpServletResponse response,
                           @PathVariable("fileName") String fileName) {
        // TODO: 2019/10/31 数据库查询文件是否存在
        // 模拟文件下载
        File downloadFile = new File(winUploadFolder, fileName);
        if (!downloadFile.exists()) {
            return "文件不存在";
        }
        try {
            FileInputStream fis = new FileInputStream(downloadFile);
            // 设置相关格式
            response.setContentType("application/force-download");
            // 设置下载后的文件名以及header
            String userAgent = request.getHeader("User-Agent");
            response.addHeader("Content-disposition", "attachment;fileName=" + DownloadUtils.getFileName(userAgent, fileName));
            OutputStream os = response.getOutputStream();
            // 常规操作
            byte[] buff = new byte[1024];
            int len = 0;
            while ((len = fis.read(buff)) != -1) {
                os.write(buff, 0, len);
            }
            fis.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return "成功";
    }
}
```

## 3. 静态资源访问

* addResourceHandler: 表示浏览器需要访问的地址
* addResourceLocations: 项目会去寻找的文件目录

```java
@Configuration
public class WebMVCConfig implements WebMvcConfigurer {

    @Value("${file.winUploadFolder}")
    private String winUploadFolder;

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        /* 添加对外部静态资源的访问 */
        registry.addResourceHandler("/static/**").addResourceLocations("file:" + winUploadFolder);
    }
}
```

# ---END---

