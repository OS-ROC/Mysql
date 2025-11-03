# SQL DDL 完整知识整理文档  
> 日期：2025-11-03

---

## 目录

1. [SQL 概述与分类](#1-sql-概述与分类)  
2. [SQL 通用语法规范](#2-sql-通用语法规范)  
3. [DDL - 数据库操作](#3-ddl---数据库操作)  
4. [DDL - 表操作（核心）](#4-ddl---表操作核心)  
   - 4.1 查询  
   - 4.2 创建表  
   - 4.3 修改表（`ALTER TABLE`）  
   - 4.4 删除表  
5. [MySQL 数据类型详解](#5-mysql-数据类型详解)  
   - 5.1 日期时间类型  
   - 5.2 数值类型  
   - 5.3 字符串类型  
6. [MySQL 环境操作流程](#6-mysql-环境操作流程)  
7. [DDL 核心操作思维导图](#7-ddl-核心操作思维导图)  
8. [示例代码与注意事项](#8-示例代码与注意事项)  
9. [快速索引表](#9-快速索引表)  

---

## 1. SQL 概述与分类

| 分类 | 全称 | 说明 |
|------|------|------|
| **DDL** | Data Definition Language | 数据定义语言，用于定义数据库对象（数据库、表、字段） |
| **DML** | Data Manipulation Language | 数据操作语言，用于对数据库表中的数据进行增删改 |
| **DQL** | Data Query Language | 数据查询语言，用于查询数据库中表的记录 |
| **DCL** | Data Control Language | 数据控制语言，用于创建数据库用户、控制数据库的访问权限 |



---

## 2. SQL 通用语法规范

1. SQL语句可以单行或多行书写，以**分号 `;`** 结尾  
2. SQL语句可以使用空格/缩进来增强可读性  
3. **MySQL数据库的SQL语句不区分大小写**，建议关键字使用大写  
4. **注释方式**：
   - 单行注释：`-- 注释内容`（MySQL特有）
   - 多行注释：`/* 注释内容 */`



---

## 3. DDL - 数据库操作

| 操作 | 命令 |
|------|------|
| 查看所有数据库 | `SHOW DATABASES;` |
| 查看当前数据库 | `SELECT DATABASE();` |
| 创建数据库 | `CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];` |
| 删除数据库 | `DROP DATABASE [IF EXISTS] 数据库名;` |
| 使用数据库 | `USE 数据库名;` |


---

## 4. DDL - 表操作（核心）

### 4.1 查询

```sql
SHOW TABLES;                    -- 查询当前数据库所有表
DESC 表名;                      -- 查询表结构
SHOW CREATE TABLE 表名;         -- 查询指定表的建表语句
```


### 4.2 创建表
```sql
CREATE TABLE 表名(
    字段1 类型 [COMMENT '字段1注释'],
    字段2 类型 [COMMENT '字段2注释'],
    字段3 类型 [COMMENT '字段3注释'],
    ...
    字段n 类型 [COMMENT '字段n注释']
) [COMMENT '表注释'];
```
注意：
[...] 表示可选项
最后一个字段后面没有逗号
最后一个字段后面可选项有逗号

### 4.3 修改表（`ALTER TABLE`）

| 操作 | 命令 |
|------|------|
| **添加字段** | `ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT '注释'] [约束];` |
| **修改数据类型** | `ALTER TABLE 表名 MODIFY 字段名 新类型(长度);` |
| **修改字段名和类型** | `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT '注释'] [约束];` |
| **删除字段** | `ALTER TABLE 表名 DROP 字段名;` |

#### 示例：

```sql
-- 为 emp 表增加 nickname 字段
ALTER TABLE emp ADD nickname varchar(20) COMMENT '昵称';

-- 将 nickname 字段修改为 username，类型为 varchar(30)
ALTER TABLE emp CHANGE nickname username varchar(30) COMMENT '昵称';

-- 删除 username 字段
ALTER TABLE emp DROP username;
```

### 4.4 删除表
```sql
DROP TABLE [IF EXISTS] 表名;           -- 删除表结构和数据
TRUNCATE TABLE 表名;                   -- 删除表中全部数据，重建表（保留结构）
```
注意：在删除表时，表中的全部数据也会被删除



## 5. MySQL 数据类型详解
### 5.1 日期时间类型

| 类型 | 大小 | 范围 | 格式 | 描述 |
|------|------|------|------|------|
| `DATE` | 3 | 1000-01-01 至 9999-12-31 | `YYYY-MM-DD` | 日期值 |
| `TIME` | 3 | -838:59:59 至 838:59:59 | `HH:MM:SS` | 时间值或持续时间 |
| `YEAR` | 1 | 1901 至 2155 | `YYYY` | 年份值 |
| `DATETIME` | 8 | 1000-01-01 00:00:00 至 9999-12-31 23:59:59 | `YYYY-MM-DD HH:MM:SS` | 混合日期和时间值 |
| `TIMESTAMP` | 4 | 1970-01-01 00:00:01 至 2038-01-19 03:14:07 | `YYYY-MM-DD HH:MM:SS` | 混合日期和时间值，时间戳 |


### 5.2 数值类型

| 类型 | 大小 | 有符号(SIGNED)范围 | 无符号(UNSIGNED)范围 | 描述 |
|------|------|---------------------|-----------------------|------|
| `TINYINT` | 1 byte | -128 ~ 127 | 0 ~ 255 | 小整数值 |
| `SMALLINT` | 2 bytes | -32768 ~ 32767 | 0 ~ 65535 | 大整数值 |
| `MEDIUMINT` | 3 bytes | -8388608 ~ 8388607 | 0 ~ 16777215 | 大整数值 |
| `INT` / `INTEGER` | 4 bytes | -2147483648 ~ 2147483647 | 0 ~ 4294967295 | 大整数值 |
| `BIGINT` | 8 bytes | -(2^63) ~ (2^63)-1 | 0 ~ (2^64)-1 | 极大整数值 |
| `FLOAT` | 4 bytes | ±3.402823466 E+38 | 0 和 (1.175494351 E-38 ~ 3.402823466 E+38) | 单精度浮点数值 |
| `DOUBLE` | 8 bytes | ±1.7976931348623157 E+308 | 同上 | 双精度浮点数值 |
| `DECIMAL(M,D)` | 依赖于M(精度)和D(标度)的值 | 依赖于M和D | 依赖于M和D | 小数值（精确定点数） |





### 5.3 字符串类型

| 类型 | 大小 | 描述 |
|------|------|------|
| `CHAR` | 0~255 bytes | 定长字符串 |
| `VARCHAR` | 0~65535 bytes | 变长字符串 |
| `TINYBLOB` | 0~255 bytes | 不超过255个字符的二进制数据 |
| `TINYTEXT` | 0~255 bytes | 短文本字符串 |
| `BLOB` | 0~65 535 bytes | 二进制形式的长文本数据 |
| `TEXT` | 0~65 535 bytes | 长文本数据 |
| `MEDIUMBLOB` | 0~16 777 215 bytes | 二进制形式的中等长度数据 |
| `MEDIUMTEXT` | 0~16 777 215 bytes | 中等长度文本数据 |
| `LONGBLOB` | 0~4 294 967 295 bytes | 二进制形式的极大文本数据 |
| `LONGTEXT` | 0~4 294 967 295 bytes | 极大文本数据 |



## 6. MySQL 环境操作流程

| 步骤 | 操作 |
|------|------|
| 1. MySQL下载及安装 | MySQL 社区版 |
| 2. MySQL启动 | `net start mysql80` |
| 3. MySQL客户端连接 | `mysql -h127.0.0.1 -P3306 -uroot -p` |
| 4. MySQL数据模型 | 数据库 → 表 |







## 8. 示例代码与注意事项

### 示例建表语句

```sql
CREATE TABLE emp (
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT '编号',
    name VARCHAR(50) COMMENT '姓名',
    age TINYINT UNSIGNED COMMENT '年龄',
    gender ENUM('男','女') DEFAULT '男' COMMENT '性别'
) COMMENT '员工表';
```
## 9. 快速索引表

| 关键词 | 跳转链接 |
|--------|----------|
| 查看所有数据库 | [§3](#3-ddl---数据库操作) |
| 创建表语法 | [§4.2](#42-创建表) |
| 添加字段 | [§4.3](#43-修改表alter-table) |
| 修改字段类型 | [§4.3](#43-修改表alter-table) |
| 修改字段名 | [§4.3](#43-修改表alter-table) |
| 删除字段 | [§4.3](#43-修改表alter-table) |
| 删除表 | [§4.4](#44-删除表) |
| `TRUNCATE` vs `DROP` | [§4.4](#44-删除表) |
| 日期时间类型 | [§5.1](#51-日期时间类型) |
| 数值类型 | [§5.2](#52-数值类型) |
| 字符串类型 | [§5.3](#53-字符串类型) |
| MySQL 启动命令 | [§6](#6-mysql-环境操作流程) |
| DDL 思维导图 | [§7](#7-ddl-核心操作思维导图) |