---
title:
date:
categories:
tags:
---

operator  有and or 两个选项， 感觉应该是es词条的关系，比如 `在哪里` 被分解为 `在` `哪里`  如果是 and 那么只有同时拥有这两个词条的记录才能被匹配到， or 则是满足其一即可

```json
{
  "min_score": 8,
  "query": {
    "match": {
      "msg": {
        "query": "在哪里",
        "operator": "and", // or
        "zero_terms_query": "all"
      }
    }
  },
  "suggest": {
    "my-suggestion": {
      "text": "明天去哪里",
      "term": {
        "field": "msg"
      }
    }
  }
}
```
