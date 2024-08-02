크흑...

Job parameter 중복불가
다중 job 불가
일반 tasklet -> reader processor writer로 나눔
entity aggregate의 중요성
bulkinsert를 위해 테이블 기본키 생성 전략 수정 
reader에서 list로 리턴하면 각 요소마다 porcess가 도는구나
그리고 writer 마지막에 한번에 insert가 나가는구나
—> writer에서 마지막 bulkinsert만 구현하면 성능 괜찮?
—> reader에서 페치조인으로 읽어오고 싶은데 흠
—> Job을 분리하지 않고 Step으로 개발한 이유 -> 데드락
—> JobParameter 겹치면 뻑남 



- Custom
https://docs.spring.io/spring-batch/reference/readers-and-writers/custom.html
https://jojoldu.tistory.com/339
https://jojoldu.tistory.com/140
- N + 1 
https://jojoldu.tistory.com/414

- 참고
https://blog.naver.com/rinjyu/222847088860
https://spring.io/projects/spring-batch#learn
