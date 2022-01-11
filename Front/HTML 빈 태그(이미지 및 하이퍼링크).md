# HTML

## 빈 태그
---------------------------
### 개념
- 여는 태그와 닫는 태그 사이Content가 없는 요소이다. "단일 태그"라고도 하며 주로 문서에 무언가를 첨부하기 위해 사용된다.


### 종류
```html
<br /> : 텍스트 안에 줄바꿈(*Carriage Return)을 생성합니다.(Carriage Return : 현재 위치를 나타내는 커서를 맨 앞으로 이동시킨다는 뜻)
<hr /> : 가로방향의 Division Line을 생성합니다.(까만 가로 줄)
<img /> : 이미지를 생성합니다.
<input /> : 사용자의 데이터를 받을 수 있는 대화형 컨트롤러를 생성합니다. 다양한 종류의 입력 데이터 유형이 있습니다.
<link /> : 현재 문서와 외부 리소스의 관계를 명시합니다.
<meta /> : <base>, <link>, <script>, <style>, <title?과 같은 다른 메타 관련 요소로 나타낼 수 없는 메타데이터를 나타냅니다.
```

--------------------------
## 이미지 태그 
- 웹 페이지에서 주로 사용되는 이미지의 종류
   - jpeg(.jpg .jpeg) : 휴대폰, 카메라 사진
   - gif(.gif) : 움직이는 이미지, 움짤
   - png(.png) : 배경을 투명하게 할 때 사용(사진 첨부할 때 자주 사용)

- HTML 문서에 이미지를 삽입할 때에는 <img> 태그를 사용한다.

- 상대경로 : 현재 위치한 페이지(파일)를 기준으로 찾아가는 경로
   - ./ : 현재 폴더(./1_banana1.png or banana1.png)


- 절대 경로 : 현재 어떤 폴더에 있던 간 최상위 경로부터 하나씩 타고 들어가 찾아갈 수있게 작성한 경로
   - ../ : 상위폴더
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>1_image</title>
</head>
<body>
  <!-- 형식
    <img src="이미지경로" alt="그림이 안 뜰 때 대체할 문자열">
  -->
	<p>
		바나나1<img src="1_banana1.png" alt="">
	</p>
	<p><!-- 현재폴더 안에 있는 image폴더 안에 있는 banana -->
		바나나2 <img src="images/1_banana2.png" alt="">
	</p>
	<p><!-- 절대경로로 할 경우 이클립스에서 컴파일 할 시 이미지가 뜨지 않음. 왜냐하면 서버를 거쳐서 페이지를 만들기 때문이다.
			서버가 드라이브에 접근할 수 없기 때문에 이미지가 안 뜨는 것. 하지만 직접 폴더에서 HTML 파일을 더블클릭 해 열 경우 사진이 옳바르게 띄워진다. 
			그러므로 왠만하면 상대경로로 줄 것! 가급적 같은 폴더 내에서 저장해서 사용하기
      -->
		바나나3 <img src="D:\polit\HTML_CSS_Javascript\workspace\day05\WebContent\1_banana3" alt="">
	</p>
</body>
</html>
```

--------------------

## 하이퍼링크
- HTML 파일 링크를 걸 때 사용
- 현재 페이지에서 다른 페이지로 이동하고자 할 때 사용한다.
- 보통 링크라고 부른다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<!-- 형식
		<a href="페이지경로, 주소" target="보여줄창(이름)">문자열 또는 이미지 설명</a>

		target : 내가 원하는 창에서 띄우기
		target="" : 현재 브라우저에서 열기("_self" 생략) 
		target="_blank" : 새 창에서 열기
		target="_top" : 가장 상위 창에서 연다(프레임을 무시하며, 전체 브라우저 창에서 작동한다. 부모가 없으면_self처럼 작동)
		target="frame name" : 지정된 프레임 안에서 열기
	 -->
	 <a href="https://www.naver.com" target="_blank">이것은 하이퍼링크</a>
</body>
</html>
```

