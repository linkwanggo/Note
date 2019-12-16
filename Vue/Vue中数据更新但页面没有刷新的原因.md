# Vue中数据更新但页面没有刷新的原因

## 遇到的问题：

### 1. 如图所示

图片和商品信息无法跟随鼠标选中的特定sku进行切换..... 有点郁闷

![](https://i.loli.net/2019/11/22/St5LGW7ywRKAUhV.png)

错误代码：

```javascript
loadData() {
    ly.http.post("/search/page", this.search).then(res => {
        const { data } = res;
        this.total = data.total;
        this.totalPage = data.totalPage;
        this.goodsList = data.items;
        // 将goods中的skus处理成json对象
        his.goodsList.forEach(goods => {
            goods.skus = JSON.parse(goods.skus);
            // 初始化被选中的sku
            goods.selectedSku = goods.skus[0];
        });
    }).catch(err => {
        console.log(err);
    })
}
```

正确代码：

```javascript
loadData() {
    ly.http.post("/search/page", this.search).then(res => {
        const { data } = res;
        this.total = data.total;
        this.totalPage = data.totalPage;
        // 将goods中的skus处理成json对象
        data.items.forEach(goods => {
            goods.skus = JSON.parse(goods.skus);
            // 初始化被选中的sku
            goods.selectedSku = goods.skus[0];
        });
        // 将数据处理完成后再赋值给Vue对象，这样便能够监测数据的变化
        this.goodsList = data.items;
    }).catch(err => {
        console.log(err);
    })
}
```

### 2. 错误原因

**selectedSku没有正确加载成为Vue的观察对象，导致selectedSku数据变更时无法触发Vue刷新页面**

# ---END---

