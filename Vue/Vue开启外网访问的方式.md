# Vue开启外网访问的方式

## 1. 修改config/index.js

```js
module.exports = {
  dev: {
    host: '0.0.0.0', // 将 host设置为 0.0.0.0
    port: 9001, // can be overwritten by process.env.PORT, if port is in use, a free one will be determined
    ..........
  },
```

## 2. 在build/webpack.dev.conf.js的WebpackDevServer中配置useLocalIp: true

**这样打开页面就会是ip:port的形式，同时你也可以通过localhost:port或127.0.0.1:port打开页面。**

```js
const devWebpackConfig = merge(baseWebpackConfig, {
  mode: 'development',
  devtool: config.dev.devtool,
  //...
  devServer: {
    useLocalIp: true,//避免打开浏览器为0.0.0.0，需手动改IP的情况
    //...
  },
  //...
}
```

# ---END---