request封装
=========

http 请求参数获取:
------------

(基本和原生的 `$_GET`, `$_POST` 类似，但是对输入数据会加入 `addslashes() `和 `htmlspecialchars()` 处理, 这主要看你在引用框架时是否做了设定) 查看引用框架的初始化设定： [https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L42](https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L42)

* 示例：  
```php
<?php
request::get('name'); // 相当于 $_GET['name']  
    
request::get('id','int');//相当于 intval($_GET['id'])  
    
request::getAll(); //相当于 $_GET  
        
    
request::post('name');//相当于 $_POST['name']  
    
request::post('id','int');//相当于 intval($_POST['id'])  
    
request::postAll(); //相当于 to $_POST
request::cookieAll(); //相当于 to $_COOKIE

```