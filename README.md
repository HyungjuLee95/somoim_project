# somoim_project
### 제작기간 : 6월 9일 ~ 7월 14일 (테스트 및 설계 포함 / 실 코딩 기간 2주 정도)
### 맡은 부분 
  > 페이지 CSS 파트, comment(댓글), reply(대댓글), 쪽지 기능, 고객센터, 이벤트 페이지(관리자 전용 CRUD),좋아요 기능, bug report, <br>
  > 소모임 가입 여부에 따른 메뉴 확인, 메인 화면 페이징(더보기)
### 개발 스택
1. Frontend : html, css, js
2. Backend : js, mybatis, sts
3. Database : mongoDB, Oracle

### 목표
> 원하는 기능을 아주 간단하게 표현할 수 있도록 해보자


#### DB 설계(전체)
ORACLE SQL base.

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/79b75eb7-38e4-4710-ac1f-a909cf2a2efe)
---
### Comments Part DB설계

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/be8e2f70-fb41-4095-b3de-84eab0e6fc45)


![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/b5a1a769-e6c2-467a-8562-afc6fb824a1b)

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

### 좋아요(Good_count) 

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
### 쪽지 기능(Message)

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/73b63795-52d8-4241-995b-73d1a18bc20a)
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/7dc1839b-89df-459c-9703-387cafcf9e9c)

> 쪽지 기능의 경우, 아래와 같이 DB를 설계
---
##### MSG_TITLE		쪽지 제목
##### RECEIVER_ID		수신자 아이디
##### GUBUN	수신/발신 구분
##### CREATE_DATE	수신/발신 날짜
##### MSG_CONTENT	쪽지 내용
##### SENDER_ID		발신자 아이디
##### USER_ID		유저 아이디
##### READ_YN		읽음표시
---
> 웹소켓을 이용하려 하였으나, 시간적인 문제로 인하여 LongPolling 기능을 이용.
> LongPolling을 선택한 이유 : LongPolling은 일정 시간마다 서버에 요청을 보내 다시 응답을 받는 것입니다. 그렇다면 지속적으로 서버에 요청이 들어가기 때문에 이것도 서버에 부담이 될 수 있습니다.<br><br>
> 그렇지만 마이페이지에 쪽지함이 있으며, 우리 페이지의 주목적은 소모임 게시판의 활용이며, 소모임 게시판을 이용하여 소통하는 것입니다. <br><br>
> 이 말은 즉, 마이페이지에 머물러있는 시간보다 소모임 게시판에 머물러있는 시간이 많은 것입니다.
> 하여, 마이페이지 하단에 위치하고있는 쪽지 기능의 경우, LongPolling으로 진행을 하여도 서버에 큰 부담이 없을 것으로 판단을 하였습니다.
> Js를 통해 구성을 하였으며, Controller에서 전체 내역에 대한 목록을 json형태로 반환해주면 ajax를 통해 데이터를 처리하며, <br><br> setTimeout(longPolling, 5000); 와 재귀함수의 형태로 longpolling 방식을 이용하여 진행하였습니다.
>
---
### Comments and Reply 코드 설계

   BoardVO

