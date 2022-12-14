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


<br>
<hr>
<br>

## @RequestBody
> 얘도 결론부터 말하면 "@Setter는 전혀 필요 없다! 아무것도 필요없다 Spring HTTP Message Converter의 Jackson2HttpMessageConverter가 내부적으로 ObjectMapper를 사용해 바인딩해준다"
> 
























































> [참고1](https://minchul-son.tistory.com/546)
> [참고2](https://hyeon9mak.github.io/model-attribute-without-setter/)
