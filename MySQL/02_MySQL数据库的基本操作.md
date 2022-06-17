# 第二章 MySQL数据库的基本操作

## 2.1 创建数据库

> 在进行数据库操作之前首先需要创建数据库，本节将介绍创建数据库的方法。

### 2.1.1  创建数据库的语法形式

创建数据库是在系统磁盘上划分一块区域用于数据的存储和管理，如果管理员在设置权限的时候为用户创建了数据库，则可以直接使用，否则需要自己创建数据库。在MySQL中创建数据库的基本语法如下：

```mysql
CREATE DATABASE database_name;
```

> database_name为要创建的数据库的名称，该名称不能与已经存在的数据库重名。

### 2.1.2 创建数据库实例

在MySQL安装完成之后，系统自动创建几个默认的数据库，这几个数据库存放在 `data` 目录下，用户可以使用数据库查询语句 “SHOW DATABASES;” 进行查看，如图 2-1 所示。

![](img/图2-1 默认数据库.png)

<div align = "center" style="font-weight:bold">图2-1 默认数据库</div>
从查询结果可以看出，数据库列表中有18个数据库，这些数据库各有用途，除去自己创建的11个数据库，7个为默认数据库，其中`mysql`是必须的，记录用户的访问权限， `test` 数据库通常用于测试工作，其他数据库用于了解即可。

【例 2-1】创建数据库 `mytest`。

输入语句如下：

```mysql
CREATE DATABASE mytest;
```

按 Enter 键执行语句，创建 mytest 的数据库，如图 2-2 所示。

<img src="img/图2-2 创建数据库 mytest.png" style="zoom:200%;" />

<div align = "center" style="font-weight:bold">图2-2 创建数据库 mytest</div>
使用数据库查询语句 “SHOW DATABASES;” 查看数据库 mytest 是否成功创建，执行结果如 图 2-3 所示。

![](img/图2-3 查看数据库.png)

<div align = "center" style="font-weight:bold">图2-3 查看数据库</div>
用户可以在数据库列表中看到刚刚创建的数据库 mytest 以及其他已经存在的数据库。

## 2.2 查看与选择数据库

> 在数据库创建完成之后用户才可以查看和选择数据库。

### 2.2.1 查看数据库

使用 `SHOW CREATE DATABASE`  可以查看指定的数据库。

【例 2-2】查看数据库 mytest。

输入语句如下：

```mysql
SHOW CREATE DATABASE mytest;
```

执行结果如图 2-4 所示。

![](img/图2-4 查看新建数据库.png)

<div align = "center" style="font-weight:bold">图2-4 查看新建数据库</div>
从上面的执行结果可以看出，数据库创建会成功显示相应的创建信息。

### 2.2.2 选择数据库

使用 `USE`语句可以选择数据库，语法格式如下：

```mysql
USE database_name;
```

【例 2-3】 选择数据库 `mytest`

输入语句如下：

```mysql
USE mytest
```

执行结果如图 2-5 所示。

<img src="img/图2-5 选择数据库.png" style="zoom: 200%;" />

<div align = "center" style="font-weight:bold">图2-5 选择数据库</div>
从上面的执行结果可以看出，数据库 `mytest` 被成功选择 。

## 2.3 删除数据库

删除数据库是将已经存在的数据库从磁盘空间中清除，数据库中的所有数据也一起被删除。

### 2.3.1 删除数据库的语法形式

MySQL 删除数据库的基本语法格式如下：

```mysql
DROP DATABASE database_name;
```

其中，`database_name` 是要删除的数据库名称，如果指定的数据库名不存在，则删除出错。

### 2.3.2 删除数据库实例

下面以删除数据库 `mytest` 为例进行讲解。

【例 2-4】删除数据库 `mytest`。

输入语句如下：

```mysql
DROP DATABASE mytest;
```

执行结果如图 2-6 所示。

<img src="img/图2-6 删除数据库.png" style="zoom: 200%;" />

<div align = "center" style="font-weight:bold">图2-6 删除数据库</div>
数据库 `mytest` 被删除之后，使用 SHOW CREATE DATABASE 查看数据库定义， 结果如图 2-7 所示。

