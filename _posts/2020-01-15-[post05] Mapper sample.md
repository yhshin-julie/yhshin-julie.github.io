---
title: "[post05] Mapper sample"
date: 2020-01-15 16:00:00 -0400
categories: dev-spring
toc: true
toc_sticky: true
---

### SqlMap

## context-mybatis.xml
- datasource 관련된 내용과 sqlSessionFactory, sqlSessionTemplate 에 관한 설정을 한다.
- 실제 쿼리가 담겨있는 xml파일의 위치와 typeAlias 로 설정된 model 클래스 정보가 들어있다.

```xml
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="configLocation" value="classpath:/META-INF/sqlmap/sql-mapper-config.xml"/>
      ①
      <property name="mapperLocation">
                <list>
                       <value> classpath*:/META-INF/sqlmap/mapper/**/*.xml</value>
                </list> 
      </property>
      
      <property name="dataSource" ref="dataSource"/>
      
      <property name="plugins">
                <list>
                       <bean class="yhshin.framework.mybatis.MybatisRowboundsInterceptor">
                             <property name="dbType" value="oracle" />
                       </bean>
                </list>
      </property>
</bean>
```

1) 쿼리 매핑을 하기 위 XML파일을 검색하여 해서 자동으로 추가하도록 설정하는 역할.

## sql-mapper-config.xml

```xml
<configuration>
  <settings>
    <setting name="jdbcTypeForNull" value="VARCHAR"/>
    <setting name="mapUnderscoreToCamelCase" value="true" />
    <setting name="cacheEnabled" value="true" />
    <setting name="defaultExecutorType" value="REUSE" />
    <setting name="callSettersOnNulls" value="true" />
  </settings>
  
  ①
  <typeAliases>
     <typeAlias alias="egovMap" type="egovframework.rte.psl.dataaccess.util.EgovMap"/>
     <typeAlias alias="yhshinMap" type="yhshin.framework.mybatis.YhsinMap"/>
  </typeAliases>
</configuration>
```

1) resultType으로 사용하기 위한 Map의 alias 지정 작업.

# 선언부

```xml
<?xml sersion="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

①
<mapper namespace="yhshin.julie.post.projectEx.PgManateMapper">
```

1) namespace 선언 : 프로젝트 내의 명명규칙에 따라 구성한다.

## select 문

```xml
<select ①id="selectPgList"  ②parameterType="Map"  ③resultType="yhshinMap" >
        SELECT * 
          FROM DUAL
</select> 
```

1) queryId는 Service에서 호출하는 명칭과 동일하게 구성되어야 한다.   
2) parameterType은 Map 으로 사용 : Service단에서 List 형태를 for문을 사용해서 Map형태로 꺼냈기 때문에.  
3) resultType은 EgovMap을 상속받은 YhshinMap을 사용(sql-mapper-config.xml에 typeAlias로 지정해야 가능함)  


## insert/update/delete 문 
```xml
<insert id="insertPgManage" parameterType="java.util.Map">
       INSERT 
         INTO TABLE_NAME 
         (  COL1
          , COL2
          , COL3
         )
         VALUES
         (  #{col1}
          , #{col2}
          , #{col3}
         )
</insert>
```
```xml
<update id="updatePgManage" parameterType="java.util.Map">
        UPDATE TABLE_NAME
           SET COL1 = #{col1}
              ,COL2 = #{col2}
         WHERE COL3 = #{col3}
</update>
```
```xml
<delete id="deletePgManage" parameterType="java.util.Map">
        DELETE 
          FROM TABLE_NAME
</delete>
```

## Sql Batch 처리(insert/update/delete Batch 문)
```xml
<insert id="insertPgBatch" parameterType="java.util.List">
   ①<foreach collection="List" item="item" index="index" separator=" " open="INSERT ALL" close="SELECT * FROM DUAL">
        INTO TABEL_NAME
        (  COL1
          , COL2
          , COL3
         )
         VALUES
         (  #{item.col1}
          , #{item.col2}
          , #{item.col3}
         )
     </foreach>
</insert>
```

1) foreach 문 구성 
  - collection : Service 단에서 전달받은 인자(List OR Array 형태만 가능)
  - item : 전달받은 인자 값을 alias 명 지정
  - index : 반복되는 구문 번호(0부터 오름차순으로 증가)
  - open : 해당 구문이 시작 될 때 삽입 할 문자열
  - close : 해당 구문이 종료될 때 삽일 할 문자열
  - separator : 반복되는 사이에 출력 할 문자열

## if 제어문
if test="조건" 문이 true 이면 태그 안의 쿼리문이 실행된다.  
  
```xml
<select id="selectMyPgList" parameterType="Map" resultType="yhshinMap">
         SELECT A.PROGRAM_ID    /*프로그램ID*/
              , A.PROGRAM_NM    /*프로그램NM*/
           FROM T_TABLE_AAA A
              , T_TABLE_BBB B
          WHERE A.PROGRAM_ID = B.PROGRAM_ID
            
            AND A.PROGRAM_SE = '01'
            AND A.PROGRAM_CODE = #{programCode}
            
            AND B.PROGRAM_NO IN ('001', '002')
            
            <if test="programNm != '' and programNm != null">
                AND A.PROGRAM_NM LIKE '%'||#{programNm}||'%' 
            </if>
</select>
```

## choose 제어문 
  - when 태그와 otherwise 태그를 제공
  - when 태그 : <when 변수명 == "조건"> 이 true일때 실행
   (해당 조건의 쿼리문이 실행되면, 다른 <choose> 문이나 <otherwise>문은 실행되지 않음)
  - otherwise 태그 : 모든 <when> 태그의 조건이 만족하지 않을때 실행 

```xml  
<select id="selectMyPgList parameterType="Map" resultType="java.lang.String">
        SELECT LPAD(MAX(PROGRAM_ID) + #{programCnt}, 6, '0') AS PROGRAM_ID
          FROM T_TABLE_AAA
         WHERE PROGRAM_CODE = #{programCode}
         
         <choose>
                  <when test="programCode != null and programCode == '01'
                         AND PROGRAM_ID = '1'
                  </when>
                  <when test="programCode != null and programCode == '02'
                         AND PROGRAM_ID = '2'
                  </when>
                  <when test="programCode != null and programCode == '03'
                         AND PROGRAM_ID = '3'
                  </when>
                  <otherwise>
                         AND PROGRAM_ID = '99'
                  </otherwise>
         </choose>
</select>
```

## CLOB/BLOB 데이터 처리
- SELECT 문에서는 프로젝트 내의 Map에서 Byte[]형태로 자동 변경되었음._(어떤 방법인지 알아봐야겠다.)_
- INSERT, UPDATE문에서는 JdbcType을 설정해주어야 한다.

`#{clobDesc:CLOB}` 또는 `#{blobDesc:BLOB}`
