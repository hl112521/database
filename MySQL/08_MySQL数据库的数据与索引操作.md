# 第八章 MySQL数据库的数据与索引操作

## 8.1 插入数据记录

> **语句向数据库表中插入新的数据记录。插入的方式有插入完整的记录、插入记录的一部分、插入多条记录以及插入另一个查询的结果，下面将介绍这些内容。**

### 8.1.1.插入完整的数据记录

使用基本的INSERT语句插入数据要求指定表名称和插入到新记录中的值，基本语法格式如下：

```MySQL
INSERT INTO table_name(column_list)VALUES(value_list);
```

table＿name 指定要插入数据的表名，column＿list指定要插入数据的哪些列，value＿list指定每个列应对应插入的数据。

本章将使用样例表 product，创建语句如下：

```MySQL
CREATE TABLE product
(
	id		INT UNSIGNED NOT NULL AUTO_INCREMENT,
	name	CHAR(20) NOT NULL DEFAULT '',
	price INT(11) NOT NULL DEFAULT 0,synopsis CHAR(60) NULL,
	PRIMARY KEY(id)
);
```

向表中的所有字段插入值的方法有两种：一种是指定所有字段名，另一种是完全不指定字段名。

【例8-1】在product表中插入一条新记录，id值为1、name值为冰箱、price 值为2100、synopsis 值为'这是最新型号的冰箱'。

在执行插入操作之前使用SELECT语句查看表中的数据：

```MySQL
SELECT * FROM product;
```

查询结果如图8-1所示。

结果显示当前表为空，没有数据，接下来执行插入操作：

```MySQL
INSERT INTO product(id,name,price,synopsis)

VALUES(1 '冰箱' ,2100,'这是最新型号的冰箱');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-2所示。

<img src="img/图8-1.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-1 查看表中的数据

<img src="img/图8-2.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-2 插入数据到数据表

可以看到插入记录成功。在插入数据时指定了product表的所有字段，因此将为每一个字段插入新的值。

INSERT 语句后面的列名称的顺序可以不是定义product表时的顺序，即插入数据时不需要按照表定义的顺序插入，只要保证值的顺序与列字段的顺序相同就可以，例如下面的例子。

【例8-2】在product 表中插入一条新记录，id值为2、name值为洗衣机、price 值为 2800、synopsis值为'这是最新型号的洗衣机'。

SQL 语句如下：

```MySQL
INSERT INTO product(price,name,id,synopsis) 
VALUES(2800,'洗衣机', 2,'这是最新型号的洗衣机');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-3所示。

<img src="img/图8-3.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-3 插入指定序列数据

由结果可以看到，INSERT语句成功插入了一条记录。

在使用 INSERT插入数据时允许列名称列表column＿list为空，此时值列表中需要为表的每一个字段指定值，并且值的顺序必须和数据表中字段定义时的顺序相同。

【例8-3】在product表中插入一条新记录。

SQL 语句如下：

```MySQL
INSERT INTO product
VALUES(3,'空调',3600,'这是最新型号的空调');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-4所示。

<img src="img/图8-4.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-4  匹配表字段插入数据

### 8.1.2.为表的指定字段插入数据

为表的指定字段插入数据，就是在INSERT 语句中只向部分字段插入值，而其他字段的值为表定义时的默认值。

【例8-4】在product表中插入一条新记录，name值为电风扇、price 值为 150、synopsis值为'这是最新型号的电风扇。

SQL 语句如下：

```MySQL
INSERT INTO product(name,price,synopsis)
VALUES('电风扇',150,'这是最新型号的电风扇');
```

使用SELECT查询表中的记录，SQL语句如下：

```MySQL
SELECT *FROM product;
```

查询结果如图8-5所示。

<img src="img/图8-5.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-5 为指定字段插入数据

可以看到插入记录成功。例如这里的id字段，查询结果显示该字段自动添加了一个整数值4。

在插入记录时，如果某些字段没有指定插入值，MySQL将插入该字段定义时的默认值。下面的例子说明在没有指定列字段时插入默认值。

【例8-5】在product表中插入一条新记录，name值为电脑、synopsis值为'这是主流配置的电脑'。SQL 语句如下：

```MySQL
INSERT INTO product(name,synopsis)VALUES('电脑','这是主流配置的电脑');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-6所示。

