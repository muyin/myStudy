# node.js中使用sqlite3

## 一、sqlite简介

sqlite是一款轻量级的数据库，sqlite的第一个版本是2000年就发布了的，经过十多年的历练，显然sqlite目前已经相当成熟。sqlite最大的特点就是没有任何数据库服务器，无论是C、java、node.js，只需要相应的驱动，就可以直接对数据库进行读写，速度是相当的快。相比之下，其他的sql数据库必须启动一个服务，而且还要安装、配置，sqlite简洁的令人感动。

由于没有了服务器，sqlite的sql执行速度相当快。它是一个简单的单文件关系数据库，主要应用是嵌入式领域、移动互联网、火狐浏览器、小型web应用程序等，由于对并发连接支持非常不好，因此在信息管理系统中用的不多。但本人由于各种原因在做一个管理系统的项目中使用了sqlite，跑在大约1000用户的环境下，数据量达到了5000万的级别，居然没有出现任何问题。看来sql不可貌相，蚂蚁有时候也能干大象干的事。

### 为什么要使用SQLite
+ SQLite不需要一个单独的服务器进程或操作的系统（无服务器的）
+ 一个完整的SQLite数据库是存储在一个单一的跨平台的磁盘文件
+ SQLite非常小，轻量级，完全配置时小于400KiB, 省略可选功能配置时小于250KiB
+ SQLite使用ANSI-C编写的，并提供了简单和易于使用的API
+ SQLite可在UNIX(Linux, Mac OS-X, Android, IOS)和Windows(Win32, WinCE, WinRT)中运行

## 二、node.js安装sqlite3模块

和其他语言一样，node.js仍然只需要一个module就可以对sqlite进行操作了。

`npm find sqlite`

发现sqlite的模块居然有几十个，足见node.js目前的火爆程度，小小的sqlite就有这么多可用模块。

其中一个就叫做sqlite3，解释如下：

sqlite3               Asynchronous, non-blocking SQLite3 bindings 异步，非阻塞sqlite3绑定。

node.js最大的特点就是异步，这款sqlite的驱动太符合node的核心思想了，经过与其他模块的比较，最终还是选择了这款。

