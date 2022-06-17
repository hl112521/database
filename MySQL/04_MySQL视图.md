# 第四章 MySQL视图

## 4.1.视图的概念

 视图（View）是一个由查询语句定义数据内容的表，表中的数据内容就是SQL查询语句的结果集，行和列的数据均来自SQL查询语句中使用的数据表。之所以说视图是虚拟的表，是因为视图并不在数据库中真实存在，而是在引用视图时动态生成的。

视图一经定义便存储在数据库中，与其相对应的数据并没有像表那样在数据库中再存储一份，通过视图看到的数据只是存放在基本表中的数据。对视图的操作与对表的操作一样，可以对其进行查询、修改和删除。当对通过视图看到的数据进行修改时，相应的基本表的数据也要发生变化，同时，若基本表的数据发生变化，则这种变化也可以自动反映到视图中。

下面有employee 表和 office表，在employee表中包含了员工的编号和姓名，在office表中包含了员工的编号和部门，现在公布姓名和部门，应该如何解决？通过学习后面的内容就可以找到完美的解决方案。

表设计如下：

```mysql
CREATE TABLE employee
(
	s_id 	INT,
	name 	VARCHAR(40)
);
CREATE TABLE office
(
	s_id		INT,
	department 	VARCHAR(40)
);
```

为表中插入演示数据，SQL语句如下：

```mysql
INSERT INTO employee VALUES(1,'王丽'), (2,'张毅'), (3,'刘天佑');

INSERT INTO office VALUES(1,'财务部'), (2,'销售部'), (3,'管理部');
```

通过DESC语句查看表的设计，可以获得字段、字段的定义、是否为主键、能否为空、默认值和扩展信息。

视图提供了一个很好的解决方法，创建一个视图，这些信息来自表的部分内容，其他信息不取，这样既能满足要求也不破坏表原来的结构。

那么使用视图和查询数据表又有哪些优势呢？

（1)使用视图简单，操作视图和操作数据表完全是两个概念，用户不用理清数据表之间复杂的逻辑关系，而且将经常使用的SQL数据查询语句定义为视图，可以有效地避免代码重复，从而减少工作量。

（2)使用视图安全，用户只访问到视图给定的内容集合，这些都是数据表的某些行和列，避免用户直接操作数据表引发的一系列错误。

（3)使用视图相对独立，应用程序访问是通过视图访问数据表，从而程序和数据表之间被视图分离。如果数据表有变化，完全不用去修改SQL语句，只需要调整视图的定义内容，不用调整应用程序代码。

（4)复杂的查询需求：可以进行问题分解，然后创建多个视图获取数据，将视图联合起来就能得到需要的结果了。

视图的工作机制：在调用视图的时候才会执行视图中的SQL进行取数据操作。视图的内容没有存储，而是在视图被引用的时候才派生出数据，这样不会占用空间，由于是即时引用，视图的内容总是与真实表的内容一致。

视图这样设计最主要的好处就是比较节省空间，当数据内容总是一样时，那么就不需要维护视图的内容，维护好真实表的内容就可以保证视图的完整性。

## 4.2.视图的基本操作

> 在理解视图的基本概念后需要掌握视图的基本操作，包括视图的创建、视图的修改和视图的删除。

### 4.2.1.创建视图

创建视图的语法如下：

```mysql
CREATE [ALGORITHM={UNDEFINED|MERGE|TEMPTABLE}]
VIEW view_name AS
SELECT colum_name(s) FROM table_name
[WITH [CASCADED|LOCAL] CHECK OPTION];
```

 各个参数的含义如下。

（1）ALGORITHM：可选参数，表示视图选择的算法。

（2）UNDEFINED：MySQL 将自动选择所要使用的算法。

（3）MERGE：将视图的语句与视图定义合并起来，使得视图定义的某一部分取代语句的对应部分。

（4）TEMPTABLE：将视图的结果存入临时表，然后使用临时表执行语句。

（5）view_name: 指创建视图的名称，可包含其属性列表。

（6） column_name（s）：指查询的字段，也就是视图的列名。

（7）table＿name：指从哪个数据表获取数据，这里也可以从多个表获取数据，格式写法请读者自行参考
SQL联合查询。

（8）WITH CHECK OPTION：可选参数，表示更新视图时要保证在视图的权限范围内。

