config
======

使用config 类获取配置:
---------------

*   使用 `config::get()` 用来获取config 的数据, 比如`config::get('default.sitename');` 将会获取 `APP_PATH/``config/default.php  `
    中下标为 sitename 的数组键值，若没有将返回null；[示例](https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L45)  
    
*   config 新特性，多目录的config 支持：从 version 1.5 开始，可以支持多个config 目录的配置了，（默认支持 APP_PATH./config/\* 目录下的文件） 。  
    *   如果想支持更多文件，可以在`htdocs/index.php` 中加入更多config 目录。添加多个config  
        目录：如`config::addPath('/usr/local/phpenv/conf/')` 、`config::addPath('/usr/local/phpenv/conf2/')` 等。  
        
    *   优先级：会优先从 `APP_PATH./config/` 目录去寻找，找不到则按照配置顺序依次寻找其他目录。  
        

* * *

*   框架提供了简化的函数来代替 `config::get()`, 在代码中直接使用 `config()` 即可。（前提是需要首先全局声明过 `util::loadMislenious()`, 详情见 [吊炸天的a、d、de、e、t等单字母函数](bian-jie-de-dan-zi-mu-a-d-de-e-t-deng-han-shu.html) ）