# RTDB(Real-Time Database, 실시간 DB)

<br/>

> 출처: http://blog.i6020345.com/2018/10/16/RTDB-Real-Time-Database-%EC%8B%A4%EC%8B%9C%EA%B0%84-DB/

<br/>

제 입장에서 RTDB보단 RDB(Relational Database, 관계형 DB)가 익숙합니다 뭐 Oracle이나 MS-SQL, MariaDB 같은걸 사용해왔으니까요…

사실 이 글을 작성하는 (2018-10-16) 기준으로는 RDB도 실시간이지 않나? 라는 생각을 해봅니다만은..ㅠㅠ

그래서 용처에따른 구분을 하고싶어서 검색해본 결과로는

> RTDB (Real-Time Database)는 현장의 `운전 데이터 (Process Variable, Set Point, Output, Mode, 등)`를 실시간으로 수집하여 저장하여 시스템입니다.<br/><br/>
> 주로 RTDB의 데이터는 `Time Stamp, Tag, Description, Value, Confidence를 가진 단순 데이터`로써, ORACLE / MS-SQL 같은 RDB (Relational DataBase) 보다는
> 사용자가 찾을 때 빠르게 데이터를 지원할 수 있도록 `시간 순으로 ASCII 형태로 차곡 차곡 쌓아놓는 방식으로 저장` 합니다.<br/><br/>
> `물론, Tag 관련 정보 (Tag Name, Description, Data Type, Range, Set Value, 등)는 RDB에 저장하는 것이 효과적입니다.`
> 따라서 잘 설계된 RTDB는 RDB와 상호 연결이 쉽도록 구성되어 있습니다.<br/><br/>
> 또한, RTDB는 전문가가 아닌 현업을 잘 알고 있는 공정 엔지니어들이 운전 / 이력 / 생산 / 경보 / 설비 관리를 위하여 쉽게 할 수 있도록
> 다양한 Application 기능들이 포함되어 있어야 합니다.<br/><br/>
> DB사랑넷 - RTDB

에서 읽었던 댓글이 이해가 빠르게 되더라고요.

실제로 이번 참여하고있는 MES프로젝트에서도 RTDB의 용도는 `현장의 운전데이터에 집중되어 사용`되고 있네요
