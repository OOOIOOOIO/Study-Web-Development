# Web 개발을 위한 Eclipse 기본 설정

## Encoding 설정 방법
- Window tab --> Preference --> encoding 검색<br>
 --> Workspace, CSS, HTML, JSP, XML의 Encoding을 UTF-8로 변경(각 변경마다 Apply 해줘야된다.)
 
 <br>
 
 --------------------------
 
 <br>
 
## Perspective 변경 
 - Window tab --> Perspective --> Open Perspective --> other ---> Java EE
 - 혹은 우측 상단에서 Open Perspective --> Java EE

<br>

--------------------------
<br>

## Server : Tomcat 9.0 사용
- 구글에 Tomcat 검색 --> 톰캣 홈페이지 접속 --> 좌측 다운로드에서 Tomcat 9 클릭<br>
--> 64 bit WIndow zip 설치(본인 사양에 맞게) --> 압축파일 풀기 압출 풀기 

- 서버 켜는법
   - Tomcat 폴더 --> bin 폴더 --> start.bat 파일 실행
   - 혹시나 start.bat 파일을 실행하였는데 cmd창이 바로 꺼진다면, 환경변수 혹은 자바와 JDK 버전이 안 맞는 문제일 경우가 크다.<br>
   - 환경변수 문제 : 환경 변수1 편집 --> 환경 변수 클릭 --> 시스템 변수에 JAVA_HOME 새로 만들고 경로는 C:\Program Files\Java\jdk1.8.0_291(개인별 상이)<br>
   --> 시스템 변수에 있는 Path에 %JAVA_HOME&\bin 새로 만들기
   - 자바와 JDK 버전 문제 : 다시 깔아라!

- Tomcat 포트번호 바꾸기(본인 같은 경우 Oracle의 포트번호와 겹쳐서 바꿨다)
   - Tomcat 폴더 --> conf 폴더 --> sever.xml 파일--> Connector 9090으로 바꾼다(원래 8080) --> 다시 start.bat 실행

### 이후 브라우저를 열고 localhost:9090 실행 시 톰캣 배경화면 뜨면 성공!

<br>

-----------------------------
<br>

## Eclipse에서 Sever 등록하는 법
- Window tab --> Show View --> Servers --> Apache file --> Tomcat 9.0 --> Next<br>
Tomcat 최상위 폴더로 경로 설정 --> 파일 Add --> Finish

<br>

## 서버 등록 확인하는 법
- Buildpath --> Web APP Library가 있다면 성공

<br>

## 웹 브라우저 설정
- Window --> Web browser --> 원하는 브라우저 설정

<br>

## 프로젝트 생성시 주의할점(서버 추가 후)
- Target runtime--> APACHE Tomcat v9.0 설정
- web.xml 체크 후 생성

<br>

-------------------------------

<br>

## 웹 버전 프로젝트 배포 및 받는 하는 법 
- 프로젝트 우클릭 --> WAR file --> destination(다운받을 경로지정) --> Export source files(Java 파일까지 베포) --> Finish

- 프로젝트 우클릭 --> import --> WAR file --> Browser --> Finish

<br>

----------------------------

<br>

# 기타 

## 경로 지정 
- 상대경로 : 현재 위치한 페이지(파일)을 기준으로 찾아가는 경로
   - ./ : 현재폴더(./파일이름 or 파일이름)
   -  ../ 상위폴더(../들어갈폴더 or 파일이름)

- 절대경로 : 현재 어떤 폴더에 있던 간 최상위 경로부터 하나씩 타고 들어가 찾아갈 수 있게 작성한 경로
   - C:\\~~~~~~~ or D:\\~~~~~~~













