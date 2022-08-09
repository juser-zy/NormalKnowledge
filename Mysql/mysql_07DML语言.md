>**INSERT命令添加**


```bash
INSERT INTO `grade` (`gradename`) VALUES ('大四')
-- 主键自增，可以省略

-- 多个插入
INSERT INTO `grade` (`gradename`) VALUES ('大三'),('大二'),('大一')

INSERT INTO `student` (`name`,`pwd`,`sex`) VALUES ('张三','aaaaa','男')
```

>**UPDATE命令修改**


```bash

UPDATE `student` SET `name` = 'jzy' WHERE id = 2

--修改多个属性，逗号隔开
UPDATE `student` SET `name` = 'jzy',email = '1669903129@qq.com' WHERE id = 2

```

操作符   |   意义  | 范围 | 结果
-----| ---|   ---| ---
=| 等于 | 5 = 6 | false
 <>或者！= |不等于 |5<>6|true
    > |
    < |
    betweeen .. and ..|
    AND |
    OR|

>**DELETE命令删除**


```bash

DELETE FROM 表名 WHERE id = 2

```

>**TRUNCATE命令清空**


```bash

TRUNCATE 表名

```

>delete和TRUNCATE区别

- 相同点：都能删除数据
- 不同点：
	- TRUNCATE  重新设置自增列
	- TRUNCATE  不会影响事务