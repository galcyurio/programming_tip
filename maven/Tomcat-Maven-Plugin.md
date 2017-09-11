# Tomcat maven plugin guide

1. Tomcat 의 /conf/tomcat-users.xml 파일에 아래처럼 추가하고 gui 로 하고 싶으면 <user> 태그에서 roles 속성을 manager-gui 로 바꾸어주면 된다.

````xml
<tomcat-users>
	<role rolename="manager-script" />
	<role rolename="manager-gui" />
	<role rolename="manager-jmx" />
	<role rolename="manager-status" />
	<user username="abcd" password="1234" roles="manager-script"/>
</tomcat-users>
````


2. Maven plugin 을 pom.xml 에 추가
````xml
<plugin>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<groupId>org.apache.tomcat.maven</groupId>
	<version>2.2</version>
	<configuration>
		<path>/board</path>
		<url>http://localhost:8080/manager/text</url>
		<username>abcd</username>
		<password>1234</password>
	</configuration>
</plugin>
````

2.2. 소스상에 비밀번호를 남기고 싶지 않은 경우

````xml
<!-- username 과 password 는 Maven 의 conf/settins.xml 에서 를 입력 -->
<servers>
	<server>
		<id>serverName</id>
		<username>abcd</username>
		<password>1234</password>
	</server>
</servers>

<!-- pom.xml 수정 -->
<plugin>
	<artifactId>tomcat7-maven-plugin</artifactId>
	<groupId>org.apache.tomcat.maven</groupId>
	<version>2.2</version>
	<configuration>
		<path>/board</path>
		<url>http://localhost:8080/manager/text</url>
		<server>serverName</server>
	</configuration>
</plugin>
````

3. 웹어플리케이션을 Tomcat 의 webapps 밖에 설정하고 context 에 해당 path를 지정한다. myApp.war 를 webapps 와 같은 레벨의 위치에 복사

````xml
<!-- Tomcat의 conf/server.xml 수정 -->
<Host name="localhost" appBase="webapps" unpackWARs="true" autoDeploy="false" deployOnStartup="false" >
	<Context path="" docBase="${catalina.home}/myApp" reloadable="false" > </Context>
</Host>
````

3.1. 쉬운 방법으로 myApp.war 를 ROOT.war 로 변경하여 deploy 한다. maven 을 쓸 경우 다음과 같이 finalName 을 설정해서 빌드하거나 서버에 올릴때 ROOT.war 로 변경한다.
````xml
<build>
		<!-- 기본 finalName
		<finalName>${artifactId}-${version}</finalName> -->
		<finalName>ROOT.war</finalName>
</build>
````

4. tomcat/webapps/manager/META-INF/context.xml 에서 해당되는 ip를 풀어주어야 한다.
Replace allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" with allow=".*

````xml
<Context antiResourceLocking="false" privileged="true" >
<Valve className="org.apache.catalina.valves.RemoteAddrValve"
 allow=".*" />
</Context>
````

5. 이후 tomcat7:redeploy 를 해주면 된다.