npm上集成sqlite的库主要有两个——sqlite3和realm。realm是一个理想的选择方案，它最初是为移动app设计的，在node也可以运行的，但是不支持Windows系统。sqlite3是一个专为nodejs设计的，在nodejs上面生态更健壮，因此最终选择[sqlite3](https://github.com/mapbox/node-sqlite3)。sqlite3几乎支持所有版本的nodejs，同时也可以和nwjs集成。

`npm install sqlite3`

就将sqlite的驱动安装进来了。

## 三、调用sqlite3的接口

``` nodejs代码
// file:test.js  注意：测试时发现路径/temp/必须先存在才能在该目录下生成数据库文件1.db，否则会报错
var sqlite3 = require('sqlite3');
var db = new sqlite3.Database('/tmp/1.db',function() {
  db.run("create table test(name varchar(15))",function(){
    db.run("insert into test values('hello,world')",function(){
      db.all("select * from test",function(err,res){
        if(!err)
          console.log(JSON.stringify(res));
        else
          console.log(err);
      });
    })
  });
});
```
执行：
`node test.js`

[{"name":"hello,world"}]

怎么样，还是很简单的吧，插入的时候使用db.run()，查询的时候使用db.all()

由于是异步，必须在执行sql后注册回调函数，这样如果连续执行几十条sql，那代码就跟楼梯差不多了。但这就是node的特点，所以还是要慢慢适应。

node中操作sql确实不如在java中方便，但也可以像java一样使用ORM来实现对象-数据库的映射。但最好还是用和node绝配的mongodb，这样写起代码来才爽！

-------------------------------
## sqlite3的常见api

sqlite3的api都是基于函数回调的，因为nodejs中没有像java的jdbc那种官方的数据库客户端接口，因此每个数据库的api都不一样，这里简单介绍几个sqlite3重要的api。
### 新建并打开数据库

> new sqlite3.Database(filename, [mode], [callback])

该方法返回一个自动打开的数据库对象，参数：

filename：有效值是一个文件名，如：“mydatebase.db”，数据库打开之后会创建一个“mydatebase.db”的文件用于保存数据。如果文件名是“:memory:”，表示是一个内存数据库（类似h2那种），数据不会持久化保存，当关闭数据库时，内容将丢失。

mode（可选）：数据库的模式，共3种值：sqlite3.OPEN_READONLY（只读），sqlite3.OPEN_READWRITE（可读写）和sqlite3.OPEN_CREATE（可以创建）。 默认值为OPEN_READWRITE |OPEN_CREATE。

callback（可选）:则当数据库成功打开或发生错误时，将调用此函数。 第一个参数是一个错误对象，当它为空时，表示打开成功。

打开一个数据库，如：
```
//数据库的名字是"mydatebase.db"
var database;
database = new sqlite3.Database("mydatebase.db", function(e){
 if (err) throw err;
});
//也可以使用内存型，数据不会永久保存
database = new sqlite3.Database(":memory:", function(e){
 if (err) throw err;
});
```
执行后会在项目的根目录生成一个“mydatebase.db”文件，这就是sqlite保存数据的文件了。

### 关闭数据库

> Database#close([callback])
该方法可以关闭一个数据库连接对象，参数：

callback（可选）:关闭成功的回调。 第一个参数是一个错误对象，当它为“null”时，表示关闭成功。

### 执行DDL和DML语句(run),创建或更改表格并插入或更新表格数据

> Database#run(sql, [param, ...], [callback])
该方法可以执行DDL和DML语句，如建表、删除表、删除行数据、插入行数据等，参数：

sql：要运行的SQL字符串。sql的类型是DDL和DML，DQL不能使用这个命令。执行后返回值不包含任何结果，必须通过callback回调函数获取执行结果。

param，...（可选）：当SQL语句包含占位符（?）时，这里可以传对应的参数。 这里有三种传值方法，如：
```
// 直接通过参数传值.
db.run("UPDATE tbl SET name = ? WHERE id = ?", "bar", 2);
// 将值封装为一个数组传值.
db.run("UPDATE tbl SET name = ? WHERE id = ?", [ "bar", 2 ]);
// 使用一个json传值.参数的前缀可以是“:name”，“@name”和“$name”。推荐用“$name”形式
db.run("UPDATE tbl SET name = $name WHERE id = $id", {$id: 2, $name: "bar"});
```
关于占位符的命名，sqlite3还支持更复杂的形式，这里不再扩展，有兴趣了解的话请查看官方文档。

callback（可选）：如果执行成功，则第一个参数为null，否则就是出错。

如果执行成功，上下文this包含两个属性：lastID和changes。lastID表示在执行INSERT命令语句时，最后一条数据的id；changes表示UPADTE命令和DELETE命令时候，影响的数据行数。
```
db.run("UPDATE foo SET id = 1 WHERE id <= 500", function(err) {
    if (err) throw err;
    //使用this.changes获取改变的行数
    assert.equal(500, this.changes);
    done();
});
```
### 执行多条语句（exec）

> Database#exec(sql, [callback])

Database#exec与Database＃run函数一样，都是DDL和DML语句，但是Database#exec可以执行多条语句，并且不支持占位符参数。
```
database.run("CREATE TABLE foo (id INT)", function(e){
    if(e !== null){ throw e; }
    //循环生成sql语句，批次插入多条数据
    var sql = "";
    for(var i = 0 ; i < 500; i ++){
        sql += 'INSERT INTO foo VALUES(' + i + ');'
    }
    database.exec(sql, done)
});
```

### 查询一条数据(get)

> Database#get(sql, [param, ...], [callback])

sql：要运行的SQL字符串。sql的类型是DQL。这里仅返回第一条查询到的数据。  

param，...（可选）：同Database#run的param参数

callback（可选）：同样是返回null代表执行成功。回调的签名是function（err，row）。如果查询结果集为空，则第二个参数为undefined；否则第二个参数值是查询到的第一个对象，他是个json对象，属性名称对应于结果集的列名称，因此查询的每一列都应该给出一个列表名。

### 查询所有数据(all)

> Database#all(sql, [param, ...], [callback])

sql：要运行的SQL字符串。sql的类型是DQL。和Database#get不同，Database#all会返回所有查询到的语句。

param，...（可选）：同Database#run的param参数

callback（可选）：同样是返回null代表执行成功。回调的签名是function(err, rows) 。rows是一个数组，如果查询结果集为空数组。

! 注意，Database#all首先检索所有结果行并将其存储在内存中。 对于数据量可能很大的查询命令时候，请使用Database＃each函数或Database＃prepare代替这个方法。

### 遍历数据

> Database#each(sql, [param, ...], [callback], [complete])

与Database＃run函数相同，都是查询多条数据，但是具有以下区别：

回调的签名是function（err，row）。如果结果集成功但为空，则不会调用回调。对于每个检索到的行，该方法都会调用一次回调。执行顺序与结果集中的行顺序完全对应。

调用所有行回调后，如果存在complete回调函数，将调用这个回调。第一个参数是一个错误对象，第二个参数是检索行数。

### 预编译SQL相关api 

在java的jdbc中，有个PreparedStatement相关的api，可以预编译sql语句，执行的时候再链接具体参数。这样的好处是可以减少sql语句被编译的次数。在sqlite3中，也存在实现这样功能的api。

> Database#prepare(sql, [param, ...], [callback])

Database#prepare执行后，会返回一个命令对象，这个命令对象可以反复执行。下面看看这个命令对象（statement ）的api：

> Statement#run([param, ...], [callback])  
> Statement#get([param, ...], [callback])  
> Statement#all([param, ...], [callback])  
> Statement#each([param, ...], [callback])  

以上api方法与Database的同名方法调用方式相同。不同点是这里的Statement对象是可以复用的，避免了重复编译sql语句，因此项目中更推荐使用上述方法。

! 注意，这些方法的param参数都会对Statement对象绑定参数，在下一次执行的时候，如果没有重新绑定参数，是会使用上一次参数的。

### 绑定参数 

> Statement#bind([param, ...], [callback])

Database#prepare执行的时候，是可以绑定参数的。不过使用此方法可以全重置语句对象和行游标，并删除所有先前绑定的参数，实现重新绑定的功能。

### 重置语句的行游标

> Statement#reset([callback])

重置语句的行游标，并保留参数绑定。使用此功能可以使用相同的绑定重新执行相同的查询。

### 数据库事务

事务是关系型数据库中的一个重要部分，sqlite自然也是支持事务的，但是sqlite3并没有提供特殊API去实现的事务相关的操作，只能靠SQL语句去控制事务。这里举一个事务相关的例子。
```js
var db = new sqlite3.Database(db_path);
db.run("CREATE TABLE foo (id INT, txt TEXT)");
db.run("BEGIN TRANSACTION");
var stmt = db.prepare("INSERT INTO foo VALUES(?, ?)");
for (var i = 0; i < count; i++) {
    stmt.run(i, randomString());
}
db.run("COMMIT TRANSACTION");
```

### 基于promise对sqlite3API的封装 

sqlite3的API是node早期的API风格，对异步的书写风格并不友好，很容易出现“金字塔回调”式的代码。为了让对API的调用更加优雅，我们往往会把回调封装成Promise。事实上这个工作并不需要我们自己做，sqlite3生态下已经有其他库可以实现这样的功能。sqlite就是一个这样的库。他基于sqlite3，只手用Promise重新封装了一下sqlite3的API，使其代码风格更加优雅，也更容易使用。

### 注意事项及定义
+ DDL: 数据定义语言，包括CREATE(创建新表/视图/对象), ALTER(修改表), DROP(删除表/视图/对象)
+ DML: 数据操作语言，包括INSERT(创建一条记录), UPDATE(修改记录), DELETE(删除记录)
+ DQL: 数据查询语言，包括SELECT
--------------------
+ SQLite不区分大小写 
+ **问题：** require('sqlite3').verbose();执行verbose方法的作用是什么？  
  **答案：** 在操作时打印详细信息方便调试，应该只在开发环境中使用。其实verbose这个单词在很多地方出现，比如命令行参—verbose，一般缩写为-v。很多命令都支持这个参数。都是差不多一个意思。

-----------------------------------------------------------
###  引入SQLite3模块
```js
var fs = require('fs');
var file = 'test.db';//这里写的就是数据库文件的路径
var exists = fs.existsSync(file);
var sqlite3 = require('sqlite3').verbose();
var db = new sqlite3.Database(file);
```

### 增删改查操作
```js
//增
var sql1 = db.prepare("insert into 表名 values (内容，跟mysql一样)");
sql1.run();
//console.log(sql1);//可以这样检查
 
//删
var sql2 = db.prepare("delete from 表名 where id = 1");
//console.log(sql2);
sql2.run();
 
//改
var sql3 = db.prepare("update 表名 set name = winston where id = 1");
sql3.run();
 
//查
//查一个表的所有数据
db.all("select * from 表名",function(err,row){
    console.log(JSON.stringify(row));
})
//查一条数据
db().each("select * from 表名",function(err,row){
     console.log(row);
})
```

### SQLite3 API介绍

在nodejs的模块安装模块下，进入sqlite3/lib目录下，打开sqlite3.js文件查看，操作数据库主要是用Database，Database相关的函数有：run、prepare、each、get、all、exec、map和close。

**Database**  
用法：new sqlite3.Database(filename,[mode],[callback])。  
功能：返回数据库对象并且自动打开和连接数据库，它没有独立打开数据库的方法。  

**close**  
用法：close([callback])。  
功能：关闭和释放数据库对象。  

**run**  
用法：run(sql,param,...],[callback])。  
功能：运行指定参数的SQL语句，完成之后调用回调函数，它不返回任何数据，在回调函数里面有一个参数，SQL语句执行成功，则参数的值为null,反之为一个错误的对象，它返回的是数据库的操作对象。在这个回调函数里面当中的this,里面包含有lastId(插入的ID)和change(操作影响的行数,如果执行SQL语句失败，则change的值永远为0)。  

