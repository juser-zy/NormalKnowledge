>主要解决pojo类中的属性名字段和数据库中列名不一致问题

==ResultMap技术==

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.zyy.dao.UserMapper">

    <resultMap id="userMap" type="com.zyy.pojo.User">
        <!--  column数据库列名    property实体类中的属性名   -->
        <result column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="pwd" property="password"/>
    </resultMap>

    <select id="getUserById" parameterType="int" resultMap="userMap">
        select id, name, pwd from `user` where id = #{id} limit 1
    </select>

</mapper>
```

- `resultMap` 元素是 MyBatis 中最重要最强大的元素
- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。
- `ResultMap` 的优秀之处——你完全可以不用显式地配置它们。
