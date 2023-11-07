class实例化
========

*   本framework 默认支持了 autoload，, 具体定义参照 [autoload defination](https://github.com/ricolau/autophp/blob/master/framework/auto.php#L112), use the [spl_autoload_register()](https://github.com/ricolau/autophp/blob/master/framework/auto.php#L33) function.
*   实例化一个类：比如实例化一个类： `$a = new model_user();`
    
*   实例化一个类方法二：此外，任意继承 class base 的类比如 `class demo extends base{}`，都可以通过 `demo::instance($arg1, $arg2, ...) `得到实例化的类。
    

* * *

*   单例方式实例化一个类：任意继承 class base 的类比如 `class demo extends base{}`，都可以通过 `demo::singleton() `得到实例化的全局单例唯一类。（单例实例化不支持任何参数！）
    
*   优缺点：
    
*   优点：大家都能想到，就是可以按照实例化的方式去实例化了。也可以直接用现代化的ide 比如netbeans、phpstorm 之类的来点击到类方法定义了。
*   缺点：无法强制控制class 的实例化场合，比如无法强制控制只能在 model层才能调用cache、导致control 等层都可以随意调用cache，甚至model 和view 层也可以调用 request 获取用户请求参数。这个就要靠代码规范来约束了。