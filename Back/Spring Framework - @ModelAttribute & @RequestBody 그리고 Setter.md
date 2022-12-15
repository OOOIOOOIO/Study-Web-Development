# @ModelAttribute와 @RequestBody에서 @Setter가 필요할까!
> 예전에 이걸로 골머리를 썩었는데, 오랜만에 프로젝트하려니까 헷갈려서 정리한다!

<br>

## @ModelAttribute
> 우선 결론부터 말하면 "기본 생성자만 있을 경우 @Setter를 통해 바인딩을 하고, 만약 다른 생성자가 있을 경우 그 생성자를 통해 바인딩을 해준다."


- ### 기본 생성자(자동 생성) + @Setter 사용
- Spring MVC 흐름
   - 기본 생성자가 있는지 확인 후 객체 생성(new 연산자)
   - 이후 @Setter를 통해 바인딩
![image](https://user-images.githubusercontent.com/74396651/207614774-83f12f9b-9b9f-4346-835d-3f47c8fdbb9a.png)

<br>

- ### 생성자 사용
- Spring MVC 흐름
   - 마찬가지로 기본 생성자가 있는지 확인 후 없다면
   - 기존 생성자에 바인딩(파라미터 수가 가장 적은 생성자에 바인딩!)
![image](https://user-images.githubusercontent.com/74396651/207614956-cad67a90-516a-435c-8144-b6ae3c071a9e.png)

<br>

## 간단한 작동 원리

![image](https://user-images.githubusercontent.com/74396651/207617789-acb22d9b-f4cb-43e0-b7dc-3a993efdef91.png)

<br>

## 정리
- @ModelAttribute는 값을 객체로 바인딩할 때 프로퍼티 접근법을 사용한다!
- [상세](https://blog.karsei.pe.kr/59)


> [참고1](https://minchul-son.tistory.com/546)
> [참고2](https://hyeon9mak.github.io/model-attribute-without-setter/)

<br>
<hr>
<br>

## @RequestBody
> 얘도 결론부터 말하면 "@Setter는 전혀 필요 없다! 아무것도 필요없다 Spring HTTP Message Converter의 Jackson2HttpMessageConverter가 내부적으로 ObjectMapper를 사용해 바인딩해준다"
> [상세히 설명되어 있네1](https://blogshine.tistory.com/445)
> [상세히 2](https://blogshine.tistory.com/446)

![image](https://user-images.githubusercontent.com/74396651/207630479-f0fc5a22-4b18-4f64-be4a-47a01147c5f8.png)


### 기본 생성자 존재
- 알아서 바인딩 된다.

### 기존 생성자(기본 생성자 없음)
- 기본 생성자가 없는 상황이고 오직 파라미터를 가진 생성자가 1개만 있다면 Object를 생성할 수 있다.
- Jackson에서 기본생성자, Getter, Setter가 없으면 자동으로 @JSonCreator를 해당 생성자에 붙여 작동하도록 도와준다.

### 기존 생성자(기본 생성자 없은, 파라미터 1개)
- 단일 생성자에 단일 파라미터가 있는 경우 JackSon이 @JsonCreator를 추가하는 기능이 동작하지 않는다.
- 따라서 직접 생성자에 @JsonCreator를 붙여줘야 한다.

<br>

## 정리
1. 일반적인 상황에서는 기본 생성자가 필수다.
    - RestController에서 @RequestBody를 바인딩하기 위해 ObjectMapper를 사용하는데 기본 생성자로 객체를 생성하기 때문이다.
2. Property 기반 클래스(@JsonProperty, @JsonAutoDetect)인 경우 기본 생성자 없이 생성이 가능하다.
3. 기본 생성자가 없고, 파라미터를 받는 생성자가 있다면 생성이 가능하다.
4. 기본 생성자가 없고, 파라미터를 "1개"만 받는 생성자라면 @JsonCreater를 추가해주어야 생성이 가능하다.
5. Setter나 getter는 값을 바인딩할 때 필요하지 않지만!
6. Setter와 Getter 모두 없는 경우 ObjectMapper가 바인딩하는데 오류가 생긴다.(둘 중 하나는 필요햐다. 값을 꺼내야 하므로 getter를 자주 사용한다)

```
// @Getter + 기본생성자 + @JsonCreator : 성공
// @Getter + @JsonCreator : 성공
// @Getter + 기본생성자(생략) : 성공
// 기본생성자 : 실패

==> @Getter는 필수!! + 기본 or 파라미터 생성자
```






