# somoim_project
### 제작기간 : 6월 9일 ~ 7월 15일 
### 맡은 부분 
  > 페이지 CSS 파트, comment(댓글), reply(대댓글), 쪽지 기능, 고객센터, 이벤트 페이지(관리자 전용 CRUD), bug report, 소모임 가입 인원 추가
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
>  Comment와 reply의 테이블을 따로 두지 않고, 동일하게 구성을 하였습니다. 왜냐하면 댓글과 대댓글을 "특정 게시글에 아랫단에 달리는 글"이라는 공통 속성으로 생각을 했을 때, Depth라는 칼럼을 둔다면 테이블을 따로 두지 않고 mapper를 통해 sql문 자체적으로 해당 기능을 수행할 수 있을 것이라고 판단했습니다.<br>
> 하여, 소모임이 개설되고 소모임 별 게시판에 있는 글에 댓글이 달리는 것으로 소모임마다 부여가되는 Somoim_num과 게시글마다 부여가 되는 Somoim_board_num을 기본 베이스로 구분 인자로 두는 칼럼을 생성했습니다.<br>
> 처음 댓글과 대댓글을 고민할 때, 어떻게 특정 댓글에 작성이 된 대댓글의 정보만을 구분해올 수 있을까? 라는 고민을 하였으며, 고민의 결과로 아래의 칼럼을 생각했습니다.<br>
> 대댓글의 경우에는 위의 두가지는 베이스로 가지되, 추가적으로 대댓글(reply)가 달리는 부모 댓글의 고유 번호(num)을 두었습니다.<br><br>

> 이미지를 보면 외래키로 som_board_num과 som_member_num을 두었는데, 그 이유는 처음 고민을 하던 당시에<br>
> 1. 소모임의 멤버가 아닐 경우, 댓글 및 대댓글을 작성할 수 없다.<br>
> 2. 설계 당시 각 게시물의 경우, DB 테이블에서 고유의 값을 갖는다.<br>
> 이 두가지에 대한 고려로 외래키를 설정하였습니다.<br>
---


