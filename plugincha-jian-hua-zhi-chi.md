框架通过plugin 实现钩子机制的支持。

名词定义：

1.  $tag 作为触发时机，可以任意定义；不需要显式声明，只在调用的时候调用即可。
2.  plugin，可以将一个plugin 注册到指定的 $tag 上，在对应时机调用时执行。必须继承 plugin_abstract 才可以被调用；
3.  plugin__context 钩子的上下文，由于钩子的实现是侵入式的，所以plugin__context用来给plugin 传递逻辑执行的上下文，可以在同一个tag 的若干 plugin 中依次传递。

```php
<?php

//钩子注册：
plugin::register(dispatcher::plugin_before_run, new plugin_init());
plugin::register(dispatcher::plugin_before_run, new plugin_demo());//同一个tag 上注册多个plugin

plugin::register(auto::plugin_shutdown, new plugin_end());


//钩子执行
plugin::call(dispatcher::plugin_before_run, $ptx);



//执行钩子，并利用钩子的上下文 plugin_context 实现判断和逻辑中断
$ptx = new plugin_context(__METHOD__,array());
plugin::call(dispatcher::plugin_before_run, $ptx);
if($ptx->breakOut !== null){
    return $ptx->breakOut;
}

```

钩子具有以下特性：

1.  可以自定义任意 $tag 作为触发时机；
2.  每个 $tag 上可以注册若干个 plugin，并按照注册顺序依次执行；
3.  可以多次或者任意自定义触发 action，这个特性会导致一个进程中钩子可以被执行多次。比如代码中多次包含 plugin::call($tag)；
4.  可以根据 $tag 获取其下面有哪些 plugin；
5.  支持plugin 执行的上下文传递；

应用示例：

1.  服务重连，可以参考 cache_redis::connect() 中对应的实现。
2.  log 写入，可以参考 performance::flush() 中的对应实现；
3.  命令执行错误时，通过钩子实现重试，参考 cache_codis::__call() 实现；
4.  debug日志输出，进程结束前的回调，参考 auto::shutdownCall()