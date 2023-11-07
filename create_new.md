新建一个APP
=======

*   定义 `AUTOPHP_PATH`，`framework` 的磁盘路径，比如：`define('AUTOPHP_PATH', '/usr/local/php/framework');`
*   定义 `APP_PATH`, app 的磁盘路径，比如 `define('AUTOPHP_PATH', dirname(APP_PATH) . '/framework');`
*   `require 'auto.php';`//load 框架代码
*   `auto::run();`//app入口启动方式：
*   关闭php的 magic_quota 通过 php.ini
*   创建class 目录的 controller、model 等目录，比如 [https://github.com/ricolau/autophp/tree/master/demo/class](https://github.com/ricolau/autophp/tree/master/demo/class)
*   完整的示例请见[https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php](https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php)