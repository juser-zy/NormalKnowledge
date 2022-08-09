```bash
-- 使用set语句来改变自动提交模式
SET autocommit = 0;   /*关闭*/
SET autocommit = 1;   /*开启*/
 
-- 注意:
---  1.MySQL中默认是自动提交
---  2.使用事务时应先关闭自动提交
 
-- 开始一个事务,标记事务的起始点
START TRANSACTION  
 
-- 提交一个事务给数据库
COMMIT
 
-- 将事务回滚,数据回到本次事务的初始状态
ROLLBACK
 
-- 还原MySQL数据库的自动提交
SET autocommit =1;
 
-- 保存点
SAVEPOINT 保存点名称 -- 设置一个事务保存点
ROLLBACK TO SAVEPOINT 保存点名称 -- 回滚到保存点
RELEASE SAVEPOINT 保存点名称 -- 删除保存点

/*
课堂测试题目
A在线买一款价格为500元商品,网上银行转账.
A的银行卡余额为2000,然后给商家B支付500.
商家B一开始的银行卡余额为10000
创建数据库shop和创建表account并插入2条数据
*/
 
CREATE DATABASE `shop`CHARACTER SET utf8 COLLATE utf8_general_ci;
USE `shop`;
 
CREATE TABLE `account` (
  `id` INT(11) NOT NULL AUTO_INCREMENT,
  `name` VARCHAR(32) NOT NULL,
  `cash` DECIMAL(9,2) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
 
INSERT INTO account (`name`,`cash`)
VALUES('A',2000.00),('B',10000.00)
 
-- 转账实现
SET autocommit = 0; -- 关闭自动提交
START TRANSACTION;  -- 开始一个事务,标记事务的起始点
UPDATE account SET cash=cash-500 WHERE `name`='A';
UPDATE account SET cash=cash+500 WHERE `name`='B';
COMMIT; -- 提交事务
# rollback;
SET autocommit = 1; -- 恢复自动提交
```