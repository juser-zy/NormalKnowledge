>dao层的接口完善好

**UserMapper.java**
```java
package com.jzy.dao;  
  
import com.jzy.pojo.User;  
  
import java.util.List;  
import java.util.Map;  
  
public interface UserMapper {  
    //查询全部用户  
    List<User> getUserList();  
  
    //根据ID查询  
    User getUserById(int id);  
  
    //insert  
    int addUser(User user);  
  
    //万能的map  
    int addUser2(Map<String,Object> map);  
  
    int updateUser(User user);  
  
    int deleteUser(int id);  
}
```

>UserMapper对应接口完善sql语句

**UserMapper.xml**
```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper  
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">  
  
<!--namespace绑定一个对应的Dao/Mapper接口  
select id,name,pwd from mybatis.user where id = #{id}  
-->  
<mapper namespace="com.jzy.dao.UserMapper">  
  
    <select id="getUserList" resultType="com.jzy.pojo.User">  
        select * from mybatis.user    </select>  
  
    <select id="getUserById" resultType="com.jzy.pojo.User" parameterType="int">  
        select * from mybatis.user where id = #{id}    </select>  
  
    <insert id="addUser" parameterType="com.jzy.pojo.User">  
        insert into mybatis.user (id,name,pwd) values (#{id},#{name},#{pwd})    </insert>  
  
    <insert id="addUser2" parameterType="map">  
        insert into mybatis.user (id,name,pwd) values (#{userId},#{userName},#{userPwd})    </insert>  
  
    <update id="updateUser" parameterType="com.jzy.pojo.User">  
        update mybatis.user set name=#{name} , pwd=#{pwd} where id=#{id}    </update>  
  
    <delete id="deleteUser" parameterType="int">  
        delete from mybatis.user where id = #{id}    </delete>  
  
</mapper>
```

**UserDaoTest.java**
```java
package com.jzy.dao;  
import com.jzy.pojo.User;  
import com.jzy.utils.MybatisUtils;  
import org.apache.ibatis.session.SqlSession;  
import org.junit.Test;  
  
import java.util.HashMap;  
import java.util.List;  
  
/**  
 * Created with IntelliJ IDEA. * User: JWhale * Date: 2022/8/28 * Time: 下午 10:22  
 * Description: */public class UserDaoTest {  
    @Test  
    public void getUserList() {  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        List<User> userList = userMapper.getUserList();  
        for (User user:userList) {  
            System.out.println(user);  
        }  
        sqlSession.close();  
    }  
  
    @Test  
    public void getUserById(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);  
        User user = userMapper.getUserById(1);  
        System.out.println(user);  
        sqlSession.close();  
    }  
  
    @Test  
    public void addUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int num = mapper.addUser(new User(4, "dsad", "654321"));  
        if(num > 0){  
            System.out.println("插入成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void updateUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int res = mapper.updateUser(new User(4, "金", "123456"));  
        if(res > 0) {  
            System.out.println("修改成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void deleteUser(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        int res = mapper.deleteUser(4);  
        if(res > 0){  
            System.out.println("删除成功");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
    }  
  
    @Test  
    public void addUser2(){  
        SqlSession sqlSession = MybatisUtils.getSqlSession();  
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);  
        HashMap<String, Object> map = new HashMap<>();  
        map.put("userId",5);  
        map.put("userName","hello");  
        map.put("userPwd","123456321");  
        int res = mapper.addUser2(map);  
        if(res > 0){  
            System.out.println("success");  
        }  
        sqlSession.commit();  
        sqlSession.close();  
  
    }  
}
```