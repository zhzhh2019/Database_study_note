## MySql:
### 一、表结构修改
#### 1、添加
添加单列:　　　　
```
ALTER TABLE 表名 ADD 列名 数据类型
```
添加多列:　　　　
```
ALTER TABLE 表名 ADD 列名1 数据类型1,Add 列名2 数据类型2
```
#### 2、修改
修改单列数据类型:
```
ALTER TABLE 表名 CHANGE COLUMN 列名 数据类型
```
同时修改多列数据类型:　　
```
ALTER TABLE 表名 CHANGE COLUMN 列名 数据类型,CHANGE COLUMN 列名 数据类型
```
修改表字段长度

```
alter table user modify column name varchar(50);
```

#### 3、删除
删除单列:　　
```
ALTER TABLE 表名 DROP COLUMN 列名
```
删除多列:　　
```
ALTER TABLE 表名 DROP COLUMN 列名1,DROP COLUMN 列名2
```
#### 4、同时添加和修改多列:　　
```
ALTER TABLE 表名 ADD 列名1 数据类型1,CHANGE COLUMN 列名 数据类型,DROP COLUMN 列名1（COLUMN 关键字可以省略）
```
注释：MySql中没有直接的语法可以在增加列前进行判断该列是否存在，解决方案是写一个存储过程来完成此任务。
###二、存储过程
也是一个判断表存不存在，不存在则新建
```
drop procedure if EXISTS crm_add_col_createrid
;
DELIMITER$$
CREATE PROCEDURE crm_add_col_createrid()
BEGIN
IF NOT EXISTS (SELECT * FROM information_schema.columns WHERE table_schema = DATABASE()  AND table_name = 'crm_customerinfo' AND column_name = 'createrid') THEN
    ALTER TABLE crm_customerinfo ADD COLUMN createrid int;
END IF; 
END$$
DELIMITER;
call crm_add_col_createrid;
```
#### 5、查看表信息

```
select column_name,column_comment,data_type,column_type from information_schema.columns where table_name=表名;
```
#### 6、备份表

```
MySQL（为了保持通用性，兼容GTID环境，统一采用如下的备份表方式）
create table ft_main_67_bak_20200513 like formtable_main_67;
或者
insert into ft_main_67_bak_20200513 select * from formtable_main_67;
```
