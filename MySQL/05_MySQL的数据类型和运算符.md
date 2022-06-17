# 第五章 MySQL的数据类型和运算符

## 5.1.MySQL的数据类型

> MySQL支持多种数据类型，主要有数值类型、时间/日期类型、字符串类型和二进制类型。

### 5.1.1.常见的数据类型

在MySQL中常见的数据类型如下。

（1）整数类型：包括 TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT，浮点数类型 FLOAT和DOUBLE,定点数类型DECIMAL。

（2)日期／时间类型：包括YEAR、TIME、DATE、DATETIME和TIMESTAMP。

（3)字符串类型：包括 CHAR、VARCHAR、BINARY、VARBINARY、BLOB、TEXT、ENUM 和 SET等。

（4)二进制类型：包括 BIT、BINARY、VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。
下面详细介绍这些数据类型的使用方法。

### 5.1.2.整数类

 MySQL 提供了多种数值数据类型，不同的数据类型取值范围不同，可以存储的值的范围越大，其所需要的存储空间也就越大，因此用户要根据实际需求选择适合的数据类型。MySQL供的整数类型主要有TINYINT、SMALLINT、MEDIUMINT、INT(INTEGER)、BIGINT.整数类型的字段属性可以添加AUTO_INCREMENT自增约束条件。表5-1列出了MySQL中整数型数据类型的说明。

<div align = "center" style="font-weight:bold">表5-1 MySQL中的整数型数据类型

| 类型名称     | 说 明          | 存储需求 |
| ------------ | -------------- | -------- |
| TINYINT      | 很小的整数     | 1个字节  |
| SMALLINT     | 小的整数       | 2个字节  |
| MEDIUMINT    | 中等大小的整数 | 3个字节  |
| INT(INTEGER) | 普通大小的整数 | 4个字节  |
| BIGINT       | 大整数         | 8个字节  |

 该表中显示，不同类型的整数存储时所需的字节不同，占用字节最少的是TINYINT类型，占用字节最多的是BIGINT,相应的占用字节多的类型所能存储的数字范围也就大。用户可以根据占用的字节数计算出每一种整数类型的取值范围，例如TINYINT的存储需求是一个字节（8 bits),那么TINYINT无符号数的最大值为28-1,即255;TINYINT有符号数的最大值为25-1,即127.用这种计算方式可以计算出其他整数类型的取值范围，如表5-2所示。

<div align = "center" style="font-weight:bold">表5-2 整数类型的取值范围

| 数据类型     | 有符号数的取值范围                       | 无符号数的取值范围     |
| ------------ | ---------------------------------------- | ---------------------- |
| TINYINT      | -128~127                                 | 0~255                  |
| SMALLINT     | -32768~32767                             | 0~65535                |
| MEDIUMINT    | -8388608~8388607                         | 0~16777215             |
| INT(INTEGER) | -2147483648~2147483647                   | 0~4294967295           |
| BIGINT       | -9223372036854775808~9223372036854775807 | 0~18446744073709551615 |

MSQL 支持选择在该类型关键字后面的括号内指定整数值的显示宽度（例如INT(4))。INT(M)中的M指示最大显示宽度，最大有效显示宽度是 4,需要注意的是显示宽度与存储大小或类型包含的值的范围无关。该可选显示宽度规定用于显示宽度小于指定的字段宽度的值时从左侧填满宽度。显示宽度只是指明MySQL最大可能显示的数值个数，数值个数如果小于指定的宽度，显示会由空格填充；如果插入了大于显示宽度的值，只要该值不超过该类型整数的取值范围，数据依然可以插入，而且显示无误。

例如，假设声明一个INT类型的字段：

```mysql
id INT(4)
```

该声明指出，在id字段中的数据一般只显示4位数字的宽度。假如向id字段中插入数值10000,当使用SELECT语句查询该列值的时候，MySQL显示的是完整的带有5位数字的10000,而不是4位数字。

其他整型数据类型也可以在定义表结构时指定所需要的显示宽度，如果不指定，则系统为每一种类型指定默认的宽度值，如例5-1所示。

【例5-1】创建一个 `tem1` 表，其中字段a、b、c、d、e的数据类型分别为TINYINT、SMALLINT、EDIUMINT、INT、BIGINT。

SQL 语句如下：

```mysql
CREATE TABLE tem1 (a TINYINT, b SMALLINT, C MEDIUMINT, d INT, e BIGINT);
```

执行成功后使用DESC查看表结构，显示如图5-1所示。

<img src="img/图5-1 查看表结构.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-1 查看表结构

 由 MySQL 执行结果可以看出，虽然在定义数据表的时候未指明各数据类型的显示宽度，但是系统给每种数据类型添加了不同的默认显示宽度，这些显示宽度能够保证显示每一种数据类型的取值范围内所有的值。例如TINYINT有符号数和无符号数的取值范围分别是－128~127和0~255,由于符号占用一个数字位，所以TINYINT默认的显示宽度是4.同理，其他整数类型的默认显示宽度与其有符号数的最小值的宽度相同。

不同的整数类型的取值范围不同，所需的存储空间也不同，因此在定义数据表的时候要根据实际需求选择最合适的类型，这样做有利于节约存储空间，还有利于提高查询效率。

现实生活中的很多情况需要存储带有小数部分的数值，下一节将介绍 MySQL 支持的能保存小数的数据类型。 

### 5.1.3.浮点数类型和定点数类型

 在MySQL 中使用浮点数和定点数表示小数。浮点数类型有两种，即单精度浮点数类型（FLOAT)和双精度浮点数类型（DOUBLE);定点数类型只有一种，即DECIMAL.浮点数类型和定点数类型都可以用（M,D)来表示，其中M称为精度，表示总共的位数；D称为标度，表示小数的位数。浮点数类型的取值范围为M（1~255)和D(1~30,且不能大于M-2),分别表示显示宽度和小数位数。M和D在FLOAT和DOUBLE 中是可选的，FLOAT和DOUBLE类型将被保存为硬件所支持的最大精度。DECIMAL的默认D值为0、M值为10。

表5-3列出了MySQL中的小数类型和存储需求。

<div align = "center" style="font-weight:bold">表5-3 MySQL中的小数类型 

| 类型名称          | 说 明              | 存储需求  |
| ----------------- | ------------------ | --------- |
| FLOAT             | 单精度浮点数       | 4个字节   |
| DOUBLE            | 双精度浮点数       | 8个字节   |
| DECIMAL(M,D)、DEC | 压缩的“严格”定点数 | M+2个字节 |

 DECIMAL 类型不同于FLOAT和DOUBLE,它实际上是以字符串存储的。DECIMAL 可能的最大取值范围与DOUBLE一样，但是其有效的取值范围由M和D的值决定。如果改变M而固定D,则其取值范围将随M的变天而变大；如果固定M而改变D,则其取值范围将随D的变大而变小（但精度增加）。由表5-3可以看出，DECIMAL的存储空间并不是固定的，而是由其精度值M决定，占用M+2个字节。

FLOAT类型的取值范围如下。

有符号的取值范围：－3.402823466E+38~-1.175494351E-38

无符号的取值范围：0和1.175494351E-38~3.402823466E+38

DOUBLE类型的取值范围如下。

有符号的取值范围：－1.7976931348623157E+308~-2.2250738585072014E-308

无符号的取值范围：0和2.2250738585072014E-308~1.7976931348623157E+308

【例5-2】创建tem2表，其中字段a、b、c的数据类型分别为FLOAT(5,1)、DOUBLE(5,1)和DECIMAL(5,1),并向表中插入数据7.26、7.56和7.125。

SQL 语句如下：

```mysql
create table tem2 (a FLOAT(5,1),b DOUBLE(5,1),C DECIMAL(5,1));
```

向表中插入数据：

```mysq
INSERT INTO tem2 values(7.26,7.56,7.125);
```

结果显示如图5-2所示。

从MySQL的执行结果可以看到在插入数据时出现了一个警告信息，使用“show warnings;”语句查看警告信息，结果如图5-3所示。 

<img src="img/图5-2 添加数据.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-2 添加数据

<img src="img/图5-3 警告信息.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-3 警告信息

 警告信息中显示字段c,也就是定义的定点数据类型DECIMAL(5,1)在插入7.125的时候被警告字段被截断，而字段a和字段b在插入7.26和7.56时未给出警告。

执行SELECT语句查看tem2表中的内容： 

```mysql
 SELECT * FROM tem2;
```

查询结果如图5-4所示。

<img src="img/图5-4 查看数据插入结果.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-4 查看数据插入结果

由执行结果看出，虽然字段a和字段b的FLOAT和DOUBLE数据类型在插入超过其精度范围的小数时MySQL系统未给出警告，但是对插入的数据做了四舍五入的处理。同样，DECIMAL类型在对超出其精度范围的插入值做出四舍五入处理的同时，系统还会给出截断插入值的警告。

### 5.1.4.日期/时间类型

在MySQL 中有多种表示日期与时间的数据类型，主要有DATETIME、DATE、TIMESTAMP、TIME和YEAR.例如，当只需要记录年份信息时可以只用YEAR类型，而没有必要使用DATE.每一个类型都有合法的取值范围，当插入不合法的值时系统会将“零”值插入到字段中。

表5-4列出了MySQL中的日期／时间类型。

<div align = "center" style="font-weight:bold">表5-4 日期／时间类型 