（9）CASCADED：在更新视图时满足所有相关视图和表的条件才进行更新。

（10）LOCAL：在更新视图时满足该视图本身定义的条件即可更新。

该语句能创建新的视图，如果给定了OR REPLACE子句，该语句还能替换已有的视图。select_statement是一种SELECT语句，它给出了视图的定义。该语句可从基表或其他视图进行选择。

> 注意：该语句要求具有针对视图的CREATE VIEW权限，以及针对由SELECT语句选择的每一列上的某些权限。对于在SELECT 语句中其他地方使用的列，必须具有 SELECT 权限。如果还有OR REPLACE子句，必须在视图上具有DROP权限。

【例4-1】在数据表employee上创建一个名称为e_view的视图。

SQL 语句如下：

```mysql
CREATE VIEW e_view 
AS SELECT * FROM employee;
```

执行结果如图4-1所示。

执行结果显示为Query OK，表示代码执行成功。创建视图并不影响以前的数据，因为视图只是一个虚拟表。使用DESC语句查询视图的结构，结果如图4-2所示。

<img src="img/图4-1 创建e_view视图.png" style="zoom:200%;" />

<div align = "center" style="font-weight:bold"> 图4-1 创建e_view视图</div>

<img src="img/图4-2 查询视图的结构.png" style="zoom:200%;" />

<div align = "center" style="font-weight:bold"> 图4-2 查询视图的结构</div>

从结果可以看出，e view视图的结构和原数据表employee是一样的。表和视图共享数据库中相同的名称空间，因此数据库不能包含具有相同名称的表和视图。

视图必须具有唯一的列名，不能有重复，就像基表那样。在默认情况下，由SELECT语句检索的列名将用作视图的列名。

下面开始查询e_view视图中的数据，SQL 语句如下：

```mysql
SELECT * FROM e_view;
```

 执行结果如图4-3所示。

<img src="img/图4-3 e_view视图中的数据.png" style="zoom:200%;" />

<div align = "center" style="font-weight:bold">图4-3 e_view视图中的数据</div>

【例4-2】在数据表office上创建名为 office_view的视图。

创建视图的SQL 语句如下：

```mysql
CREATE VIEW office_view (bb)
AS SELECT department FROM office;
```

执行后使用DESC语句查看视图的结构，结果如图4-4所示。

<img src="img/图4-4 office_view 视图的结构.png"/>

<div align = "center" style="font-weight:bold">图4-4 office_view 视图的结构</div>

从结果可以看出，office＿view视图的属性为bb，在创建视图时指定了属性列表。在该实例中SELECT语句查询了office表的 department字段，那么 office＿view视图的bb属性对应 office 表的 department字段：在使用视图时用户接触不到实际操作的表和字段，这样就无形中增加了数据库的安全性。

下面查询 office＿view视图中的数据，SQL语句如下：

```mysql
SELECT *FROM office_view;
```

执行结果如图4-5所示。

<img src="img/图4-5 office_view 视图中的数据.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-5 office_view 视图中的数据</div>

在MySQL中也可以在两个或者两个以上的表中创建视图，使用CREATE VIEW 语句来实现。

 【例4-3】在employee 表和office表上创建视图em_view。

代码如下：

```mysql
CREATE VIEW em_view (name,  bumen)
AS SELECT employee.name, office.department 
FROM employee, office WHERE employee.s_id= office.s_id; 
```

执行结果如图4-6所示。

<img src="img/图4-6 创建em_view视图.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-6 创建em_view视图</div>

成功创建后查询视图中的数据，SQL语句如下：

```mysql
SELECT * FROM em_view;
```

执行结果如图4-7所示。

<img src="img/图4-7 查询数据.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-7 查询数据</div>

这个例子解决了刚开始提出的那个问题，通过这个视图可以很好地保护基本表中的数据。这个视图中的信息很简单，只包含了姓名和部门。name字段对应employee表中的 name字段，bumen字段对应 office表中的department字段。

### 4.2.2.查看视图基本信息

查看视图的信息可以使用 SHOW TABLE STATUS，具体语法格式如下：

```mysql
SHOW TABLE STATUS LIKE '视图名'; 
```

【例4-4】使用SHOW TABLE STATUS查看视图的信息。

代码如下：

```mysql
SHOW TABLE STATUS LIKE 'em_view';
```

执行结果如图4-8所示。

