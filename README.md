### Intern Project  
## 운동 처방 프로젝트(BackEnd)  
- 개발환경 : IntelliJ, GitLab, Java, Spring, Gradle, PostgreSQL 
- 측정 관련 테이블 구축 및 데이터 입력
- 관리자 페이지 측정 관련 CRUD API 개발(측정 프로그램, 장비, 방법, 값, 항목 등)
- 사용자 페이지 측정 항목 선택 및 측정값 입력 등의 API 개발

------
## 채팅 프로젝트(BackEnd, FrontEnd)
- 개발환경 : IntelliJ, Java, SpringBoot, Gradle, MySQL, AndroidStudio
- [BackEnd](https://github.com/dayeondayeon-internship/CloudChat_Server)
- [FrontEnd](https://github.com/dayeondayeon-internship/CloudChat_Android)
### TextWebSocketHandler override
- afterConnectionEstabilshed : 웹 소켓 연결이 되면 호출
- handleTextMessage : 메시지 발송시 동작되는 메소드
- afterConnectionClosed : 웹 소켓 연결 종료되면 호출
[ afterConnectionEstablished]
```java 
public void afterConnectionEstablished(WebSocketSession session) {
        tempSessions.put(session.getId(), session);
}
``` 

[ handleTextMessage ]
1. message, user, room이 유효한 값인지 검사
2. roomSession에 방 객체 들어가 있는지 확인
- 존재하지 않는다면 먼저 껍데기만 넣어 두고, 메시지로부터 <roomIdx, <sender(userIdx), session>> 정보 조합해서 roomSession에 저장
- Map<Long, Map<Long, WebSocketSession>> roomSessions : < roomIdx, < UserIdx, session >> 를 저장.
    - roomIdx, UserIdx는 receiveMessage로부터 추출, session은 tempSession
```java 
if (!this.roomSessions.containsKey(receiveMessage.getRoomIdx())) {
    Map<Long, WebSocketSession> r = new ConcurrentHashMap<>();
    this.roomSessions.put(receiveMessage.getRoomIdx(), r);
}

// 해당 룸의 Map에 session이 없으면 session을 넣음.
if (!this.roomSessions.get(receiveMessage.getRoomIdx()).containsKey(session.getId())) {
    this.roomSessions.get(receiveMessage.getRoomIdx()).put(receiveMessage.getSender(), session);
}

if (this.tempSessions.containsKey(session.getId())) {
    this.tempSessions.remove(session.getId());
}
```
3. 최초의 방 입장이라면 entrance 테이블에도 저장
4. 주고받는 메시지를 message 전송 및 테이블에 저장
- 현재 접속중이 아닌 방의 입장한 사람에게는 notification 전송

### sendNotification
1. 방의 정보, 입장한 사용자 정보 find
2. 보내야 할 대상자 : 메시지를 보내는 사람, 세션에 접속중인 사람, firebase 토큰 값이 저장되어 있지 않은 사용자를 제외한 나머지
3. 메시지를 보내는 사람의 닉네임 + 메시지 내용으로 알림을 보내기

-----
### Team Project  
- since 2020 ~ 
## [LittleTown(BackEnd)](https://github.com/dayeondayeon/LittleTown_BackEnd)
- 개발환경 : IntelliJ, Git, Java, SpringBoot, Gradle, MySQL, AWS RDS, EC2
- 농촌 배경 농작물 수확게임으로 주인공은 다른 유저에게 메일 쓰기, 옷 정리, 창고 정리가 가능 
- [ [API](https://github.com/dayeondayeon/LittleTown_BackEnd/wiki) ] : 로그인, 회원가입, 현 상태 저장하기, 이메일 작성/수신, 옷장 관리, 창고 수납 등 구현
- AWS 환경 구성, 데이터베이스 설계 및 구축 


## [Spectory(BackEnd)](https://github.com/TeamSpectory/Spectory_BackEnd)
- 개발환경 : IntelliJ, Git, Java, SpringBoot, Gradle, MySQL, AWS RDS, S3, EC2
- 자기소개 작성을 위한 경험 정리용 어플리케이션 
- 로그인(jwt 발급), 회원가입, 프로필 조회, 글 작성하기, 게시글 상세정보 확인하기 구현
- AWS 환경 구성 및 데이터베이스 구축

## [MateMate](https://github.com/Rabbit-Squad)
- 개발환경 : VScode, Git, Java, AndroidStudio, JavaScript, NodeJS, MySQL, AWS EC2, RDS
- 함께 밥 먹을 식사 모임을 위한 어플리케이션 
- 안드로이드 개발
  - 로그인, 회원가입 시 유효성 검사, 글 및 신청자 목록, (kakao place API) 위치 검색 및 상세 정보
- 서버 개발
  - 로그인, 전체 글 조회, 내가 작성한 글 조회, 참여 신청, 내 게시글 신청자 조회 기능 서버 API 개발
  - AWS 인프라 한경 구축
  - 추후 MVC 패턴 적용을 통한 리팩토링 작업 수행 

## [MemoryMap(Android)](https://github.com/MEMORY-MAP-BY-TTUKDDAK/MMAP-ANDROID)
- 개발환경 : AndroidStudio, Java, Git
- 여행기록을 지도에 시작적으로 보여주는 어플리케이션 서비스
- 회원가입, (GooglMap API) 위치정보 지도에 표시 및 사용자로부터 입력받은 장소의 위치정보 계산, 글 상세 정보 수정, 로그아웃
- Retrofit2 사용한 서버-클라이언트 통신 구현 