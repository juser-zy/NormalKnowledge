## 分类
- 主键索引
	- 主键 : 某一个属性组能唯一标识一条记录
	- 特点 :
		-   最常见的索引类型
		-   确保数据记录的唯一性
		-   确定特定数据记录在数据库中的位置
- 唯一索引
	- 作用 : 避免同一个表中某数据列中的值重复
	- 与主键索引的区别
		- 主键索引只能有一个
		- 唯一索引可能有多个
```bash
CREATE TABLE `Grade`(
    `GradeID` INT(11) AUTO_INCREMENT PRIMARYKEY,
    `GradeName` VARCHAR(32) NOT NULL UNIQUE
    -- 或 UNIQUE KEY `GradeID` (`GradeID`)
)
```
- 常规索引
	- 作用 : 快速定位特定数据
	- 注意 :
		-   index 和 key 关键字都可以设置常规索引
		-   应加在查询找条件的字段
		-   不宜添加太多常规索引,影响数据的插入,删除和修改操作
```bash
CREATE TABLE `result`(
    -- 省略一些代码
    INDEX/KEY `ind` (`studentNo`,`subjectNo`) -- 创建表时添加
)
```
- 全文索引
- 百度搜索：全文索引
	- 作用 : 快速定位特定数据
	- 注意 :
		- 只能用于MyISAM类型的数
		- 只能用于CHAR , VARCHAR , TEXT数据列类型
		- 适合大型数据集
```bash
/*
#方法一：创建表时
    　　CREATE TABLE 表名 (
                字段名1  数据类型 [完整性约束条件…],
                字段名2  数据类型 [完整性约束条件…],
                [UNIQUE | FULLTEXT | SPATIAL ]   INDEX | KEY
                [索引名]  (字段名[(长度)]  [ASC |DESC])
                );
#方法二：CREATE在已存在的表上创建索引
        CREATE  [UNIQUE | FULLTEXT | SPATIAL ]  INDEX  索引名
                     ON 表名 (字段名[(长度)]  [ASC |DESC]) ;
#方法三：ALTER TABLE在已存在的表上创建索引
        ALTER TABLE 表名 ADD  [UNIQUE | FULLTEXT | SPATIAL ] INDEX
                             索引名 (字段名[(长度)]  [ASC |DESC]) ;
                            
                            
#删除索引：DROP INDEX 索引名 ON 表名字;
#删除主键索引: ALTER TABLE 表名 DROP PRIMARY KEY;
#显示索引信息: SHOW INDEX FROM student;
*/
 
/*增加全文索引*/
ALTER TABLE `school`.`student` ADD FULLTEXT INDEX `studentname` (`StudentName`);
 
/*EXPLAIN : 分析SQL语句执行性能*/
EXPLAIN SELECT * FROM student WHERE studentno='1000';
 
/*使用全文索引*/
-- 全文搜索通过 MATCH() 函数完成。
-- 搜索字符串作为 against() 的参数被给定。搜索以忽略字母大小写的方式执行。对于表中的每个记录行，MATCH() 返回一个相关性值。即，在搜索字符串与记录行在 MATCH() 列表中指定的列的文本之间的相似性尺度。
EXPLAIN SELECT *FROM student WHERE MATCH(studentname) AGAINST('love');
 
/*
开始之前，先说一下全文索引的版本、存储引擎、数据类型的支持情况
MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引；
MySQL 5.6 及以后的版本，MyISAM 和 InnoDB 存储引擎均支持全文索引;
只有字段的数据类型为 char、varchar、text 及其系列才可以建全文索引。
测试或使用全文索引时，要先看一下自己的 MySQL 版本、存储引擎和数据类型是否支持全文索引。
*/
```