```
@Data
public class Somoim_BoardVO{

    private int num;
    private String title;
    private  String content;
    private String save_image;
    private Date write_date;
    private int view_count;
    private int good_count;
    private int vote_num;
    private int som_member_num;
    private int somoim_num;
    private String user_id;
    private MultipartFile file;
    private String save_name;

    private int startRow;
    private int endRow;

}

```
Controller
---
```

 @RequestMapping(value = "/join_selectOne.do", method = RequestMethod.GET)
    public String join_selectOne(Somoim_BoardVO vo, Model model,Somoim_Question_VoteVO qvo,Somoim_Choice_Vote ch_vo) {
        log.info("join_selectOne.do().....{}", vo);
        List<Somoim_BoardVO> infos = service.select_user_info();
        log.info("infos..{}", infos);


                String userId = (String)session.getAttribute("user_id");
                Somoim_BoardVO user_profile= new Somoim_BoardVO();
                user_profile.setUser_id(userId);

                Somoim_BoardVO User_save_name = service.LOGIN_ID_PROFILE(user_profile);

                model.addAttribute("User_save_name", User_save_name);


//--------------------------------------
        Somoim_BoardVO vo2 = service.selectJoin(vo);
        log.info("test...{}", vo2);
        for (Somoim_BoardVO info : infos) {
            if (vo2.getUser_id().equals(info.getUser_id())) {
                vo2.setSave_name(info.getSave_name());
            }
        }
//--------------------------------------

        model.addAttribute("vo2", vo2);

        if(vo2.getVote_num() != 0){

            log.info("getVoteNum..."+vo2.getVote_num());
            qvo.setNum(vo2.getVote_num());

            Somoim_Question_VoteVO qvo2 = service.SELECT_VOTE_NUM(qvo);
            log.info("qvo2_test...{}",qvo2);

            model.addAttribute("qvo2", qvo2);

            ch_vo.setSom_qvote_num(qvo2.getNum());
            List<Somoim_Choice_Vote> ch_vo2 = service.SELECT_CHOICE_ITEM(ch_vo);
            for(Somoim_Choice_Vote vo3 : ch_vo2){

            }

            model.addAttribute("ch_vo2",ch_vo2);




        }


//--------------------------------------


        som_commentsVO cvo = new som_commentsVO();
        cvo.setSom_board_num(vo2.getNum());
        cvo.setSave_name(vo2.getSave_name());
        System.out.println("vo2.getNum!!!!!!!!!!!!:" + vo2.getNum());
        cvo.setSomoim_num(vo2.getSomoim_num());
        System.out.println("cvo:" + cvo.toString());
        List<som_commentsVO> coms = commService.selectAll(cvo);

// 프로필이 변경이 되었는지 아닌지를 비교하여, 프로필이 변경이 되었을 경우, 변경된 프로필로 set 될 수 있도록 하는 부분 #프로필 변경
        for (som_commentsVO com : coms) {
            for (Somoim_BoardVO info : infos) {

                if (com.getUser_id().equals(info.getUser_id())) {
                
                    if (!com.getSave_name().equals(info.getSave_name())) {
                       
                        com.setSave_name(info.getSave_name());
                    
                    }
                }
            }
            log.info(com.toString());

        }


        System.out.println("coms:" + coms.toString());


        som_comm_commentsVO c_cvo = new som_comm_commentsVO();
        c_cvo.setSom_board_num(cvo.getSom_board_num());
        c_cvo.setSomoim_num(cvo.getSomoim_num());
        log.info("cvo.getnum..{}", cvo.getNum());
        List<som_comm_commentsVO> c_coms = new ArrayList<som_comm_commentsVO>();
        c_coms = c_commService.selectAll(c_cvo);


        System.out.println("c_cvo:" + c_cvo);


        System.out.println("c_cvo.getSom_board_num:" + c_cvo.getSom_board_num());
        System.out.println("c_coms:" + c_coms.toString());


        model.addAttribute("coms", coms);
        model.addAttribute("c_coms", c_coms);
        service.vvcountup(vo);

//--------------------------------------

//        String user_id = (String) session.getAttribute("user_id");
        vo.setUser_id(user_id);
        Somoim_BoardVO good_count_mem= service.select_all_goodList(vo);
        log.info("user_id..{}", user_id);
        model.addAttribute("good_count_mem", good_count_mem);
        log.info("good_count_mem..{}", good_count_mem);

//--------------------------------------
        return "board/join_selectOne";
    }
```
### 겪었던 문제 
1) 기존 코드가 완성된 상태에서 유져가 변경했을 시 바로 반영이 되어야하는데, 이에 대한 board에서의 profile 사진 변경에 대한 DB 칼럼이 구성되어 있지 않음을 확인하였습니다.
> detail > 댓글 대댓글에는 댓글을 작성한 사람의 프로필 사진이 들어갑니다. 최초에 가입을 진행한 상태에서 유져의 개인 프로필을 변경했을 경우, 변경 전에 작성되어 있는 댓글 혹은 대댓글의 프로필 사진은 변경이 되지 않고, 변경 후에 새로 작성된 댓글 대댓글에만 변경된 프로필이 적용되는 문제를 확인하였습니다.<br>
>   
> 해결 > 우선, 코드가 구성이 된 상태와 협업으로 진행이 되는 과정 중에 테이블을 수정할 경우, 문제가 발생할 수 있음을 고려하여 어떠한 부분에서 프로필이 변경이 될 경우, 가장 직접적으로 변경이 되는 부분을 확인하였고, 그 부분의 값을 불러와 해당 값을 get해서 필요한 객체에 set을 시켜 필요한 정보를 불러와 진행하였으며, 원했던 변경 후 바로 적용이 가능하게 완성하였습니다.(#프로필 변경 부분 참고)

2) 게시글에 대해서 좋아요를 눌렀을 때, 좋아요 취소가 되지 않았던 문제.(정확하게는 3일, 일주일 뒤 취소를 어떻게 할 것인가?)
> detail > 어띠보면 단순한 문제일 수 있습니다. 브라우져에 로그인하였을 때, 세션에 유져의 ID가 저장이 되도록 설정을 해놓았기에, session에 저장된 아이디와 비교하요 최초 좋아요를 눌렀다면 좋아요 카운트가 올라가게하며, session에 저장된 아이디와 추가로 session에 넣어준 좋아요에 대한 id가 같으면 취소 버튼이 활성화될 수 있도록 할 수도 있습니다. 다만, 문제는 좋아요라는 것은 지금은 좋아요를 눌렀으나 하루 이틀 혹은 일주일 뒤에 좋아요를 취소할 수 있어야합니다. 그렇다면 session에 저장하는 것만으로는 어려움이 있을 수 있습니다.<br
> 
> 해결 > 좋아요에 대한 table을 따로 생성했습니다. 이 table의 역할은 좋아요를 눌렀을 경우, 해당 게시글을 찾을 수 있는 아주 간단한 최소 칼럼으로 구성되어 있습니다. 하여, 어떠한 유져가 좋아요를 눌렀다면 해당 user_id와 소모임에 대한 정보를 넣어주는 것입니다. 이렇게한다면 좋아요 취소의 경우, 해당 db에 데이터가 있어 null 결과값이 null이 아니라면 좋아요 취소 버튼이 활성화되고, 취소를 한다면 해당 리스트에서 취소를 누르는 좋아요 기록을 삭제만 해준다면 이후에 별도에 다른 작업 없이 좋아요를 다시 누를 수 있도록 하였습니다.

