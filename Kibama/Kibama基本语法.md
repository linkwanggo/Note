# Kibama基本语法

```
# 新增
POST articleindex/article
{
  "title": "elasticsearch入门",
  "content": "基础入门"
}

# 查询全部
GET articleindex/article/_search
{
  "query": {
    "match_all": {}
  }
}

# 按ID查询文档
GET articleindex/article/1

# 基本条件查询
GET articleindex/article/_search
{
  "query": {
    "match": {
      "title": "十次方"
    }
  }
}

# 修改 
PUT articleindex/article/Zk_kwW8BAuYJaNWna477
{
  "title":"SpringBoot2.0正式版",
  "content": "发布了吗"
}

# 修改 【如果我们在地址中的ID不存在，则会创建新文档】
PUT articleindex/article/1
{
  "title": "十次方课程好给力",
  "content": "知识点很多"
}

# 删除【一般用update代替】
DELETE articleindex/article/1

# IK分词
# 普通分词器【无法给中文智能分词】
GET _analyze
{
  "analyzer": "chinese",
  "text": ["我是程序员"]
}

# IK分词器 【ik_smart】
GET _analyze
{
  "analyzer": "ik_smart",
  "text": ["我是程序员"]
}

# IK分词器 【ik_max_word】
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": ["我是程序员"]
}

# 自定义词库 【默认分词将“传智播客”识别为词】
# 1. 进入 /usr/local/var/homebrew/linked/elasticsearch-full/libexec/plugins/ik/config
GET _analyze
{
  "analyzer": "ik_smart", 
  "text": ["传智播客"]
}

# 测试
GET tensquare/article/_search
{
  "query": {
    "match_all": {}
  }
}


```

