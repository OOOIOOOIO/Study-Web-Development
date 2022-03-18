# MyBatis

## MyBatis란
> &nbsp;객체 지향 언어인 자바의 관계형 데이터베이스 프로그래밍을 좀 더 쉽게 할 수 있게 도와주는 개발 프레임 워크로서 
JDBC를 통해 데이터베이스에 엑세스 하는 작업을 갭슐화하고 일반 SQL 쿼리 저장, 프로시저 및 고급 맵핑을 지원하며
모든 JDBC 코드 및 매개 변수의 중복작업을 제거한다. MyBatis에서는 프로그램에 있는 SQL쿼리들을 한 구성파일에 구성하여
프로그램 코드와 SQL을 분리할 수 있는 장점을 가지고 있다.<br>
&nbsp;기존 JDBC 방식과 달리 SQL문을 XML 파일에 작성함으로써 코드가 줄어들고 SQL문 수정이 편해지며 DBCP 기법을 사용하여 커넥션을 여러개 생성하기 때문에 JDBC만 사용하는 것보다 작업 효율과 가독성이 좋아진다.

<br>

## MyBatis 설치 및 사용

```

설치법!
	blog.mybatis.org --> products --> 3.5.7 zip파일 다운

사용법!
	https://mybatis.org/mybatis-3/ko/getting-started.html

```

<br>

## MyBatis 특징
- MyBatis는 복잡한 쿼리나 다이나믹한 쿼리에 강하지만 비슷한 구조를 가진 쿼리를 남발할 수 있다는 단점이 있다. 
- 프로그램 코드와 SQL 쿼리문의 분리로 코드의 간결성 및 유지보수성 향상된다.
- resultType, resultClass 등 VO를 사용하지 않고 조회 결과를 사용자 정의 DTO, MAP 등으로 맵핑하여 사용할 수 있다.
- 빠른 개발이 가능하여 생산성이 향상된다.
- 다양한 프로그램잉 언어로 구현이 가능하다. ex)Java, C#, .NET, Ruby

<br>

## MyBatis 구조

