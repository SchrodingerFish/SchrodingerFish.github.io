---
title: MyBatis 查询常用语法
date: 2025-11-08 14:20:11
categories:
    - Java
tags:
    - Java
    - MyBatis
    - SQL
---
# MyBatis 查询常用语法

## 1. 动态SQL

### if 条件判断
```xml
<select id="getUserList" resultType="User">
    SELECT * FROM user
    WHERE 1=1
    <if test="name != null and name != ''">
        AND name LIKE CONCAT('%', #{name}, '%')
    </if>
    <if test="age != null">
        AND age = #{age}
    </if>
</select>
```

### choose (when/otherwise)
```xml
<select id="getUserByCondition" resultType="User">
    SELECT * FROM user
    WHERE
    <choose>
        <when test="name != null and name != ''">
            name LIKE CONCAT('%', #{name}, '%')
        </when>
        <when test="email != null and email != ''">
            email = #{email}
        </when>
        <otherwise>
            status = 1
        </otherwise>
    </choose>
</select>
```

### trim (where/set)
```xml
<!-- trim 替代 where -->
<select id="getUserList" resultType="User">
    SELECT * FROM user
    <trim prefix="WHERE" prefixOverrides="AND |OR ">
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </trim>
</select>

<!-- trim 替代 set -->
<update id="updateUser">
    UPDATE user
    <trim prefix="SET" suffixOverrides=",">
        <if test="name != null">name = #{name},</if>
        <if test="age != null">age = #{age},</if>
        <if test="email != null">email = #{email},</if>
    </trim>
    WHERE id = #{id}
</update>
```

### where 和 set 标签
```xml
<!-- where 标签自动处理 AND/OR -->
<select id="getUserList" resultType="User">
    SELECT * FROM user
    <where>
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </where>
</select>

<!-- set 标签自动处理逗号 -->
<update id="updateUser">
    UPDATE user
    <set>
        <if test="name != null">name = #{name},</if>
        <if test="age != null">age = #{age},</if>
        <if test="email != null">email = #{email},</if>
    </set>
    WHERE id = #{id}
</update>
```

## 2. 循环操作

### foreach 遍历集合
```xml
<!-- IN 查询 -->
<select id="getUserByIds" resultType="User">
    SELECT * FROM user
    WHERE id IN
    <foreach item="id" collection="list" open="(" separator="," close=")">
        #{id}
    </foreach>
</select>

<!-- 批量插入 -->
<insert id="batchInsertUser">
    INSERT INTO user (name, age, email) VALUES
    <foreach collection="list" item="user" separator=",">
        (#{user.name}, #{user.age}, #{user.email})
    </foreach>
</foreach>

<!-- 批量更新 -->
<update id="batchUpdateUser">
    <foreach collection="list" item="user" separator=";">
        UPDATE user SET 
        name = #{user.name}, age = #{user.age}
        WHERE id = #{user.id}
    </foreach>
</update>
```

## 3. 复杂关联查询

### 一对一关联
```xml
<!-- 使用 association -->
<resultMap id="userDetailMap" type="User">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="age" column="age"/>
    <!-- 一对一关联 -->
    <association property="profile" javaType="UserProfile">
        <id property="id" column="profile_id"/>
        <result property="avatar" column="avatar"/>
        <result property="bio" column="bio"/>
    </association>
</resultMap>

<select id="getUserWithProfile" resultMap="userDetailMap">
    SELECT u.*, p.id as profile_id, p.avatar, p.bio
    FROM user u
    LEFT JOIN user_profile p ON u.id = p.user_id
    WHERE u.id = #{id}
</select>
```

