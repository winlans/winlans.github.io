---
title: doctrine queryBuilder
categories:
- symfony
tags:
- orm
- symfony
---



为了能够方便的切换数据库，我们有必要使用doctrine的queryBuilder， 但是估计很多人都是不喜欢的(我也是)，

之前尝试用的时候，发现在doctrine定义的`SELECT`	语法中并没有`CONCAT, GROUP_CONCAT` 这些有时候会用到的函数，于是就放弃了，今天才发现原来我们还可以这么用。。。。

```php
$em = $this->getEntityManager();
$qb = $em->createQueryBuilder();
$qb->add('select', 'concat(\'ttt\', u.id)')
          ->from(User::class, 'u')
          ->where($qb->expr()->eq('u.id','?1'))->setParameter(1,171);
$res = $qb->getQuery()->getArrayResult();
var_dump($res);exit;
```

