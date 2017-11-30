---
title: Php_Mongodb
date: 2017-11-30 20:00:00
categories:
- mongo
tags:
- mongo
---



旧有的`MongodbClient`类已经被废弃了，现在PHP 连接mongo的时候使用新的`MongoDB\Driver\Manager`  来创建，下面是随便简单的封装了一个类，用来增删改查，只是实现了简单了，复杂的还是看文档比较靠谱:smile:



![](http://ww1.sinaimg.cn/large/9adc532agy1fm08kw6msnj20tw06lt90.jpg)

```php
<?php
/**
 * Created by PhpStorm.
 * User: raye
 * Date: 17/11/30
 * Time: 10:06
 */
use MongoDB\Driver\Manager;
use MongoDB\Driver\Query;
use MongoDB\Driver\BulkWrite;
use MongoDB\Driver\ReadPreference;
class MongodbManage {
    /** @var  Manager  $instance*/
    private static $instance;
    private static $_dbname;
    private $manager;
    private $_host = '192.168.1.83';
    private $_port = 27017;
    private $_bulk;

    private function __construct($dbname)
    {
        self::$_dbname = $dbname;
        $this->_bulk = new BulkWrite();
        $this->manager = new Manager('mongodb://' . $this->_host .':'. $this->_port . '/' . self::$_dbname);
    }

    public static function instance($dbname){
        if (self::$_dbname == $dbname && self::$instance instanceof self) {
            return self::$instance;
        }
        self::$instance = new self($dbname);
        return self::$instance;
    }

    public function insert($collection, array $data){
        if (empty($collection) || empty($data)) {
            throw new Exception('collection or insert data can not be null');
        }

        try{
            $this->_bulk->insert($data);
            return $this->manager->executeBulkWrite(self::$_dbname .'.'. $collection, $this->_bulk);
        }catch (Exception $e) {
            new Exception($e->getLine() . '--' . $e->getMessage());
        }
    }

    public function find($collection, array $filter = [], $returnArr = false, array $options = [], $readPreference = 'primary'){
        if (empty($collection)) {
            throw new Exception('collection can not be null');
        }

        $query = new Query($filter, $options);
        $res = $this->manager->executeQuery(self::$_dbname .'.'. $collection, $query, new ReadPreference($readPreference))->toArray();

        if (!$returnArr){
            return $res;
        }

        array_walk($res,function (&$value){
            $value = (array)$value;
        });
        return $res;
    }

    public function multiInsert($collection, array $data){
        if (empty($collection) || empty($data)) {
            throw new Exception('collection and data can not be empty');
        }
        foreach ($data as $value) {
            $this->_bulk->insert($value);
        }
        $res = $this->manager->executeBulkWrite(self::$_dbname .'.'. $collection, $this->_bulk);
        return $res and $res->getInsertedCount() > 0 ? true : false;
    }
    public function update($collection, $filter, array $data) {
        if (empty($collection) || empty($filter)) {
            throw new Exception('collection and filter can not be null');
        }
        $this->_bulk->update($filter, $data);
        $res = $this->manager->executeBulkWrite(self::$_dbname .'.'. $collection,$this->_bulk);

        return $res->getModifiedCount() > 0 ? true : false;
    }

    public function delete($collection, $filter, $options = []){
        if (empty($collection) || empty($filter)) {
            throw new Exception('collection and filter can not be null');
        }
        $this->_bulk->delete($filter, $options);
        $res = $this->manager->executeBulkWrite(self::$_dbname .'.'. $collection, $this->_bulk);

        return $res->getDeletedCount() > 0 ? true : false;
    }

}
```