| 类型名称  | 日期格式            | 日期范围                                 | 存储需求 |
| --------- | ------------------- | ---------------------------------------- | -------- |
| YEAR      | YYYY                | 1901~2155                                | 1个字节  |
| TIME      | HH:MM:SS            | -838:59:59~838:59:59                     | 3个字节  |
| DATE      | YYYY-MM-DD          | 1000-01-01~9999-12-31                    | 3个字节  |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 1000-01-01 00:00:00~9999-12-31 23:59:59  | 8个字节  |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:001~2038-01-19 03:14:07 | 4个字节  |

1. YEAR

（1)YEAR 类型使用单字节表示年份，在存储时只需要一个字节，用户可以使用不同格式指定YEAR的值。

（2)以4位字符串或者4位数字格式表示 YEAR,其范围为＇1901'~'2155',输入格式为YYYY或YYYY.例如输入‘2015’或2015,插入到数据库的值都是2015。

（3)以两位字符串格式表示YEAR,范围为＇00'~'99'.'00'~'69'和＇70'~'99'范围的值分别被转换为2000~2069和1970~1999范围的YEAR值。输入＇0'与＇00'取值相同，皆为2000,插入超过取值范围的值将被转换为2000。

（4)以两位数字表示的YEAR,范围为1~99.1~69和70~99范围的值分别被转换为2001~2069和1970~1999范围的YEAR值。注意，0值被转换为0000,而不是2000。

【例5-3】创建tem3表，定义数据类型为YEAR的字段a,向表中插入值2018、2018、2226。

首先创建tem3表：

```
CREATE TABLE tem3(a YEAR);
```

然后向表中插入数据：

```
INSERT INTO tem3 VALUES(2018),('2018');
```

执行结果如图5-5所示。

<img src="img/图5-5.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-5 创建表并插入数据

再次向表中插入数据：

```mysql
INSERT INTO tem3 VALUES('2226');
```

执行结果如图5-6所示。

<img src="img/图5-6.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-6 再次插入数据

MySQL给出一条错误提示，字段a中插入的第3个值2226超出了YEAR类型的取值范围，此时不能正确地执行插入操作，使用SELECT语句查看结果：

```mysql
SELECT *FROM tem3;
```

执行结果如图5-7所示。

<img src="img/图5-7.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-7 超范围数据不被插入

由上述结果可以看出，插入值无论是数值型还是字符串型，都可以被正确地存储到数据库中；但是假
如插入值超过了YEAR类型的取值范围，则插入失败。

【例5-4】向tem3表的字段a中插入用一位和两位字符串表示的YEAR值，分别为＇0'、＇00'、88和25。

为了方便观察结果，可先清空tem3表中原有的数据：

```mysql
DELETE FROM tem3;
```

然后插入测试数据并查看数据：

```mysql
INSERT INTO tem3 values('0'),('00'),('88'),('25');
```

结果如图5-8所示。

<img src="img/图5-8.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-8 插入规范数据

使用SELECT语句查询tem3表的数据：

```mysql
SELECT *FROM tem3;
```

结果如图5-9所示。

<img src="img/图5-9.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-9 查看新添加数据

从执行结果可以看出，字符串＇0'和＇00'的作用相同，都被转换成了2000年，字符串＇88被转换成了1988年，字符串＇25'被转换为2025年。

【例5-5】向tem3表的字段a中插入用一位和两位数字表示的YEAR值，分别为0、00、66和33。

为了方便观察结果，可先清空tem3表中原有的数据：

```mysql
DELETE FROM tem3;
```

然后插入测试数据并查看数据：

```mysq
INSERT INTO tem3 VALUES(0),(00),(66),(33);
```

执行结果如图5-10所示。

<img src="img/图5-10.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-10 插入年份类型数据

使用SELECT语句查询tem3表的内容，结果如图5-11所示。

<img src="img/图5-11.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-11 新数据插入结果

从执行结果可以看出，数字0和00的作用相同，都被转换成了0000年，数字66被转换成了2066年，数字33被转换成了2033年。

2. TIME

TIME 类型用于只需记录时间信息的值，需要3个字节存储，格式为＇HH:MM:SS',其中HH表示小时，MM表示分钟，SS表示秒。TIME类型的取值范围为－838:59:59~838:59:59,小时部分会取这么大的值是因为TIME类型不仅可以用来表示一天的时间（必须小于24小时），还可能是某个事件过去发生的时间或者两个事件之间的时间间隔（可以大于24小时，甚至可以为负值）。用户可以使用多种格式指定TIME的值。

（1)'D HH:MM:SS'格式的字符串，也可以使用＇HH:MM:SS'、＇HH:MM'、＇D HH:MM'、＇D HH'或＇SS'
等“非严格”的语法，其中D表示日，取值范围是0~34.在插入数据库时，D被转换为小时保存，格式为'D * 24+HH'。

（2)'HHMMSS'格式的没有分隔符的字符串或HHMMSS 格式的数值，假定是有意义的时间。例如，'105508'被理解为＇10:55:08',但＇106508'是不合法的，因为其分钟部分超出合理范围，在存储时将变为00:00:00.

【例5-6】创建tem4表，然后向字段a中插入时间值，分别为＇12:50:06'、＇12:58'、＇315:25'、＇608'、＇28'.

SQL 语句如下：

```mysql
CREATE TABLE tem4(a TIME);
INSERT INTO tem4 values('12:50:06'),('12:58'),('3 15:25'),('6 08'),('28');
```

执行结果如图5-12所示。

<img src="img/图5-12.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-12 添加时间类型数据

使用SELECT语句查询tem4表的内容，结果如图5-13所示。

从上述结果可以看出，字符串值12:50:06被转换为 12:50:06;字符串＇12:58后面补秒数 00被转换为12:58:00;字符串＇3 10:55经过计算（3x24+15=87HH)最终被转换为87:25:00;字符串＇608经过计算（6x24+08=152HH)最终被转换为152:00:00;字符串＇28'被转换为00:00:28。

<img src="img/图5-13.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-13 添加时间类型数据结果

【例5-7】向tem4表中插入值＇122509'、101059、＇0'、135966。

为了方便观察结果，可先清空tem4表中原有的数据：

```mysql
DELETE FROM tem4;
```

插入前3项测试数据：

```mysql
INSERT INTO tem4 values ('122509'),(101059),('0');
```

执行结果如图5-14所示。

<img src="img/图5-14.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-14 插入新数据

接着插入第4项测试数据：

```mysql
INSERT INTO tem4 values(135966);
```

执行结果如图5-15所示。

<img src="img/图5-15.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-15 数据超范围提示

执行结果显示，当插入的TIME类型数据超过规定范围时系统将报错，因为数值135966中的秒钟超过了59,数据不能插入。

查看tem4表中的结果，如图5-16所示。

```mysql
SELECT * FROM TEM4;
```

从执行结果可以看出，字符串＇122509'被转换为 12:25:09;数值 101059 被转换为 10:10:59;字符串＇0'
被转换为00:00:00;数值135966因为秒钟超过取值范围，没有被插入到表中。

<img src="img/图5-16.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-16 数据插入结果

 【例5-8】向tem4表中插入系统当前时间。

为了方便观察结果，可先清空tem4表中原有的数据：

```mysql
DELETE FROM tem4;
```

然后插入测试数据：

```mysql
INSERT INTO tem4 values (CURRENT_TIME),(NOW());
```

查看tem4表中的数据：

```mysql
SELECT * FROM tem4;
```

执行结果如图5-17所示。

<img src="img/图5-17.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-17 插入系统时间数据

从执行结果可以看出，获取当前系统时间函数CURRENT TIME和NOWO将当前系统时间插入到字段a中。

注意：因为输入INSERT语句的时间不同，获取到的结果可能不同。

3. DATE

DATE类型用在仅需要存储日期值的时候，不存储时间，存储需要3个字节，格式为＇YYYY-MM-DD',其中YYYY表示年，MM表示月，DD表示日。在给DATE类型的字段赋值时可以使用字符串类型或者数值类型的数据，只要符合DATE的日期格式即可。

常用的DATE格式如下：

- 以＇YYYY-MM-DD'或者＇YYYYMMDD'字符串格式表示日期，取值范围为＇1000-01-01'~9999-12-31'。例如输入＇2018-12-31'或者＇20181231',插入数据库中的日期都是2018-12-31。
- 以＇YY-MM-DD'或者＇YYMMDD'字符串格式表示日期，YY表示两位的年份值，但两位的年份值不能清楚地表示具体的年份，因为不知道在哪个世纪。MySQL使用以下规则解释两位的年份值：00~69'范围的年份转换为＇2000~2069',70~99'范围的年份值转换为＇1970~1999.例如输入＇18-12-31',插入数据库的日期是2018-12-31;输入＇98-12-31',插入数据库的日期是1998-12-31。
- 以YYMMDD数值格式表示日期，00~69范围的年份值转换为2000~2069;70~99范围的年份值转换为1970~1999.例如输入181231,插入数据库的日期是2018-12-31;输入981231,插入数据库的日期是1998-12-31。

使用CURRENT_DATE或者NOWO插入当前计算机系统的日期。

【例5-9】创建tem5表，定义字段a为DATE数据类型，向表中插入用4位字符表示年份的数据，例如字符串格式的数据1968-12-25'、19681225'和20180828'。

首先创建tem5表：

```mysql
CREATE TABLE tem5(a DATE);
```

向表中插入数据并查看插入结果：

```mysql
INSERT INTO tem5 values('1968-12-25'),('19681225'),('20180828');
```

使用SELECT语句查看tem5表的数据，执行结果如图5-18所示。

