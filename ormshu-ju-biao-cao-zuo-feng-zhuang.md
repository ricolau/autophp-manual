定位：
---

本framework 的 orm 不想封装成 active record 类似的实现，由于active record 会提升对数据库对象操作的复杂度，提升学习成本和开发代价。

目前的orm 类主要希望定位于面向db table粒度的操作，即 table query tools，而不是面向每一条table record，这一点需要清晰了解。

因此，orm 会主要提供关于 db operation object——pdo、table query、sql 相关的功能封装和调用。

实例化与查询，有两种方式：
-------------

* * *

### 第一种，继承方式推荐：

```php
<?php

class user extends orm{
////default 为db alias，user 为 table name


    protected $_dbAlias = 'default';
    protected $_table = 'user';

    public function getUserinfoById($uid){
        $uinfo = $this->where('id=?', array($uid))->getOne();// fetchOne 会只返回一条记录
        return $uinfo;
    }

    public function getUserinfoById2($uid){
        $uinfos = $this->where('id=?', array($uid))->select();  //select 会返回一个二维数组，哪怕只有一条记录
        $uinfo = array_pop($uinfos);
        return $uinfo;
    }

}

```

### 第二种，外部调用方式，不推荐：

```php
<?php

//default 为db alias，user 为 table name
$uid = 1;

//通过链式操作，一行就可以得到查询结果
$uinfo = orm::magicInstance('default','user')->where('id=?', array($uid))->getOne();

```

防止sql 注入的特性实现，参数化执行：
--------------------

* * *

防止sql injection 主要通过pdo 的参数化执行特性实现，所以工程师在代码开发过程中，一定要将外部获取的参数，通过问号 的形式参数化执行。

具体请见下方的增、删、改、查等示例即可。

具体使用：
-----

* * *

### 获取最后一条（刚刚）查询的sql 内容：

由于orm 本身的操作主要通过链式操作完成，所以基本不需要工程师直接写sql，但是我们很多时候要排查最终生成的sql 是否正确，或者取出来最后执行的sql 写入log：

```php
class user extends orm{
////default 为db alias，user 为 table name


    protected $_dbAlias = 'default';
    protected $_table = 'user';

    public function getUserinfoById($uid){
        $uinfo = $this->where('id=?', array($uid))->getOne();// fetchOne 会只返回一条记录


        $query = $this->getLastQuery();//返回的是一个数组！
        log::add('info', $query);//记录一条日志

        return $uinfo;
    }





}

```

### 增、删、改、查、count 的实现：

```php
class user extends orm{
////default 为db alias，user 为 table name


    protected $_dbAlias = 'default';
    protected $_table = 'user';


    //insert 实现
    public function add($userinfo){

        //是否在插入后返回对应的primary key id，默认为 false，可以不传
        $getLastInsertId = true;

        //直接insert，不会做 fields检测，错了就会抛出异常、
        $add1 = $this->insert($userinfo, $getLastInsertId);

        //先做table fields 匹配检测，过滤掉$userinfo 中多余的字段，然后insert
        $add2 = $this->autoInsert($userinfo, $getLastInsertId);


        return 'test insert';
    }

    //delete 实现
    public function delete($uid){
        //指定where 条件的删除。如果where 条件为空，为了安全起见防止全表删除，orm 会抛出异常
        $delete = $this->where('id=?', array($uid))->delete(); 
        return $delete;
    }

    //update 实现
    public function update($uid, $userinfo){

        //是否在update 后返回对应的 AffectedRows，默认为 false，可以不传
        $returnAffectedRows = true;


        //指定where 条件的update。如果where 条件为空，为了安全起见防止全表被update，orm 会抛出异常
        //不会做 fields检测，错了就会抛出异常
        $update = $this->where('id=?', array($uid))->update($userinfo, $returnAffectedRows);


        //指定where 条件的update。如果where 条件为空，为了安全起见防止全表被update，orm 会抛出异常            
        //先做table fields 匹配检测，过滤掉$userinfo 中多余的字段，然后update
        $update2 = $this->where('id=?', array($uid))->autoUpdate($userinfo, $returnAffectedRows);

        return 'test update';
    }


    //select 实现
    public function select(){

        //这里做示例用，select 暂时不接受参数了，参数可以任意指定


        //单条查询
        $row = $this->where('id=?', array($uid))->getOne();

        //多条查询，为了便于查看，链式操作代码换行一下
        $row = $this->where('id>?', array(10))
                    ->order('id desc')  //可选项，按指定方式排序
                    ->limit(20,20)   //可选项，控制返回结果条数
                    ->select();//将返回一个数组


        return 'test select';
    }


    //count 实现
    public function count(){
        //count 一下 id>10 的用户数
        $num = $this->where('id>?', array(10))->count(); 
        return $num;  //直接是一个数字
    }


}

```

### 自定义sql 复杂查询的实现：

```php
<?php
orm::whereMatch(); //此function 可以替代  orm::where() 在链式调用中的作用， 实现更加简化的复杂查询拼接。

orm::query();//不需要指定 where 条件等，直接可以写入一条sql 进行查询
orm::queryFetch();//同上，会增加fetch 返回结果的过程

```

额外的思考：
------

* * *

orm 在传统的 mvc 里面，一般会放到 model 层，由model 来继承 orm；

但是在 dmvc 中，会多出来一层 data layer，这个时候，倾向于使用dao 层来继承 orm；