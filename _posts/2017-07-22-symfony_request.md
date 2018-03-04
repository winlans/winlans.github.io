---
title: Symfony request
date: 2017-07-22
categories:
- framework
tags:
- symfony
---

要与用户进行交互,请求与响应是必要的，也是必须的。这篇主要是介绍symfony如何接受用户的请求。

<!-- more -->

1. 获取request 对象
  symfony 通过httpFoundation组件来实现用户请求的接收，通过

```php
use Symfony\Component\HttpFoundation\Request;

$request = Request::createFromGlobals();
```

我们就可能拿到一个request对象，并且在任何地方都可使用，通过查看symfony的入口文件，我们发现，symfony在入口处，就获取到了request对象，并传入内核的处理机制中。

```php
<?php

use Symfony\Component\HttpFoundation\Request;

require __DIR__.'/../app/autoload.php';
include_once __DIR__.'/../app/bootstrap.php.cache';

$kernel = new AppKernel('prod', false);
$kernel->loadClassCache();
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
$response->send();
$kernel->terminate($request, $response);
```

2.request 对象中包含了什么，怎么获取呢
  为了查看request中包含了什么，我们可以点击`Request::createFromGlobals();`
  通过函数的注释，以及代码我们可以很直白的看到，**request对象中包含了一系列超全局数组中的数据，并将超全局数组中的数据封装到request的属性中**

```php
     // 用php的超全局数组中的值，创建一个request对象
    public static function createFromGlobals()
    {
        // With the php's bug #66606, the php's built-in web server
        // stores the Content-Type and Content-Length header values in
        // HTTP_CONTENT_TYPE and HTTP_CONTENT_LENGTH fields.
        $server = $_SERVER;
        if ('cli-server' === PHP_SAPI) {
            if (array_key_exists('HTTP_CONTENT_LENGTH', $_SERVER)) {
                $server['CONTENT_LENGTH'] = $_SERVER['HTTP_CONTENT_LENGTH'];
            }
            if (array_key_exists('HTTP_CONTENT_TYPE', $_SERVER)) {
                $server['CONTENT_TYPE'] = $_SERVER['HTTP_CONTENT_TYPE'];
            }
        }

        $request = self::createRequestFromFactory($_GET, $_POST, array(), $_COOKIE, $_FILES, $server);

        if (0 === strpos($request->headers->get('CONTENT_TYPE'), 'application/x-www-form-urlencoded')
            && in_array(strtoupper($request->server->get('REQUEST_METHOD', 'GET')), array('PUT', 'DELETE', 'PATCH'))
        ) {
            parse_str($request->getContent(), $data);
            $request->request = new ParameterBag($data);
        }

        return $request;
    }
```

通过继续深究源代码，我们可以了解symfony整个获取request对象的过程，这个就不深究了，我们看看request对象是如何使用的

3.request对象的具体使用
    request的属性中，其中
    request: [ParameterBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/ParameterBag.html);
    query: [ParameterBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/ParameterBag.html);
    cookies: [ParameterBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/ParameterBag.html);
    attributes: [ParameterBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/ParameterBag.html);
    是属于`ParameterBag`的对象实例，包含了一系列对应的方法，其中最常用的是`get()`，除了post，其余的一律要通过request对象获取到对象的实例，才可以，比如`$request->query->get('get_filed_name')`
    **files & server & headers 实现了各自的对象实例，具有不同的方法**
     files: [FileBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/FileBag.html);
    server: [ServerBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/ServerBag.html);
    headers: [HeaderBag](http://api.symfony.com/2.7/Symfony/Component/HttpFoundation/HeaderBag.html).
    当然，为了区分记住用户，seesion信息，也是必要的，可以通过request对象的`$request->getSession()`来获取到seesion的对象，`getSession()`也具有很多方法，这里就不详细介绍了。

4.返回指定头部信息
   可以使用request对象中headers属性，headers属性中的set()方法可以让我们返回指定的头部信息。

5.设置cookie
  `$response->headers->setCookie(new \Symfony\Component\HttpFoundation\Cookie('session_id','test'));`