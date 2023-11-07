多语言支持示例：

语言包默认路径： `APP_PATH/i18n/`，

可以增加自定义路径，通过 `i18n::addPath($path)`实现，在语言调用时，程序将依次去多路径下寻找语言包，直到找到为止。

* * *

语言包地址，以 zh-cn 语言为例 `APP_PATH/i18n/zh-cn.php`， 文件内容如下：

```php
<?php
return array(

'title'=>'你好的标题',

'tplDemo'=>'%s 这个世界, 你是一个 %s'

);

```

多语言场景调用示例：

```php
<?php

//设置语言为 zh-cn。建议全局设置一次即可，比如在 htdocs/index.php 中设置
i18n::setLanguag('zh-cn'); 


$taga = 'title'; //指定想获取的tag 
$text = i18n::get($tag); //根据指定 tag 获取当前语言下的文本
//获取到的文本内容为 "hello title"  

$tag = 'tplDemo';//指定想获取的tag 
$fill = array('你好', '男生');//对 tag 中的变量进行赋值
$text = i18n::vget($tag, $fill);//根据 tag 和对应 tag 中变量的赋值获取内容
//获取到的内容为 "hello world, you are a boy"

//获取当前语言设置
i18n::getLanguage(); //获取到的语言为i18n::setLanguag($value) 设置的value

```