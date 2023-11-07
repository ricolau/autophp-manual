dispatcher/URL路由
================

dispatcher 路由：
--------------

*   path_deep，表示url 解析深度。
*   url 解析，默认为2层级的url 深度解析，也可以定制为3层级的url 深度解析支持（最大3级）。
*   url 解析深度设置： [示例](https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L79)

uri detector:
-------------

*   用来获取request uri 的方法，请见： [dispatcher::detectUri()](https://github.com/ricolau/autophp/blob/master/framework/dispatcher.php#L126)

*   对于path_deep = 2（层）级深度的url 解析:
    
    *   链式解析调用：  
```php
<?php
dispatcher::instance()->           //获取实例
        setPathDeep(dispatcher::path_deep2)->           //设置url解析深度，2层
        setDefaultController('index')->           //设置默认的controller ，当controller 为空的时候执行 
        setDefaultAction('index')->           //设置默认的action ，action 为空的时候执行
        setUri($uri)->           //设置uri，可以随意设置任意 uri，注意要设置为  /controller/action 
类似的格式才会被解析正确 
        dispatch()->           //开始路由，获取到底要执行哪个controller 和 action，准备就绪
        run();        //开始执行上一步路由后的 controller 和 action 
        
```
        
*   对于 path_deep = 3（层）级别深度的url 解析:
    
    *   3层比2层，会在controller 之上多了一层module 层（注意不是model！）。对应的 classes、view/template 等都需要多出来一层！
    *   链式解析调用：  
```php
<?php
dispatcher::instance()-> //获取实例```
    setPathDeep(dispatcher::path_deep3)->           //设置url 解析深度为3层 <br />
    setDefaultModule('index')->                //设置默认的module
    setDefaultController('index')->           //设置默认的controller ，当controller 为空的时候执行 <br /> 
    setDefaultAction('index')->           //设置默认的action ，action 为空的时候执行 <br />
    setUri($uri)->           //设置uri，可以随意设置任意 uri，注意要设置为  /controller/action  类似的格式才会被解析正确 <br />
    dispatch()->           //开始路由，获取到底要执行哪个controller 和 action，准备就绪 <br />
    run();                          //开始执行上一步路由后的 controller 和 action <br />

```
        
*   404异常，url 找不到对应的controller、action 用来执行时，
    *   会抛出异常exception_404，可以在调用`dispatcher::run()` 的时候捕获。
*   大于path_deep 的url 处理：
    
    *   对于大于 path_deep 的url，dispatcher 会按照key/value 的方式放到` $_GET` 中，也就是 `request::get() `的方式可以获取。
    *   比如当path_deep=2时：  
        `/view/detail/id/251/name/rico` 的url 会被解析到 `controller=view`, `action=detail` 中执行。然后将 `id=251&name=rico`当作 http url get的方式获取的参数放到 request 中，通过 `request::get('id')`, `request::get('name')` 的方式可以获取到。
*   其他方法：
    
    *   获取当前正在执行的 module（只针对3层目录结构有效）：`dispatcher::instance()->getModuleName();`
    *   获取当前正在执行的 controller：`dispatcher::instance()->getControllerName();`
    *   获取当前正在执行的 action：`dispatcher::instance()->getActionName();`
*   高级方法：
    *   `dispatcher::setUri()` ，在当前版本中，框架没有提供比较优雅的router 等正则方案，但是并不意味着不能支持优雅的实现。  
        比如： `/view/51` ，可以通过php coding的代码来解析此url 并转化为 `/view/detail/id/51`的路径，通过 setUri() 的形式设置，然后dispatcher 解析并执行。