# VueElementAdmin设置默认主题（Theme）失效的解决方案

[cover](https://tva1.sinaimg.cn/large/00831rSTly1gdol7lduxej30np0j476g.jpg)

## BUG产生原因:

进行Theme切换的关键监听函数`async theme(val)`放在了`defaultTheme`之后！

理论上的执行流程应该是`async theme(val)` 监听到`defaultTheme`中的`theme`发生变化后执行主题替换操作，

而BUG代码由于将`theme(val)`放在了`defaultTheme`之后，导致`defaultTheme`监听到`theme`发生变更后，`async theme(val)`却还未加载完成处于未生效状态，因此第一次的主题替换操作自然就会block掉。

```javascript
  computed: {
    defaultTheme() {
      return this.$store.state.settings.theme
    }
  },
  watch: {
    defaultTheme: {
      handler: function(val, oldVal) {
        this.theme = val
      },
      immediate: true
    },
    async theme(val) {
      const oldVal = this.chalk ? this.theme : ORIGINAL_THEME
      if (typeof val !== 'string') return
      const themeCluster = this.getThemeCluster(val.replace('#', ''))
      const originalCluster = this.getThemeCluster(oldVal.replace('#', ''))
      console.log(themeCluster, originalCluster)

      const $message = this.$message({
        message: '  Compiling the theme',
        customClass: 'theme-message',
        type: 'success',
        duration: 0,
        iconClass: 'el-icon-loading'
      })

      const getHandler = (variable, id) => {
        return () => {
          const originalCluster = this.getThemeCluster(ORIGINAL_THEME.replace('#', ''))
          const newStyle = this.updateStyle(this[variable], originalCluster, themeCluster)

          let styleTag = document.getElementById(id)
          if (!styleTag) {
            styleTag = document.createElement('style')
            styleTag.setAttribute('id', id)
            document.head.appendChild(styleTag)
          }
          styleTag.innerText = newStyle
        }
      }

      if (!this.chalk) {
        const url = `https://unpkg.com/element-ui@${version}/lib/theme-chalk/index.css`
        await this.getCSSString(url, 'chalk')
      }

      const chalkHandler = getHandler('chalk', 'chalk-style')

      chalkHandler()

      const styles = [].slice.call(document.querySelectorAll('style'))
        .filter(style => {
          const text = style.innerText
          return new RegExp(oldVal, 'i').test(text) && !/Chalk Variables/.test(text)
        })
      styles.forEach(style => {
        const { innerText } = style
        if (typeof innerText !== 'string') return
        style.innerText = this.updateStyle(innerText, originalCluster, themeCluster)
      })

      this.$emit('change', val)

      $message.close()
    }
  },
```

## 解决方案：

调整监听函数`async theme(val)`和`defaultTheme`的顺序，让`async theme(val)`优先加载，即可解决掉BUG。

## 修改默认主题的流程：

### 修改`src/styles/element-variables.scss`

添加一个自己喜欢的主题颜色

```
/* theme color */
$--color-theme: #304156;
```

修改导出的默认主题

```
// the :export directive is the magic sauce for webpack
// https://www.bluematador.com/blog/how-to-share-variables-between-js-and-sass
:export {
  theme: $--color-theme;
}
```
## ---END---

