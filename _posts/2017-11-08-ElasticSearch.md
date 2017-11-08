---
title: ElasticSearch
date: 2017-11-08 15:45:20
category: 
- ES
tags:
- elastic
description: 
---
## ElasticSearch

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
  >  },
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






测试有没有增量更新
