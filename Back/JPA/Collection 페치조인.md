- Collection fetch join 쓰면 페이징 불가 -> 메모리에 정렬하는데 노답
  - dto에서 lazy로 가져오기 & bacth size
  - from 기준 toOne(객체)는 걍 다 fetch로
- Collection fetch join은 딱 1개밖에 못함(2개 안됨)

## 해결법
- Map, Set 으로 바꾸던가
- 분리하던가
