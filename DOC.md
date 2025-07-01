## thymeleaf
 - 서버 사이드HTML 렌더링
   - 타임리프는 백엔드 서버에서 HTML을 동적으로 런더링 하는 용도로 사용된다.
 - 네츄럴 템플릿
   - 순수 HTML을 그대로 유지하면서, 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿 이라 한다.
 - 스프링 통합 지원
 
## thymeleaf 기본 기능
 - 선언 : <html xmlns:th="http://www.thymeleaf.org">
 - 기본표현식
   - 변수 표현식 : ${...}
   - 선택 변수 표현식 : *{...}
   - 메시지 표현식 : #{...}
   - 조각 표현식 : ~{...}


## text 표현식
 - th:text >> <span th:text="${date}">
 - 태그의 속성이 아니라 직접 데이터를 출력하려면 [[...]] 으로 표현하면된다. >> [[${data}]]
 - escape 
   - 웹브라우저 : Hello <b>Spring ! </b>
   - 소스보기 :   Hello &lt;b&gt;Spring!&lt;/b&gt;
   - 기본적으로 escape 됨으로써 < -> &lt;  > -> &gt;  처리된다.
 - unescape 되기 위해 : th:utext // [(...)]  하면된다.
 - 기본적으로 escaped 처리 되어 사용하는게 맞다.

## 변수 표현식 : SpringEL ${data} 
   - Object
      - ${user.username} = userA
      - ${user['username']} = userA
      - ${user.getUsername()} = userA
   - List
     - ${users[0].username} = userA
     - ${users[0]['username']} = userA
     - ${users[0].getUsername()} = userA
   - Map
     - ${userMap['userA'].username} = userA
     - ${userMap['userA']['username']} = userA
     - ${userMap['userA'].getUsername()} = userA

## 지역변수 사용
 - <div th:with="[사용할 변수명]=${변수에 들어갈 데이터}"> </div>
 - 해당 div 태그 내부에서만 사용할수 있는 지역변수이다.

## 타임리프의 기본 객체들
 - Request Parameter >> 타임 리프는 param.XXX 으로 간단하게 얻을 수 있다.
 - session >> session.XXXX 
 - 스프링빈에 직접 접근하는방법
   - ${@helloBean.hello('Spring!')} >>> ${@[beanName].[함수명](파라미터)}

## 유틸리티 객체와 날짜
 - **https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects
 - #message : 메시지, 국제화 처리
 - #uris : URI 이스케이프 지원
 - #dates : java.util.Date 서식 지원
 - #calendars : java.util.Calendar 서식 지원
 - #temporals : 자바8 날짜 서식 지원
 - #numbers : 숫자 서식 지원
 - #strings : 문자 관련 편의 기능
 - #objects : 객체 관련 기능 제공
 - #bools : boolean 관련 기능 제공
 - #arrays : 배열 관련 기능 제공
 - #lists , #sets , #maps : 컬렉션 관련 기능 제공

## JAVA8 날짜
 - LocalData, LocalDateTime, Instant를 사용하려면 추가 라이브러리가 필요하다. 


## Url
 - 타임리프에서 url 생성하려면 @(...) 문법을 사용한다.
   - () 된부분은 파라미터를 맵핑시켜주고 맵핑되지 않는 부분은 쿼리 파라미터로 쭉 붙는 형식이다.
   - th:href="@{/hello}" >>   /hello
   - th:href="@{/hello(param1=${param1}, param2=${param2})}"  >> /hello?param1=data1&param2=date2 
   - th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}" >>  /hello/data1/data2
   - th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}" >>  /hello/data1?param2=data2

## 리터럴
 - 소스 코드상에 고정된 값을 말하는 용어
 - 타임리프는 다음과 같은 리터럴이 있다.
   - 문자 : 'hello'
   - 숫지 : 10
   - 불린 : true, false
   - null : null
 - 변수가 아닌 문자열은 타임리프 속성으로 넣으려면 '' 에 넣어야 되나  `A-Z` , `a-z` , `0-9` , `[]` , `.` , `-`, `_` 같은 나열된 문자열은 사용하지 않아도 자동 으로 인식됨
 - 하지만 띄어쓰기가 중간에 있거나할경우는 '' 을 꼭 넣어줘야 리터럴로 인식된다.
 - 리터럴 대체 문자 !! >>>  |....|   ... 이안에서 사용되는 ${} 변수만 치환되어 문자열로 보이게된다.
 - <span th:text="|hello ${data}|">

## 연산자
 - Elvis 연산자 : ${...} ?: "데이터가 없습니다." >> ... 이 데이터가 존재하지 않으면  ?: 이후의 문자 또는 값이 출력된다.
 - No-Operation  : th:text=" ${...} ?: _ " >> ...에 데이터가 존재하지 않으면 해당 태그를 랜더링 하지 않고 기본 문자열이 보이게 된다.
   - <li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li> >> <li>데이터가 없습니다.</li> 노출
   
