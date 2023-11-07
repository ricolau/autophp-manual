由于在开发中，大家经常会用到一些函数，要写多次，而且很麻烦很长，

因此框架自身将一些常用的函数或debug 调试内容封装成了单字母函数，方便调用，一旦你习惯它们，你将会爱上它们。

函数定义：
-----

* * *

这些函数的定义位于 `util::loadMiscellaneous()`，因此你需要在 `htdocs/index.php` 或者 `daemon/loader.php` 中调用这个声明函数。

具体定义的内容代码如下，节选自 class util：

```php
<?php

class util{

//略过N多其他代码
    public static function loadMiscellaneous() {

        if (!function_exists('a')) {

            function a() {
                echo 'yeah, dear, im RicoLau<ricolau@qq.com>, i leave this!';
            }

        }
        //can be used instead of  util::dump($a, $b ...), as d($a, $b)
        if (!function_exists('d')) {

            function d() {
                $args = func_get_args();
                call_user_func_array(array('util', 'dump'), $args);
            }

        }
        if (!function_exists('t')) {

            function t($time = null,$format = 'Y-m-d H:i:s') {
                if($time===null){
                    $time = time();
                }
                return date($format,$time);
            }

        }
        if (!function_exists('de')) {

            function de() {
                $args = func_get_args();
                call_user_func_array(array('util', 'dump'), $args);
                exit;
            }

        }
        //can be used instead of config::get($key), as  c('default.domain')  ;
        if (!function_exists('config')) {

            function config($key) {
                if (class_exists('config') && auto::hasRun()) {
                    return config::get($key);
                }
                return null;
            }

        }

        //can be used instead of  echo htmlspecialchars()
        if (!function_exists('e') && !function_exists('eg')) {
            function e($string, $flags = ENT_COMPAT, $encoding = 'UTF-8', $double_encode = true) {
                echo htmlspecialchars($string, $flags, $encoding, $double_encode);
            }
            function eg($string, $flags = ENT_COMPAT, $encoding = 'UTF-8', $double_encode = true) {
                return htmlspecialchars($string, $flags, $encoding, $double_encode);
            }
        }

        //can be used instead of i18n::get($key) or  i18n::vget($key, $fillData);
        if (!function_exists('lang')) {
            function lang($key, $args = null) {
                if (!class_exists('i18n') || !auto::hasRun()) {
                    return null;
                }
                if ($args === null) {
                    return call_user_func_array(array('i18n', 'get'), array($key));
                }
                return call_user_func_array(array('i18n', 'vget'), array($key, $args));
            }

        }

        if (!function_exists('url')) {
            function url($path,$args = array()) {
                return url::get($path, $args);
            }
        }

        if (!function_exists('domain')) {
            function domain($domain = null) {
                if ($domain === null) {
                    return url::getDomain();
                } else {
                    return url::setDomain($domain);
                }
            }

        }

    }

    //略过N多其他代码

    }

```

函数介绍：
-----

* * *

### d() 这个function 可以理解为vardump() 的一个变种，在用法上和 var_dump() 是一样的，可以支持N多参数。

但是输出结果则有很大不同， 比如：

```php
<?php

$a = array('tom'=>array('age'=>15,'gender'=>'boy'),'jelly'=>array('age'=>16,'gender'=>'girl'));


var_dump($a);

echo "\n\n===================\n\n";

d($a)

```

对应的输出结果为：

> array(2) {
> 
> ["tom"]=>
> 
> array(2) {
> 
>```
> ["age"]=&gt;
> 
> int(15)
> 
> ["gender"]=&gt;
> 
> string(3) "boy"
> 
>```
> 
> }
> 
> ["jelly"]=>
> 
> array(2) {
> 
>```
> ["age"]=&gt;
> 
> int(16)
> 
> ["gender"]=&gt;
> 
> string(4) "girl"
> 
>```
> 
> }
> 
> }
> 
> \===================
> 
> array(2){
> 
> ["tom"]=>array(2){
> 
> ["tom"]["age"]=>int(15)
> 
> ["tom"]["gender"]=>string(3) "boy"
> 
> }
> 
> ["jelly"]=>array(2){
> 
> ["jelly"]["age"]=>int(16)
> 
> ["jelly"]["gender"]=>string(4) "girl"
> 
> }
> 
> }

### `de()` 可以理解为 是 `d()` 执行后执行 `exit()；`

这个方式会很方便在程序中打断点调试。

### `config()` 相当于 `config::get()；`

### `t()` 相当于 `date('Y-m-d H:i:s')`， `t('Y')`相当于 `date('Y');`

### 其他函数比如` e()`， `url()`， `domain() `等请自己看下源代码即可。