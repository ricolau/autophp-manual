特性: 期望可完全替代php 的弱类型数组,当做强类型靠谱使用;

1.  使用前, `struct` 的所有 `property` 必须声明才可以;不声明的 `set` 和 `get` 都将 `throw exception`;
2.  赋值时,类型检测,强类型检测;
3.  `property` 默认值均为 `null`;
4.  `json` 转换支持;
5.  数组转换支持,支持最高8维数组!!性能考虑;
6.  子类 `struct`,无法定义 `public` 的 `property`,必须使用 `struct` 规定的声明方式;
7.  支持iterator 数组形式的遍历;
8.  支持unset(),可以将实例化的 struct 中某个property unset 掉.但不影响struct 定义,可以重新赋值.
9.  继承自 `class base`,支持 `struct::instance()` 形式的初始化;
10.  暂时不考虑对于` __serialize()` ` __clone() `等的处理

@exmaples below

```php
<?php

    //@uses ======================= style one, with recommendation ======================= 
    class b extends struct {

        //public $age = 18;//this will throw exception

        protected $_structDefine = array(
            'gender' => struct::type_bool,
            'age' => struct::type_int,
            'obj' => struct::type_int,
        );
        protected $_strictMode = true;     //whether throw exception when property type not match with definations

    }

    try {

        echo "######### demo use one ############\n";
        $b2 = new b();
        $b2->age = 20000;

        $pa = new b();
        $pa->age = 20;
        $pa->obj = $b2;



        echo "=======================\n";
        echo "convert to json:";
        var_dump($pa->toJson());




        echo "=======================\n";
        echo "convert to array:";
        var_dump($pa->toArray());

        echo "=======================\n";
        echo "each property show in list:\n";
        foreach($pa as $k => $v) {
            var_dump($k,'=>', $v);
            echo "----------\n";
        }



        echo "=======================\n";
        echo "exception test: \n";

        $b2->age = '20';
        $a->fff = 2222; // this will throw exception


    } catch(Exception $e) {
        var_dump("exception caught :",$e);
    }
    //exit;
    //@uses =======================  the other style, deprecated ======================= 


    try {

        echo "######### demo use the other one  ############\n";

        $st = array("name" => struct::type_string, 'age' => struct::type_int, 'gender' => struct::type_int);
        $a = new struct($st);


        $a->age = 20;
        $a->name = 'rico,hahahahaha';
        //var_dump($a->fff); // this will throw exception

        echo "=======================\n";
        echo "test property exist age:";
        var_dump($a->propertyExist('age'));




        echo "=======================\n";
        echo "convert to json:";
        var_dump($a->toJson());



        echo "=======================\n";
        echo "after unset property of this object (will not affect the struct defination):\n";
        unset($a->age);
        var_dump($a->toJson());



        echo "=======================\n";
        echo "each property show in list:\n";
        foreach($a as $k => $v) {
            var_dump($k,'=>', $v);
            echo "----------\n";
        }


        echo "=======================\n";
        echo "exception test: \n";



    } catch(Exception $e) {
        var_dump($e);
    }

```