3) 추가 번회로 css나 html 구조에 대한 문제
> 흔히 배웠던 것들로는 input과 form action을 사용하여 간단하게 처리할 수 있다. 이것은 근데 form 안에 버튼이 있어야하며, 값이 제출이 되어야한다. 그렇다면 보통은 버튼 옆에 input이 있어야하며, 아무리 margin padding 등으로 이쁘게 조절을 하여도 이쁘게 조절이 되지 않는다.  다른 부분에 input 태크에 클래스를 부여하고, 그 클래스가 설정된 input에 값이 설정될 수 있도록 js를 통해 진행하였습니다.
> 해당 코드 참고
---
```
>    function replaceWithForm(button) {
        var parentDiv = button.parentNode;

        var form = document.createElement('form');
        form.action = 'som_dcomm_insertOK.do';

        var div = document.createElement('div');
        div.className = 'join_commnets_insert_section';

        var inputText = document.createElement('input')
        inputText.type = 'text';
        inputText.placeholder = '대댓글 작성';
        inputText.name = 'content';
        inputText.value = '${c_com.content}';
        inputText.style.width = '80%';
        inputText.style.borderTop = 'none';
        inputText.style.borderLeft = 'none';
        inputText.style.borderRight = 'none';




        var buttonSubmit = document.createElement('button');
        buttonSubmit.type = 'submit';
        buttonSubmit.textContent = '댓글 작성';
        buttonSubmit.style.marginLeft = '10px';
        buttonSubmit.style.width = '80px';
        buttonSubmit.style.height = '40px';
        buttonSubmit.style.fontSize = '0.7rem';
        buttonSubmit.style.textAlign = 'center';
        buttonSubmit.style.justifyContent = 'center';

        var arrowRightIcon = document.createElement('i');
        arrowRightIcon.className = 'fa-solid fa-arrow-right';


        div.appendChild(arrowRightIcon);

        div.appendChild(inputText);
        div.appendChild(buttonSubmit);

        form.appendChild(div);

        parentDiv.replaceChild(div, button);
    }
</script>
```
---