<img src="img/图8-6.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-6 非指定数据字段默认数据为0

可以看到，在本例的插入语句中没有指定 price字段值，查询结果显示price 字段在定义时指定默认为0，因此系统自动为该字段插入0值。

### 8.1.3.同时插入多条数据记录

NSERT 语句可以同时向数据表中插入多条记录，在插入时指定多个值列表，每个值列表之间用逗号分隔开，基本语法格式如下：

```MySQL
INSERT INTO table_name (column_list)

VALUES(value list1),(value_list2),··,(value_listn);
```

value＿listl、value＿list2、···、value＿listn分别表示插入记录的字段的值列表。

【例8-6】在product表的 name、price和synopsis字段中指定插入值，同时插入两条新记录。

SQL 语句如下：

```MySQL
INSERT INTO product(name, price, synopsis) 
VALUES('热水器',2100,'这是最新型号的热水器'),('消毒柜',1200,'这是最新型号的消毒柜');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-7所示。

<img src="img/图8-7.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-7  同时插入多条数据记录

由结果可以看到，INSERT 语句执行后product表中添加了两条记录，其name、price 和 synopsis字段分别为指定的值，id字段为 MySQL添加的默认的自增值。

【例8-7】在product表中不指定插入列表，同时插入两条新记录。

SQL 语句如下：

```MySQL
INSERT INTO product
VALUES(8,'饮水机',260,'这是最新型号的饮水机'),
(9,'吸尘器',850,'这是最新型号的吸尘器');
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-8所示。

<img src="img/图8-8.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-8 不指定字段列表插入多条数据记录

由结果可以看到，INSERT语句执行后product表中添加了两条记录，与前面介绍的单个INSERT的语法不同，product表名后面没有指定插入字段列表，因此VALUES 关键字后面的多个值列表都要为每一条记录的每一个字段列指定插入值，并且这些值的顺序必须和product表中字段定义的顺序相同。

## 8.1.4.插入查询结果

INSERT 可以将SELECT语句查询的结果插入到表中，如果想从另外一个表中合并个人信息到product表，不需要把每一条记录的值一个一个输入，只需要使用一条 INSERT语句和一条SELECT语句组成的组合语句即可快速地从一个或多个表中向一个表中插入多个行。其基本语法格式如下：

```MySQL
INSERT INTO table name1  (column_list1) 

SELECT (column_list2) FROM table_name2 WHERE (condition)
```

table namel 指定待插入数据的表；column listl 指定待插入表中要插入数据的那些列；table name2指定插入数据是从哪个表中查询出来的；column list2 指定数据来源表的查询列，该列表必须和 column＿listl列表中的字段个数相同、数据类型相同；condition指定SELECT语句的查询条件。

【例8-8】从product＿new表中查询所有记录，并将其插入到product表中。

首先创建一个名为product＿new的数据表，其结构与product表的结构相同，SQL语句如下：

```MySQL
CREATE TABLE product_new 
(
	id          INT UNSIGNED NOT NULL AUTO_INCREMENT,
    name       CHAR(20) NOT NULL DEFAULT '',
	price       INT(11) NOT NULL DEFAULT 0,
	synopsis   CHAR(60) NULL,
	PRIMARY KEY (id)
);
```

向product＿new表中添加两条记录：

```MySQL
INSERT INTO productnew
VALUES(10,'电磁炉',260,'这是最新型号的电磁炉'),
(11,'电饭煲',480,'这是最新型号的电饭煲');
```

查询 product＿new表中的数据：

```MySQL
SELECT * FROM product new;
```

查询结果如图8-9所示。

<img src="img/图8_9.png" alt="img" style="zoom: 150%;" />

	<div align = "center" style="font-weight:bold">图8-9 将查询数据插入到指定数据表中
	    
	</div>

可以看到插入记录成功，product_new表中现在有两条记录。接下来将product＿new表中的所有记录插入到product表中，SQL 语句如下：