<img src="img/图5-18.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-18 tem5表的数据

由执行结果可以看出，不同格式的字符串型的日期数据被正确地插入到数据表中。

【例5-10】向tem5表中插入用两位字符表示年份的字符串数据，例如＇56-10-15'、＇561015'、＇001015'、251015'。

为了方便观察结果，可先清空tem5表中原有的数据：

```mysql
DELETE FROM tem5;
```

然后插入测试数据：

```mysql
INSERT INTO tem5 values('56-10-15'),('561015'),('001015'),('251015');
```

使用SELECT语句查看tem5表的数据，执行结果如图5-19所示。

<img src="img/图5-19.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-19 插入字符串数据
【例5-11】向tem5表中插入用两位数字表示年份的日期值，例如961225、001225、181225。为了方便观察结果，可先清空tem5表中原有的数据：

```mysql
DELETE FROM tem5;
```

然后插入测试数据：

```mysql
INSERT INTO tem5 values (961225),(001225),(181225);
```

使用SELECT语句查看tem5表的数据，执行结果如图5-20所示。

<img src="img/图5-20.png"  style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-20  插入测试数据

注意：如果把96-12-31作为数值类型插入到字段a，系统会提示错误，如图5-21所示。

<img src="img/图5-21.png" alt="img" style="zoom:150%;" />

<div align = "center" style="font-weight:bold">图5-21  系统提示信息

【例5-12】向tem5表中插入系统当前时间。

为了方便观察结果，可先清空tem5表中原有的数据：

```mysql
DELETE FROM tem5;
```

然后插入测试数据：

```mysql
INSERT INTO tem5 values (CURRENT_DATE);
```

SELECT语句查看tem5表的数据，执行结果如图5-22所示。

<img src="img/图5-22.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-22  获取系统当前时间（1）

在插入当前时间时也可以使用NOWO函数：

```mysql
INSERT INTO tem5 values(NOW());
```

使用SELECT语句查看tem5表的数据，执行结果如图5-23所示。

4.DATETIME

DATETIME类型同时包含日期和时间信息，存储需要8个字节，格式为＇YYYY-MM-DD HH：MM：SS＇，其中YYYY表示年，MM表示月，DD表示日，HH表示小时，MM表示分钟，SS表示秒。在给DATETIME类型的字段赋值时可以使用字符串类型或者数值类型的数据，只需符合DATETIME的日期格式即可。

<img src="img/图5-23.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-23 获取系统当前时间（2）

常用的DATETIME格式如下：

以＇YYYY-MM-DD HH：MM：SS＇或者＇YYYYMMDDHHMMSS＇字符串格式表示日期时间，取值范围为1000-01-01 00：00：00＇～9999-12-31 23：59：59＇。例如输入＇2014-12-31 11：28：30或者＇20141231112830＇， 插入数据库的DATETIME值都是2014-12-31 11：28：30。 

以＇YY-MM-DD HH：MM：SS＇或者＇YYMMDDHHMMSS＇字符串格式表示日期时间，在这里YY表示年份值。和DATE类型一样，＇00～69范围的年份值转换为＇2000～2069＇，＇70～99＇范围的年份值转换为＇1970～1999。例如输入＇14-12-31 11：28：30＇，插入数据库的DATETIME类型值为2014-12-31 11：28：30；输入＇951231112830＇，插入数据库的DATETIME 类型值为 1995-12-31 11：28：30。 

以 YYYYMMDDHHMMSS 或者 YYMMDDHHMMSS 数值格式表示日期时间，例如输入20141231112830，插入数据库的DATETIME类型值为2014-12-31 11：28：30；输入951231112830， 插入数据库的DATETIME 类型值为 1995-12-31 11：28：30。 

【例5-13】创建tem6表，定义字段a的数据类型为DATETIME，向表中插入用4位字符表示年份的字符串数据，例如＇1988-08-08 08：08：08＇、＇19880808080808＇和＇20180606060606＇。 

首先创建tem6表：

```MySQL
CREATE TABLE tem6(a DATETIME);
```

向表中插入数据并查看插入结果：

```MySQL
INSERT INTO tem6 values ('1988-08-08 08:08:08'), ('19880808080808'), ('20180606060606');
```

<img src="img/图5-24.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-24 插入年月日及时间数据（1）

使用SELECT语句查看tem6表的数据，执行结果如图5-25所示。

由执行结果可以看出，不同格式的字符串型的日期时间数据被正确地插入到数据表中。

【例5-14】向tem6表中插入用两位字符表示年份的字符串数据，例如＇88-08-08 08：08：08＇、880808080808＇、'180606060606'。 

为了方便观察结果，可先清空tem6表中原有的数据：

```MySQL
DELETE FROM tem6;
```

然后插入测试数据：

```MySQL
INSERT INTO tem6 values ('88-08-08 08:08:08'),('880808080808'),('180606060606'); 
```

使用SELECT语句查看tem6表的数据，执行结果如图5-26所示。

<img src="img/图5-25.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-25 插入显示结果

<img src="img/图5-26.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-26 插入年月日及时间数据（2）

【例5-15】向tem6表中插入数值类型的日期时间值，例如19881111080808、180606060606。

为了方便观察结果，可先清空tem6表中原有的数据：

```MySQL
DELETE FROM tem6;
```

然后插入测试数据：

```MySQL
INSERT INTO tem6 values (19881111080808),(180606060606);
```

使用SELECT语句查看tem6表的数据，执行结果如图5-27所示。

【例5-16】向tem6表中插入系统当前日期和时间。

为了方便观察结果，可先清空tem6表中原有的数据：

```MySQL
DELETE FROM tem6;
```

然后插入测试数据：

```MySQL
INSERT INTO tem6 values (CURRENT_TIMESTAMP);
```

用SELECT语句查看tem6表的数据，执行结果如图5-28所示。

<img src="img/图5-27.png" alt="img" style="zoom: 150%;" />

图5-27 插入显示结果

<img src="img/图5-28.png" alt="img" style="zoom: 150%;" />

图5-28  数据显示结果

在插入当前时间时不可以使用NOWO函数，例如执行以下语句：

```MySQL
INSERT INTO tem6 values(NOW());
```

使用SELECT语句查看tem6表的数据，执行结果如图5-29所示。

从执行结果可以看出，获取当前系统日期时间插入到字段a中。

> **注意：MySQL 允许用“不严格”的语法插入日期，任何标点符号都可以用作日期部分或时间部分的间隔符。例如＇95-12-13 11：28：30＇、95.12.31 11＋28＋30＇、95／12／31 11＊28＊30＇和＇95＠12＠31 11＾28＾30＇等表示方法是等价的，这些值都可以正确地插入到数据库中。**

<img src="img/图5-29.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-29 获取当前日期时间数据

TIMESTAMP的显示格式与 DATETIME 相同，显示宽度固定在19个字符，格式为 YYYY-MM-DDHH：MM：SS，存储需要4个字节。但是 TIMESTAMP 列的取值范围小于DATETIME 的取值范围，为＇1970-01-01 00：00：01＇UTC～＇2038-01-19 03：14：07＇UTC（UTC是Coordinated Universal Time的缩写，指的是 世界标准时间），因此在插入数据时要保证在合法的取值范围内。

【例5-17】创建tem7表，定义字段a的数据类型为TIMESTAMP，向表中插入各种形式的日期时间数据，例如＇1988-08-08 08：08：08＇、＇19880808080808＇和＇180606060606＇，以及＇88＠12＠31 11* 11*11＇、180616121212 NOWO。 

首先创建表 tem7：

```MySQL
CREATE TABLE tem7(a TIMESTAMP);
```

向表中插入测试数据并查看插入结果：

```MySQL
INSERT INTO tem7 values('1988-08-08 08:08:08'),( '19880808080808'),( '180606060606'); INSERT INTO tem7 VALUES('88@12@31 11*11*11'),(180616121212),(NOW()); 
```

使用SELECT语句查看tem7表的数据，执行结果如图5-30所示。

<img src="img/图5-30.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-30 通过不同方式添加日期时间数据

由执行结果可以看出，不同格式的日期时间数据都被正确地插入到数据表中。

【例5-18】向tem7表中插入当年日期时间，查看插入值，并更改时区为东6区，再次查看插入值。为了方便观察结果，可先清空tem7表中原有的数据：

```MySQL
DELETE FROM tem7;
```

然后插入测试数据为当前时区的系统时间（时区是东8区）：

```MySQL
INSERT INTO tem7 values (NOW());
```

使用SELECT语句查看tem7表的数据，执行结果如图5-31所示。

接着修改当前时区为东6区，SQL语句如下：

```MySQL
set time_zone='+6:00';
```

然后再次使用SELECT语句查看插入时的日期时间值，执行结果如图5-32所示。

<img src="img/图5-31.png" alt="img" style="zoom: 150%;" />

图5-31 当前数据信息

<img src="img/图5-32.png" alt="img" style="zoom: 150%;" />

图5-32 修改数据时区信息

由上述结果可以看出，因为东6区比东8区慢两个小时，所以查询结果在经过时区转换之后值减少了两个小时。

### 5.1.5.字符串类型

字符串类型用于存储字符串数据，MySQL支持两种字符串数据，即文本字符串和二进制字符串。本小节所讲的是文本字符串类型。文本字符串可以进行区分或不区分大小写的串比较，也可以进行模式匹配查找。MySQL中的字符串类型指的是CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT、 ENUM和SET。表5-5列出了MySQL中的字符串数据类型。

