---
title: MySQL小知识
date: 2022-08-31 12:25:01
updated: 2023-11-9 19:45
tags: [mysql]
categories: [数据库, mysql]
---
# 前言
`SHOW TABLES FROM database` 与以下条目等价
```sql
use database;
SHOW TABLES;
```

# 表层结构
## information_schema
| 表         | 注解                                                    |
| ---------- | ------------------------------------------------------- |
| SCHEMATA   | `SHOW DATABASES` 命令的结果取自此表，数据库信息在这此储 |
| TABLES     | `SHOW TABLES` 命令结果取自此表，表格信息在此存储        |
| COLUMNS    | `SHOW COLUMNS` 命令结果取自此表，表列信息在此存储       |
| STATISTICS | `SHOW INDEX` 命令结果取自此表, 索引信息在此存储         |

# 等效命令
## show databases
```sql
SELECT SCHEMA_NAME FROM information_schema.SCHEMATA;
```

## show tables FROM database
```sql
SELECT TABLE_NAME FROM information_schema.TABLES where TABLE_SCHEMA = 'database';
```

## show columns FROM database.table
```sql
SELECT COLUMN_NAME FROM information_schema.COLMNS where TABLE_SCHEMA = 'database' AND TABLE_NAME = 'table'; 
```

# 环境信息获取
## 数据库数据位置
```sql
SELECT @@datadir;
```

## 数据库安装位置
```sql
SELECT @@basedir;
```

## 系统类别
```sql
SELECT @@VERSION_COMPILE_OS;
```

## MySQL版本
```sql
SELECT VERSION();
```

## 可执行注释
{% p red::注意: 5位数字是低于或等于当前版本才会运行后面的命令 %}
/\*!(数据库版本5位数字)(要执行的命令)*/  
列如数据库版本5.8.17
```sql
/*!50817SELECT VERSION();*/
```
任何版本都要执行
```sql
/*!00000SELECT VERSION();*/
```

## 当前连接数据库的人数
```sql
SELECT CONNECTION_ID();
```

## 当前用户信息
```sql
SHOW PROCESSLIST;
```
