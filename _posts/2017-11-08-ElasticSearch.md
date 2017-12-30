---
title: ElasticSearch
date: 2017-11-08 15:45:20
category: 
- elastic
tags:
- elastic
description: 主要记录工作中，碰到的问题， 只是为了记录，所以开了这篇。
---
> 新入手的elasticsearch， 很多都不太懂，这里打算记录一些关于es的坑坑，或许对很多人来说，这根本就不能算是坑，但是我不会啊。。。。。

- range搜索时间的时候不生效，指定了查询的日期格式之后正常。

  > mapping
  >
  > ```
  > "created": {
  >           "type": "date",
  >           "format": "yyyy-MM-dd HH:mm:ss"
  >         },
  >  "updated": {
  >           "type": "date",
  >           "format": "yyyy-MM-dd HH:mm:ss"
  >  		},
  > ```
  >
  > query 
  >
  > ```json
  > "range": {
  >       "created": {
  >        "gt": "2015-10-9 00:21:20",
  >         "format": "yyyy-MM-dd HH:mm:ss"
  >          }
  > }
  > ```
  >
  > ​

- 指定字段为date，但是索引数据的时候失败了

  ```
  要索引的日期  2017-11-02 12:21:00

  "created": {
       "type": "date"
  },
  ```

  具体的不清楚是为什么，但是加上了日期格式之后就正常了

  ```
  "created": {
       "type": "date",
       "format": "yyyy-MM-dd HH:mm:ss"
  },
  ```



### ps

- 我们知道，es中的存储方式与关系型数据库中的有点类似:

  | elasticSearch |  mysql   |
  | :-----------: | :------: |
  |     index     | database |
  |     type      |  table   |
  |     field     |  field   |

  但是刚才看文档的时候发现， es 已经计划移除type这个概念了， 但是这是为什么呢？原来，我们总是认为es的存储方式类似上表， 但是其实他们是有很大的不同的。MySQL一个库中不同表之间是相互独立的，不同表之间的同名字段也是不会造成任何影响的，而在ES中却不同，同一个index下， 不同type间同名的field的映射定义必须是完全一致的，可能你会碰到这么一种情况。 在同一个索引下，有A 和 B 两种类型， 但是当你想删除A类型中的 date 字段和B类型中的name字段时，这时就尴尬了，删除不了。而且， 少量的字段差异，会导致数据分散，使底层Lucence压缩文档的效率降低。



测试有没有增量更新