<div align = "center" style="font-weight:bold">表5-5 MySQL中的字符串数据类型

| 类型名称   | 说  明                                      | 存储需求                                                    |
| ---------- | ------------------------------------------- | ----------------------------------------------------------- |
| CHAR(M)    | 固定长度非二进制字符串                      | M个字节，1≤M≤255                                            |
| VARCHAR(M) | 变长非二进制字符串                          | L＋1个字节，在此L≤M和1≤M≤255                                |
| TINYTEXT   | 非常小的非二进制字符串                      | L＋1个字节，在此L＜28                                       |
| TEXT       | 小的非二进制字符串                          | L＋2个字节，在此L＜216                                      |
| MEDIUMTEXT | 中等大小的非二进制字符串                    | L＋3个字节，在此L＜224                                      |
| LONGTEXT   | 大的非二进制字符串                          | L＋4个字节，在此L＜232                                      |
| ENUM       | 枚举类型，只能有一个枚举字符串值            | 1个或2个字节，取决于枚举值的数目（最大值为65535)            |
| SET        | 一个集合，字符串对象可以有零个或多个SET成员 | 1、2、3、4或8个字节，取决于集合成员的数量（最多为64个成员） |

VARCHAR和TEXT类型是变长类型，它们的存储需求取决于值的实际长度（表5-5中用L表示），而不是取决于类型的最大可能长度。例如，一个VARCHAR（10）字段能保存最大长度为10个字符的字符串，实际的存储需求是字符串的长度L加上1个字节。例如字符串＇teacher＇，L是7，而存储需求是8个字节。

1．CHAR和VARCHAR类型

CHAR（M）为固定长度字符串，在定义时指定字符串长度，当保存时在右侧填充空格以达到指定的长度。M表示字符串长度，M的取值范围是0～255。例如，CHAR（4）定义了一个固定长度的字符串字段，其包含的字符个数最大为4。当检索到CHAR的值时，尾部的空格将被删除掉。

VARCHAR（M）是长度可变的字符串，M表示最大的字段长度，M的取值范围是0～65535。VARCHAR的最大实际长度由最长字段的大小和使用的字符集确定，而其实际占用的空间为字符串的实际长度加1。例如，VARCHAR（40）定义了一个最大长度为40的字符串，如果插入的字符串只有20个字符，则实际存储的字符串为20个字符和一个字符串结束字符。VARCHAR在进行值保存和检索时尾部的空格仍保留。

【例5-19】将不同字符串存储到CHAR（5）和VARCHAR（5）数据类型的字段中，差别如表5-6所示。

<div align = "center" style="font-weight:bold">表5-6 CHAR（5）与VARCHAR（5）的存储差别

|  插入值   |   CHAR(5)    | 存储需求 | VARCHAR(5) | 存储需求 |
| :-------: | :----------: | :------: | :--------: | :------: |
|    ''     | '          ' | 5个字节  |     ''     | 1个字节  |
|   'ab'    |  'ab      '  | 5个字节  |    'ab'    | 3个字节  |
|  'abcd'   |   'abcd  '   | 5个字节  |   'abcd'   | 5个字节  |
|  'abcde'  |   'abcde'    | 5个字节  |  'abcde'   | 6个字节  |
| 'abcdefg' |   'abcde'    | 5个字节  |  'abcde'   | 6个字节  |

从表5-6可以看出，CHAR（5）定义了固定长度为5的字段，不管存入的字符串长度为多少，所占用的空间都是5个字节。VARCHAR（5）定义的字段所占的字节数为实际字符串长度加1。

【例5-20】创建tem8表，定义字段a为CHAR（5）、字段b为VARCHAR（5），向表中插入字符串数据＇ab＇。首先创建tem8表：

```MySQL
CREATE TABLE tem8(a CHAR(5),b VARCHAR(5));
```

向表中插入数据并查看插入结果：

```MySQL
INSERT INTO tem8 values('ab ','ab ');
```

使用SELECT语句查看tem8表的数据，执行结果如图5-33所示。

为了更明显地显示出字段a和b存储的字符串不同，使用MySQL的CONCAT函数返回带有连接参数“（”和“）”的结果，如图5-34所示。

SQL 语句如下：

```MySQL
SELECT concat('(',a,')'),concat('(',b,')') FROM tem8;
```

<img src="img/图5-33.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-33 定义字段并插入数据

<img src="img/图5-34.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-34 字符串对比结果

由结果可以看出，字段a的CHAR（5）在存储字符串＇ab  的时候将末尾的3个空格自动删除了，而字段b的VARCHAR（5）保留了空格。

2．TEXT 类型

- TEXT 字段保存非二进制字符串，例如文章内容、评论和留言等。当保存或查询TEXT字段的值时不删除尾部空格。TEXT类型分为4种，即TINYTEXT、TEXT、·TINYTEXT的最大长度为255（28-1）个字符。
- TEXT的最大长度为65535（216-1）个字符。
- MEDIUMTEXT的最大长度为16777215（224-1）字符。
- LONGTEXT的最大长度为4294967295或4GB（232-1）个字符。MEDIUMTEXT和LONGTEXT，不同的TEXT类型所需的存储空间和数据长度不同。

3．ENUM类型

ENUM是一个字符串对象，其值为创建表时在字段规定中枚举的一列值，语法格式如下：

```MySQL
字段名 ENUM ('值','值2',···'值n')
```

字段名指的是将要定义的字段名称，值n指的是枚举列表中的第n个值。ENUM类型的字段在取值时只能从指定的枚举列表中取，而且一次只能取一个值。如果所创建的成员中有空格，其尾部的空格将自动被删除。ENUM值在内部用整数表示，每个枚举值均有一个索引值，列表值所允许的成员值从1开始编号，MySQL 存储的就是这个索引编号。枚举最多可以有65535个元素。

例如定义ENUM类型的字段（'first'，'second'，＇third＇），该字段可以取的值和每个值的索引如表5-7所示。

<div align = "center" style="font-weight:bold">表5-7 ENUM类型的取值范围

|   值   | 索  引 |
| :----: | :----: |
|  NULL  |  NULL  |
|   ''   |   0    |
| first  |   1    |
| second |   2    |
| third  |   3    |

ENUM值依照索引顺序排列，并且空字符串排在非空字符串之前，NULL值排在其他所有枚举值之前。

【例5-21】创建tem9表，定义字段a为ENUM数据类型，字段a的枚举列表为（＇mingming＇，＇xiaohai，＇tianyi＇），查看字段a的成员并显示其索引值。

首先创建tem9表：

```MySQL
CREATE TABLE tem9 (a ENUM('mingming', 'xiaohai', 'tianyi'));
```

向表中插入数据并查看插入结果：

```mysql
INSERT INTO tem9 values(NULL),('xiaohai'),( 'mingming'),( 'tianyi');
```

使用a＋0查看表中字段a的值的索引值，执行结果如图5-35所示。

<img src="img/图5-35.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图5-35 查看索引值

由执行结果可以看出，字段a枚举列表中的索引值跟定义的时候一致。

如果插入的值不在ENUM类型的取值范围内，将会报错，例如以下插入数据的操作：

```mysql
INSERT INTO tem9 values('leilei');
```

执行结果如图5-36所示。

<img src="img/图5-36.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-36 报错信息

如果ENUM类型加上NOT NULL属性，其默认值为取值类别中的第一个元素；如果不加NOT NULL属性，其默认值为NULL，而且不能插入NULL值。

【例5-22】创建tem10表，定义字段a为ENUM类型，枚举列表值为（＇body＇，＇gile），向tem10表中插入测试数据。

首先创建tem10表：

```mysql
CREATE TABLE tem10 (a ENUM('body','gile')NOT NULL);
```

向表中插入数据并查看插入结果：

```MySQL
INSERT INTO tem10 values ('gile');
```

查看表中字段a的值，执行结果如图5-37所示。

<img src="img/图5-37.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-37 插入测试数据

向tem10表中插入NULL值，语句如下：

```MySQL
INSERT INTO tem10 values(NULL);
```

执行提示错误信息，如图5-38所示。

<img src="img/图5-38.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-38 报错信息

4．SET类型

SET类型是一个字符串对象，可以有零个或多个值。SET字段最多可以有64个成员，其值为创建表时规定的一列值。当指定包括多个SET成员的SET字段值时，各成员之间用逗号隔开。其语法格式如下：

```mysql
SET('值1', '值2',···'值n')
```

与ENUM类型相同，SET值在内部用整数表示，列表中的每一个值都有一个索引编号。在创建表时，SET成员值的尾部空格将自动被删除。注意，ENUM类型的字段只能从定义的字段值中选择一个值插入，而SET类型的字段可以从定义的列值中选择多个字符的联合。

如果插入SET字段中的值有重复，则MySQL自动删除重复的值；插入SET字段的值的顺序不重要，MySQL 会在存入数据库的时候按照定义的顺序显示；如果插入了不正确的值，在默认情况下MySQL将忽视这些值，并给出相应警告。

【例5-23】创建teml1表，定义字段a为SET类型，取值列表为（＇a＇，＇b＇，＇c），插入测试数据（＇a＇）、（＇a，b，c＇）、（＇b，a，c＇）、（＇a，b）以及（＇d，e＇）。

首先创建teml1表：

```MySQL
CREATE TABLE tem11 (a SET('a','b','c'));
```

向表中入数据并查看插入结果：

```MySQL
INSERT INTO tem11 values ('a'), ('a,b,c'), ('b,a,c'), ('a,b');
```

