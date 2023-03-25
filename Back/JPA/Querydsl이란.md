# Query DSL
> Query이란 오픈소스 프로젝트로 JPQL을 Java 코드로 작성할 수 있는 라이브러리다.

<hr>


# 설정(강의 버전)

### build.gradle
````

dependencies {
//Querydsl 추가
    implementation 'com.querydsl:querydsl-jpa' 
    annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa"
    annotationProcessor "jakarta.annotation:jakarta.annotation-api"
    annotationProcessor "jakarta.persistence:jakarta.persistence-api"
    
    
}


//Querydsl 추가, 자동 생성된 Q클래스 gradle clean으로 제거 clean {
    delete file('src/main/generated')
  }

````

### Q 타입 생성 확인
- ## gradle로 빌드할 때

![image](https://user-images.githubusercontent.com/74396651/227722362-f7103f40-a795-482c-943f-2955b9c1ca5d.png)
![image](https://user-images.githubusercontent.com/74396651/227722438-a47dc963-2f0e-4d96-a445-c7d88fed7778.png)

- ## Intellij로 빌드할 떄

![image](https://user-images.githubusercontent.com/74396651/227722457-9e22a082-5696-4d17-b51d-243ba6db22f2.png)

<br>
<hr>
<br>

# 설정(스프링부트 2.7.x 기준)

### build.gradle
````
//Querydsl 추가
// queryDSL 설정
	implementation "com.querydsl:querydsl-jpa"
	implementation "com.querydsl:querydsl-core"
	implementation "com.querydsl:querydsl-collections"
	annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jpa" // querydsl JPAAnnotationProcessor 사용 지정
	annotationProcessor "jakarta.annotation:jakarta.annotation-api" // java.lang.NoClassDefFoundError (javax.annotation.Generated) 대응 코드
	annotationProcessor "jakarta.persistence:jakarta.persistence-api" // java.lang.NoClassDefFoundError (javax.annotation.Entity) 대응 코드
  


// Querydsl 설정부
def generated = 'src/main/generated'

// querydsl QClass 파일 생성 위치를 지정
tasks.withType(JavaCompile) {
	options.getGeneratedSourceOutputDirectory().set(file(generated))
}

// java source set 에 querydsl QClass 위치 추가
sourceSets {
	main.java.srcDirs += [ generated ]
}

// gradle clean 시에 QClass 디렉토리 삭제
clean {
	delete file(generated)
}

````

<hr>

### Q 타입 생성 확인 및 주의점
1. Querydsl Q파일 생성 위치가 다릅니다. 기존 영한님 강의대로 $build 로 시작하는 설정을 사용하면 테스트 실행 시 Q파일의 위치를 찾지 못해서 테스트가 실패합니다.

2. 
- Gradle -> Tasks -> build -> clean
- Gradle -> Tasks -> build -> build 혹은 classes
기존 영한님 교안에는 빌드 시 Gradle -> Tasks -> other -> compileQuerydsl 로 Q파일을 생성하지만, 이 방법의 경우 other에 해당 메뉴가 없습니다. 그래서 빌드 시에는 그냥 build 메뉴의 build 혹은 classes 로 빌드하시면 Q파일이 생깁니다.

3. 영한님 강의에서는 gradle build 폴더가 대부분 git 버전관리에 포함되지 않으므로 따로 설정할 필요가 없지만, 이 경우 Q파일이 소스폴더에 들어가므로 .gitignore 에 아래와 같이 별도로 경로를 설정해 주어야 합니다.
- /src/main/generated



<br>
<br>
<br>
<br>

참조 : 인프런 김영한님 강의

