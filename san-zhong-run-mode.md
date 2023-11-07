三种run mode
==========

run mode 定义:
------------

*   框架一共分为三种运行模式，分别用于 开发、测试、生产环境。
*   三种value定义： `auto::mode_dev`, `auto::mode_test`, `auto::mode_online `。
*   设置：可以用 `auto::setMode($mode)` 来设置；（$mode = auto::mode_dev, auto::mode_test, auto::mode_online 其中之一）
*   读取：可以用 `auto::getMode()` 来获取当前运行模式，来和三种value 来做对比进行判断。

*   常用的，可以用来控制 `config` 的配置，在不同mode 下返回不同值。也可以用来控制debug 等信息在不同mode 下的摘要级别