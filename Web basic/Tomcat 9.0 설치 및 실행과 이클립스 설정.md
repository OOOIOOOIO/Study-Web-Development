## Tomcat 설치 및 환경설정
- 설치 
    -  구글에 tomcat 검색 --> 톰캣 홈페이즈 접속 --> 왼쪽 다운로드에서 Tomcat 9 클릭
--> 64 bit WIndow zip 설치(컴퓨터 사양에 따라 다름) --> 압축파일 원하는 곳에 압출 풀기 

- 서버 켜는 법
    - Tomcat 폴더 --> bin 폴더 --> start.bat 파일 실행
    - 혹시나 start.bat이 바로 꺼진다면, 환경변수 -->
시스템 변수에서 JAVA_HOME 새로 만들고 경로는 C:\Program Files\Java\jdk1.8.0_291
--> 시스템 변수의 Path에 %JAVA_HOME&\bin 새로 만들기 

- 포트번호 바꾸기 
    -  Tomcat 폴더 --> conf 폴더 --> sever.xml --> Connector 9090으로 바꾸기(원래 8080인데 지금 오라클 포트번호가 8080이기 때문에 겹쳐서 안됨)
--> 다시 start.bat 실행

- 이후 브라우저 열고 localhost:9090 실행 시 톰켓 배경화면 뜨면 설치 성공 

<br>

---------------------------------------------------------
<br>

## Java에서 웹버전으로 프로젝트와 파일 생성 방법 및 서버 연결
- perspective : JAVA EE(Wevb 개발)로 설정

- 서버 연결 
   - Window --> Show View --> Servers 클릭 --> 아파치파일 --> 톰캣 9.0 --> NEXT 
--> 톰캣 최상위 폴더 경로 설정 --> 파일 Add --> Finish

- 서버 등록 확인하는 법 
    - Buildpath --> Web APP Library가 있다면 성공
    - 만약 프로젝트가 서버에 연결이 안되어 있다면 Severs에 연결되어 있는 Tomcat 더블클릭 --> Modules 클릭 --> Add Web Module에서 프로젝트 추가 후 저장

- 웹 브라우저 설정
    - Window --> Web browser --> 원하는 브라우저 클릭 

- project 생성(서버를 먼저 연결한 후 생성) 
    - Alt + Shift + N --> Dinamic Web project --> web.xml 생성 클릭 --> Finish

- project 생성시 주의할 점(서버 추가 후)
    - run target --> APACHE인지 확인, web.xml 체크했는지 확인. 

- 파일 만들기
    - project 안에 있는 Web content(프론트) 클릭--> Ctrl + N --> JSP File 클릭 및 생성(Web content 안에 만들어줘야 된다.)

- 인코딩 설정 
    - Window tab --> Preferences -- enc 검색 --> Workspace, CSS, HTML, JSP, XML 각각 Apply 해줘야 된다.)

<br>

-----------------------------------------

<br>

## 프로젝트 emport & import
- 웹버전 프로젝트 베포하는 법
    - 프로젝트 우클릭 --> Export --> WAR file --> destination 알아서 경로지정 --> Export source files(자바파일까지) --> finish

- 웹버전 프로젝트 받는 법
   - import --> WAR file