<img src="img/图2-7 查看数据库定义.png" style="zoom: 200%;" />

<div align = "center" style="font-weight:bold">图2-7 查看数据库定义</div>
## 2.4 数据库存储引擎

>  数据库存储引擎是数据库底层软件组件，数据库管理系统（DBMS）使用数据引擎进行创建、查询、更新和删除数据的操作。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能，使用不同的存储
> 引擎还可以获得特定的功能。现在许多数据库管理系统支持多种数据引擎。MySQL的核心就是存储引擎。

### 2.4.1  MySQL 存储引擎简介

MySQL 提供了多种不同的存储引擎，包括处理事务安全表的引擎和处理非事务安全表的引擎。在 MySQL  中不需要在整个服务器中使用同一种存储引擎，可以针对具体的要求对每一个表使用不同的存储引擎。MySQL 支持的存储引擎有InnoDB、MyISAM、Memory、Merge、Archive、Federated、CSV、BLACKHOLE 等。用户可以使用 SHOW ENGINES 语句查看系统支持的引擎类型，结果如下。

![](img/图2-8 MySQL存储引擎.png)

<div align = "center" style="font-weight:bold">图2-8 MySQL存储引擎</div>
Support 列的值表示某种引擎是否能使用，其中 `YES` 表示可以使用， `NO` 表示不能使用， `DEFAULT` 表示该引擎为当前默认存储引擎。

### 2.4.2 InnoDB 存储引擎

 InnoDB 是事务型数据库的首选引擎，支持事务安全表（ACID），支持行锁定和外键。MySQL的默认存储引擎为InnoDB，InnoDB的主要特点如下：

（1）InnoDB 给 MySQL提供了具有提交、回滚和崩溃恢复能力的事务安全（ACID兼容）存储引擎。InnoDB 锁定在行级，并且也用SELECT 语句提供一个类似Oracle的非锁定读。这些功能增加了多用户部署和性能。在SQL 查询中可以自由地将InnoDB类型的表与MySQL其他类型的表混合起来，甚至在同一个查询中也可以混合。

（2）InnoDB 是为处理巨大数据量时的最大性能设计，它的CPU效率可能是任何其他基于磁盘的关系数据库引擎所不能匹敌的。

（3）InnoDB存储引擎被完全与MySQL服务器整合，InnoDB存储引擎为在主内存中缓存数据和索引而维持它自己的缓冲池。InnoDB的表和索引在一个逻辑表空间中，表空间可以包含数个文件（或原始磁盘分区）。这与MyISAM 表不同，比如在MyISAM表中每个表被保存在分离的文件中。InnoDB表可以是任何尺寸，即使在文件尺寸被限制为2GB的操作系统上。

（4）InnoDB 支持外键完整性约束（FOREIGN KEY）。在存储表中的数据时每张表的存储都按主键顺序存放，如果没有显式地在定义表时指定主键，InnoDB会为每一行生成一个6字节的ROWID，并以此作为主键。

（5）InnoDB 被用来在众多需要高性能的大型数据库站点上。InnoDB不创建目录，在使用InnoDB时MySQL 将在MySQL数据目录下创建一个名为ibdatal的10MB大小的自动扩展数据文件，以及两个名为ib＿logfile0和ib＿logfile1的5MB大小的日志文件。

### 2.4.3 MyISAM 存储引擎

 MyISAM基于ISAM存储引擎，并对其进行扩展。它是在Web、数据仓储和其他应用环境下最常使用的存储引擎之一。MyISAM拥有较高的插入、查询速度，但不支持事务。MyISAM的主要特点如下：

（1）大文件（达63位文件长度）在支持大文件的文件系统和操作系统上被支持。

（2）当把删除、更新及插入混合的时候，动态尺寸的行的碎片更少，这要通过合并相邻被删除的块来完成，若下一个块被删除，则扩展到下一块自动完成。

（3）每个MyISAM表的最大索引数是64，这可以通过重新编译来改变。每个索引最大的列数是16个。

