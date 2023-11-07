APP目录树解释
========

*   class ，class定义
*   class/controller, controller class定义文件
*   class/model， model class定义
*   class/plugin， 插件class定义
*   config , config 目录，用于放配置文件
*   daemon ，用于放置守护进程或者crontab 等脚本
*   htdocs， http请求默认的目录
*   htdocs/index.php http 请求默认的唯一入口，所有的url 都rewrite 到这个文件上来
*   i18n， 多语言支持文件夹，用于放语言包，形式基本和 config 类似
*   view， 模板层，所有的模板放在这里
*   view/slot slot文件目录，用于存储slot 内容
*   view/template 模板文件目录
*   view/template/index/index.php ，示例用，默认为 controller=index & action=index 时的模板引用