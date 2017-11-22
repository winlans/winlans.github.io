```php
$em = $this->getEntityManager();
$qb = $em->createQueryBuilder();
$qb->add('select', 'concat(\'ttt\', u.id)')
          ->from(User::class, 'u')
          ->where($qb->expr()->eq('u.id','?1'))->setParameter(1,171);
$res = $qb->getQuery()->getArrayResult();
var_dump($res);exit;
```