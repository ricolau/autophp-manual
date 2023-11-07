debug
=====

debug mode:
-----------

*   可以通过以下方式打开或者关闭， `$debugMode = true; auto::setDebug($debugMode);`
*   建议放在 `auto::run()` 之前，这个是全局开关。
*   打开后每个http request 、cli 模式的命令行下，会print 出来很多debug 信息。（在ajax 请求里不会打印debug 信息）