## 속성값 설정
 - 타임리프 태그 속성 > 기존 속성을 th:* 로 지정한 속성으로 대체하고, 기존 속성이 없으면 새로 만들어준다.
 - checked 
   - 기존에는 checked 속성이 있기만해도 checked 처리가 되지만
   - 타임리프에서 th:checkd="" 사용하면 true, false 에 따라서 체크 여부를 구분할 수 있다.
 - th:attr="disabled=${...} ? 'A' : 'B' >> disabled를 동적으로 처리하겠다는 의미고 ${...} 이 참이면 'A'를 거짓이면 'B' 값을 주겠다는의미 (삼항연산 가능)
 - th:attr="속성=값" >>> 단일 값 처리

## 반복문
 - th:each 를 사용한다
 - 반복 상태유지
    - <tr th:each="user, userStat : ${users}"></tr>
    - 두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명 user + Stat가 되어 사용할 수 있다.


## 조건부 평가
 - if, unless 제공됨
 - 해당 조건을 만족해야 해당 태그가 노출되고 그렇지 않으면 해당 태그는 날라간다.
 - switch, case 제공됨 
   - th:case="*" 는 switch문에 만족하는 조건이 없을때 사용되는 디폴트 조건이다.


## 주석
 - HTML 주석                  <!-- -->                                        >> 렌더링하지 않고 주석처리되어보임
 - 타임리프 파서 주석           <!--/* */-->  || <!--/*--> ...  <--*/-->        >> 해당부분은 아예 사라짐
 - 타임리프 프로토타입 주석      <!--/*/ /*/-->                                  >> 타임리프로 렌더링 된 경우에만 보이도록

## 블록 : 일반적인 반복으로 처리하기 힘들때 사용
 - <th:block th:each="user : ${users}"> <div>...</div><div>...</div> </th:block>
 - block안에 each 문의 size만큼  안쪽의 내용이 통으로 반복된다.
 - 즉 <div>...</div><div>...</div><div>...</div><div>...</div><div>...</div><div>...</div> 총6개의 div가 출력된다.
 - block 으로 묶어서 block 내부를 반복 시킬수 있다. <th:block> 은 런더링 시에 제거된다!

## 자바스크립트 인라인
 - 타임리프는 자바스크립트에서 타임리프를 편리하게 사용할 수 있는 자바스크립트 인라인 기능을 제공한다
 - <script th:inline="javascript">
 - html 내의 <script> 태그내에서 thymeleaf 문법사용을 편리하게 해주는 속성으로
 - 텍스트 렌더링 >> [[${user.username]] >> userA >> "userA"  
 - 내추철 템플릿 >> /*[[${user.username}]]*/ "test username";   
   - /*[[${user.username}]]*/ 값이 렌더링이 되면 해당값을 쓰고
   - 그렇지 않으면 "test username"; 값을 사용한다. >> 기본일시 무조건 이게 노출됨
 - 객체 처리를 진행해준다
   - 기존 : user.toString() 값이 노출되는데
   - 사용 : 객체를 JSON으로 변환하여 노출시켜준다.
 - 인라인 each
   - [# th:each="user, stat: ${users}"] var user[[${stat.count}]] = [[${user}]]  [/]    ... 안에서 해당 each문에 대한 변수를 사용할 수 있다.
   - 결과 >>
    - var user1 = {"username":"UserA","age":10}
    - var user2 = {"username":"UserB","age":20}
    - var user3 = {"username":"UserC","age":30}


## 템플릿 조각
 - 웹페이지를 개발 할때 공통 영역이 많다. 타임리프는 이를 위해 템플릿 조각과 레이아웃 기능을 지원한다.
 - ~{...} 를 사용하는것이 원칙 부분
 - ~{template/fragment/footer :: copy} 
   - template/fragment/footer 해당 경로의 파일에서
   - th:fragment="copy" 부분을 찾아서 사용한다
   - 변수도 사용 할 수 있다. th:fragment="copyParam (param1, param2)"
 
 - <footer th:fragment="copy">
 - <footer th:fragment="copyParam (param1, param2)">
    
 - <div th:insert="~{template/fragment/footer :: copy}"></div>
   - insert 의 경우는  insert 속성이 있는 <div> 태그 안쪽으로 해당 조각이 들어가는 모양새이고
 - <div th:replace="~{template/fragment/footer :: copy}"></div>
   - replace는 <div> 태그가 사라지고 해당 조각으로 대체 되는 모양새이다.
 - <div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터2')}"></div> 

## 템플릿 레이아웃
 - use
   -  template/layout/base 경로의 파일의 common_header 값을 가진 fragment를 찾아서 replace 하는데
   - title, links 값은 기존의 원본 태그의 하위의 title 태그 자체를 사용한다(::title, ::link), 
   - <head th:replace="template/layout/base :: common_header(~{::title},~{::link})"><title></title><link></link></head>
 - base
   - use 에서 보낸 ::title >> 태그 그자체를 replace 해준다   
   - <head th:fragment="common_header(title,links)">
   - <title th:replace="${title}">레이아웃 타이틀</title> // n 개의 경우 th:block을 통해 넣어준다.
   - <th:block th:replace="${links}" />