![MyBatis Database Access 구조](https://images.velog.io/images/changyeonyoo/post/5678a014-ef59-4b22-985c-885f5d81b246/999CFA505BBB65D32C.jpg)

<br>

## MyBatis 데이터 액세스 계층
![MyBatis Database Access 구조](https://images.velog.io/images/changyeonyoo/post/bce0a67f-0493-433f-a728-53cee12c5e51/99BE40445C5D2C5719.png)

<br>

## MyBatis 실행해보기
- 코드를 작성하기 전 Build Path와 WEB-INF -> lib파일에 mybatis.jar와 ojdbc6.jar를 등록해주어야 한다.

<br>

## 필수 xml 파일 경로
- src/com/it/mybatis/config.xml
- src/com/it/mybatis/SqlMapConfig.xml
- src/com/it/mapper/user.xml
- src/com/it/dao/UserDAO.java
- src/com/it/dto/UserDTO.java

<br>

## config.xml : MyBatis DB설정 파일
- 경로
   - src/com/sh/mybatis/config.xml

<br>

```xml
<?xml version="1.0" encoding="UTF-8" ?> 
<!-- config : 설계도 , mybatis 홈페이지에서 복붙해옴. -->
<!DOCTYPE configuration 
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <typeAliases> <!-- user.xml(SQL쿼리문)의 resultType을 보다 쉽게 사용할 수 있게 해준다.-->
  	<typeAlias alias="userdto" type="com.koreait.app.user.dao.UserDTO"/>
  </typeAliases>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">     <!-- JDBC 설정 -->
        <property name="driver" value="oracle.jdbc.driver.OracleDriver"/>  <!-- value="${driver}" -->
        <property name="url" value="jdbc:oracle:thin:@localhost:1521:XE"/> <!-- value="${url}" -->
        <property name="username" value="~~~"/>                            <!-- value="${username}" -->
        <property name="password" value="~~~"/>                            <!-- value="${password}" -->
      </dataSource>
    </environment>
  </environments>
  <mappers> <!--맵퍼 설정 : SQL 구문을 작성하는 곳이 여기입니다~~ 를 알려준다.  -->
    <mapper resource="com/it/mapper/user.xml"/> 
  </mappers>
</configuration>

```

<br>

## SqlMapConfig.java : config.xml을 이용해 DB와 연결
```java
package com.it.mybatis;

import java.io.IOException;
import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class SqlMapConfig {
	private static SqlSessionFactory factory;
	
	// 클래스 초기화 블럭, static 블럭(클래스가 처음 로딩될 때 딱 한번만 수행)을 이용한 singleton
	// static을 factory를 구현해 앞으로 이 factory만 계속 사용하게 할 수 있다.
	static {
		try { // alt + shift + z
			// 설계도 위치(MyBatis 설정 파일)
			String resource = "./com/it/mybatis/config.xml";
			// 공학자, Reader getResourceAsReader(String resource)
			Reader reader = Resources.getResourceAsReader(resource);
			// 공장   =  건축가.build(공학자), SqlSessionFactory build(Configuration config)
			factory = new SqlSessionFactoryBuilder().build(reader);
			
		} catch (IOException ioe) {
			System.out.println("초기화 문제 발생" + ioe);
		}
	}
	
	// factory 사용 함수
	public static SqlSessionFactory getFactory() {
		return factory;
	}
  	/*
    	SqlSessionFactoryBuilder : Mybatis 설정 파일(config.xml)을 바탕으로 SqlSessionFactory를 생성한다.
    	SqlSessionFactory : SqlSession을 생성한다.
    	SqlSession : SQL문을 실행하고 트랜잭션을 관리 및 실행시킨다. Thread-Safe하지 않으므로 thread마다 필요에 따라 생성해야한다.
	*/

}

```

<br>

## user.xml : Mapper 파일로서 SQL 작성
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 이 파일이 mapper 파일이야~ 라고 알려주는 부분 (config.xml에서 이 부분만 가져와서 수정한다. config -> mapper 변경) -->
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
<mapper namespace="User">
	<!-- 중복체크 하기 위한 -->
	<select id="checkId" parameterType="string" resultType="_int"><!-- id는 메소드명과 동일하게 줌, 기본자료형은 _int, _float, ... -->
		SELECT COUNT(*) FROM TEST_USER WHERE USERID = #{userid} <!-- DAO에서 넘겨주는 매개변수와 같은 이름 -->
		
	</select>
	<!-- 로그인 하기 위한 -->
	<select id="login" parameterType="hashmap" resultType="_int" >
		SELECT COUNT(*) FROM TEST_USER WHERE USERID = #{userid} AND USERPW = #{user_pw}
	
	</select>
	<!-- 회원가입하기 위한 -->
	<insert id="join" parameterMap="com.koreait.dto.UserDTO">
		<!-- 디비버에서 따오느는 법 :  디비버 -> 커넥션 -> 스키마 -> 테이블 -> 제네리터 -> 복붙 -->
		INSERT INTO WEB.TEST_USER
		(USERID, USERPW, USERNAME, USERGENDER, ZIPCODE, ADDR, ADDRDETAIL, ADDRETC, USERHOBBY)
		VALUES(#{userid},#{userpw},#{username},#{usergender},#{zipcode},#{addr}, #{addrdetail},
		#{addretc}, #{hobbystr}) 
	</insert>
	
	<!-- #{}은 DTO의 getter를 이용해 찾아오기 때문에 DTO에 getter를 직접 만들어줘서 원하는 데이터를 가져오게 할 수 있다. -->
</mapper>

```

<br>

## UserDAO.java : 연결된 DB를 통해 실제로 데이터 
```java
package com.it.dao;

import java.util.HashMap;

import org.apache.ibatis.session.SqlSession;

import com.it.dto.UserDTO;
import com.it.mybatis.SqlMapConfig;

public class UserDAO {
	// 생수병
	SqlSession sqlsession;
	
	public UserDAO() {
		//openSession : db와 연결
		// true로 설정시 auto commit
		sqlsession = SqlMapConfig.getFactory().openSession(true); 
	}
	
	// 회원가입, 객체로 넘길 수도 있다.
	//.insert : return type : int, 업데이트된 행의 개수를 리턴한다.
	public boolean join(UserDTO newUser) { 
		int result = sqlsession.insert("User.join", newUser); // mapper의 이름 User 그리고 select id가 join
		
		return result == 1;
	}
	
	// 중복확인, user.xml에서 SQL쿼리를 COUNT()로 했기 떄문에 0, 1로 체크
	public boolean checkId(String userid) { 
		int result = 1;
		// selectOne("맵퍼 쿼리문",(#{}에 들어갈) 매개변수) : 하나의 객체를 가져온다. 
		// 검색 결과가 있는 경우  return 1, 없는 경우 return null, 여러개인 경우 예외가 발생한다.
		result = sqlsession.selectOne("User.checkId", userid); //user.xml(mapper)에 있는 User라는 네임의 checkId를 가진 맵퍼 
		return result == 0;
	}
	
	// 로그인, user.xml에서 SQL쿼리를 COUNT()로 했기 떄문에 0, 1로 체크
	public boolean login(String userid,String userpw) { 
		int result = 0;
		HashMap<String, String> datas = new HashMap<String, String>();
		datas.put("userid", userid);
		datas.put("user_pw", userid);
		result = sqlsession.selectOne("User.login", datas);
		
		return result == 1;
	}
}

```

<br>

## UserDTO.java 
```java
package com.it.dto;

public class UserDTO {
	private String userid;
	private String userpw;
	private String username;
	private String usergender;
	private String zipcode;
	private String addr;
	private String addrdetail;
	private String addretc;
	private String[] userhobby;
	
	public String getUserid() {
		return userid;
	}
	public void setUserid(String userid) {
		this.userid = userid;
	}
	public String getUserpw() {
		return userpw;
	}
	public void setUserpw(String userpw) {
		this.userpw = userpw;
	}
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getUsergender() {
		return usergender;
	}
	public void setUsergender(String usergender) {
		this.usergender = usergender;
	}
	public String getZipcode() {
		return zipcode;
	}
	public void setZipcode(String zipcode) {
		this.zipcode = zipcode;
	}
	public String getAddr() {
		return addr;
	}
	public void setAddr(String addr) {
		this.addr = addr;
	}
	public String getAddrdetail() {
		return addrdetail;
	}
	public void setAddrdetail(String addrdetail) {
		this.addrdetail = addrdetail;
	}
	public String getAddretc() {
		return addretc;
	}
	public void setAddretc(String addretc) {
		this.addretc = addretc;
	}
	public String[] getUserhobby() {
		return userhobby;
	}
	public void setUserhobby(String[] userhobby) {
		this.userhobby = userhobby;
	}
	public String getHobbystr() {
//		MyBatis			실제로 찾는것
		
//		#{userid} ---> getUserid()
//		#{hobbystr} ---> getHobbystr()
		
		String hobbyStr = userhobby[0];
		for(int i=1;i<userhobby.length;i++) {
			hobbyStr+= ","+userhobby[i];
		}
		return hobbyStr;
	}
	
}

```

<br>

<hr>

# 정리를 마치며
> &nbsp;확실히 JDBC --> DBCP &JNDI를 거쳐 MyBatis로 오니까 이해에 도움은 되지만 아직 좀 어려운 것 같다... 그리고 역시  JDBC보다 훨씬 편하고 실용적인 것 같다. 익숙해지기 위해선 시간이 좀 걸릴 것 같다... 으... 헷갈린다...