### 좋아요(Good_count) 코드 설계

#### -join_selectOne.do에서 사용된 좋아요
> ![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/1d2fc52d-aee0-4626-81ec-92c4bc2f818c)

용도 > 좋아요 버튼과 좋아요 취소의 경우에 위에 설명한 것과 같이 리스트에 해당 값이 null인가 아닌가에 따라서 좋아요 버튼과 취소 버튼이 활성화됩니다. 하여, 이 부분을 통하여 좋아요 table에 값이 null인지 아닌지를 판단해주는 지표라고 생각할 수 있습니다.<br>

good_count_mem 은 DAOimpl에서 goodcountmem과 연결이 되어있습니다. 여기서 sqlsession을 이용하고 있으며, 이용된 쿼리문은 good_count_list에서 목록이 있는지 없는지를 확인하고있으며, 이 반환관은 값이 있으냐(!=null), 겂이 없느냐(=null)로 정보를 반환하게 됩니다. 이에 따라 아레 html에서 해당 정보를 이용하여 원하는 정보를 표시합니다.

#### -좋아요 관련 메서드
> ![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/b556d8d3-fcd8-43b1-b1de-df504e221211)  <br>
> 용도 > adding_good_count_list라는 것은 좋아요를 누를 경우, 해당 좋아요를 나중에라도 취소할 수 있도록, 그리고 좋아요한 게시글을 따로 표시해줄 때 편리하기 위해, 필요하기 때문에 리스트에 담는 것을 의미합니다.<br>
> 생각해본다면 DB TABLE에 추가되어 있는 것들에 대해서 해당 게시글에 대한 정보를 포함한 데이터 수를 counting한다면 그것이 좋아요 갯수가 되는 것이기도 하기에 위에 말한 나중에 취소할 수 있는 것, 좋아요한 게시글을 따로 표시해주는 것, 좋아요 갯수를 표시하는 것에서 모두 편리성을 가져갈 수 있을 것으로 파단이 되어 팀원들에게 위의 방법에 대한 이야기에 대해 의견을 전달 및 반영하여 계획하고 도입하였습니다.

### -html에서의 이용
> ![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/45b53346-b444-4f03-99d1-af6f956264ae)

> Community 부분에도 해당 부분을 팀원에게 설명해주며, 위와 비슷한 방식으로 코드 구성을 할 수 있도록 도와주었습니다.


---
### 관리자 전용 페이지에 대한 CRUD / 

이벤트 페이지, 공지 등 유져가 사용하는 것이 아닌 관리자자의 직접적인 CRUD가 필요한 부분에 대해서는 작성자만 글을 작성하고 수정, 삭제할 수 있어야합니다.<br>
아래와 같은 부분을 고민하였습니다.<br>
1) js를 통하여 특정 아이디로 로그인 했을 때, 작성 혹은 삭제의 버튼이 확인될 수 있도록 한다.
2) 테이블을 따로 만들어서 해당 아이디에 대한 관리자 권한 확인을 서버에서 진행 후, 정보를 반환하여 처리한다(참일 경우 !=null, 거짓일 경우 =null)