继续向表中插入数据：

```MySQL
INSERT INTO tem11 values ('d,e');
```

执行结果如图5-39所示。

<img src="img/图5-39.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-39  添加数据信息

由于插入的数据d和e不是SET集合中的值，系统提示错误。

使用SELECT语句查看tem11表的数据，执行结果如图5-40所示。

<img src="img/图5-40.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-40 数据添加效果

由结果可以看出，如果插入的值不按SET定义时的顺序排列，则会自动排序插入，例如＇b，a，c＇，最终结果为＇a，b，c＇；如果试图插入非SET集合中的值，系统会提示错误，禁止插入。

### 5.1.6.如何选择数据类型

MySQL 提供了大量的数据类型，为了优化存储，提高数据库的性能，在不同情况下应使用最精确的类型。在选择数据类型时，在可以表示该字段值的所有类型中应使用占用存储空间最少的数据类型，因为这样不仅可以减少存储（内存、磁盘）空间，从而节省1／0资源（检索相同数据情况下），还可以在进行数据计算的时候减轻CPU负载。

1．整数和浮点数

如果插入的数据不需要小数部分，则使用整数类型存储数据；如果需要小数部分，则使用浮点数类型。例如，如果字段的取值范围为1～50000，选择SMALLINT UNSIGNED是最好的；如果需要存储带有小数位的值，例如3.1415926，则需选择浮点数类型。

浮点数类型包括FLOAT和DOUBLE类型。DOUBLE类型的精度比FLOAT高，因此当需要的存储精度较高时选择DOUBLE类型。

2．浮点数和定点数

浮点型FLOAT和DOUBLE相对于定点型DECIMAL来说，在长度固定的情况下，浮点型能表示的数据范围更大。当一个字段被定义为浮点型后，如果插入数据的精度超过该列定义的实际精度，则插入值会被四舍五入到实际定义的精度值，然后插入，四舍五入的过程不会报错。由于浮点型容易四舍五入产生误差，因此当精度要求比较高时要使用定点型DECIMAL来存储。

定点数实际上是以字符串形式存放的，所以定点数可以更精确地保存数据。如果实际插入的数值精度大于实际定义的精度，则MySQL会进行警告（默认的SQLMode 下），但是数据按照实际精度四舍五入后插入；如果SQLMode是在TRADITIONAL（传统模式）下，则系统会直接报错，导致数据无法插入。

在数据迁移中，FLOAT（M，D）是非标准SQL定义，数据迁移可能出现问题，最好不要使用。另外，两个浮点型数据进行减法和比较运算时也容易出问题，因此在进行计算的时候一定要注意，如果要进行数值比较，最好使用定点型DECIMAL。

3．日期与时间类型

在MySQL 中选择日期类型的原则如下：

（1）根据实际需要选择能够满足应用的最小存储的日期类型。如果应用只需要记录“年份”，那么用1个字节来存储的YEAR类型完全能够满足，而不需要用4个字节来存储的DATE类型，这样不仅能节约存储，更能够提高表的操作效率。

（2）如果要记录年月日时分秒，并且记录的年份比较久远，那么最好使用 DATETIEM，而不要使用TIMESTAMP，因为TIMESTAMP表示的日期范围比DATETIME 短很多。

（3）如果记录的日期需要让不同时区的用户使用，那么最好使用TIMESTAMP，因为日期类型中只有它能够和实际时区相对应，而且当插入一条记录时如果没有指定 TIMESTAMP 这个字段值，MySQL 会把TIMESTAMP字段设为当前时间，因此当需要在插入记录的同时插入当前时间时使用TIMESTAMP比较方便。

4．CHAR和VARCHAR

CHAR和VARCHAR类型相似，都是用来存储字符串，但它们保存和检索的方式不同。CHAR属于固定长度的字符类型，而 VARCHAR 属于可变长度的字符类型。CHAR 会自动删除插入数据的尾部空格，VARCHAR不会删除尾部的空格。

由于CHAR是固定长度的，所以它的处理速度比VARCHAR快得多，但是其浪费存储空间，程序需要对行尾空格进行处理，所以对于那些长度变化不大且对查询速度有较高要求的数据可以考虑用CHAR类型来存储。

另外，在MySQL中不同的存储引擎对CHAR和VARCHAR的使用原则有所不同，概括如下。

- MyISAM存储引擎：建议使用固定长度的数据列代替可变长度的数据列。
- MEMORY 存储引擎：目前都使用固定长度的数据行存储，因此无论是使用 CHAR 还是使用VARCHAR列都没有关系，两者都是作为CHAR类型处理。
- InnoDB 存储引擎：建议使用VARCHAR类型。对于InnoDB数据表，内部的行存储格式没有区分固定长度和可变长度列（所有数据行都使用指向数据列值的头指针），因此在本质上使用固定长度的CHAR列不一定比使用可变长度的VARCHAR列性能要好，因此主要的性能因素是数据行使用的存储总量。由于CHAR平均占用的空间多于 VARCHAR，因此使用VARCHAR 来最小化需要处理的数据行的存储总量和磁盘I／O是比较好的。

5．ENUM和SET

ENUM只能取单值，它的数据列表是一个枚举集合，它的合法取值列表最多允许有65535个成员。当需要从多个值中选取一个时可以使用ENUM，例如性别字段适合定义为ENUM类型，每次只能从“男”和“女”中取一个值。

SET 可以取多个值，它的合法取值列表最多允许有64个成员。空字符串也是一个合法的SET值。当需要取多个值的时候适合使用SET类型，例如要存储一个人的特长，最好使用SET类型。

ENUM和SET的值是以字符串形式出现的，但在MySQL内部实际上是以数值索引的形式存储它们。

6．BLOB和TEXT

一般在保存少量字符串的时候可以选择CHAR或者VARCHAR，而在保存大文本时选择使用TEXT或者BLOB，二者之间的主要差别是BLOB能用来保存二进制数据，比如照片、音频信息等；而TEXT只能保存字符数据，比如一篇文章或者日记。

BLOB与TEXT存在以下常见问题：

（1）BLOB 和 TEXT 值会引起一些性能问题，特别是在执行了大量的删除操作时。删除操作会在数据表中留下很大的“空洞”，以后填入这些“空洞”的记录在插入的性能上会有影响。为了提高性能，建议定期使用OPTIMIZE TABLE功能对这类表进行碎片整理，避免因为“空洞”导致性能问题。

（2）使用合成的（Synthetic）索引来提高大文本字段（BLOB或TEXT）的查询性能。

（3）在不必要的时候避免检索大型的BLOB或TEXT值。

例如，SELECT＊查询就不是很好的想法，除非能够确定作为约束条件的WHERE子句只会找到所需要的数据行，否则很可能毫无目的的在网络上传输大量的值。

（4）把BLOB或TEXT列分离到单独的表中。

在某些环境中，如果把这些数据列移动到第二张数据表中，可以把原数据表中的数据列转换为固定长度的数据行格式，那么它就是有意义的。这会减少主表中的碎片，可以得到固定长度数据行的性能优势。它还可以使主数据表在运行SELECT＊查询的时候不会通过网络传输大量的BLOB或TEXT值。

## 5.2.MySQL常用的运算符

> **运算符连接表达式中的各个操作数，其作用是指明对操作数所进行的运算。常见的运算有数学运算、比较运算、位运算以及逻辑运算。通过运算符可以更加灵活地使用表中的数据，常见的运算符类型有算术运算符、比较运算符、逻辑运算符、位运算符。本节将介绍各种运算符的特点和使用方法。**

### 5.2.1.运算符概述

运算符是告诉MySQL执行特定算术或逻辑操作的符号。MySQL中的运算符很丰富，主要有四大类，即算术运算符、比较运算符、逻辑运算符和位运算符。

1．算术运算符

算术运算符用于各类数值运算，包括加（＋）、减（一）、乘（＊）、除（／）、求余（或称取模运算，％）。

2．比较运算符

比较运算符用于比较运算，包括大于（＞）、小于（＜）、等于（＝）、大于等于（＞＝）、小于等于（＜＝）、不等于（！＝），以及IN、BETWEEN AND、IS NULL、GREATEST、LEAST、LIKE、REGEXP等。 

3．逻辑运算符

逻辑运算符的求值所得结果为1（TRUE）或0（FALSE），这类运算符有逻辑非（NOT或者！）、逻辑与（AND或者＆＆）、逻辑或（OR或者）、逻辑异或（XOR）。

4．位运算符

参与位运算的操作数按二进制位进行运算，位运算符包括位与（＆）、位或（）、位非（～）、位异或（＾）、左移（＜＜）、右移（＞＞）6种。

接下来对MySQL中各种运算符的使用进行详细介绍。

### 5.2.2.算数运算符

算术运算符是SQL中最基本的运算符，MySQL中的算术运算符如表5-8所示。

<div align = "center" style="font-weight:bold">表5-8 MySQL中的算术运算符

| 运算符 |       作  用       |
| :----: | :----------------: |
|   +    |      加法运算      |
|   -    |      减法运算      |
|   *    |      乘法运算      |
|   /    |  除法运算，返回商  |
|   %    | 求余运算，返回余数 |

下面分别介绍不同算术运算符的使用方法。

【例5-24】创建teml2表，定义数据类型为INT的字段a，插入值66，对a值进行算术运算。

首先创建tem12表，输入语句如下：

```MySQL
CREATE TABLE tem12(a INT);
```

向字段a插入数据66：

