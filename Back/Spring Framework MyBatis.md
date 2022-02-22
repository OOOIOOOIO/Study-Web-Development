<br>

# DataBase 연결

## Oracle과 연동하기

```
	Build Path 추가 -> 왼쪽 메뉴에 바로 위에있는 Deployment Assembly 선택
	-> Add -> Java Build Path ~~ 선택 -> ojdbc6.jar 선택 -> Apply
```

<br>

## Spring-Mybatis

>&nbsp; Spring-Mybatis를 이용하기 위해선 pom.xml에 mybatis 라이브러리를 설치해주어야 한다. ~~~~~~
SQL문이 짧고 간결한 경우에는 어노테이션을 이용해서 쿼리문을 작성해준다. SQL문이 복잡하거나 길어지는 경우에는 어노테이션보다 XML을 이용하는 것이 좋다. 
Spring-Mybatis의 경우 Mapper 인터페이스와 XML을 연동해서 동시에 이용할 수 있다. 
인터페이스객체.메소드()를 사용하는 순간 해당하는 인터페이스의 경로를 namespace로 가지고 있는 xml 파일로 찾아가서 메소드명과 동일한 id의 쿼리문을 수행하여 결과로 돌려준다. <br>
&nbsp;Mybatis는 내부적으로 JDBC의 PreparedStatement를 이용해서 SQL을 처리한다. 따라서 SQL에 전달되는 파라미터는 JDBC에서와 같이 ? 로 치환되어서 처리된다. 복잡한 SQL의 경우 ?로 나오는 값이 제대로 전달 되었는지 확인하기가 쉽지 않고 실행한 SQL의 내용을 정확히 확인하기 어렵기 때문에 log4jdbc-log4j2 라이브러리를 사용하여 어떤 값인지를 확인할 수 있다. <br>
&nbsp;테스트 코드 실행시 많은 양의 로그가 출력되기 때문에 불편할 수 있다. 이럴 때에는 로그의 레벨을 이용해서 수정해 준다. resources/log4j.xml 파일에 있는 level 태그를 수정한다.

## mapper test

## mybatis 동적 쿼리문 -  mapper.xml