두 가지 방법에 대하여 의논하여 결정했던 내용은 1번이었으며, 이유는 "관리자 계정은 몇개 되지 않을 것이므로 js를 통하여 관리자 계정을 통하여 추가해주자"라는 의견이 많아서 1번으로 선택 후 진행하였습니다.<br>
다만, 1번의 경우를 고려하였을 때, 별도로 설정을 해주지 않는다면 개발자 도구에서 코드를 확인하였을 때, 관리자 계정이 쉽게 드러날 수 있다는 문제가 있었으나, 우선적으로 진행하기로 하였습니다.<br>

진행하였던 js 코드는 아래와 같습니다.

```

 <c:if test="${user_id eq 'tester'}">
            <a href="InsertEvents.do"><button>작성</button></a>
<!--             관리자 계정 "tester"만 보이는 메뉴 --></c:if>
```
단순하게 처리하였지만, db TABLE을 따로 두어 구성하는 것이 더욱 안정적일 수 있겠다라는 생각을 하였습니다.



### 소모임 가입 여부에 따른 top 메뉴 확인 
이 부분은 아래의 부분입니다. 소모임이 개설이 된 후, 비가입자가 해당 소모임을 방문하였을 때에는 소모임 페이지 내에서 아래 화면이 확인이 됩니다.
#### 비가입자가 확인한 소모임 내 상위 메뉴
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/0ea0961b-8b87-49fc-827e-f80d38a65723)

#### 가입 후, 소모임 내 상위 메뉴
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/f7e19348-ce03-4ca2-8eb8-ee3cc00667ba)
<br>
비가입자는 홈 메뉴만 확인이 되며, 가입자의 경우에는 모든 메뉴가 확인이 되고 있습니다.
아래 코드를 이용하였습니다.
```
<c:set var="member_menu_check" value="false" />
<c:set var="som_member_check" value="false" />

<%-- somoimUser_id 리스트를 돌려 일치하는 값이 있는지 확인 --%>
<c:forEach items="${somoimUser_id}" var="som_user_id">
    <c:if test="${som_user_id.user_id eq user_id}">
        <c:set var="member_menu_check" value="true" />
        <c:set var="som_member_check" value="true" />
        <jsp:include page="./som_top_menu.jsp"></jsp:include>
    </c:if>
</c:forEach>

        <%-- member_menu_check 변수가 false일 경우에 출력 --%>
        <c:if test="${!member_menu_check}">
            <div class="join_gnb">
                <li><a href="som_selectOne.do?num=${num}">홈</a></li>
            </div>
        </c:if>
```
우선, 구성할 때의 생각은 이렇습니다.
1) 멤버인지 아닌지, 어떤 메뉴를 보여줄지.
이에따라 JSTL과 JSP를 이용하여 구성하였으며, 생각한 것은 처음 member_menu_check이라는 변수와 som_member_check이라는 두개의 변수를 flase로 초기화를 해주고 <br>
,somoimUser_id(somoim에 가입한 유져 아이디 리스트)를 el로 받아 foreach를 사용하여 user_id와 일치하는 값이 있는 지를 확인하여 각 변수의 true로 설정해줍니다.<br>
이것을 토대로 어떠한 메뉴를 표출할지 flase, true 의 값에 따라 해당 값을 출력하게 합니다.


### 홈화면 페이징(더보기 처리)

#### 더보기 누르기 전 
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/73682f63-30f7-4c03-a6bf-ceb9d7f19d62)

위와같이 등록된 최신 모임 4개를 홈 화면에 출력합니다. 이후에 더보기를 누르면 다음 4개의 항목을 출력합니다.<br>
여기서 유져가 다음 더보기를 누른다면 모임에 대한 관심이 있음으로 판단을 하여, 전체 목록을 확인할 수 있는 페이지로 이동을 할 수 있도록 하였습니다.