```MySQL
INSERT INTO tem12 value(66);
```

接下来对a值进行加法和减法运算：

```MySQL
SELECT a,a+10,a-15+10,a+10-15,a+22.6 FROM tem12;
```

执行结果如图5-41所示。

由计算结果看到，可以对a字段的值进行加法和减法运算，而且由于“＋”和“-”的优先级相同，所以先加后减和先减后加之后的结果是相同的。

<img src="img/图5-41.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-41  使用加减运算符

【例5-25】对tem12表中的a进行乘法、除法运算。

SQL 语句如下：

```mysql
SELECT a,a*2,a/2,a/5,a%84 FROM tem12;
```

执行结果如图5-42所示。

<img src="img/图5-42.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-42  使用乘除运算符

由计算结果可以看到，在对a进行除法运算的时候由于66无法被5整除，所以MySQL将a／5的结果保存到小数点后面4位，结果为13.2000；66除以4的余数为2，因此取余运算a％4的结果为2。

在进行数学运算时除数为0无意义，因此除法运算中的除数不能为0，如果被0除，则返回结果NULL。

【例5-26】除数为0时运算的结果：用0除a。

SQL 语句如下：

```mysql
SELECT a,a/0,a%0 FROM tem12;
```

执行结果如图5-43所示。

<img src="img/图5-43.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-43  错误信息提示

由计算结果可以看出，对a进行除法运算和取余运算的结果均为NULL。

### 5.2.3.比较运算符

一个比较运算符的结果总是1、0或者NULL，比较运算符经常在SELECT查询条件子句中使用，用来查询满足指定条件的记录。MySQL中的比较运算符如表5-9所示。

<div align = "center" style="font-weight:bold">表5-9 MySQL中的比较运算符

|   运算符    | 作  用                           |
| :---------: | :------------------------------- |
|      =      | 等于                             |
|     <=>     | 安全等于（可以比较NULL）         |
|   <>(!=)    | 不等于                           |
|     <=      | 小于等于                         |
|     >=      | 大于等于                         |
|      <      | 小于                             |
|      >      | 大于                             |
|   IS NULL   | 判断一个值是否为 NULL            |
| IS NOT NULL | 判断一个值是否不为NULL           |
|    LEAST    | 当有两个或多个参数时返回最小值   |
|  GREATEST   | 当有两个或多个参数时返回最大值   |
| BETWEEN AND | 判断一个值是否落在两个值之间     |
|   IS NULL   | 与IS NULL相同                    |
|     IN      | 判断一个值是IN列表中的任意一值   |
|   NOT IN    | 判断一个值不是IN列表中的任意一值 |
|    LIKE     | 通配符匹配                       |
|   REGEXP    | 正则表达式匹配                   |

下面依次讨论各个比较运算符的使用方法。

1．等于运算符＝

“＝”用来判断数字、字符串和表达式是否相等，如果相等，返回值为1，否则返回值为0。

【例5-27】使用“＝”进行相等判断。

SQL 语句如下：

```MySQL
SELECT 5=6,'8'=8, 668=668, '0.02'=0, 'keke'='keke' , (2+40)=(20+22),NULL=NULL;
```

执行结果如图5-44所示。

由结果可以看到，在进行判断时＇8＇＝8和668＝668的返回值相同，都是1。因为在进行比较判断时MySQL 自动进行了转换，把字符＇8＇转换成数字 8；＇keke＇＝＇keke＇为相同的字符比较，因此返回值为 1；表达式2＋40和20＋22的结果都为42，结果相等，因此返回值为1；由于“＝”不能用于空值的判断，因此返回值为NULL。

<img src="img/图5-44.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-44 使用“＝”进行相等判断

在进行数值比较时有以下规则：

（1）若有一个或两个操作数为NULL，则比较运算的结果为NULL。

（2）若同一个比较运算中的两个操作数都是字符串，则按照字符串进行比较。

（3）若两个操作数均为整数，则按照整数进行比较。

（4）若一个字符串和一个数字进行相等判断，则MySQL可以自动将字符串转换为数字。

2．安全等于运算符＜＝＞

＜＝＞运算符具有＝运算符的所有功能，唯一不同的是＜＝＞可以用来判断 NULL 值。当两个操作数均为NULL时，其返回值为1，而不为NULL；当其中一个操作数为NULL时，其返回值为0，而不为NULL。【例5-28】使用“＜＝＞”进行相等判断。

SQL 语句如下：

```mysql
SELECT 5<=>6,'8'<=>8,8<=>8,'0.08'<=>0,'k'<=>'k',(2+4)<=>(3+3),NULL<=>NULL;
```

 执行结果如图5-45所示。

<img src="img/图5-45.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图5-45 使用“＜＝＞”进行相等判断

由结果可以看到，“＜＝＞”在执行比较操作时和“＝”的作用是相似的，唯一的区别是“＜＝＞”可以用来对NULL进行判断，当两个操作数都为NULL时返回值为1。

3．不等于运算符＞或者！＝

“”或者“！＝”用于数字、字符串、表达式不相等的判断，如果不相等，返回值为 1，否则返回值为0。注意，这两个运算符不能用于判断空值。

【例5-29】使用“”和“！＝”进行不相等判断。

SQL 语句如下：

```mysql
SELECT 're'<>'ra',3<>4,1!=1,2.2!=2,(2+0)!=(2+1),NULL<>NULL;
```

执行结果如图5-46所示。

<img src="img/图5-46.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-46 使用“＜＞”和“！＝”进行不相等判断</div>
由结果可以看到，两个不等于运算符的作用相同，都可以进行数字、字符串、表达式的比较。

例如：

由结果可以看到，两个不等于运算符的作用相同，都可以进行数字、字符串、表达式的比较。例如：

```MySQL
SELECT 're'>='ra',3>-4,6>=6,8.8>-6,(1+10)>=(2+20),NULL>=NULL;
```

执行结果如图5-47所示。

<img src="img/图5-47.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-47 不相等判断

4．小于或等于运算符＜＝

“＜＝”用来判断左边的操作数是否小于等于右边的操作数，如果小于或等于，返回值为1，否则返回值为0。注意，“＜＝”不能用于判断空值。

【例5-30】使用“＜＝”进行比较判断。

SQL 语句如下：

```MySQL
SELECT 're'<='ra',5<=6,8<=8,8.8<=8,(1+10)<=(20+2),NULL<=NULL;
```

执行结果如图5-48所示。

<img src="img/图5-48.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-48 使用“＜＝”进行比较判断
由结果可以看出，当左边操作数小于或等于右边时返回值为1，例如＇re＇＜＝＇ra＇，re的第2位字符e在字母表中的顺序小于ra的第2位字符a，因此返回值为0；当左边操作数大于右边操作数时返回值为0，例如8.8＜＝8，返回值为0；同样，在比较NULL值时返回NULL。

5．小于运算符＜

“＜”运算符用来判断左边的操作数是否小于右边的操作数，如果小于，返回值为1，否则返回值为0。注意“＜”不能用于判断空值。

【例5-31】使用“＜”进行比较判断。

SQL 语句如下：

```MySQL
SELECT 'ra'<'re',5<6,8<8,8.8<8,(1+10)<(20+2),NULL<NULL;
```

执行结果如图5-49所示。

<img src="img/图5-49.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-49 使用“＜”进行比较判断

6．大于或等于运算符＞＝

“＞＝”运算符用来判断左边的操作数是否大于或者等于右边的操作数，如果大于或者等于，返回值为1，否则返回值为0。注意，“＞＝”不能用于判断空值。

【例5-32】使用“＞＝”进行比较判断。

SQL 语句如下：

```MySQL
SELECT 'ra'>='re',5>=6,8>=8,8.8>=8,(10+1)>=(20+2),NULL>=NULL; 
```

执行结果如图5-50所示。

<img src="img/图5-50.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-50 使用“＞＝”进行比较判断

由结果可以看到，当左边操作数大于或者等于右边操作数时返回值为1，例如8＞＝8；当左边操作数小于右边操作数时返回值为0，例如5＞＝6；同样，在比较NULL值时返回NULL。

7．大于运算符＞

“＞”运算符用来判断左边的操作数是否大于右边的操作数，如果大于，返回值为1，否则返回值为0。

> **注意，“＞”不能用于判断空值。**

【例5-33】使用“＞”进行比较判断。

SQL 语句如下：

```MySQL
SELECT 'ra'>'re',5>6,8>8,8.8>8,(10+1)>(20+2),NULL>NULL;
```

执行结果如图5-51所示。

<img src="img/图5-51.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-51 使用“＞”进行比较判断

由结果可以看到，当左边操作数大于右边操作数时返回值为1，例如8.8＞8；当左边操作数小于右边操作数时返回0，例如5＞6；同样，在比较NULL 值时返回NULL。

8．IS NULL（ISNULL）、IS NOT NULL运算符

IS NULL和ISNULL 用于检验一个值是否为NULL，如果为NULL，返回值为1，否则返回值为0；ISNOT NULL 用于检验一个值是否为非NULL，如果为非NULL，返回值为1，否则返回值为0。

【例5-34】使用IS NULL、ISNULL和IS NOT NULL判断NULL值和非NULL值。

SQL 语句如下：

```MySQL
SELECT NULL IS NULL,ISNULL(NULL),ISNULL(66),66 IS NOT NULL;
```

执行结果如图5-52所示。

<img src="img/图5-52.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图5-52 判断NULL值和非NULL值</div>
由结果可以看出，ISNULL和ISNULL的作用相同，使用格式不同；ISNULL和IS NOT NULL的返回值正好相反。

