# polling과 pulling의 차이

<br/>

### polling

서버에 결과를 주기적으로 요청하는 것.

즉, 클라이언트가 서버에 질의를 던지고 그 질의에 대한 결과를 서버가 대답해 주는 것.
(주기적으로 결과값을 계속 요청)

사용자 정의된 프로토콜을 이용한 서버, 클라이언트가 여기에 대부분 속함.

<br/>

### pulling

서버의 데이터를 주기적으로 가져가는 것.

즉, 클라이언트가 서버의 데이터를 알아서 가져가는 것.

클라이언트에서 Database 직접 쿼리해서 받아가는 경우가 대표적.
