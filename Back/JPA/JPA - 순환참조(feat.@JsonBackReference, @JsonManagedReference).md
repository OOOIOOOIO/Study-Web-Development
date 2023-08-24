# 순환참조(feat.@JsonBackReference, @JsonManagedReference)
RDB만 사용하면 절대 발생하지 않을 일... 하지만 나는 JPA를 사용하기 떄문에 객체 그래프를 기반으로 쿼리를 작성하고! 데이터를 가져온다. 이로 인해... @OneToMany, @ManyToOne 관계일 경우 서로가 서로를 무한히 호출하는 무한루프를 돌 수 있는데, 이것이 순환참조이다! 이를 손쉽게 해결하는 방법은 toStrin을 재정의해서 빼주는 방법인데 이보다 더 간편한 해결방법은 @OneToMany(연관관계주인)에 @JsonManagedReference 붙여주고 @ManyToOne에 @JsonBackReference를 붙여주면 된다.
<br>
<hr>
<br>


## @JsonBackReference 

<img width="457" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/987ce186-de6f-433d-bc7a-85494d450c93">


<br>
<hr>
<br>

## @JsonManagedReference

<img width="784" alt="image" src="https://github.com/OOOIOOOIO/Study-Web-Development/assets/74396651/6eb2fe1c-423a-4e21-b135-5b19c54ef42e">
