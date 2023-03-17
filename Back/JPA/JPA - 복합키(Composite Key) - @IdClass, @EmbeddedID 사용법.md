# 복합키(Composite Key) - @IdClass, @EmbeddedID
> Entity를 생성하다 보면 PK(primary key)가 여러 개가 되는 경우가 발생한다. 이러한 경우 어떠한 방식으로 Entity를 작성해야 하는지 알아보도록 하자.<br>
> JPA에서는 2가지 방식을 제공한다.
- @IdClass
- @EmbeddedId


## @IdClass
![image](https://user-images.githubusercontent.com/74396651/225780916-5eaa461b-f3b1-417c-8bf3-bb272653f9b8.png)

### - @IdClass 작성 시 지켜야할 사항
- Serializable을 구현해야 한다.
- Equals, HashCode를 구현해야 한다.(위 사진에는 lombok을 이용했다.)
- 기본 생성자가 존재해야 한다.
- public class여야 한다.
- Entity class의 @Id 필드명과 동일한 필드를 가지고 있어야 한다.

![image](https://user-images.githubusercontent.com/74396651/225781133-fd7da489-b75d-4513-92bd-b8422b32d964.png)

<hr>

## @EmbeddedId

![image](https://user-images.githubusercontent.com/74396651/225781172-dc6697f3-5fde-4688-a88a-790cca11ab0a.png)

> 이렇게 두가지 형식이 존재하는데 각각 선언하는 방법도 다르지만 실제로 쿼리를 조회한 후 값을 조회할 때도 차이가 존재한다.

- @IdClass
  - member.getTeamId()
- @EmbeddedId
  - memver.getMemberd.getTeamId() 

![image](https://user-images.githubusercontent.com/74396651/225781563-3827f47d-4773-490b-bedd-46f568aa9575.png)