9．BETWEEN AND 运算符

其语法格式为“expr BETWEEN min AND max”。如果 expr 大于或等于min且小于或等于max，则BETWEEN的返回值为1，否则返回值为0。

【例5-35】使用BETWEEN AND进行值区间判断。

SQL 语句如下：

```MySQL
SELECT 66 BETWEEN 0 AND 100,6 BETWEEN 0 AND 10,10 BETWEEN 0 AND 5;
```

执行结果如图5-53所示。

<img src="img/图5-53.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-53 值区间判断

10．LEAST 运算符

其语法格式为“LEAST（值1，值2，···值n）”，其中值n表示参数列表中有n个值。在有两个或多个参数的情况下，其返回最小值。假如任意一个自变量为NULL，则LEASTO的返回值为NULL。

【例5-36】使用LEAST运算符进行大小判断。

SQL 语句如下：

```MySQL
SELECT least(6,3), least(6.2,5.2,3.6),least('a', 'c', 'b'),least(100,NULL);
```

 执行结果如图5-54所示。

<img src="img/图5-54.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-54 使用LEAST运算符进行大小判断

由结果可以看到，当参数中是整数或者浮点数时LEAST将返回其中的最小值；当参数为字符串时返回字母表顺序最靠前的字符；当比较值列表中有NULL时不能判断大小，返回值为NULL。

11．GREATEST 运算符

其语法格式为“GREATEST（值1，值2，···值n）”，其中值n表示参数列表中有n个值。当有两个或多个参数时返回值为最大值，假如任意一个自变量为NULL，则GREATESTO的返回值为 NULL。

【例5-37】使用GREATEST运算符进行大小判断。

SQL 语句如下：

```MySQL
SELECT greatest(6,3),greatest(6.2,5.2),greatest('a','c'),greatest(10,NULL);
```

执行结果如图5-55所示。

当参数中是整数或者浮点数时，GREATEST将返回其中的最大值；当参数为字符串时返回字母表顺序中最靠后的字符；当比较值列表中有NULL时不能判断大小，返回值为NULL。

<img src="img/图5-55.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-55 使用GREATEST 运算符进行大小判断

12．IN、NOT IN 运算符

IN运算符用来判断操作数是否为IN列表中的一个值，如果是，返回值为1，否则返回值为0。NOT IN运算符用来判断操作数是否为IN列表中的一个值，如果不是，返回值为1，否则返回值为0。【例5-38】使用IN、NOT IN运算符进行判断。

使用IN运算符的SQL语句如下：

```MySQL
SELECT 6 IN (5,8,'ke'), 'bb' IN (9,10,'ke');
```

执行结果如图5-56所示。

<img src="img/图5-56.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-56  使用IN运算符进行判断

使用NOT IN运算符的SQL语句如下：

```MySQL
SELECT 6 NOT IN (5,6,'ke'), 'bb' NOT IN (20,4,'sd');
```

执行结果如图5-57所示。

<img src="img/图5-57.png" alt="img" style="zoom: 150%;" /> 

<div align = "center" style="font-weight:bold">图5-57 使用 NOT IN 运算符进行判断

由结果可以看到，IN和NOTIN的返回值正好相反。

在左侧表达式为NULL的情况下，或是表中找不到匹配项并且表中的一个表达式为NULL的情况下，IN的返回值均为NULL。

【例5-39】存在NULL值时的IN查询。

SQL 语句如下：

```MySQL
SELECT NULL IN (2,6,'ke'),10 IN (20,30,NULL,'ke'); 
```

执行结果如图5-58所示。

<img src="img/图5-58.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-58 存在NULL值时的IN查询

IN也可用于SELECT语句中进行嵌套子查询，在后面的章节中将具体讲解。

13．LIKE 运算符

LIKE 运算符用来匹配字符串，其语法格式为“expr LIKE匹配条件”。如果expr满足匹配条件，则返回值为1（TRUE）；如果不匹配，则返回值为0（FALSE）。若expr或匹配条件中的任何一个为NULL，则结果为NULL。

LIKE运算符在进行匹配时可以使用下面两种通配符。

（1）％：匹配任何字符，甚至包括零字符。

（2）＿：只能匹配一个字符。

【例5-40】使用LIKE运算符进行字符串匹配运算。

SQL 语句如下：

```MySQL
SELECT 'keke' LIKE 'keke', 'keke' LIKE 'kek_','keke' LIKE 'se','keke' LIKE 'k   ','k'LIKE NULL;
```

 执行结果如图5-59所示。

<img src="img/图5-59.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-59 字符串匹配运算（1）

由结果可以看出，指定匹配字符串为keke。在第1组比较中，keke直接匹配keke字符串，满足匹配条件，返回值为1；在第2组比较中，“kek”表示匹配以kek开头、长度为4位的字符串，keke 正好4个字符，满足匹配条件，因此匹配成功，返回值为1；在第3组比较中，“％e”表示匹配以字母e结尾的字符串，keke满足匹配条件，匹配成功，返回值为1；在第4组比较中，“k＿＿＿”表示匹配以k开头、长度为4位的字符串，keke满足匹配条件，返回值为1；在第5组比较中，当字符“k”与NULL匹配时结果为NULL。

14．REGEXP 运算符

REGEXP运算符用来匹配字符串，其语法格式为“exprREGEXP匹配条件”。如果expr满足匹配条件，返回1；如果不满足，返回0。若expr或匹配条件中的任意一个为NULL，则结果为NULL。

REGEXP运算符在进行匹配时常用下面几种通配符。

（1）＾：匹配以该字符后面的字符开头的字符串。

（2）＄：匹配以该字符前面的字符结尾的字符串。

（3）：匹配任何一个单字符。

（4）［···］：匹配方括号内的任何字符。例如［abc］匹配a、b或c。为了指定字符的范围，使用“-”，例如［a-z］匹配任意字母，而［0-9］匹配任意数字。

（5）＊：匹配零个或多个在它前面的字符。例如，“x＊”匹配任意数量的＇x字符，“［0-9］＊”匹配任意数

量的数字，而“＊”匹配任意数量的任意字符。

【例5-41】使用REGEXP运算符进行字符串匹配运算。

SQL 语句如下：

```MySQL
SELECT 'keke'REGEXP'^k','keke'REGEXP'e$', 'keke'REGEXP'.ke','keke'REGEXP'[ab]';
```

执行结果如图5-60所示。

 <img src="img/图5-60.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-60 字符串匹配运算（2）

由结果可以看到，指定匹配字符串为keke。“＾k”表示匹配任何以字母k开头的字符串，满足匹配条件，因此返回值为1；“e＄”表示匹配任何以字母e结尾的字符串，满足匹配条件，因此返回值为1；“．ke”表示匹配任何以ke结尾的字符串，满足匹配条件，因此返回值为1；“［ab］”表示匹配任何包含字母a或者b的字符串，指定字符串中没有字母a或b，不满足匹配条件，因此返回值为0。

### 5.2.4.逻辑运算符

在SQL中，所有逻辑运算符的求值所得结果均为TRUE、FALSE或NULL。在MySQL中，它们分别显示为1（TRUE）、0（FALSE）和NULL。其中大多数与其他的SQL数据库通用，MySQL中的逻辑运算符如表5-10所示。

<div align = "center" style="font-weight:bold">表5-10 MySQL中的逻辑运算符

|  运算符   |  作  用  |
| :-------: | :------: |
| NOT或者 ! |  逻辑非  |
| AND或者&& |  逻辑与  |
| OR或\|\|  |  逻辑或  |
|    XOR    | 逻辑异或 |

下面分别介绍不同逻辑运算符的使用方法。

1．NOT或者！

逻辑非运算符NOT或者“！”表示当操作数为0时返回值为1，当操作数为1时返回值为0，当操作数为NULL时返回值为NULL。

【例5-42】分别使用逻辑非运算符NOT和“！”进行逻辑判断。

NOT运算符的SQL语句如下：

```MySQL
SELECT NOT 6,NOT (6-6),NOT -6,NOT NULL,NOT 6+6;
```

执行结果如图5-61所示。

 <img src="img/图5-61.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-61  NOT 逻辑判断 </div>
“！”运算符的SQL语句如下：

```MySQL
SELECT !6,!(6-6),!-6,!NULL,!6+6;
```

执行结果如图5-62所示。

<img src="img/图5-62.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-62   “！”逻辑判断

由结果可以看到，前4列NOT和“！”的返回值相同，最后一列结果不同。出现这种结果的原因是NOT和“！”的优先级不同。NOT的优先级低于“＋”，因此“NOT6＋6”先计算“6＋6”，然后再进行逻辑非运算，由于操作数不为0，所以“NOT6＋6”的最终返回值为0；另一个逻辑非运算符“！”的优先级高于“＋”运算符，因此“！6＋6”先进行逻辑非运算“！6”，结果为0，然后再进行加法运算“0＋6”，所以最终返回值为6。

提示：在使用运算符时一定要注意不同运算符的优先级，如果不能确定优先级的顺序，最好使用括号，以保证运算结果正确。

2．AND或者&&

逻辑与运算符AND或者“＆＆”表示当所有操作数均为非零值并且不为NULL时返回值为1，当一个或多个操作数为0时返回值为0，其余情况返回值为NULL。

【例5-43】分别使用逻辑与运算符AND和“＆＆”进行逻辑判断。

AND运算符的SQL语句如下：

