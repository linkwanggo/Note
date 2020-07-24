# 关于Vue model改变但数据未刷新的情况

其实问题应该是产生于某个大json里的某个字段(也是json)在初始化时没有指定里面的key，在后面没有通过Vue.set()去赋值，而是用=去赋值所导致，其实不用$forceUpdate也能解决这个问题。 这篇说得挺详细↓ http://www.qiutianaimeili.com/html/page/2019/03/7802qotr1x9.html