#### 더보기를 누른 후(한번)
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/e8d710ca-5b07-4bc7-91a8-de432acd358e)

#### 더보기를 한번 더 누름(두번)
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/705df6ac-6ca1-4d5b-ac8e-ae26a4e21a95)
위와 같이 전체 페이지로 이동합니다.

구성한 코드는 아래와 같습니다.
```
<script type="text/javascript">
   $(document).ready(function() {
      var currentPage = 1; // 시작 페이지 번호
      var itemsPerPage = 4; //표시 항목 수
      $("#more_info").click(function() {
         currentPage++; // 페이지 번호 증가
         loadMoreItems();
      });
      function loadMoreItems() {
          if (currentPage > 2) {
              window.location.href = "som_selectAll.do";
              return;
          }
         $.ajax({
            url: "json_somoim_selectAll.do", // json 목록 가져오기
            method: "GET",
            data: { page: currentPage, itemsPerPage: itemsPerPage },
            dataType: "json",
            success: function(response) {
               console.log("ajax on");
               console.log("response", response);
               //불러온 항목 처리 및 가공, 출력(html에 추가)
               var items = response;
               var html = "";
               var startIndex = (currentPage - 1) * itemsPerPage;
               var endIndex = startIndex + itemsPerPage;
               if (startIndex >= items.length) {
                  // 요청한 페이지에 추가 항목이 없는 경우
                  $(".more_but").hide();
                  alert("모든 게시글을 표시했습니다!");
                  return;
               }
               if (endIndex > items.length) {
                  // 마지막 페이지에서 아이템의 인덱스 조정
                  endIndex = items.length;
               }
               for (var i = startIndex; i < endIndex; i++) {
                  var vo = items[i];
                  console.log("vo[i]...{}", vo);
                  console.log("vo.save_img...{}", vo.somoim_img);
                  console.log("o.som_member_num...{}", vo.category);
                  console.log("vo.title...{}", vo.som_title);
                  console.log("vo.write_date...{}", vo.create_date);
                  html += '<li>';
                  html += ' <div class="moimst_list_01_img_box">';
                  html += ' <img style="height: 100%" src="resources/uploadimg/' + vo.somoim_img + '">';
                  html += ' </div>';
                  html += ' <div class="main_selectList">';
                  html += ' <p class="hashtag">' + vo.category + '</p>';
                  html += ' <h1>' + vo.som_title + '</h1>';
                  html += ' <p class="sub_tit">' + vo.som_title + '</p>';
                  html += ' </div>';
                  html += '</li>';
               }
               $(".selectAJAX").append(html);
            },
            error: function(xhr, status, error) {
               console.error(error);
            }
         });
      }
   });
```
---
@RestController

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/357f6fa7-bc88-4b45-91f5-921c34ba7904)

---

전체 개설된 소모임에 대한 정보를 json으로 가져옵니다. 비동기 통신으로 서버에 리다이렉트 없이 더보기 화면이 표출되도록 진행하였습니다.<br>
기존 항목 4개는 기본적으로 두되, 이후, 기존 페이지 넘버로 하는 것과 비슷한 방식으로 2번째 페이지를 추가하는 느낌, 즉, 더보기 버튼을 누르면 기존 이미 보여진 4개는 두고, 추가적으로 두번째 페이지에 해당되는 4개의 항목이 추가적으로 생성이 되게 구상하고 이에 따라 코드를 작성하였습니다. <br>

#### 겪었던 문제는 기존 4개를 어떻게 표시할 것이냐? "페이징 처리" 자체에 대한 코드 구성 원리 및 인덱스에 관련한 식을 어떻게 구성할 것인가.
이 부분은 처음 페이징 처리를 진행하였던 것이라, 모든 것을 이해하고 넘어가려하니 시간이 조금 소요된 부분이 좀 있는 것 같습니다.<br>
다만, 이부분에 대해서 각각의 변수를 첫페이지를 출력하는 미리 출력하는 방식으로 생각하고 구성하였더니 조금 편리했으며, 이해한 후에는 간단하게 처리할 수 있었던 것 같습니다.<br>


