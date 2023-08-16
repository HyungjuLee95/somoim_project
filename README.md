# somoim_project
### 제작기간 : 6월 9일 ~ 7월 15일 
### 맡은 부분 
  > 페이지 CSS 파트, comment(댓글), reply(대댓글), 쪽지 기능, 고객센터, 이벤트 페이지(관리자 전용 CRUD),좋아요 기능, bug report, 소모임 가입 인원 추가, <br>
  > 소모임 가입 여부에 따른 메뉴 확인
### 개발 스택
1. Frontend : html, css, js
2. Backend : js, mybatis, sts
3. Database : mongoDB, Oracle


#### DB 설계(전체)
ORACLE SQL base.

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/79b75eb7-38e4-4710-ac1f-a909cf2a2efe)
---
### Comments Part DB설계

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/be8e2f70-fb41-4095-b3de-84eab0e6fc45)

 > Db에 대한 설명
>  Comment와 reply의 테이블을 따로 두지 않고, 동일하게 구성을 하였습니다. 왜냐하면 댓글과 대댓글을 "특정 게시글에 아랫단에 달리는 글"이라는 공통 속성으로 생각을 했을 때, Depth라는 칼럼을 둔다면 테이블을 따로 두지 않고 mapper를 통해 sql문 자체적으로 해당 기능을 수행할 수 있을 것이라고 판단했습니다.<br><br>
> 하여, 소모임이 개설되고 소모임 별 게시판에 있는 글에 댓글이 달리는 것으로 소모임마다 부여가되는 Somoim_num과 게시글마다 부여가 되는 Somoim_board_num을 기본 베이스로 구분 인자로 두는 칼럼을 생성했습니다.<br><br>
> 처음 댓글과 대댓글을 고민할 때, 어떻게 특정 댓글에 작성이 된 대댓글의 정보만을 구분해올 수 있을까? 라는 고민을 하였으며, 고민의 결과로 아래의 칼럼을 생각했습니다.<br><br>
> 대댓글의 경우에는 위의 두가지는 베이스로 가지되, 추가적으로 대댓글(reply)가 달리는 부모 댓글의 고유 번호(num)을 두었습니다.<br><br><br>

> 이미지를 보면 외래키로 som_board_num과 som_member_num을 두었는데, 그 이유는 처음 고민을 하던 당시에<br><br>
> 1. 소모임의 멤버가 아닐 경우, 댓글 및 대댓글을 작성할 수 없다.<br><br>
> 2. 설계 당시 각 게시물의 경우, DB 테이블에서 고유의 값을 갖는다.<br><br>
> 이 두가지에 대한 고려로 외래키를 설정하였습니다.<br><br>

---

### 좋아요(Good_count) DB 설계

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/ef6bed75-60f0-403f-a898-da33aefd2e3b)

> 소모임 좋아요 기능의 경우에는 "소모임에 가입된 인원"이 글을 확인하고 누를 수 있는 기능입니다.<br><br>
> 즉, 소모임에 가입된 인원인 경우에만 해당 소모임에서 사용할 수 있는 기능입니다.<br><br>
> 위에 제가 맡은 파트에서 팀원들과 의논을 하여, "소모임 비가입자"는 소모임의 메인페이지 부분만 확인가능하며, 다른 메뉴는 보이지 않게 설정하기로 하였습니다.<br><br>
> 그렇기 때문에 "애초에 메뉴가 보이지 않을 것이기에" 별도에 fk 설정없이 설계하였습니다.<br><br>
> 또한, "좋아요" 표시가 있다면, 본인이 선택했던 좋아요에 대한 "취소"가 있어야합니다. 이는 오늘하고 브라우져를 종료하지 않고 할수도, 브라우져를 종료하고 할수도, 일주일 뒤에 취소를 할 수 있습니다.<br><br
> 그렇기에 언제든 좋아요 및 취소를 할 수 있도록 단순하게 게시글(List_num = somoim_board_num)을 기록하는 칼럼과 좋아요 기능을 활성화한 유져의 id를 기록하도록 하였습니다.<br><br>
> good_count 칼럼의 경우에는 좋아요를 활성화 했느냐(0 or 1)로 구분을 하는 인자입니다.<br><br>
> 본래 목표를 sql문을 통해 mapper로 구분해 내겠다를 목표로 잡았기에 추가 칼럼을 두었습니다. <br><br>
> 다만, 칼럼이 굳이 필요하느냐?를 생각해보았을 때, 코딩을 통해 해당 리스트에 List_num과 user_id가 일치하는 항목이 있느냐 없느냐로 구분을 할 수 있는 점을 항시 염두해두었습니다.

---
### 쪽지 기능

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/73b63795-52d8-4241-995b-73d1a18bc20a)

> 쪽지 기능의 경우, 아래와 같이 DB를 설계
---
#### MSG_TITLE		쪽지 제목
#### RECEIVER_ID		수신자 아이디
#### GUBUN	수신/발신 구분
#### CREATE_DATE	수신/발신 날짜
#### MSG_CONTENT	쪽지 내용
#### SENDER_ID		발신자 아이디
#### USER_ID		유저 아이디
#### READ_YN		읽음표시
---
> 