<img src="img/图4-8 查看视图的信息.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold"> 图4-8 查看视图的信息</div>

执行结果显示，表的Comment的值为VIEW说明该表为视图，其他信息为NULL说明这是一个虚表用同样的方法查看数据表employee，SQL语句如下：

```mysql
SHOW TABLE STATUS LIKE 'employee';
```

 执行结果如图4-9所示。

<img src="img/图4-9 查看数据表信息.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-9 查看数据表信息</div>

从查询结果来看，这里的信息包含了存储引擎、创建时间等，但Comment信息为空，这就是视图和表的区别。

### 4.2.3.查看视图详细信息

使用SHOW CREATE VIEW语句可以查看视图的详细信息，语法格式如下：

```mysql
SHOW CREATE VIEW 视图名；
```

【例4-5】使用SHOW CREATE VIEW查看视图的详细信息。

代码如下：

```mysql
SHOW CREATE VIEW em_view;
```

执行结果如图 4-10 所示。

<img src="img/图4-10 查看视图的详细信息.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-10 查看视图的详细信息</div>

执行的结果显示视图的名称、创建视图的语句等信息。

### 4.2.4.修改视图

 视图的修改是指修改了数据表的定义，当视图定义的数据表字段发生变化时需要对视图进行修改以保证查询的正确进行。在MySQL中使用 CREATE OR REPLACE VIEW语句可以修改视图。当视图存在时可以对视图进行修改，当视图不存在时可以创建视图。

CREATE OR REPLACE VIEW 语句的语法格式如下：

```mysql
CREATE OR REPLACE [ALGORITHM={UNDEFINED|MERGE|TEMPTABLE}]
VIEW 视图名［（属性清单）］
AS SELECT语句
[WITH [CASCADED|LOCAL] CHECK OPTION];
```

各个参数的含义如下。

（1）ALGORITHM：可选，表示视图选择的算法。

（2）UNDEFINED：表示MySQL将自动选择所要使用的算法。

（3）MERGE：表示将使用视图的语句和视图定义合并起来，使得视图定义的某一部分取代语句的对应部分。

（4）TEMPTABLE：表示将视图的结果存入临时表，然后使用临时表执行语句。

（5）视图名：表示要创建的视图的名称。

（6）属性清单：可选，指定了视图中各个属性的名词，在默认情况下与 SELECT 语句中查询的属性相同。

（7）SELECT 语句：一个完整的查询语句，表示从某个表中查出某些满足条件的记录，将这些记录导入视图中。

（8）WITH CHECK OPTION：可选，表示修改视图时要保证在该视图的权限范围之内。

（9）CASCADED：可选，表示修改视图时需要满足跟该视图有关的所有视图和表的条件，该参数为默认值。

（10）LOCAL：表示修改视图时只要满足该视图本身定义的条件即可。

可以发现修改视图的语法和创建视图的语法只有 OR REPLACE 的区别，在使用 CREATE OR REPLACE的时候，如果视图已经存在，则进行修改操作，如果视图不存在，则创建视图。

【例4-6】修改视图em_view。

代码如下：

```mysql
CREATE OR REPLACE VIEW em_view (id, name, bumen)
AS SELECT employee.s_id,employee.name, office.department 
FROM employee, office WHERE employee.s_id= office.s_id; 
```

在修改代码之前首先使用 DESC em_view 语句查看一下em_view视图，以便和更改之后的视图进行对比。

查看结果如图 4-11所示。

<img src="img/图4-11 查看em_view视图.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-11 查看em_view视图</div>

执行修改视图语句，代码如下：

```mysql
CREATE OR REPLACE VIEW em_view (id, name, bumen)
AS SELECT employee.s_id,employee.name, office.department 
FROM employee, office WHERE employee.s_id= office.s_id; 
```

使用 DESC em_view 语句查看修改后的变化， 如图 4-12所示。

<img src="img/图4-12 完成修改.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-12 完成修改</div>

从执行结果来看，相比原来的em_view视图，新的视图多了一个字段。

除了可以使用CREATE OR REPLACE修改视图以外，用户还可以使用ALTER修改视图，语法格式如下：

```mysql
ALTER [ALGORITHM ={UNDEFINED |MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS SELECT_statement
[WITH [CASCADED |LOCAL] CHECK OPTION]
```

