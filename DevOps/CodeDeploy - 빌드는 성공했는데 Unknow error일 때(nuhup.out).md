## Unkown error

<img width="1325" alt="image" src="https://user-images.githubusercontent.com/74396651/230445698-cdec6bce-3880-4243-b53b-886a9ab1c6e6.png">

왜 발생한지 봤더니 CodeDeploy는 nohup 실행 시 무한 대기를 한다고 한다. nuhup이 끝나기 전까지 CodeDeploy도 끝나지 않는단다... 이를 해결하기 위해 아래처럼 nohup.out 파일을 표준 입츌력 용도로 사용한다.

> 마지막 2>&1 & 의 뜻은


![image](https://user-images.githubusercontent.com/74396651/230446005-798508de-a956-40d8-bd0a-39cb87d2f5c0.png)





<hr>

## 기존 배포 스크립트
```
nohup java -jar \
        -Dspring.config.location=classpath:/application.yml,/home/ubuntu/app/application-db.yml,/home/ubuntu/app/application-oauth.yml \
        $REPOSITORY/$JAR_NAME 2>&1 &
        
```

## 바뀐 후
```
nohup java -jar \
        -Dspring.config.location=classpath:/application.yml,/home/ubuntu/app/application-db.yml,/home/ubuntu/app/application-oauth.yml \
        $REPOSITORY/$JAR_NAME > $REPOSITORY/nohup.out 2>&1 &
```


<br>
<br>
<br>
<br>

[참고1](https://mkil.tistory.com/258)
[참고2](https://velog.io/@tigger/%EB%B0%B0%ED%8F%AC-%EC%9E%90%EB%8F%99%ED%99%94-%EA%B5%AC%EC%84%B1)