```MySQL
INSERT INTO product(id, name, price, synopsis)
SELECT id, name, price, synopsis FROM product_new;
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-10所示。

<img src="img/图8-10.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-10  查看插入数据结果

由结果可以看到，INSERT语句执行后product表中多了两条记录，这两条记录和product＿new 表中的记录完全相同，数据转移成功。

## 8.2.修改数据记录

在MySQL中使用UPDATE语句修改数据表中的记录，可以修改特定的行或者同时修改所有的行。其基本语法结构如下：

```MySQL
UPDATE table name

SET column_name1=value1,column_name2=value2,··.,column_namen=valuen

WHERE (condition);
```

column＿namel、column＿name2、···、column＿namen 为指定更新的字段的名称；valuel、value2、···、 valuen 为相对应的指定字段的更新值；condition指定修改的记录需要满足的条件。当更新多个列时，每个“列-值”对之间用逗号隔开，最后一列之后不需要逗号。

【例8-9】在product表中更新id值为11的记录，将price字段值改为6600，将name字段值改为跑步机，将synopsis修改为'这是最新型号的跑步机。

SQL 语句如下：

```MySQL
UPDATE product SET name='跑步机'，price=6600，

synopsis='这是最新型号的跑步机'WHERE id=11；
```

在执行修改操作后，可以使用SELECT 语句查看数据是否发生变化：

```MySQL
SELECT * FROM product;
```

查询结果如图8-11所示。

<img src="img/图8-11.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-11  更新指定列数据

</div>

【例8-10】在product表中更新 price 值为2000～3000的记录，将 synopsis字段值都改为'这是最新型号的产品'。

```MySQL
UPDATE product SET synopsis='这是最新型号的产品' WHERE price BETWEEN 2000 AND 3000;
```

在执行更新操作前，可以使用SELECT语句查看数据是否发生变化:

```MySQL
SELECT FROM product
WHERE Drice BETWEEN 2000 AND 3000;
```

查询结果如图8-12所示。

<img src="img/图8-12.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-12 更新为统一数据

由结果可以看到，UPDATE执行后成功地将表中符合条件的记录的synopsis字段值都改为了'这是最新型号的产品'。

## 8.3.删除数据记录

从数据表中删除数据使用DELETE语句，DELETE 语句允许用WHERE子句指定删除条件。DELETE语句的基本语法格式如下：

```MySQL
DELETE FROM table_name [WHERE <condition>];
```

table＿name 指定要执行删除操作的表；“［WHERE＜condition＞］”为可选参数，指定删除条件，如果没有WHERE子句，DELETE语句将删除表中的所有记录。

【例8-11】在product表中删除name等于洗衣机的记录。

在执行删除操作前使用SELECT语句查看当前name='洗衣机的记录：

```MySQL
SELECT * FROM product WHERE name= '洗衣机';
```

查询结果如图8-13所示。

<img src="img/图8-13.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-13  删除指定名称列数据

可以看到，现在表中有name='洗衣机的记录，下面使用DELETE语句删除记录：

```MySQL
DELETE FROM product WHERE name='洗衣机'；
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product WHERE name='洗衣机'；
```

查询结果如图8-14所示。

<img src="img/图8-14.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-14  查询结果

查询结果为空，说明删除操作成功。

【例8-12】在product表中使用DELETE语句同时删除多条记录，在前面的UPDATE 语句中将price字段值为2000～3000的记录的synopsis字段值修改为'这是最新型号的产品，在这里删除这些记录。

SQL 语句如下：

```MySQL
DELETE FROM product WHERE price BETWEEN 2000 AND 3000; 
```

在执行删除操作前使用SELECT语句查看当前的数据：

```MySQL
SELECT * FROM product WHERE price BETWEEN 2000 AND 3000; 
```

查询结果如图8-15所示。

<img src="img/图8-15.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-15  查询需要删除的多条记录

可以看到，price字段值为2000～3000的记录存在于表中，下面使用DELETE删除这些记录：

语句执行完毕，查看执行结果：

```MySQL
SELECT* FROM product WHERE price BETWEEN 2000 AND 3000;
```

查询结果如图8-16所示。

<img src="img/图8-16.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-16  删除多条记录

查询结果为空，表明删除多条记录成功。

【例8-13】删除 product表中的所有记录。

SQL 语句如下：

```MySQL
DELETE FROM product;
```

在执行删除操作前使用SELECT语句查看当前的数据:

```MySQL
SELECT * FROM product; 
```

查询结果如图8-17所示。

<img src="img/图8-17.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-17 显示所有数据记录

结果显示product表中还有8条记录，执行DELETE语句删除这8条记录：

```MySQL
DELETE FROM product;
```

语句执行完毕，查看执行结果：

```MySQL
SELECT * FROM product;
```

查询结果如图8-18所示。

查询结果为空，表明删除表中的所有记录成功，现在product表中已经没有任何数据记录。

<img src="img/图8-18.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-18  删除所有记录

## 8.4.索引概述

索引是对数据库表中一列或多列的值进行排序的一种结构，使用索引可以提高数据库中特定数据的查询速度。

索引是一个单独的、存储在磁盘上的数据库结构，它们包含着对数据表中所有记录的引用指针。索引用于快速找出在某个或多个列中有一特定值的行，所有 MySQL 列类型都可以被索引，对相关列使用索引是减少查询操作时间的最佳途径。

例如数据库中有10万条记录，现在要执行查询“SELECT＊FROM table WHERE num＝100000”。如果没有索引，必须遍历整个表，直到num等于100000的这一行被找到为止；如果在num列上创建索引，MySQL不需要任何扫描，直接在索引里面找100000就可以得知这一行的位置。可见，索引的建立可以加快数据库的查询速度。

索引的优点主要如下：

（1）通过创建唯一索引可以保证数据库表中每一行数据的唯一性。

（2）可以大大加快数据的查询速度这也是创建索引最主要的原因。

（3）在实现数据的参考完整性方面可以加速表和表之间的连接。

（4）在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间。

增加索引也有许多不利的方面，主要如下：

（1）创建索引和维护索引要耗费时间，并且随着数据量的增加所耗费的时间也会增加。

（2）索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间，如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。

（3）当对表中的数据进行增加、删除和修改的时候索引也要动态维护，这样就降低了数据的维护速度。

## 8.5.索引的分类

MySQL的索引可以分为以下几类。

**1．普通索引和唯一索引**

普通索引：MySQL中的基本索引类型，允许在定义索引的列中插入重复值和空值。

唯一索引：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。主键索引是一种特殊的唯一索引，不允许有空值。

唯一索引：索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。主键索引是一种特殊的唯一索引，不允许有空值。

**2．单列索引和组合索引**

·单列索引：即一个索引只包含单个列，一个表可以有多个单列索引。

·组合索引：在表的多个字段组合上创建的索引，只有在查询条件中用了这些字段的左边字段时索引才会被使用。使用组合索引遵循最左前缀集合。

**3．全文索引**

全文索引类型为FULLTEXT，在定义索引的列上支持值的全文查找，允许在这些索引列中插入重复值和空值。全文索引可以在CHAR、VARCHAR或者TEXT类型的列上创建。在MySQL中只有MyISAM存储引擎支持全文索引。

**4．空间索引**

空间索引是对空间数据类型的字段建立的索引，MySQL 中的空间数据类型有4种，分别是GEOMETRY、POINT、LINESTRING 和POLYGON。MySQL 使用 SPATIAL 关键字进行扩展，使得能够 用与创建正规索引类似的语法创建空间索引。创建空间索引的列必须将其声明为NOTNULL，空间索引只能在存储引擎为MyISAM的表中创建。

## 8.6.创建和查看索引

在使用 CREATE TABLE 创建表时，除了可以定义列的数据类型以外，还可以定义主键约束、外键约束或者唯一性约束，而不论创建哪种约束，在定义约束时相当于在指定列上创建了一个索引。在创建表时创建索引的基本语法格式如下：

```MySQL
CREATE TABLE table_name [col_name data_type]

