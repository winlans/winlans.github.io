---
title: php 减少mysql查询大量数据的内存占用
date: 2018-1-12 19:00:00
categories:
- php
tags:
- optimizing

---



> 有时候我们需要从MySQL中查询大量的数据， 如果直接放入到数组中， 再去遍历的很可能会产生内存饱满的情况， 这时， 我们可以利用php的生成器， 以减少内存的占用， 顺利的处理数据



利用生成器的写法， 300多条的数据，内存占用 `699048 byte`

```php
<?php
/**
 * Created by PhpStorm.
 * User: raye
 * Date: 18/1/12
 * Time: 17:08
 */


class MysqlOperation{
    private static $instance;

    private function __construct()
    {
        $db_name = 'bi';
        $host = '192.168.1.100';
        $port = '3306';
        $c = [
            'user' => 'root',
            'pwd' => 123456,
            'options' =>[]
        ];
        self::$instance = new \PDO("mysql:dbname=$db_name;host=$host;port=$port", $c['user'], $c['pwd'], (array)$c['options']);
    }

    public static function getInstance(){
        if (!self::$instance instanceof PDO) {
            new self();
        }
        return self::$instance;
    }
}

$func = function () {
    $mysql = MysqlOperation::getInstance();
    $statement = $mysql->prepare('SELECT uuid FROM `user`');
    $statement->execute();

    while ($row = $statement->fetch(PDO::FETCH_COLUMN)){
        yield $row;
    }
};

foreach ($func() as $item) {
//    var_dump($item);
}

var_dump(xdebug_peak_memory_usage());   
```



使用通常的写法， 内存占用`762248 byte`  

```php
$func = function () {
    $mysql = MysqlOperation::getInstance();
    $statement = $mysql->prepare('SELECT uuid FROM `user`');
    $statement->execute();
    $uuids = $statement->fetchAll(PDO::FETCH_COLUMN);
    return $uuids;
};

foreach ($func() as $item) {
//    var_dump($item);
}

var_dump(xdebug_peak_memory_usage());
```

