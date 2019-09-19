# 表、字段统计信息
1. 创建表，插入数据
2. hive analyze表
3. 查表信息
4. hive analyze字段
5. 查字段信息
## textfile表
```
CREATE TABLE `wcz.t_text`(
  `id` int, 
  `code` string)
stored as textfile
;
insert into wcz.t_text values (1,'a'),(2,'bb')
;
-- 查记录条数
desc formatted wcz.t_text--2（未analyze，已有）
desc formatted wcz.t_text id--无，analyze后才有
-- 统计
analyze table wcz.t_text COMPUTE STATISTICS for columns
```  
## orc表
```
CREATE TABLE `wcz.t_orc`(
  `id` int, 
  `code` string)
stored as orc
;
insert into wcz.t_orc values (1,'a'),(2,'bb')
;
-- 查记录条数
desc formatted wcz.t_orc--2（未analyze，已有）
desc formatted wcz.t_orc id--无，analyze后才有
-- 统计
analyze table wcz.t_orc COMPUTE STATISTICS for columns
``` 
## parquet表
```
CREATE TABLE `wcz.t_parquet`(
  `id` int, 
  `code` string)
stored as parquet
;
insert into wcz.t_parquet values (1,'a'),(2,'bb')
;
-- 查记录条数
desc formatted wcz.t_parquet--2（未analyze，已有）
desc formatted wcz.t_parquet id--无，analyze后才有
-- 统计
analyze table wcz.t_parquet COMPUTE STATISTICS for columns
```
## 分区表（一个分区相当于一张表）
```
drop table wcz.t_part;
CREATE TABLE `wcz.t_part`(
  `id` int, 
  `code` string)PARTITIONED BY ( 
  `yyyy` int)
stored as parquet
;
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
insert into wcz.t_part partition(yyyy) values (1,'a',2018),(2,'bb',2019)
;
-- 查记录条数
desc formatted wcz.t_part partition(yyyy=2018)--2（未analyze，整体只显示分区数，带分区才显示条数）
desc formatted wcz.t_part id partition(yyyy=2018) --无，analyze后才有
-- 统计
analyze table wcz.t_part COMPUTE STATISTICS for columns
```


## 收集
```
-- 非分区表
analyze table wcz.test_orc COMPUTE STATISTICS
-- 分区表
analyze table wcz.test_partition partition(year_id,mon_id) COMPUTE STATISTICS
-- 字段
analyze table wcz.test_orc COMPUTE STATISTICS for columns('col1,col2')--可不写字段
```
## 查询
```
-- 查询表
DESC EXTENDED wcz.test_orc;
desc formatted wcz.test_array;--推荐
SHOW TBLPROPERTIES wcz.test_array;--spark下查询结果不全
-- 查询字段
desc formatted wcz.test_array id
```

