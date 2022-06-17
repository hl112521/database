# 第十章 使用MySQL触发器

## 10.1.触发器的概念

触发器（Trigger）是一个特殊的存储过程，不同的是执行存储过程要使用CALL 语句来调用，而触发器的执行不需要使用CALL 语句来调用，也不需要手工启动，只要一个预定义的事件发生就会被MySQL自动调用。

例如，当对一个数据表进行插入、更新或删除等操作时可以激活触发器并执行触发器。触发程序经常用于加强数据的完整性约束和业务规则等。触发程序类似于约束，但比约束更灵活，具有更精细、更强大的数据控制能力。

触发程序的优点如下：

（1）触发程序的执行是自动的，当对触发程序相关表的数据做出相应的修改后立即执行。

（2）触发程序可以通过数据库中相关的表层叠修改另外的表。

（3）触发程序可以实施比FOREIGN KEY约束、CHECK约束更为复杂的检查和操作。

## 10.2.创建触发器

> **触发器可以查询其他表，而且可以包含复杂的SQL语句。本节将介绍如何创建触发器。**

### 10.2.1.创建单条执行语句触发器

创建一个触发器的语法格式如下：

```MySQL
CREATE TRIGGER trigger_name trigger_time trigger_event ON tbl name FOR EACH ROW trigger_stmt 
```

其中，trigger＿name 标识触发器的名称，由用户自行指定；trigger＿time 标识触发时机，可以指定为 before或after； trigger＿event 标识触发事件，包括INSERT、UPDATE和DELETE；tbl＿name标识建立触发器的表名，即在哪张表上建立触发器；trigger＿stmt是触发器程序体。触发器程序可以使用BEGIN和END作为开始和结束，中间包含多条语句。

【例10-1】创建一个单条执行语句的触发器。

首先创建数据表newstudent，表中有两个字段，分别为id字段和name字段。

```MySQL
CREATE TABLE newstudent (id INT, name VARCHAR(50));
```

然后创建一个名为in＿newstu的触发器，触发的条件是向数据表 newstudent中插入数据之前对新插入的id字段值进行加1求和计算。

```MySQL
CREATE TRIGGER in_newstu BEFORE INSERT ON newstudent

FOR EACH ROW SET @ss =NEW.id +1;
```

设置变量的初始值为0：

```mysql
SET @ss =0;
```

插入数据，启动触发器：

```MySQL
INSERT INTO newstudent VALUES(1,'李小璐'),(2,'王昆');
```

再次查询变量ss的值，结果如图10-1所示。

<img src="img/图10-1.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-1 查询变量ss的值

从结果可以看出，在插入数据时执行了触发器in_newstu。

【例10-2】创建一个触发器，当插入的id等于3时将姓名设置为“童林”。

SQL 语句如下：

```MySQL
DELIMITER //

CREATE TRIGGER name_student

BEFORE INSERT

ON newstudent

FOR EACH ROW

BEGIN

IF new.id=3 THEN

set new.name＝'童林';

END IF;

END//

DELIMITER//
```

执行过程如图10-2所示。

<img src="img/图10-2.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-2 创建触发器

往数据表中插入演示数据，检查触发器是否启动，SQL语句如下：

```MySQL
INSERT INTO newstudent VALUES(3,'童林');
```

查询 newstudent表中的数据，结果如图10-3所示。

<img src="img/图10-3.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-3 查询 newstudent 表中的数据
    
</div>

可见插入的数据中的name字段发生了变化，说明触发器正常执行了。

### 10.2.2.创建多条执行语句触发器

创建多条执行语句的触发器的语法格式如下：

```MySQL
CREATE TRIGGER trigger name trigger time trigger_event
ON tbl name FOR EACH ROW trigger_stmt
```

其中，trigger＿name 标识触发器的名称，由用户自行指定；trigger＿time 标识触发时机，可以指定为 before或after； trigger＿event标识触发事件，包括INSERT、UPDATE和DELETE；tbl＿name标识建立触发器的表名，即在哪张表上建立触发器；trigger＿stmt是触发器程序体。触发器程序可以使用BEGIN和END作为开始和结束，中间包含多条语句。

【例10-3】创建一个包含多条执行语句的触发器。

首先创建数据表testl、test2和test3，SQL 语句如下：

```MySQL
CREATE TABLE test1(a1 INT);

CREATE TABLE test2(a2 INT);

CREATE TABLE test3 (a3 INT);
```

创建触发器，当向 testl 插入数据时将al的值进行加100操作，然后将该值插入到a2字段中，将al的值进行加200操作，然后将该值插入到a3字段中。

