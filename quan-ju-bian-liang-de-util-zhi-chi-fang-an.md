在本framework 中，是禁止使用 global 这样的，全局变量的坏处不再赘述。

但是全局变量的使用场景还是有的，因此，framework 通过 util 类来实现支持 全局变量，主要分为以下内容：

```php
<?php

全局变量的赋值与获取，按照 key=>value形式，注：key为string或者int类型，value没有类型限制。
util::set('name',  'rico');

util::get('name'); //得到返回值 rico；


//全局变量实现 increment 和 decrement：

util::set('age',  16);

util::incr('age', 2);//第二个参数 step 可以不传，默认为1
util::decr('age', 1)；//第二个参数 step 可以不传，默认为1

util::get('age'); //得到返回值17

```