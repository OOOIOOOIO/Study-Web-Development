# DBCP & JNDI
> &nbsp;JDBC, DBCP, JNDI 모두 JAVA에서 DB와 커넥션을 하기 위해 사용하는 방법이다. 그리고 모두 ojdbc6.jar파일이 필요하다.
 DBCP와 JNDI에 대해 설명하기 전에 Connection과 Connection pool 그리고 Datasource에 대해 먼저 알아보자.
 [JDBC 사용법](https://github.com/OOOIOOOIO/Database_JDBC)
<br>

## Connection과 Connection pool
- JDBC에서 사용한 DriverManager.getConnection(url, user, password)은 실제 자바프로그램과 데이터베이스를 네트워크상에서 연결해주는 메소드이며
Connection의 객체인 conn에 연결해주어 자바 프로그램과 DB 사이를 연결한다.
- Connection은 네트워크상의 연결 자체를 의미한다.(자바프로그램과 DB사이의 길이라고 생각하면 편하다.)
> 보통 Connection 하나 당 프랜잭션 하나를 관리한다. <br>
> 트랜잭션은 하나 이상의 쿼리에서 동일한 Connection 객체를 공유하는 것을 의미한다.
- JDBC framework에서 Close가 이루어지면 Connection을 Connection Pool에 반납하게 된다. 
> Connection Pool이란 클라이언트의 요청 시점에 Connection을 연결하는 것이 아니라 미리 일정 수의 Connection을 만들어 놓고 
필요한 어플리케이션에 전달하여 이용하는 방법이다.<br>
> Application이란 JSP, Servlet, PHP 등을 말한다.

![Connection Pool](https://linked2ev.github.io/assets/img/devlog/201908/cp-s1.png)

## DataSource
- javax.sql.DataSource라는 인터페이스는 Connection Pool을 관리하는 목적으로 사용되며 DataSource ds 객체를 사용한다.
- Application에서는 이 DataSource 인터페이스를 통해 Connection을 얻어오고 반납하는 등의 작업을 구현한다. 즉, Connection pool을
어플리케이션단에서 어떻게 관리할지를 구현하는 인터페이스라고 할 수 있다.

</hr>


## DBCP(DataBase Connection Pool)
- JDBC 방식은 DB에서 정보를 가져올 때마다 매번 연결을 열고 닫기 때문에 비효율적이기에 DBCP같은 Pool 방식을 일반적으로 사용한다. 
-	사용자의 요청이 있을 때마다 DB연결을 한다면 코드가 복잡해지며 많은 요청이 있을 때 연결속도가 저하될 수 있다. 
-	따라서 미리 Connection을 만들어 두고 필요시 저장된 공간에서 가져다 쓰고 반납하는 기법이다.
- DB 커넥션 풀을 어플리케이션 소스단에 설정해 놓는 방식이다.

![DBCP와 JNDI 구성](https://t1.daumcdn.net/cfile/tistory/224CD845582D373205)

## JNDI(Java Naming and Directory Interface)
-	디렉터리 서비스에서 제공하는 데이터 및 객체를 발견하고 참고하기 위한 자바 API이며 외부에 있는 객체를 가져오기 위한 기술이다. 
WAS단에서 데이터베이스 커넥션 객체를 미리 네이밍 해두는 방식이다.

### 특징
- DB 커넥션을 WAS 단에서 제어하면서 서버에서는 하나의 Connection Pool을 가지며 이를 공유객체를 사용한다고 생각할 수 있다.
- 장점
   - DB 정보를 파악하기 쉽다. WAS 단에서 DB가 몇 개 붙어있는지 정보를 파악하기 수월하다.
   - DB Connection Pool을 효율적으로 사용할 수 있다. WAS 단에서 DB Pool을 하나로 관리하면 스태틱 객체를 생성해
    쉽게 가져다 쓸 수 있기 때문에 효율이 좋아진다고 한다.


### 동작과정 
1. 사용자 요청
2. JNDI에 등록된 DB객체 검색
3. 찾은 객체에서 커넥션을 획득
4. DB작업 종료 후 커넥션 반납

</hr>

## DBCP와 JNDI 설정
1. Servers 있는 context.xml에 <Resource> </Resource>를 열어 알맞은 속성들을 등록한다.
2. context.xml 파일을 복사해 META-INF에 붙여넣는다.
3. WEB-INF 안에 있는web.xml 파일에 <resource-ref> </resource-ref>안에 contet.xml에 맞춰 등록해준다.

### context.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
--><!-- The contents of this file will be loaded for each web application --><Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    <Resource
    	name="jdbc/oracle"
    	auth="Container"
    	type="javax.sql.DataSource"
    	driverClassName="oracle.jdbc.OracleDriver"
    	url="jdbc:oracle:thin:@localhost:1521:XE"
    	username="~~~"
    	password="~~~"
    	maxActive="20"
    	maxIdle="20"
    	maxWait="-1"
    />
  <!-- 
  		 서버가 시작 되면서 context.xml에 들려 참고해 만들 객체가 있는지 찾는다.
    	<Resource></Resource>는 우린 이런 자원이 필요해 시작되면 만들어줘라는 뜻이다.
    	
    	< 이 전체가 JNDI >
    	<DBCP 관련>
    	name 	: dbcp를 이용하기 위한 key값
    	type 	: 해당 Resource의 return type이다.
    	maxActive : 연결 최대 혀용 개수
    	maxIdle : 항상 연결상태를 유지하는 개수 (보편적으로 maxActive와 maxIdle의 개수는 같게 해준다.)
    	maxWait : 커넥션 풀에 연결 가능한 커넥션이 없을 경우 대기하는 시간(정해주지 않으면 응답 올 때까지 대기, -1은 바로 실패)
		  	
    	<JDBC 관련>
    	driverClassName
    	url
    	username
    	password
     -->
</Context>
```

### web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" id="WebApp_ID" version="4.0">
  <display-name>day05</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  <!-- context.xml에 써준 것과 동일하게 써주면 된다. JNDI 리소스 사용 설정하는 부분이다. -->
  
  <resource-ref>
  	<description>Connection</description>
  	<res-ref-name>jdbc/oracle</res-ref-name>
  	<res-type>javax.sql.DataSource</res-type>
  	<res-auth>Container</res-auth>
  </resource-ref>
  
</web-app>
```

## 사용
```java
package com.it.dao;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;

import com.it.dto.UserDTO; 	

public class UserDAO { // DAO는 DB처리를 하는 공간
	Context context; // context.xml의 객체. context.xml에서 JDBC DB연결에 필요한 사전 설정을 해준다. url, username, password, Class.forName
	DataSource ds; // DataSource를 연결할 객체. context.xml의 type이다. JNDI를 사용하는 이유는
	
	Connection conn; // 다리
	PreparedStatement ps; // 택배차
	ResultSet rs; // 저장소 
	
	public boolean join(UserDTO newUser) { // UserDTO 클래스 import
		int result = 0;
		//DB처리
		try {
			context = new InitialContext(null); // context.xml을 읽기 위한 객체(NamingException)
			// lookup(context.xml에 설정한 name을 넘기기) : name에 해당하는 객체를 찾고 해당 DataSource를 가져온다. return type이 Object이기 때문에 DataSource 타입으로 다운캐스팅 해준다.
			ds = (DataSource)context.lookup("java:comp/env/jdbc/oracle"); // 톰캣 서버의 경우 java:comp/env를 붙여준다.
			// ds를 통해 받아온 연결을 conn에 연결해준다 . 커넥션 풀로 부터 커넥션 객체를 얻어내어 conn변수에 저장
			conn = ds.getConnection();
			// 여기부터는 JDBC랑 똑같다
			String sql = "INSERT INTO TEST_USER VALUES(?,?,?)";
			
			ps = conn.prepareStatement(sql);
			ps.setString(1, newUser.getUserid());
			ps.setString(2, newUser.getUserpw());
			ps.setString(3, newUser.getUsername());
			
			result = ps.executeUpdate(); //SQLException
			
		} catch (NamingException ne) {
			System.out.println(ne);
		} catch(SQLException sqle) { // 
			System.out.println(sqle);
		}
		return result == 1;
	}
}
```

</hr>

### 정리를 마치며
> &nbsp;JDBC, DBCP, JNDI 모두 DB와 연결하기 위한 기법이며 ojdbc6.jar파일이 필요하다. buildpath와 WEB-INF lib 둘 다 등록해준다. 
 context.xml에 미리 설정한 maxActive, maxIdle, maxWait 등이 DBCP를 위한 설정이며 driverClassName, url, username, password 등은
 JDBC(DB연결)를 위한 설정이라고 생각하면 편하다. 그리고 name, type, web.xml 등은 JNDI 기법으로 WAS단에서 데이터베이스 커넥션 객체를 미리 네이밍 해두는 방식이라 생각하자..
 context.xml, web.xml 등은 JNDI 기법을 사용하기 위해 미리 네이밍을 한 것이라고 생각하자!
 <br>

### 결국 DB연결을 어떻게 얼마나 편하게 하냐! 인 것 같다. 이후 공부하는 MyBatis는 훨씬 편하다고 한다.
 




