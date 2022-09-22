![[Pasted image 20220808173508.png]]

```bash
-- 查询参加了考试的同学
SELECT s.studentno,`studentname`,`subjectno`,`studentresult` 
FROM student AS s
INNER JOIN result AS r
ON s.studentno = r.studentno


-- 查询缺考的
SELECT s.studentno,`studentname`,`subjectno`,`studentresult` 
FROM student AS s
LEFT JOIN result AS r
ON s.studentno = r.studentno
WHERE studentresult IS NULL
```

>**自连接**

- 核心：一张表变成两张一样的表
```bash
-- 查询父类对应的子类信息
SELECT c1.categoryname AS 父类,c2.categoryname AS 子类
FROM category AS c1,category AS c2
WHERE c1.categoryid = c2.pid

```
