# 프로세스 조회(jar를 찾아야한다!)
ps -ef | grep 프로세스명

<img width="1523" alt="image" src="https://user-images.githubusercontent.com/74396651/230437881-20817ba2-af99-4a90-8384-7a06ff7ebf46.png">

# pid 찾아서 출력
ps -ef | grep 프로세스명 | awk '{print $1, $2, ...}'
<img width="1203" alt="image" src="https://user-images.githubusercontent.com/74396651/230437819-a47228c9-1867-4bda-8256-fc87eae91d30.png">

# pid 죽이기
kill -9 pid
<img width="844" alt="image" src="https://user-images.githubusercontent.com/74396651/230437993-f2894512-1daa-4f73-9463-53e7af58262d.png">