```MySQL
SELECT 6 AND -6,6 AND 0, 6 AND NULL,0 AND NULL;
```

执行结果如图5-63所示。

<img src="img/图5-63.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-63 AND 逻辑判断

</div>

“＆＆”运算符的SQL语句下：

```MySQL
SELECT 6 && -6,6 && 0, 6 && NULL, 0 && NULL; 
```

执行结果如图5-64所示。

<img src="img/图5-64.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-64 ＆＆逻辑判断

由结果可以看到，AND和“＆＆”的作用相同。在“6AND-6”中没有0或NULL，因此返回值为1；在“6AND0”中有操作数0，因此返回值为0；在“6 AND NULL”中虽然有NULL，但是没有操作数0，返回结果为NULL。

3．OR或者||

逻辑或运算符OR或者“｜”表示当两个操作数均为非NULL值且任意一个操作数为非零值时结果为1，否则结果为0；当有一个操作数为NULL且另一个操作数为非零值时结果为1，否则结果为NULL；当两个操作数均为NULL时结果为NULL。

【例5-44】分别使用逻辑或运算符OR和“I”进行逻辑判断。

OR运算符的SQL语句如下：

```MySQL
SELECT 6 OR -6 OR 0,6 OR 4,6 OR NULL,0 OR NULL,NULL OR NULL;
```

执行结果如图5-65所示。

<img src="img/图5-65.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-65 OR 逻辑判断

“｜”运算符的SQL语句如下：

```MySQL
SELECT 6 || -6 || 0,6 || 4,6 || NULL,0 || NULL,NULL || NULL; 
```

执行结果如图5-66所示。

 <img src="img/图5-66.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-66  ｜逻辑判断

由结果可以看出，OR和“｜”的作用相同。在“6OR-6OR0”中有0，同时包含有非零值6和-6，返回值结果为1；在“6OR4”中没有操作数0，返回值结果为1；在“6｜NULL”中虽然有NULL，但是有操作数6，返回值结果为1；在“OORNULL”中没有非零值，并且有NULL，返回值结果为NULL；在“NULLOR NULL”中只有NULL，返回值结果为NULL。

4.XOR

逻辑异或运算符XOR表示当任意一个操作数为NULL时返回值为NULL；对于非NULL的操作数，如果两个操作数都是非零值或者都是零值，返回值结果为 0；如果一个为零值，另一个为非零值，返回值结果1。

【例5-45】使用异或运算符XOR进行逻辑判断。

SQL 语句如下：

```MySQL
SELECT 6 XOR 6,0 XOR 0,6 XOR 0,6 XOR NULL,6 XOR 6 XOR 6;
```

执行结果如图5-67所示。

<img src="img/图5-67.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-67 XOR逻辑判断

由结果可以看到，在“6XOR6”和“0XORO”中运算符两边的操作数都为非零值或者都为零值，因此返回0；在“6XOR0”中运算符两边的操作数一个为零值，另一个为非零值，返回结果为1；在“6XORNULL”中有一个操作数为NULL，返回值为NULL；在“6XOR6XOR6”中有多个操作数，运算符相同，因此从左到右依次运算，“6XOR6”的结果为0，再与6进行异或运算，因此结果为1。

### 5.2.5.位运算符

位运算符用来对二进制字节中的位进行位移或者测试处理，MySQL中提供的位运算符有按位或（|）、按位与（＆）、按位异或（＾）、按位左移（<＜）、按位右移（>＞）、按位取反（～）等运算符，如表5-11所示。

<div align = "center" style="font-weight:bold">表5-11 MySQL中的位运算符

| 运算符 |           作 用            |
| :----: | :------------------------: |
|   \|   |           按位或           |
|   &    |           按位与           |
|   ^    |          按位异或          |
|   <<   |          按位左移          |
|  ＞＞  |          按位右移          |
|   ~    | 按位取反，反转所有二进制位 |

接下来分别介绍各位运算符的使用方法。

1．按位或运算符｜

按位或运算符实际是将参与运算的两个数据按对应的二进制数进行逻辑或运算，对应的二进制位有一个或两个为1，则该位的运算结果为1，否则为0。

【例5-46】使用按位或运算符进行运算。

SQL 语句如下：

```mysql
SELECT 8|12,6|4|1;
```

执行结果如图5-68所示。

10的二进制数为1000,12的二进制数为1100，在按位或之后结果为1100，即整数12；6的二进制数为0110,4的二进制数为0100,1的二进制数为0001，在按位或之后结果为0111，即整数7。

<img src="img/图5-68.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-68  使用按位或运算符进行运算

2．按位与运算符＆

按位与运算符实际是将参与运算的两操作数按对应的二进制数逐位进行逻辑与运算，对应的二进制位都为1，则该位的运算结果为1，否则为0。

【例5-47】使用按位与运算符进行运算。

SQL 语句如下：

```MySQL
SELECT 8 & 12,6&4 &1;
```

执行结果如图5-69所示。

<img src="img/图5-69.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-69  使用按位与运算符进行运算

8的二进制数为1000,12的二进制数为1100，在按位与之后结果为1000，即整数8；6的二进制数为0110,4的二进制数为0100,1的二进制数为0001，在按位与之后结果为0000，即整数0。

3．按位异或运算符＾

按位异或运算符实际是将参与运算的两个数据按对应的二进制数逐位进行逻辑异或运算，当对应的二进制数不同时对应位的结果才为1，如果两个对应位数都为0或都为1，则对应位的运算结果为0。

【例5-48】使用按位异或运算符进行运算。

SQL 语句如下：

```MySQL
SELECT 8^12,4^2,4^1;
```

执行结果如图5-70所示。

8的二进制数为1000,12的二进制数为1100，在按位异或之后结果为0100，即十进制数4；4的二进制数为0100,2的二进制数为0010，在按位异或之后结果为0110，即十进制数6；4的二进制数为0100，1的二进制数为0001，在按位异或之后结果为0101，即十进制数5。

<img src="img/图5-70.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-70 使用按位异或运算符进行运算

4．按位左移运算符＜＜

按位左移运算符＜＜的功能是让指定二进制数的所有位都左移指定的位数。在左移指定位数之后，左边高位的数值将被移出并丢弃，右边低位空出的位置用0补齐。其语法格式为“a＜＜n”，这里的n指定值a要移动的位置。

【例5-49】使用按位左移运算符进行运算。

SQL 语句如下：

```MySQL
SELECT 6<<2,8<<1;
```

执行结果如图5-71所示。

<img src="img/图5-71.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-71  使用按位左移运算符进行运算

6的二进制数为0000 0110，左移两位之后变成0001 1000，即十进制数24；8的二进制数为 0000 1000，左移一位之后变成0001 0000，即十进制数16。

5．按位右移运算符＞

按位右移运算符＞＞的功能是让指定二进制数的所有位都右移指定的位数。在右移指定位数之后，右边低位的数值将被移出并丢弃，左边高位空出的位置用0补齐。其语法格式为“a＞＞n”，这里的n指定值a要移动的位置。

【例5-50】使用按位右移运算符进行运算。

SQL 语句如下：

```MySQL
SELECT 6>>1,8>>2;
```

执行结果如图5-72所示。

6的二进制数为0000 0110，右移一位之后变成0000 0011，即十进制数3；8的二进制数为0000 1000，右移两位之后变成0000 0010，即十进制数2。

<img src="img/图5-72.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-72 使用按位右移运算符进行运算

</div>

6．按位取反运算符～

按位取反运算符实际是将参与运算的数据按对应的二进制数逐位反转，即1取反后变为0,0取反后变为1。

【例5-51】使用按位取反运算符进行运算。

SQL 语句如下：

```MySQL
SELECT 6&~2;
```

执行结果如图5-73所示。

<img src="img/图5-73.png" alt="img" style="zoom: 150%;" />

<div align = "center" style="font-weight:bold">图5-73 使用按位取反运算符进行运算

对于逻辑运算6＆～2，由于按位取反运算符“～”的级别高于按位与运算符“＆”，因此先对2进行取反操作，取反的结果为1101；然后再与十进制数6进行运算，结果为0100，即整数4。

### 5.2.6.运算符的优先级

运算符的优先级决定了不同运算符在表达式中计算的先后顺序，表5-12列出了MySQL中的各类运算符及其优先级。

<div align = "center" style="font-weight:bold">表5-12 MySQL中的运算符（按优先级由低到高排列）

</div>

| 优先级 | 运算符                                                       |
| ------ | ------------------------------------------------------------ |
| 最低   | =（赋值运算)                                                 |
| 14     | \|\|、OR                                                     |
| 13     | XOR                                                          |
| 12     | &&、AND                                                      |
| 11     | NOT                                                          |
| 10     | BETWEEN、CASE、WHEN、THEN、ELSE                              |
| 9      | ＝（比较运算）、＜＝＞、＞＝、＞、＜＝、＜、<>、!＝、IS、LIKE、REGEXP、IN |
| 8      | \|                                                           |
| 7      | &                                                            |
| 6      | <<、>>                                                       |
| 5      | -、+                                                         |
| 4      | *、/(DIV)、%(MOD)                                            |
| 3      | ^                                                            |
| 2      | -（负号）、～（按位取反）                                    |
| 最高   | !                                                            |

可以看到，不同运算符的优先级是不同的。在一般情况下，级别高的运算符先进行计算，如果级别相同，MySQL按表达式的顺序从左到右依次计算。当然，在无法确定优先级的情况下可以使用圆括号来改变优先级，这样会使计算过程更加清晰。