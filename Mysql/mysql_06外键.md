- 代码形式
 ```bash
CREATE Table IF NOT EXISTS `student`(
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT ' 女' COMMENT '性别',
	`birthday` DATETIME DEFAULT NULL COMMENT '出生日期',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	`gradeid` INT(10) NOT NULL COMMENT '学生的年级',
	PRIMARY KEY (`id`),
	KEY `FK_gradeid` (`gradeid`),
	CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET = utf8

```

- 修改表

```bash
ALTER TABLE `student` ADD `gradeid` INT(10) NOT NULL COMMENT '学生的年级';

ALTER TABLE `student` 
ADD CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade` (`gradeid`);
ALTER TABLE 表名 
ADD CONSTRAINT 约束名 FOREIGN KEY (作为外键的列) REFERENCES 哪个表 (哪个列);

```
