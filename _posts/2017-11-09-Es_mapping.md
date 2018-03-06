---
title: Es_mappings
date: 2017-11-09 14:42:00
categories:
- elastic
tags:
- elastic
description: 主要是一些基础的es_mappings 知识
---

elasticsearch 中的mappings，有点类似与关系型数据库中每张表的约束，例如，字段类型，字段大小 。。。等等，但是es的mappings却要比其灵活得多。es 中的type就和关系型数据库的表差不多了，呵呵。

<!-- more -->

# 创建mappings

curl -XGET 'localhost:9200/_mapping/tweet?pretty'
curl -XGET 'localhost:9200/_all/_mapping/tweet?pretty'