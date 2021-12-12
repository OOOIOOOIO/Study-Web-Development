# HTML

## HTML이란
- HTML은 프로그래밍 언어가 아닌 마크업 언어이다. 마크업 언어란 우리가 보는 웹페이지가 어떻게 구조화 되어 있는지
	브라우저에게 알려주는 언어이다. HTML은 브라우저에게 데이터를 전달하고 브라우저는 우리에게 GUI를 통해 시각화해주는 것이다.
  
## HTML의 요소의 개념과 종류
```
<p>HIHIHI IM SEONG HO</p>
```
- 여는 태그(Opening Tag) : 요소의 이름(p)와 열고 닫는 꺽쇠 괄호로 구성되어 있다.
- 내용(content) : 단순한 텍스트이다.
- 닫는 태그(Closing Tag) : 요소의 이름 앞에 /(슬래쉬)가 있다.
> &nbsp;위의 것들을 통틀어 요소라고 하며 요소의 이름은 대소문자를 구분하지 않지만 가독성에 있어 소문자로 작성하는 것을 권장한다.


```
<p>HIHIHI IM SEONG HO<b>WHATWHAT</b></p>
```
- 내포된 요소(Nested Elements) : 요소 안에 다른 요소를 넣는 기법. 구조가 겹겹이 쌓이는 것
- 
### 단락 태그(Paragraph Tag) 
-  단락이란 내용상 끊어서 구분할 수 있는 하나하나의 부분을 의미하고 문단이라고 부른다.
  
```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>단락 태그</title>
</head>
<body>
	 <!-- 제어문자 -->
	 <!--
	 	< : &lt;(less than)
	 	> : &gt;(greater than)
	  	& : &amp;
	  	" : &quot;
	  	공백 : &nbsp; (HTML은 공백을 아무리 많이 쳐도 한번으로 인식한다.
	  -->
	  
	  <!-- h태그 : 제목을 표기할 때 사용하는 태그, h1 ~ h6 숫자가 커질수록 크기가 작다. 스크린 리더가 제목이라고 읽는다. -->
	  <h1>HTML</h1>
	  <h2>HTML</h2>
	  <h3>HTML</h3>
	  <h4>HTML</h4>
	  <h5>HTML</h5>
	  <h6>HTML</h6>
	  
	  <!-- 
	  	<br> : HTML의 줄바꿈 태그, 단일태그이다(열고 닫지 않는다.) 
	  	<p> : 단락 태그 	
	  -->
	  <p>
	  	HTML은 제목, 단락, 목록 등과 같은 본문을 위한 구조적 의미를 나타내는 것 뿐만 아니라<br>
	  	링크, 인용과 그 밖의 항목으로 "구조적 문서"를 만들 수 있는 방법을 제공한다.
	  </p>
	  	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;공백공백공백
	  <p>여기도 단락</p> 
</body>
</html>

<!-- 이게 주석처리 ctrl + shift + / -->
```

### 서식 태그
 ```

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>1_formatTest</title>
</head>
<body> screen reader : 접근성, 시각 장애인에게 크게 알려준다든지, 청각 장애인에게 또렷하게 보이게 해준다던지. 
  <!--
  <strong></strong>, <b></b> : 진하게
  <em></em>, <i></i> : 기울이기
  <mark></mark> : 형광
  <del></del> : 글자 가운데 줄
  <ins></ins> : 밑줄
  <sup></sup>, <sub></sub> : 위첨자, 아래첨자
  -->
	<p>
		<strong>strong 태그는 중요한 글자를 굵게 만든다. screen reader가 읽을 수 있다.</strong><br>
		이건 일반 글씨<br>
		<b>b 태그는 단순히 글자를 굵게 만든다. screen reader가 읽지 않는다.</b>
	</p>
	<p>
		<em>em 태그는 중요한 글자를 이탤릭체로 표현한다. screen reader가 읽을 수 있다.</em><br>
		<i>i 태그는 단순히 글자를 이탤릭체로 표현한다.</i>
	</p>
	<p>
		<mark>mark 태그는 글자를 하이라이팅 처리한다.</mark>
	</p>
	
	<p>
		<del>del 태그는 글자 중간에 가로줄을 그어서 표현한다.</del><br>
		<ins>ins 태그는 글자 아래쪽에 밑줄을 그어서 표현한다.</ins>
	</p>
	<p>
		x<sup>2</sup> +2x +1<br>
		H<sub>2</sub>0
	</p>



</body>
</html>
```
  

     
### 리스트 태그 
- ul : 순서가 없는 리스트. ul 안에 li 외에는 사용하면 안된다.
- ol : 순서가 있는 리스트. 마찬가지로 li만 사용해야 한다.
- li(list item) 
- dl, dt, dd : 용어와 그에 대한 정의를 모아놓은 리스트
   - dl(definition list) : 사전처럼 용어를 설명하는 목록을 만든다.
   - dt(definition term) : 용어의 제목을 넣을 때 사용한다.
   - dd(definition description) : 용어를 설명하는데 사용한다.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>2_list</title>
</head>
<body>
  <!-- ul, ol 태그 -->
	<!-- <ul>은 순서가 없어 .으로 출력한다. -->
	<h3>제 9조(서비스 이용시간)</h3>
	<ul>
		<li>서비스 이용시간은 당일 09시부터.....</li>
		<li>먹을 건 밖에서 먹고 들어와라아아아아</li>
	</ul>
	
	<!-- <ol>은 1, 2, 3... 순서있게 출력한다.
		type=? 을 줄 수 있다. abc순으로 혹은 i,ii,iii 순으로 등등
		start=? 을 줄 수 있다. 시작점을 지정한다.
		-->
	 -->
	<h3>기말고사 망한 과목 순서</h3>
	<ol type="1" start="0">
		<li>신호 및 시스텀</li>
		<li>데이터 알고리즘</li>
		<li>특허 조사 분석</li>
    
   <!--
   type 종류
      1 : 숫자(1, 2, 3, ....)
      a : 소문자(a, b, c, ...)
      A : 대문자(A, B C, ....)
      i : 로마 소문자(i, ii, iii, ...)
      I : 로마 대문자(I, II, III, ....)
  -->
	</ol>
  <!--dl, dt, dd 태그 -->
  <h2>아리</h2>
	<dl>
		<dt>스킬</dt>
		<dd>Q : 현혹의 구슬</dd>	
		<dd>W : 여우볼</dd>	
		<dd>E : 메혹</dd>	
		<dd>R : 혼령 질주</dd>
		
		<dt>평가</dt>
		<dd>아주 수려한 외모로 티어를 유지하는 챔피언</dd>
		<dd>트런들 외모였으면 100티어 정도 되는 성능</dd>
			
	</dl>
</body>
</html>
```