（4）最大的键长度是1000字节，这也可以通过编译来改变。对于键长度超过250字节的情况，使用一个超过1024字节的键块。

（5）BLOB和TEXT列可以被索引。

（6）NULL值被允许在索引的列中，其占每个键的0～1字节。

 （7）所有数字键值先以高字节位被存储，以允许一个更高的索引压缩。

（8）对于每个表的AUTO＿INCREMENT列，MyISAM通过 INSERT 和UPDATE操作自动更新这一列，这使得AUTO＿INCREMENT列更快（至少10％）。注意，在序列项的值被删除之后就不能再利用。

（9）可以把数据文件和索引文件放在不同目录。

（10）每个字符列可以有不同的字符集。

（11）有VARCHAR的表可以有固定或动态记录长度。

（12）VARCHAR和CHAR列可以多达64KB。

使用MyISAM引擎创建数据库将产生3个文件，文件的名字以表的名字开始，扩展名指出文件类型。frm文件存储表定义，数据文件的扩展名为.myd （MYData），索引文件的扩展名为.myi （MYIndex）。

### 2.4.4 MEMORY 存储引擎

 MEMORY 存储引擎将表中的数据存储在内存中，为查询和引用其他表中的数据提供快速访问。MEMORY的主要特点如下：

（1）MEMORY表可以有多达每个表32个索引，每个索引16列，以及500字节的最大键长度。

（2）MEMORY 存储引擎执行HASH和BTREE索引。

（3）可以在一个 MEMORY 表中有非唯一键。

（4）MEMORY表使用一个固定的记录长度格式。

（5）MEMORY不支持BLOB或TEXT列。

（6）MEMORY 支持AUTO＿INCREMENT列和对可包含NULL值的列的索引。

（7）MEMORY表在所有客户端之间共享（就像其他任何非TEMPORARY表）。

（8）MEMORY 表内容被存在内存中，内存是MEMORY 表和服务器在查询处理的空闲中创建的内部表共享。

（9）当不再需要MEMORY表的内容时要释放被MEMORY表使用的内存，应该执行DELETE FROM或TRUNCATE TABLE，或者整个地删除表（使用DROP TABLE）。

### 2.4.5 存储引擎的选择

不同的村粗引擎有不同的特点，适用于不同的需求，为了做出正确的选择，用户首先需要考虑每个存储引擎提供了哪些不同的功能，如表 2-1 所示。

<div align = "center" style="font-weight:bold">表2-1 存储引擎的比较</div>
|     功能     | MyISAM | MEMORY | InnoDB | Archive |
| :----------: | :----: | :----: | :----: | :-----: |
|   存储限制   | 256TB  |  RAM   |  64TB  |  None   |
|   支持事务   |   NO   |   NO   |  YES   |   NO    |
| 支持全文索引 |  YES   |   NO   |   NO   |   NO    |
|  支持数索引  |  YES   |  YES   |  YES   |   NO    |
| 支持哈希索引 |   NO   |  YES   |   NO   |   NO    |
| 支持数据缓存 |   NO   |  N/A   |  YES   |   NO    |
|   支持外键   |   NO   |   NO   |  YES   |   NO    |

 如果要提供提交、回滚和崩溃恢复能力的事务安全（ACID兼容）能力，并要求实现并发控制，InnoDB是个很好的选择。如果数据表主要用来插入和查询记录，则MyISAM引擎能提供较高的处理效率；如果只是临时存放数据，数据量不大，并且不需要较高的数据安全性，可以选择将数据保存在内存中的 Memory引擎，在MySQL中使用该引擎作为临时表存放查询的中间结果。如果只有INSERT和SELECT操作，可以选择 Archive 引擎，Archive引擎支持高并发的插入操作，但是本身并不是事务安全的。Archive 引擎非常适合存储归档数据，例如记录日志信息可以使用Archive引擎。

具体使用哪一种引擎要根据需要灵活选择，一个数据库中的多个表可以使用不同引擎以满足各种性能和实际需求，使用合适的存储引擎将会提高整个数据库的性能。