**get**  
用法：get(sql,[param,...],[callback])。   
功能：运行指定参数的SQL语句，完成过后调用回调函数。如果执行成功，则回调函数中的第一个参数为null,第二个参数为结果集中的第一行数据，反之则回调函数中只有一个参数，只参数为一个错误的对象。   

**all**  
用法：all(sql,[param,...],[callback])。  
功能：运行指定参数的SQL语句，完成过后调用回调函数。如果执行成功，则回调函数中的第一个参数为null,第二个参数为查询的结果集，反之，则只有一个参数，且参数的值为一个错误的对象。  

**prepare**  
用法：prepare(sql,[param,...],[callback])。  
功能：预执行绑定指定参数的SQL语句，返回一个Statement对象，如果执行成功，则回调函数的第一个参数为null,反之为一个错误的对象。  
 
### 具体事例
由于sqlite3 API具体使用过程中重复代码量较多，所以进行了简单封装，在使用过程中如果第一次创建数据库和表，紧接着插入数据的话，可能导致表还未创建完成就查询出现错误（Nodejs是基于异步的且顺序不可控），所以在表创建和插入数据时使用同步方式操作来保证表已经存在。封装的代码如下：

```js
/**
 * File: sqlite.js.
 * Author: W A P.
 * Email: 610585613@qq.com.
 * Datetime: 2018/07/24.
 */
 
var fs = require('fs');
var sqlite3 = require('sqlite3').verbose();
 
var DB = DB || {};
 
DB.SqliteDB = function(file){
    DB.db = new sqlite3.Database(file);
 
    DB.exist = fs.existsSync(file);
    if(!DB.exist){
        console.log("Creating db file!");
        fs.openSync(file, 'w');
    };
};
 
DB.printErrorInfo = function(err){
    console.log("Error Message:" + err.message + " ErrorNumber:" + errno);
};
 
DB.SqliteDB.prototype.createTable = function(sql){
    DB.db.serialize(function(){
        DB.db.run(sql, function(err){
            if(null != err){
                DB.printErrorInfo(err);
                return;
            }
        });
    });
};
 
/// tilesData format; [[level, column, row, content], [level, column, row, content]]
DB.SqliteDB.prototype.insertData = function(sql, objects){
    DB.db.serialize(function(){
        var stmt = DB.db.prepare(sql);
        for(var i = 0; i < objects.length; ++i){
            stmt.run(objects[i]);
        }
    
        stmt.finalize();
    });
};
 
DB.SqliteDB.prototype.queryData = function(sql, callback){
    DB.db.all(sql, function(err, rows){
        if(null != err){
            DB.printErrorInfo(err);
            return;
        }
 
        /// deal query data.
        if(callback){
            callback(rows);
        }
    });
};
 
DB.SqliteDB.prototype.executeSql = function(sql){
    DB.db.run(sql, function(err){
        if(null != err){
            DB.printErrorInfo(err);
        }
    });
};
 
DB.SqliteDB.prototype.close = function(){
    DB.db.close();
};
 
/// export SqliteDB.
exports.SqliteDB = DB.SqliteDB;
```