# 프로젝트 결과 
![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/041749e3-f659-414a-b831-38bad10b03e7)

## 느낀점 ) 
### 혼자만의 힘으로는 할 수 있으나, 그것은 한계가 있을 수 있다. 협업은 중요하며, 효율적인 방식으로 진행할 수 있도록 모두 의견을 조율하는 것, 설계하는 것 또한 능력이다.
 프로젝트를 하며 느낀 점은 정말 많은거 같습니다. 그 중에 3가지를 꼽는다면 첫 번째는 협업의 중요성, 두 번째는 코딩에 대한 즐거움, 세 번째는 포기하지 않고 타협하지 않는 것입니다.
 설계부터 테스트 기간 다 포함해서 실 코딩 시간은 3주 조금 안되는 시간이었던 것 같습니다. 페이지를 본다면 분명, 그렇게 많은 페이지들이 아닙니다. 겉으로 보았을 때에는 이게 뭐 얼마나 힘이 필요하겠어 라는 생각이 들지 모르지만 실제로 한부분 한부분 코딩이 필요하면 현실적으로 그 기간동안 혼자했다면 처음 페이지를 만들어보는 저에게는 불가능했을 것입니다. 모든 팀원들이 파트를 나누고, 협업을 하고, 서로 새로운 부분, 공부한 부분을 공유하며 함께 성장하고, 함께 일구어낸 결과인 것 같습니다. 물론 여러 잡음들도 있었지만, 모두 같이 극복하며, 이에 따라 협업의 중요성을 느낄 수 있는 계기가 되었습니다.<br>
 
 두 번째는 코딩에 대한 즐거움입니다. 물리학과를 전공하여 본래 이것저것 뜯어보고 파악하고 관찰하고 실험해보고를 즐겼던게 습관들이 코딩에 대한 즐거움에 한 몫 했던거 같습니다. 이 얘기를 하는 이유는 프로젝트를 하면서 끊임없이 찾아보고 끊임없이 생각하고 잠도 안자고 했던 것 같습니다. 그 이유는 알면 알수록, 머리에 넣으면 넣을수록, 코딩을 하면 할수록 실패해도 재밋고, 이해가 안되어도 재밋기에 즐거움을 느꼇고, 이에 따라 포기하지 않고 할 수 있었던 것 같습니다.<br>
 앞으로도 지속적으로 공부를 하며, 더욱 정진할 수 있는 원동력과 경험이 된거 같습니다.<br>
 
 세 번째는 코딩을 하면서 느꼇던 것입니다. 이 직업이 새로운 기술이 빨리빨리 적용이 되고, 이에 적응하고 끊임없이 공부를 해야하는 직종입니다. 이게 무슨 말이냐?라고 한다면 만약 포기했더라면 기능을 좀 더 이쁘게, 편리하게 작성하지 못했을 것이며, 코드를 어떻게 구상할지에 대한 생각조차 할 수 없었을 것입니다. 또한 포기해버렸다면 아예 이 프로젝트에 대한 결과를 만들어 낼 수 없었을 것이며, 제가 느꼇던 저의 소중한 경험, 지식들을 갖지 못했을 것입니다. 이를 초심삼아 계속 초심으로 열심히 공부하고 해보고를 반복하며 멋지게 성장해야겠다는 생각을 하였습니다. 하여 이후에 기존 구상하였던 코드들에 대하여 다른 방식으로 수정을 해볼 것이며, 더욱 노력하여 볼 것입니다.
 
### 홈페이지 메인 

![image](https://github.com/HyungjuLee95/somoim_project/assets/111270174/4ab329ab-dcd2-42fe-b740-1ee7c450a5e4)
