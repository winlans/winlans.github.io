---
title: doctrine queryBuilder
categories:
- symfony
tags:
- orm
- symfony
---

为了能够方便的切换数据库，我们有必要使用doctrine的queryBuilder， 但是估计很多人都是不喜欢的(我也是)，之前尝试用的时候，发现在doctrine定义的`SELECT`语法中并没有`CONCAT, GROUP_CONCAT` 这些有时候会用到的函数，于是就放弃了，今天才发现原来我们还可以这么用。。。。

> 需要特别注意的是我们在写字段名字的时候，例如`user_id` ,这种就要写成 `userId`, 与doctrine 定义entity的语法是一致的。

<!-- more -->

```php
$em = $this->getEntityManager();
$qb = $em->createQueryBuilder();
$qb->add('select', 'concat(\'ttt\', u.id)')
          ->from(User::class, 'u')
          ->where($qb->expr()->eq('u.id','?1'))->setParameter(1,171);
$res = $qb->getQuery()->getArrayResult();
var_dump($res);exit;
```

# where语句

```php
$qb = $this->getEntityManager()->createQueryBuilder();
$qu = $qb->select('u')
    ->from(User::class, 'u')
    ->where($qb->expr()->andX(
        $qb->expr()->eq('u.profileId', 9178),
        $qb->expr()->eq('u.logNum', '1'),
        $qb->expr()->eq('u.id', 177)
    ))
    ->getQuery();

$res = $qu->getArrayResult();
var_dump($res);exit;
```

下面的与上面的效果是一致的。

```php
$qb = $this->getEntityManager()->createQueryBuilder();
$qu = $qb->select('u')
    ->from(User::class, 'u')
    ->andWhere($qb->expr()->eq('u.logNum', '1'))
    ->andWhere($qb->expr()->eq('u.profileId', 9178))
    ->where($qb->expr()->eq('u.id', 177))
    ->getQuery();

$res = $qu->getArrayResult();
var_dump($res);exit;
```