针对以上封装接口的调用代码如下：
```js
/**
 * File: callSqlite.js.
 * Author: W A P.
 * Email: 610585613@qq.com.
 * Datetime: 2018/07/24.
 */
 
/// Import SqliteDB.
var SqliteDB = require('./sqlite.js').SqliteDB;
var file = "Gis1.db";
var sqliteDB = new SqliteDB(file);
 
/// create table.
var createTileTableSql = "create table if not exists tiles(level INTEGER, column INTEGER, row INTEGER, content BLOB);";
var createLabelTableSql = "create table if not exists labels(level INTEGER, longitude REAL, latitude REAL, content BLOB);";
sqliteDB.createTable(createTileTableSql);
sqliteDB.createTable(createLabelTableSql);
 
/// insert data.
var tileData = [[1, 10, 10], [1, 11, 11], [1, 10, 9], [1, 11, 9]];
var insertTileSql = "insert into tiles(level, column, row) values(?, ?, ?)";
sqliteDB.insertData(insertTileSql, tileData);
 
/// query data.
var querySql = 'select * from tiles where level = 1 and column >= 10 and column <= 11 and row >= 10 and row <=11';
sqliteDB.queryData(querySql, dataDeal);
 
/// update data.
var updateSql = 'update tiles set level = 2 where level = 1 and column = 10 and row = 10';
sqliteDB.executeSql(updateSql);
 
/// query data after update.
querySql = "select * from tiles where level = 2";
sqliteDB.queryData(querySql, dataDeal);
sqliteDB.close();
 
function dataDeal(objects){
    for(var i = 0; i < objects.length; ++i){
        console.log(objects[i]);
    }
}
```