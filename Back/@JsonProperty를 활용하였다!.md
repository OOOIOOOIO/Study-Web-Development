# @JsonProperty
아래와 같이 Postman으로 api를 쐈는데 qNum값이 제대로 들어오지를 못하고 있었다. 이런적이 한 번도 없었는데..int를 Integer로 바꿔서 받았더니 null을 받아와서 어.. 왜이러지 당황하였다. 필드값이 같으면 잘받아야할텐데.. dto 필드값과 포스트맨 필드값을 q_num으로 해서 쐈더니 이건 또 잘 받아와지지만 Java에서 스네이크 케이스를... 사용하기엔 좀 그래서 @JsonProperty를 통해 해결해보았다!

## @JsonProperty()
- Jackson 라이브러리는 Spring boot를 사용하면 starter-web에 포함되어 있다. boot를 사용하지 않으면 따로 dependency에 추가해주어야 한다.
- @JsonProperty()은 객체를 Json 형식으로 변환할 때 Key 이름을 설정할 수 있다. 
- api에서 넘어오는 값을 객체(dto 등)에 맵핑할 때 사용하면 유용하다.(api에서 넘어오는 값을 괄호 안에 넣으면 된다.)
- json을 파싱하여 바로 return할 경우 @JsonProperty에서 설정한 값으로 리턴된다!
- 만약 너무 많은 값을 맞춰줘야할 경우 @JsonNaming()을 사용해준다!

<hr>

### Postman
![image](https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/0014481b-7d73-4d9b-a53c-3dc652375b91)

### DTO
<img width="797" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/c767bf6f-3d58-4d5d-ae68-dbf1ce9dbed8">

### Log - qNum을 못 읽음
<img width="515" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/18b4bce9-1b35-4b89-8957-c13f38432867">

<hr>

### @JsonProperty 추가
<img width="440" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/05ebfde2-0a17-4c03-9902-916b97b16148">

### 성공적으로 받아옴
<img width="477" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/2f070c78-041d-4d6c-a321-67ddfd38a84b">


<br>
<br>
<br>
[참고](https://dev-jwblog.tistory.com/120)