SQL 语句如下：

```MySQL
DELIMITER//

CREATE TRIGGER testmm BEFORE INSERT ON test1

FOR EACH ROW

BEGIN

INSERT INTO test2 SET a2=NEW.al+100;

INSERT INTO test3 SET a3=NEW.a1+200;

END//

DELIMITER;
```

执行过程如图10-4所示。

<img src="img/图10-4.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-4 创建多条执行语句的触发器

往数据表 test1中插入数据：

```MySQL
INSERT INTO test1 VALUES(1); 
```

查看数据表 testl中的数据，如图10-5所示。查看数据表test2中的数据，如图10-6所示。查看数据表test3中的数据，如图10-7所示。

<img src="img/图10-5.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图10-5 查看数据表test1中的数据
    
</div>

<img src="img/图10-6.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-6 查看数据表test2中的数据
    
</div>

<img src="img/图10-7.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-7 查看数据表test3中的数据
    
</div>

执行结果显示，在向testl表插入记录的时候test2和test3表都发生了变化。

## 10.3.查看触发器

> **查看触发器是指查看数据库中已存在的触发器的定义、状态和语法信息等，用户可以通过执行语句来查看已经创建的触发器。**

### 10.3.1.通过执行语句查看触发器

通过SHOW TRIGGERS查看触发器的语句如下：

```MySQL
SHOW TRIGGERS;
```

【例10-4】通过SHOW TRIGGERS查看触发器。

SQL语句如下:

```MySQL
SHOW TRIGGERS;
```

使用SHOW TRIGGERS查看触发器，结果如图10-8所示。

<img src="img/图10-8.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-8  使用SHOW TRIGGERS查看触发器 
    
</div>

可以看到信息显示比较混乱，如果在SHOW TRIGGERS的后面添加上“\G”，显示的信息会比较有条理，执行结果如图10-9所示。

<img src="img/图10-9.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-9  优化显示信息
    
</div>

Trigger 表示触发器的名称，在这里两个触发器的名称分别为in＿newstu和testmm；Event表示激活触发器的事件；Table 表示激活触发器的操作对象表；Timing 表示触发器触发的时间；Statement 表示了触发器执行的操作。另外还有一些其他信息，比如SQL的模式、触发器的定义账户和字符集等。

### 10.3.2.通过查看系统表查看触发器

在MySQL中所有触发器的定义都存在于INFORMATION＿SCHEMA数据库的TRIGGERS表中，用户可以通过SELECT语句来查看，具体的语法格式如下：

```MySQL
SELECT * FROM INFORMATION SCHEMA.TRIGGERS WHERE condition;
```

【例10-5】通过SELECT语句查看触发器。

代码如下：

```MySQL
SELECT * FROM INFORMATION_SCHEMA.TRIGGERS 
WHERE TRIGGER_NAME='testmm'\G 
```

上述代码通过WHERE来指定查看特定名称的触发器，查看结果如图10-10所示。

<img src="img/图10-10.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-10 通过SELECT语句查看触发器

在上面的执行结果中，TRIGGER SCHEMA表示触发器所在的数据库，TRIGGER NAME 后面是触发器的名称，EVENT＿OBJECT＿TABLE表示在哪个数据表上触发，ACTION＿STATEMENT表示触发器触发的时候执行的具体操作，ACTION＿ORIENTATION 是 ROW，表示在每条记录上都触发，ACTION＿TIMING表示了触发的时刻是BEFORE，其余的是和系统相关的信息。

用户也可以不指定触发器名称，这样将查看所有的触发器，SQL语句如下：

```MySQL
SELECT * FROM INFORMATION SCHEMA.TRIGGERS \G
```

该语句执行后会显示这个TRIGGERS表中所有的触发器信息。

## 10.4.删除触发器

使用DROP TRIGGER 语句可以删除MySQL中已经定义的触发器，删除触发器的基本语法格式如下：

```MySQL
DROP TRIGGER [schema_name.] [IF EXISTS] trigger_name
```

其中，schema name 表示数据库名称，是可选的，如果省略，将从当前数据库中舍弃触发程序；trigger＿name是要删除的触发器的名称。

使用IF EXISTS来阻止不存在的触发程序被删除的错误。如果待删除的触发程序不存在，系统会出现触发程序不存在的提示信息。

【例10-6】删除一个触发器。

SQL 语句如下：

```MySQL
DROP TRIGGER mytest.testmm;
```

在上面语句中mytest是触发器所在的数据库，testmm是一个触发器的名称。语句的执行结果如图10-11所示。

<img src="img/图10-11.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图10-11  删除触发器

触发器testmm删除成功。