该语法中的关键字和前面视图的关键字是一样的，这里就不再介绍。

【例4-7】使用ALTER语句修改e_view视图。

代码如下：

```mysql
ALTER VIEW e_view AS SELECT name FROM employee;
```

在修改代码之前首先使用DESC e_view语句查看一下e_view视图，以便和更改之后的视图进行对比。

查看结果如图4-13所示。

<img src="img/图4-13 e＿view 视图修改前.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-13 e＿view 视图修改前</div>

 执行修改视图语句，代码如下：

```mysql
ALTER VIEW e_view AS SELECT name FROM employee;
```

使用DESC e_view 语句查看修改后的变化，如图4-14所示。

<img src="img/图4-14 e_view 视图修改后.png" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-14 e_view 视图修改后</div>

从执行结果来看，相比原来的e_view视图，新的视图只剩下一个name字段。

> **注意：CREATE OR REPLACE VIEW语句不仅可以修改已经存在的视图，还可以创建新的视图，但ALTER语句只能修改已经存在的视图，因此通常选择CREATE OR REPLACE VIEW语句修改视图。**

 

### 4.2.5.更新视图

 CREATE OR REPLACE和ALTER主要是对视图的结构进行修改，其实在MySQL中还可以对视图内容进行更新，也就是对视图进行UPDATE操作，通过视图增加、删除、修改数据表中的数据，当然对视图的更新操作实际上是对数据表进行操作。

本节介绍更新视图的3种方法，即使用INSERT、UPDATE和DELETE语句更新。

【例4-8】使用UPDATE语句更新em_view视图。

代码如下：

```mysql
UPDATE em_view SET name='王小明' WHERE id=1;
```

在执行视图更新之前查看视图的信息，代码如下：

```mysql
SELECT * FROM em_view;
```

执行结果如图4-15所示。

<img src="img/图4-15 更新视图前.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-15 更新视图前</div>

查看基本表 employee 的信息， SQL语句如下：

```mysql
SELECT * FROM employee;
```

执行结果如图 4-16 所示。

<img src="img/图4-16 更新视图后.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-16 更新视图后</div>

使用 UPDATE 语句更新 em_view 视图，代码如下：

```mysql
UPDATE em_view SET name='王小明' WHERE id=1;
```

查看视图更新之后基本表的内容，代码如下：

```mysql
SELECT * FROM employee;
```

执行结果如图 4-17 所示。

<img src="img/图4-17 修改数据.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-17 修改数据</div>

在对 em_view 视图更新之后，基本表employee 的内容也更新了。

查看视图更新之后视图的内容，代码如下：

```mysql
SELECT * FROM em_view;
```

执行结果如图 4-18所示。

<img src="img/图4-18 更新后的数据表视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-18 更新后的数据表视图</div>

【例 4-9】使用INSERT 语句在基本表 employee 中插入一条记录。

代码如下：

```mysql
 INSERT INTO employee VALUES(4, '孙佳旭');
```

插入数据后查询employee表的数据，执行结果如图4-19所示。

<img src="img/图4-19 添加数据记录.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-19 添加数据记录</div>

查询e_view视图的内容是否发生变化，结果如图4-20所示。

<img src="img/图4-20 添加数据记录后的视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-20 添加数据记录后的视图

向employee表中插入一条记录，通过SELECT查看employee表和e_view视图，可以看到其中的内容也跟着更新。

【例4-10】使用DELETE语句删除e_view视图中的一条记录。

代码如下：

```mysql
DELETE FROM e_view WHERE name ='孙佳旭';
```

删除操作执行后查看e_view视图，结果如图4-21所示。

<img src="img/图4-21 删除记录.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-21 删除记录

查看数据表employee中的数据是否发生变化，执行结果如图4-22所示。

<img src="img/图4-22 完成数据的删除.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-22 完成数据的删除

在 e_view 视图中删除 name=‘孙佳旭’ 的记录，视图中的删除操作最终是通删除基本表中的相关记录实现的，查看删除操作之后的 employee 表和e_view视图，可以看到通过视图删除其所依赖的基本表中的数据。

当视图中包含以下内容时视图的更新操作不能被执行：

（1）视图中不包含基表中被定又为非空的列。

（2）在定义视图的SELECT语句后的字段列表中使用了数学表达式。

（3）在定义视图的SELECT语句后的字段列表中使用了聚合函数。

