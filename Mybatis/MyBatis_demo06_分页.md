### 分页

#### 使用limit分页

```sql
-- 语法
select * from `user` limit startIndex,pageSize;
select * from `user` limit 2;
-- 相当于
select * from `user` limit 0,2;
```

使用mybatis实现分页，核心sql

1. 接口

```java
   /**
	* 分页查询
	* @param map
	* @return
	*/
   List<User> getUserByLimit(Map<String, Integer> map);
```

   

2. Mapper.xml

```xml
   <select id="getUserByLimit" parameterType="map" resultMap="userMap">
	   select id, name, pwd from `user` limit #{startIndex},#{pageSize}
   </select>
```

   

3. 测试

```java
   @Test
   public void getUserByLimit() {

	   SqlSession sqlSession = null;
	   try {
		   sqlSession = MybatisUtils.getSqlSession();
		   UserMapper mapper = sqlSession.getMapper(UserMapper.class);

		   Map<String, Integer> map = new HashMap<>(16);
		   map.put("startIndex", 0);
		   map.put("pageSize", 2);
		   List<User> userList = mapper.getUserByLimit(map);
		   for (User user : userList) {
			   logger.info(user);
		   }
	   } finally {
		   if (sqlSession != null) {
			   sqlSession.close();
		   }
	   }

   }
```

   

#### 分页插件

![image-20220326144801005](https://typora-picture1234.oss-cn-shenzhen.aliyuncs.com/typora/img/image-20220326144801005.png)

官方文档：https://pagehelper.github.io/


