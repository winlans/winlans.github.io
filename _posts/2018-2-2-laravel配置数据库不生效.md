---
title: laravel配置数据库不生效
date: 2018-2-2 16:00:00
categories:
- framework
tags:
- laravel
---



背景：

看了一下laravel 的基础， 然后感觉这个框架也很有意思， 于是就多看了几眼， 没想到栽到了配置数据库上面， laravel配置文件的加载方式和我之前使用过的都不同， 很特别， 真的很特别。

发生了什么：

当我玩完laravel的路由和控制器的时候， 就去看laravel的模型， 然后此时去配置了数据库， 但是，奇怪的事情发生了， 不管我怎么配置都不行， 后来发现了问题是出现在配置文件的函数上面。下面是配置文件，我们可以看到， 他是通过`env()`这个函数去判断的。

```php
    return   ['mysql' => [
            'driver' => 'mysql',
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'bi'),
            'username' => env('DB_USERNAME', 'root'),
            'password' => env('DB_PASSWORD', '0000'),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8',
            'collation' => 'utf8_unicode_ci',
            'prefix' => '',
            'strict' => true,
            'engine' => null,
        ],];
```

```php
// 只摘取了函数的一部分
function env($key, $default = null)
{
        $value = getenv($key);

        if ($value === false) {
            return value($default);
        }
}
```

通过上面可以看到， `env()` 函数使用`getenv()` 这个函数去获取配置项的值， 问题就出现在这里了， 由于我是使用laravel内置的命令启动的， 所以环境变量并没有发生重载。用`phpinfo()`函数查看， 发现还是默认的`homestead`, 重启了服务之后， 一切正常了。



顺便深究了下laravel是怎么加载这个环境变量的， 摘录的源码比较多。即使这样也不是很容易说清楚。

```php
    /** 这个是Appliction.php, 入口文件会去加载他，并在laravel启动事件发生之后回调
     * Register a callback to run after loading the environment.
     *
     * @param  \Closure  $callback
     * @return void
     */
    public function afterLoadingEnvironment(Closure $callback)
    {
        return $this->afterBootstrapping(
            LoadEnvironmentVariables::class, $callback
        );
    }
```

```php
// 调用的类里面设置了， 用于回调的函数 bootstrap
// 可以看到使用了Dotenv这个类去加载了.env 文件里面的内容， 并设置到了php的环境变量中
class LoadEnvironmentVariables
{
    /**
     * Bootstrap the given application.
     *
     * @param  \Illuminate\Contracts\Foundation\Application  $app
     * @return void
     */
    public function bootstrap(Application $app)
    {
        if ($app->configurationIsCached()) {
            return;
        }

        $this->checkForSpecificEnvironmentFile($app);

        try {
            (new Dotenv($app->environmentPath(), $app->environmentFile()))->load();
        } catch (InvalidPathException $e) {
            //
        }
    }
```

```php
//可以看到Dotenv在构造函数中完成了初始化工作， 并调用了loader的loadData()方法
// loadData最终调用了loader的load()方法
class Dotenv
{
    /**
     * The file path.
     *
     * @var string
     */
    protected $filePath;

    /**
     * The loader instance.
     *
     * @var \Dotenv\Loader|null
     */
    protected $loader;

    /**
     * Create a new dotenv instance.
     *
     * @param string $path
     * @param string $file
     *
     * @return void
     */
    public function __construct($path, $file = '.env')
    {
        $this->filePath = $this->getFilePath($path, $file);
        $this->loader = new Loader($this->filePath, true);
    }

    /**
     * Load environment file in given directory.
     *
     * @return array
     */
    public function load()
    {
        return $this->loadData();
    }
    /**
     * Actually load the data.
     *
     * @param bool $overload
     *
     * @return array
     */
    protected function loadData($overload = false)
    {
        $this->loader = new Loader($this->filePath, !$overload);

        return $this->loader->load();
    }
```

```php
// loader->load()方法最终调用这个方法，完成了环境变量的设置工作
    public function setEnvironmentVariable($name, $value = null)
    {
        list($name, $value) = $this->normaliseEnvironmentVariable($name, $value);

        // Don't overwrite existing environment variables if we're immutable
        // Ruby's dotenv does this with `ENV[key] ||= value`.
        if ($this->immutable && $this->getEnvironmentVariable($name) !== null) {
            return;
        }

        // If PHP is running as an Apache module and an existing
        // Apache environment variable exists, overwrite it
        if (function_exists('apache_getenv') && function_exists('apache_setenv') && apache_getenv($name)) {
            apache_setenv($name, $value);
        }

        if (function_exists('putenv')) {
            putenv("$name=$value");
        }

        $_ENV[$name] = $value;
        $_SERVER[$name] = $value;
    }
```

整个过程就是这样的， 如果是nginx和php-fpm的配置， 我估计也有可能会出现这种情况。

1. 经测试， 当PHP以apache模块运行时， 不会出现上面的情况。