（4）在定义视图的SELECT语句中使用了DISTINCT、UNION、TOP、GROUP BY或HAVING子句。

> **注意：虽然修改视图的方法有很多，但不建议对视图频繁修改，一般将视图作为虚拟表来完成查询操作。**

### 4.2.6.删除视图

 因为视图本身只是一个虚拟表，没有物理文件存在，所以删除视图并不会删除数据，只是删除视图的结构定义。

删除视图的语法格式如下：

```mysql
DROP VIEW [IF EXISTS] view_name [,view_namel, view_name2...]
```

【例4-11】删除e_view视图。

代码如下：

```mysql
DROP VIEW IF EXISTS e_view;
```

执行结果如图4-23所示。

如果名称为e_view的视图存在，该视图将被删除。最后使用SHOW CREATE VIEW 语句查看操作结果，如图4-24所示。

<img src="img/图4-23 删除e_view 视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-23 删除e_view 视图

<img src="img/图4-24 查看删除结果.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-24 查看删除结果

可以看到e_view视图已经不存在，删除成功。

## 4.3.视图的使用

 对视图使用最多的就是查询，它和普通数据表的SELECT查询没有太多区别。

例如查询em＿view视图，代码如下：

```mysql
SELECT *FROM em_view;
```

执行结果如图4-25所示。

<img src="img/图4-25 查询视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-25 查询视图

因为视图是一种虚拟的数据表，它们的行为和数据表一样，但并不真正包含数据。它们是用底层（真正的）数据表或其他视图定义出来的“假”数据表，用来提供查看数据表中数据的另一种方法，这通常可以简化应用程序，所以用户也可以使用SHOW TABLES查找该视图，并查看视图的基本信息。

如果要选取某给定数据表的数据列的一个子集，把它定义为一个简单的视图是最方便的做法。

假设经常需要从employee 表中选取 name数据列，但又不想每次都写出所有数据列，可以使用如下代码：

```mysql
SELECT s_id, name FROM employee;
```

这虽然简单，但检索出来的数据列不都是用户想要的，解决这个矛盾的办法是定义一个视图，让它只包括想要的数据列：

```mysql
CREATE VIEW vvem AS
SELECT name FROM employee;
```

这个视图就像一个“窗口”，从中只能看到想要的数据列。这意味着可以在这个视图上使用SELECT *，而看到的将是视图定义里给出的那些数据列：

```mysql
SELECT * FROM vvem;
```

查看结果如图4-26所示。

<img src="img/图4-26 查看指定列视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-26 查看指定列视图

 如果在查询某个视图时还使用了一个WHERE子句，MySQL将在执行该查询时把它添加到那个视图的定义上，以进一步限制其检索结果：

```mysql
SELECT * FROM vvem WHERE name = '刘天佑';
```

查看结果如图4-27所示。

<img src="img/图4-27 查看指定视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-27 查看指定视图

在查询视图时还可以使用ORDER BY、LIMIT等子句，其效果与查询一个真正的数据表时的情况一样，在使用视图时只能引用在该视图定义里列出的数据列，也就是说，如果底层数据表里的某个数据列没在视图的定义里，在使用视图的时候就不能引用它：

```mysql
SELECT * FROM vvem WHERE id =1;
```

查看结果如图4-28所示。

<img src="img/图4-28 非定义里数据的显示结果.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-28 非定义里数据的显示结果

在默认情况下，视图里的数据列的名字与SELECT语句里列出的输出数据列相同。如果想明确地改用其他数据列名字，需要在定义视图时在视图名字的后面用括号列出那些新名字：

```mysql
CREATE VIEW vvem1 (v_id, v_name) AS
SELECT s_id, name FROM employee;
```

执行结果如图4-29所示。

<img src="img/图4-29 新命名数据列.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-29 新命名数据列

此后，当使用这个视图时必须使用在括号里给出的数据列名字，而非SELECT语句里的名字。例如以
下代码将会报错：

```mysql
 SELECT s_id,name FROM vvem1;
```

执行结果如图4-30所示。

正确的查询如下：

<img src="img/图4-30 引用错误数据列名称.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-30 引用错误数据列名称

```mysql
SELECT v_id,v_name FROM vvem1; 
```

执行结果如图4-31所示。

<img src="img/图4-31 数据视图.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图4-31 数据视图