[UNIQUE|FULLTEXT|SPATIAL][INDEX|KEY][index_name](col_name[length])[ASC |DESC]
```

UNIQUE、FULLTEXT 和 SPATIAL 为可选参数，分别表示唯一索引、全文索引和空间索引；INDEX和KEY为同义词，两者的作用相同，用来指定创建索引；index＿name 指定索引的名称，为可选参数，如果不指定，MySQL 默认col＿name为索引名称；col＿name为需要创建索引的字段列，该列必须从数据表中该定义的多个列中选择；length 为可选参数，表示索引的长度，只有字符串类型的字段才能指定索引长度；ASC或DESC指定升序或者降序的索引值存储。

### 8.6.1.创建和查看普通索引

普通索引是最基本的索引类型，没有唯一性之类的限制，其作用只是加快对数据的访问速度。

【例8-14】在fruit表中的city＿index字段上建立普通索引。

SQL 语句如下：

```MySQL
CREATE TABLE fruit
(
	id		INT NOT NULL,
	name	VARCHAR(50) NOT NULL,
	price	decimal(8,2) NOT NULL,
	city	VARCHAR(1003NOT NULL,
	INDEX(city)
);
```

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```MySQL
SHOW CREATE table fruit ;
```

查看结果如图8-19所示。

<img src="img/图8-19.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-19  创建索引

由结果可以看到，在fruit表的city字段上成功地创建了索引，其索引名称city由MySQL 自动添加。使用EXPLAIN 语句查看索引是否正在使用：

```MySQL
EXPLAIN SELECT * FROM fruit WHERE city ='上海';
```

查看结果如图8-20所示。

<img src="img/图8-20.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图8-20  查看索引

对EXPLAIN语句输出结果中的各个行解释如下：

（1）select＿type 行指定所使用的SELECT查询类型，这里值为SIMPLE，表示简单的SELECT，不使用UNION或子查询。其他可能的取值有PRIMARY、UNION、SUBQUERY等。

（2）table行指定数据库读取的数据表的名字，它们按被读取的先后顺序排列。

（3）type行指定了本数据表与其他数据表之间的关联关系，可能的取值有system、const、eq＿ref、ref、range、index 和All。 

（4）possible＿keys行给出了MySQL 在搜索数据记录时可以选用的各个索引。

（5）key行是MySQL实际选用的索引。

（6）key＿len行给出索引按字节计算的长度，key＿len数值越小表示越快。

（7）ref行给出了关联关系中另一个数据表里的数据列的名字。

（8）rows行是MySQL在执行这个查询时预计会从这个数据表里读出的数据行的个数。

（9）Extra行提供了与关联操作有关的信息。

可以看到，possible_keys 和 key 的值都为 cit， 在查询时使用了索引。

### 8.6.2.创建和查看唯一索引

唯一索引与前面的普通索引类似，不同的是索引列的值必须唯一，但允许有空值。如果是组合索引，
则列值的组合必须唯一。
【例10-15】创建newfruit表，在表中的id字段上使用UNIQUE关键字创建唯一索引。

```MySQL
CREATE TABLE newfruit
(
	id		INT NOT NULL,
	name	VARCHAR(50) NOT NULL,
	price	decimal(8,2) NOT NULL,
	city	VARCHAR(100) NOT NULL,
	UNIQUE INDEX UnigIdx(id)
);
```

该语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

查看结果如图8-21所示。

<img src="img/图8-21.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-21  创建和查看唯一索引

由结果可以看到，在id字段上已经成功建立了一个名为Uniqldx的唯一索引。

### 8.6.3.创建和查看多列索引

组合索引是在多个字段上创建一个索引。

【例8-16】创建fruit2表，在表中的id、name和price字段上建立组合索引。

SQL 语句如下：

```MySQL
CREATE TABLE fruit2
(
	id		INT NOT NULL,
	name	VARCHAR(50) NOT NULL,
	price	decimal (8,2) NOT NULL,
	city	VARCHAR (100) NOT NULL,
	INDEX Mmindex(id,name, price)
);
```

该语句执行完毕之后，使用SHOW CREATE TABLE 查看表结构：

```MySQL
SHOW CREATE table fruit2;
```

查看结果如图8-22所示。

<img src="img/图8-22.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-22 建立组合索引

由结果可以看到，在id、name和price字段上已经成功建立了一个名为Mmindex的组合索引。

在fruit2表中查询id和name字段，使用EXPLAIN语句查看索引的使用情况：

```MySQL
EXPLAIN SELECT * FROM fruit2 WHERE id=1 AND name='苹果';
```

查看结果如图8-23所示。

<img src="img/图8-23.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-23  查看索引

可以看到，在查询id和name字段时使用了名为Mmindex的索引，如果查询name与price的组合或者单独查询 name和price字段，将不会使用索引。

例如这里只查询name字段：

```MySQL
EXPLAIN SELECT * FROM fruit2 WHERE name='苹果';
```

查看结果如图8-24所示。

<img src="img/图8-24.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-24  查看具体字段

从结果可以看出，possible＿keys和key的值为NULL，说明在查询的时候并没有使用索引。

### 8.6.4.创建和查看全文索引

全文索引可以用于全文搜索。只有MyISAM存储引擎支持全文索引，并且只为CHAR、VARCHAR和TEXT列。全文索引只能添加到整个字段上，不支持局部（前缀）索引。

【例8-17】创建fruit3表，在表中的city字段上建立全文索引。

SQL 语句如下：

```MySQL
CREATE TABLE fruit3
(
	id		INT NOT NULL,
	name	VARCHAR(50) NOT NULL,
	price	decimal (8,2) NOT NULL,
	city	VARCHAR(100) NOT NULL,
	FULLTEXT INDEX Fullindex(city)
)ENGINE=MyISAM;
```

语句执行完毕之后，使用SHOW CREATE TABLE查看表结构：

```MySQL
SHOW CREATE table fruit3;
```

查看结果如图8-25所示。

<img src="img/图8-25.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-25 建立全文索引

由结果可以看到，在city字段上已经成功建立了一个名为Fullindex的全文索引。全文索引非常适合大型数据集，对于小的数据集，它的用处可能比较小。

## 8.7.删除索引

> **在MySQL中删除索引使用DROP INDEX 或者 ALTER TABLE, 两者可以实现相同的功能。**

### 8.7.1.使用DROP INDEX删除索引

使用DROP INDEX删除索引的基本语法格式如下：

```MySQL
DROP INDEX index_name ON table_name;
```

【例10-18】删除newfruit表中名称为UniqIdx的唯一索引。

SQL语句如下:

```MySQL
DROP INDEX UniqIdx ON newfruit;
```

语句执行完毕，使用SHOW语句查看索引是否被删除：

```MySQL
SHOW CREATE table newfruit;
```

查看结果如图8-26所示。

<img src="img/图8-26.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图8-26  使用DROP INDEX 语句删除索引

可以看到，在newfruit表中已经没有名称为UniqIdx的唯一索引，删除索引成功。

### 8.7.2.使用ALTER TABLE删除索引

使用 ALTER TABLE删除索引的基本语法格式如下：

```MySQL
ALTER TABLE table name DROP INDEX index_name;
```

【例8-19】删除fruit2表中名称为Mmindex的组合索引。

首先查看fruit2表中是否有名称为Mmindex的索引，输入SHOW语句如下：

```MySQL
SHOW CREATE table fruit2;
```

查看结果如图8-27所示。

<img src="img/图8-27.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-27  使用ALTER TABLE 删除索引 

从查询结果可以看到，fruit2表中有名称为Mmindex的组合索引。

下删除该索引，输入删除语句如下：

```MySQL
ALTER TABLE fruit2 DROP INDEX Mmindex;
```

语句执行完毕，使用SHOW语句查看索引是否被删除：

```MySQL
SHOW CREATE table fruit2;
```

查看结果如图8-28所示。

<img src="img/图8-28.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图8-28 查看删除索引

</div>

由结果可以看出，fruit2表中已经没有名称为 Mmindex 的组合索引，删除索引成功。