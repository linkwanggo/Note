# 错误信息

```
WebDriverException: Message: unknown error: Chrome failed to start: exited abnormally
  (unknown error: DevToolsActivePort file doesn't exist)
  (The process started from chrome location /usr/bin/chromium is no longer running, so ChromeDriver is assuming that Chrome has crashed.)
  (Driver info: chromedriver=73.0.3683.75,platform=Linux 5.0.2-aml-s905 aarch64)
```

## 解决方案

为chrome添加额外配置

```python
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('--no-sandbox')
chrome_options.add_argument('--disable-dev-shm-usage')
chrome_options.add_argument('--headless')
browser = webdriver.Chrome(chrome_options=chrome_options)
```

 其中
“–no-sandbox”参数是让Chrome在root权限下跑
“–headless”参数是不用打开图形界面
可以额外加这些参数获得更好体验

```python
chrome_options.add_argument('blink-settings=imagesEnabled=false')
chrome_options.add_argument('--disable-gpu')
```

