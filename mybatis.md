﻿# MyBatis

## mybatis 의 목적

1. 자바 소스에서 SQL 을 분리

2. JDBC 프로그래밍을 캡슐화
	- <mybatis 객체> SqlSession
````java
SqlSession sqlSession = sqlSessionFactory.openSession();
sqlSession.update("spms.dao.ProjectDAO.update", project);
sqlSession.commit();
````
따라서 ~Mapper.xml 에만 집중하면 된다.

## mybatis 구동 원리
예) 프로젝트 상세 정보 가져오기
1. mybatis 의 SqlSession 객체를 준비한다. SELECT 실행을 요청한다.

2. SQL 맵퍼 파일에서 아이디가 "spms.dao.ProjectDAO.update" 인 SQL 문을 찾는다.

3. mybatis는 그 SQL 문을 찾아서 JDBC 드라이버를 사용하여 SQL 문을 실행한다.

4. JDBC에서 리턴한 값을 가지고 값 객체(=value object)를 생성한다.

5. 준비한 값 객체(=VO)를 리턴한다.

## mybatis 시작
[Official tutorial reference](http://www.mybatis.org/mybatis-3/ko/getting-started.html)

1. 모든 마이바티스 애플리케이션은 SqlSessionFactory 인스턴스를 사용한다.
SqlSessionFactory인스턴스는 SqlSessionFactoryBuilder를 사용하여 만들수 있다.
SqlSessionFactoryBuilder는 XML설정파일에서 SqlSessionFactory인스턴스를 빌드할 수 있다.

2. XML파일에서 SqlSessionFactory인스턴스를 빌드하는 것은 매우 간단한다.
설정을 위해 클래스패스 자원을 사용하는 것을 추천하나
파일 경로나 file:// URL로부터 만들어진 InputStream인스턴스를 사용할 수도 있다.
마이바티스는 클래스패스와 다른 위치에서 자원을 로드하는 것으로
좀더 쉽게 해주는 Resources 라는 유틸성 클래스를 가지고 있다.

String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);


## mybatis dept 테이블과 emp 테이블 join 하기
````xml
<mapper namespace="Dept">
	<resultMap id="deptMap" type="Dept">
		<id 		column="deptno" 	property="deptno" />
		<result column="dname" 	property="dname"/>
		<result column="loc" 		property="loc"/>
		
		<collection property="empList" javaType="java.util.List" ofType="Emp">
			<id 		column="empno" 		property="empno"/>
			<result 	column="comm" 		property="comm"/>
			<result 	column="ename" 		property="ename"/>
			<result 	column="hiredate" 	property="hiredate"/>
			<result 	column="job" 			property="job"/>
			<result 	column="mgr" 			property="mgr"/>
			<result 	column="sal" 			property="sal"/>
		</collection>
	</resultMap>

	<select id="selectAll" resultMap="deptMap">
		SELECT D.DEPTNO AS DEPTNO, DNAME, LOC, COMM, EMPNO, ENAME, HIREDATE, JOB, MGR, SAL
		FROM DEPT D, EMP E
		WHERE D.DEPTNO = E.DEPTNO
	</select>
</mapper>
````


## mybatis 에서 JNDI 활용하기 
````xml
<environments default="development">
	<environment id="development">
		<transactionManager type="JDBC" />
		<dataSource type="JNDI">
			<property name="data_source" value="java:comp/env/jdbc/mariadb" />	
		</dataSource>
	</environment>
</environments>
````

mybatis-config.xml 에서 윗부분을 추가

````xml
<property name="data_source" value="java:comp/env/jdbc/mariadb" />
````

이 부분에서 jdbc/mariadb 부분을

````xml
<Resource auth="Container" driverClassName="org.mariadb.jdbc.Driver"
maxIdle="10" maxTotal="20" maxWaitMillis="-1" name="jdbc/mariadb"
password="" type="javax.sql.DataSource" url="jdbc:mariadb://localhost:3306/mvc"
username="root" />
````

server.xml 의 윗 부분의 name 속성값에 입력해준다.