f
join fetch로 한번에(eager) 가져오니까 lazy가 아니며 transaction이 필요 없는거고
batch를 설정하였기 때문에 상위 엔티티를 가져올 때 미리 size 만큼 땡겨 오기 때문에 lazy도 아니며 transaction이 필요 없는거고
lazy일 때는 transaction이 필요하다! service에 어노테이션 붙여줬으면 컨트롤러까지 오지 않는다. => 컨트롤러에서 강제 초기화 할 경우 에러 발생!