### 一对多关联
```xml
<!-- 使用 collection -->
<resultMap id="userWithOrdersMap" type="User">
    <id property="id" column="id"/>
    <result property="name" column="name"/>
    <result property="age" column="age"/>
    <!-- 一对多关联 -->
    <collection property="orders" ofType="Order">
        <id property="id" column="order_id"/>
        <result property="orderNo" column="order_no"/>
        <result property="amount" column="amount"/>
        <result property="createTime" column="create_time"/>
    </collection>
</resultMap>

<select id="getUserWithOrders" resultMap="userWithOrdersMap">
    SELECT u.*, o.id as order_id, o.order_no, o.amount, o.create_time
    FROM user u
    LEFT JOIN `order` o ON u.id = o.user_id
    WHERE u.id = #{id}
</select>
```

## 4. 多表联查

### JOIN 查询
```xml
<select id="getUserOrderInfo" resultType="map">
    SELECT 
        u.name as userName,
        u.email,
        o.order_no,
        o.amount,
        o.create_time
    FROM user u
    INNER JOIN `order` o ON u.id = o.user_id
    <where>
        <if test="userName != null and userName != ''">
            AND u.name LIKE CONCAT('%', #{userName}, '%')
        </if>
        <if test="startDate != null">
            AND o.create_time >= #{startDate}
        </if>
    </where>
    ORDER BY o.create_time DESC
</select>
```

## 5. 子查询

```xml
<select id="getUserWithOrderCount" resultType="UserVO">
    SELECT 
        u.*,
        (SELECT COUNT(*) FROM `order` o WHERE o.user_id = u.id) as orderCount
    FROM user u
    <where>
        <if test="hasOrder != null and hasOrder == true">
            AND EXISTS (SELECT 1 FROM `order` o WHERE o.user_id = u.id)
        </if>
    </where>
</select>
```

## 6. 分页查询

### 简单分页
```xml
<select id="getUserListWithPage" resultType="User">
    SELECT * FROM user
    <where>
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
    </where>
    ORDER BY id DESC
    LIMIT #{offset}, #{limit}
</select>
```

### 使用 PageHelper
```xml
<select id="getUserList" resultType="User">
    SELECT * FROM user
    <where>
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
    </where>
    ORDER BY id DESC
</select>
```

## 7. 聚合查询

```xml
<select id="getUserStatistics" resultType="map">
    SELECT 
        COUNT(*) as total,
        COUNT(CASE WHEN age >= 18 THEN 1 END) as adultCount,
        AVG(age) as avgAge,
        MAX(age) as maxAge,
        MIN(age) as minAge
    FROM user
    <where>
        <if test="status != null">
            AND status = #{status}
        </if>
    </where>
</select>

<!-- 分组查询 -->
<select id="getUserCountByStatus" resultType="map">
    SELECT 
        status,
        COUNT(*) as count
    FROM user
    GROUP BY status
    HAVING COUNT(*) > #{minCount}
</select>
```

## 8. 常用技巧

### 字符串拼接
```xml
<select id="searchUser" resultType="User">
    SELECT * FROM user
    WHERE CONCAT(name, '-', email) LIKE CONCAT('%', #{keyword}, '%')
</select>
```

### 日期处理
```xml
<select id="getUserByDateRange" resultType="User">
    SELECT * FROM user
    WHERE create_time BETWEEN #{startDate} AND #{endDate}
    <!-- 或者使用 DATE 函数 -->
    AND DATE(create_time) = DATE(#{specificDate})
</select>
```

### 排序动态化
```xml
<select id="getUserList" resultType="User">
    SELECT * FROM user
    <where>
        <if test="name != null and name != ''">
            AND name LIKE CONCAT('%', #{name}, '%')
        </if>
    </where>
    ORDER BY 
    <choose>
        <when test="orderBy == 'name'">name</when>
        <when test="orderBy == 'age'">age</when>
        <when test="orderBy == 'createTime'">create_time</when>
        <otherwise>id</otherwise>
    </choose>
    <choose>
        <when test="orderType == 'desc'">DESC</when>
        <otherwise>ASC</otherwise>
    </choose>
</select>
```

这些语法可以帮助你处理各种复杂的查询场景，提高 SQL 的灵活性和可维护性。