## 1. Cloud storage 버킷 생성

<img width="840" alt="image" src="https://user-images.githubusercontent.com/74396651/226105349-0d7bebf4-d9a1-49cf-91a3-640240bfb523.png">


<img width="841" alt="image" src="https://user-images.githubusercontent.com/74396651/226105472-2949f30c-e2a5-41bf-949a-eaac1df182c2.png">

<img width="836" alt="image" src="https://user-images.githubusercontent.com/74396651/226108000-a66a0100-f74f-41ad-9c27-07a0ec25589d.png">


<img width="845" alt="image" src="https://user-images.githubusercontent.com/74396651/226108291-a6f77fa5-2618-4beb-96a3-9e546586e35a.png">

<hr>

## 2. 서비스 사용자 권한 부여 및 Access key 발급

![image](https://user-images.githubusercontent.com/74396651/226112924-61bedfcb-e662-4fa0-9c45-47a2b3324bfd.png)

![image](https://user-images.githubusercontent.com/74396651/226112943-5f6572af-7938-447e-aff1-d293b4beedf5.png)

![image](https://user-images.githubusercontent.com/74396651/226115431-6e87f001-942d-47d2-bb13-aebfcd3339c9.png)

![image](https://user-images.githubusercontent.com/74396651/226115492-9ebfece2-f429-484b-bc86-773926f93e27.png)

<hr>

## 3. 모든 엑세스 허용

![image](https://user-images.githubusercontent.com/74396651/226120808-721cf94b-db57-4625-bf86-c7cff1105b88.png)

<hr>



## 4. Spring boot에 Access Key 적용

<img width="945" alt="image" src="https://user-images.githubusercontent.com/74396651/226119555-8e65d32d-a97f-43f1-8e33-bba352a3c60e.png">

- build.gradle 적용
```jvava
ext {
    set('springCloudGcpVersion', "3.4.6")
    set('springCloudVersion', "2021.0.6")


    // GCS
    implementation 'com.google.cloud:spring-cloud-gcp-starter'
    implementation 'com.google.cloud:spring-cloud-gcp-starter-storage'
    
    dependencyManagement {
    imports {
        mavenBom "com.google.cloud:spring-cloud-gcp-dependencies:${springCloudGcpVersion}"
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${springCloudVersion}"
    }
}

```

<hr>

## 에러들

### 403 에러 : The billing account for the owning project is disabled in state closed

![image](https://user-images.githubusercontent.com/74396651/226161000-3fc4ebf3-16aa-4454-8f35-990a254a52e5.png)

<hr>

### 400 에러 : cannot insert legacy acl for an object when uniform bucket-level access is enabled

acl 에런데...나는 그냥 코드에서 빼버렸다. 

<br>
<br>
<br>br>

[참고](https://dncjf64.tistory.com/313)

[참고](https://jyami.tistory.com/54)
[참고](https://velog.io/@hyj2508/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-GCS-%EC%97%85%EB%A1%9C%EB%93%9C%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C-%EA%B5%AC%ED%98%84)
[참고](https://nangman14.tistory.com/70)
[코드 짱](https://choo.oopy.io/35bffd94-7a41-4cfa-812c-b8aaf148604a)



