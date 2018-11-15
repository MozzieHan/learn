### hive基础 

#### hive 常用命令

- 清屏   Ctrl+L  或  !clear;
- 输入hdfs命令   dfs + hdfs命令;
- 输入系统命令     ! + 系统命令

#### hive 数据类型

##### 基本数据类型

- tinyinit/smallint/int/biginit ： 整数
- float/double ： 浮点数
- boolean： 布尔
- string/varchar/char： 字符串

##### 复杂数据类型

- array： 数组，由一系列相同数据类型的元素组成
- map： 集合，包含key->value ,可以通过key来访问元素
- struct ：结构，可以包含不同的数据类型，通过 ‘点语法’ 来访问所需要的元素

##### 时间类型

- date ： yyyy-mm-dd
- timestamp：10位整数

### HiveQL 数据/操作
#### hive 常用操作

```mysql
CREATE DATABASE test 
  LOCATION '/user/hive/worehouse/test' -- 修改数据默认存放位置
  COMMENT 'test databases'; -- 描述信息
SHOW DATABASES LIKE 't.*';
USE test;  -- 使用test数据库
SHOW TABLES;  -- 显示tables
DROP DATABASE IF EXISTS test2 CASCADE;  -- 删除数据库,CASCADE在删除库时如果库里有表就先删除库里的表
CREATE TABLE IF NOT EXISTS test.user (
    id   INT,
    name STRING COMMENT 'name',
    info STRUCT<age:INT, vaction:STRING>
  );

```

#### 外部表
```mysql
CREATE EXTERNAL TABLE IF NOT EXISTS test.tuser ( -- EXTERNAL 表明这个表是外部的
    id   INT,
    name STRING
  )
  ROW FORMAT DELIMITED FIELDS TERMINATED BY ','  -- 指明分割符
  LOCATION '/data/user';  -- 数据所在目录

CREATE EXTERNAL TABLE IF NOT EXISTS tuser2
  LIKE test.tuser 
  LOCATION '/data/user';  -- 复制表结构
```

#### 分区表
```mysql
CREATE TABLE IF NOT EXISTS user (
    id    INT,
    name  STRING
  )
  PARTITIONED BY (gender STRING, age INT); -- 先以gender分区，再以age分区
SHOW PARTITIONS user; -- 查看表中所有分区
SHOW PARTITIONS user PARTITION(gender="male"); -- 过滤分区
```
分区会在表下生成不同的目录

#### 外部分区表
```mysql
CREATE EXTERNAL TABLE IF NOT EXISTS user (
    id INT,
    name  STRINT
  )
  PARTITIONED BY (gender STRING, age INT)
  ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';
```

#### 数据操作
```mysql
-- 装载数据
LOAD DATA LOCAL INPATH '/user'
  OVERWRITE INTO TABLE user
  PARTITION (gender="male", age=13);
```














