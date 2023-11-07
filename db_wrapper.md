db wrapper
==========

数据库 wrapper层:
-------------

*   使用 [db:instance()](https://github.com/ricolau/autophp/blob/master/framework/db.php#L22) 用来获取数据库连接，但是首先，你需要配置一下数据库的别名 [db::addServer()](https://github.com/ricolau/autophp/blob/master/framework/db.php#L18)

*   比如:
    
    *   数据库的config 文件配置: [https://github.com/ricolau/autophp/blob/master/demo/config/dbmysql.php](https://github.com/ricolau/autophp/blob/master/demo/config/dbmysql.php)
    *   把数据库的config 文件读取，并配置别名: [https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L57](https://github.com/ricolau/autophp/blob/master/demo/htdocs/index.php#L57)
    *   获取数据库连接:[https://github.com/ricolau/autophp/blob/master/framework/orm.php#L70](https://github.com/ricolau/autophp/blob/master/framework/orm.php#L70)
        
    *   使用 orm: 比较推荐使用 orm 而不是直接使用db连接，尤其是对于 mysql pdo 的支持