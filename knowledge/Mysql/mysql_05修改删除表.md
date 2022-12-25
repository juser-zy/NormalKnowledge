>修改

- 修改表名 ALTER TABLE 旧表名 RENAME AS 新表名;
	- ALTER TABLE `student` RENAME AS `teacher`;
- 增加表的字段 ALTER TABLE 表名 ADD 字段名 列属性
- 修改表的字段（重命名change，修改约束modify）
- 删除表的字段
	-  ALTER TABLE `student` DROP age;


>删除

DROP TABLE IF EXISTS `teacher`;