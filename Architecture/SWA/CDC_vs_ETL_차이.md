# CDC vs ETL 차이

<br/>

원본글 링크 - [실전 DW 설계 - CDC vs ETL 차이](http://blog.daum.net/hadmond/10)

<br/>

사실 CDC든 ETL이든 원천(source) 데이터 정보를 추출하여 목표(target) 시스템에 적재하는 개념은 동일하나 그 방법과 사용목적, 적재수준에서 차이가 발생하게 됩니다.

CDC(Change Data Capture) 와 ETL(Extract-Transform-Load)의 차이점은 아래와 같습니다.

<br/>

## 1. CDC (Change Data Capture)

<br/>

### 1.1. 주목적

실시간(real time) 또는 준실시간(near real time)으로 원천 시스템(기간계, 업무계) 데이터를 읽어 들여 정보계 시스템 또느느 후선업무 시스템 등에 정보를 넘겨 어떤 처리를 하고자 할 때 사용하는 방식 입니다.

실시간 또는 준실시간에 대한 요건은 해당 사이트마다 서로 다르기 때문에 다 열거할 수는 없겠지만 한가지 예를 들명, 조기경보시스템을 운영하는 사이트에서 고객이 5천만원 이상을 인출할 시 조기경보 시스스템에서 실시간으로 경보(Alert)를 발생시키게끔 모니터링을 해 볼수 있겠죠.

<br/>

### 1.2. 사용기술

모든 거래 발생 이벤트를 추적해야 하므로, DBMS 의 종류에 관계없이 변경 이력을 관리하는 DB Archive Log 를 주로 사용합니다. CDC 솔루션의 데몬(Demon)이 떠서 일정 시간별로 archive log 를 읽어 들여 타겟 시스템에 쌓는 방식으로 대부분 작동하게 됩니다.

대다수의 경우 주로 실시간 기간계 복제용으로 사용하고 있으며 후선업무를 위해 특정 데이터 돛착을 알리는 이벤트를 포함하는 개발된 경우가 많습니다.

일부 사이트에서는 실시간으로 데이터를 추출할 원천 테이블에 트리거(trigger)를 걸어 처리하는 방식도 있기는 하지만, 트리거를 사용했을 때의 위험성(dead lock, 또는 오작동, 에러시 추적 어려움)이 너무 커서 권장할 만한 방법은 아닌 것 같습니다.

<br/>

### 1.3. 적재 수준

위에서 설명한 대로 원천 테이블이 아닌 시스템 테이블인 archive log 를 읽어 처리하므로 적재주기(시간, 일, 월)에 관계없이 모든 원천 데이터의 변경사항(inset, update, delete)이 적재 대상이 되게 됩니다.

<br/>

## 2. ETL (Extract - Transform - Load)

<br/>

### 2.1. 주목적

업무계(기간계) 시스템의 하루일과를 다 끝내고 저녁에 기간계 배치(batch) 프로그램까지 다 실행되어 기간계에서 필요한 집계처리까지 마무리한 상태에서 이를 데이터(원장, 거래, 집계)를 정보계 시스템에 넘겨 어떤 처리를 하고자 할 때 사용하는 방식 입니다.

<br/>

### 2.2. 사용기술

CDC 와 달리 시스템 테이블에 접근하는 것이 아니라 기간계의 원장테이블, 거래테이블, 집계테이블을 대상으로 그날 변경분을 찾아 변경부분만을 정보계 시스템에 전달하는 방식을 주로 사용합니다. 만약 변경분을 인식할 수 있는 컴럼이 존재하지 않는다면 매일매일 해당 테이블의 모든 데이터를 ALL-COPY 방식으로 적재해야 합니다.

따라서 DW 설계시에 ETL 대상의 원천테이블 목록이 정리되었다면 반드시 변경분만을 추출할 수 있는 환경이 되어 있는지 모든 테이블별로 확인을 해야 합니다.

가령 변경분만을 적재하는 방식으로 ETL 을 구성하려고 한다면, 아래의 3가지 조건을 만족하는지 체크해야 합니다.

```
[ETL 변경분 적재 방식 적용시 전제 사항]

(1) 변경분을 인식할 수 있는 컬럼이 존재하는가? (수정일시, 조작일시 등)
(2) 변경분을 인식할 수 있는 컬럼이 존재 한다면, 어떤 상황에서도 이 컬럼이 핸들링 되는가?
(3) 삭제시 해당 레코드가 물리적으로 삭제되지 않는가? (논리적 삭제는 OK, 삭제여부='Y' 등)
```

위의 조건을 만족하지 못하는 테이블에 대해서는 ALL-COPY 방식의 적재를 할 수 밖에 없습니다.

고객 테이블 이라든가, 거래내역 테이블 이라 든가 엄청 큰 테이블을 전체 적재 방식으로 하게 되면 부하가 상당하며 ETL 작업시간도 많이 소요되게 됩니다. 따라서 사전에 면밀한 검토가 필요합니다.

차후에 얘기 되겠지만 ALL-COPY 방식의 적재는 이력 테이블을 구축할 때 상당히 부담이 될 수 있습니다.

변경분을 알 수 없으므로, 전일 데이터와 오늘 가져온 데이터를 한건 한건 일일이 변경이 있었는지 비교하여 변경분을 찾아 낸 다음 이력을 쌓아야 하는 부담이 생기는 것입니다.

<br/>

### 2.3. 적재 수준

ETL 은 기간계의 원장테이블, 거래테이블, 집계테이블을 원처너 데이터로 하기 때문에 적재주기(시간, 일, 월)에 따라 적재 수준이 정해지게 됩니다.

즉, 적재 주기가 2시간 간격으로 정해져 있다면, 적재 수준은 2시간 마다의 최종 데이터로 한정되게 됩니다.

예를 들면, 9시 ~ 11시 데이터를 적재할 때 9시 5분에 update, 9시 50분에 delete, 10시 25분에 update 의 모든 이벤트를 알 수 없고 오직 11시, 그 시점의 데이터를 인식한다는 뜻입니다.

어찌보면 이처럼 적재 수준의 차이가 CDC와 ETL 의 가장 큰 차이라고 할 수 있습니다.

만약 적재 주기가 일별이라면 업무중에 발생된 모든 이벤트 중의 최종 이벤트만이 테이블에 남아 있으므로 적재 수준은 일(DAY)단위로 정해지겠습니다. 물론 모든 변경 이벤트가 필요하면 기간계의 이력 테이블을 정보계로 읽어 오면 변경 내역을 추적할 수 는 있겠죠.

<br/>

## 추가

<br/>

CDC와 ETL의 다른점은 여러가지가 있지만, 가장 큰 차이점은 서로 출발점이 다르다는 것이겠네요.

CDC는 데이터가 변경된 이벤트를 감지하고, 이 데이터를 처리하는데 초점이 맞추어져 있다면, ETL은 변경 이벤트 단위에 반응하기보다, 타겟 데이터 집합을 만들 소스 데이터 집합을 가져오는 것에 중점을 둔다는 점입니다.

따라서 CDC는 DB의 ARCHIVE LOG 나, TRIGGER 를 통해 발생된 변경 이벤트를 모아둔 TABLE 을 읽게 되는 것이고, ETL은 타겟테이블을 만들기 위한 소스테이블을 읽어 들이는 것입니다.

서로 둘의 지향하는 목적이 다르기에 ETL은 JOB 이라는 형태로 UI를 통해 처리프로세스를 개발하게 되고, 수많은 JOB들은 선 후 관계로 묶어 작업을 스케쥴링하는 모듈로 탑재되어 있습니다.

반면에 CDC는 어떤면에서는 EAI 성격이 더 강하다고 할 수도 있겠습니다.

정리하면, ETL도 물론 주로 전날 변경된 데이터를 가져오므로 CDC라 할 수도 있겠지만, 실시간 변경 이벤트를 잡아내어 처리하는 특화된 CDC 솔루션들이 존재하기에, 실시간 처리는 CDC, 하루정도의 갭을 가지고 일반 테이블을 대상으로 작업한다면 ETL 이라고 보시면 될것 같습니다.

<br/>
