# OLTP / OLAP

<br/>

> 출처 - https://unabated.tistory.com/entry/OLTP-OLAP?category=270013

<br/>

## OLTP (Online Transaction Processing) - 온라인 트랜잭션 처리

네트워크상의 여러 이용자가 실시간으로 데이터베이스의 데이터를 갱신하거나 조회하는 등의 단위 작업을 처리하는 방식을 말한다. 주로 신용카드 조회 업무나 자동 현금 지급 등 금융 전산 관련 부문에서 많이 발생하기 때문에 ‘온라인 거래처리’라고도 한다. 이 방식의 특징은 기존 컴퓨터 통신에서 이용해 온 온라인 방식과 달리 다수의 이용자가 거의 동시에 이용할 수 있도록 송수신 자료를 트랜잭션(데이터 파일의 내용에 영향을 미치는 거래 ·입출고 ·저장 등의 단위 행위) 단위로 압축, 비어 있는 공간을 다른 사용자들이 함께 쓸 수 있도록 한 점이다.

## OLAP (OnLine Analytical Processing) - 온라인 분석 처리

OLAP는 사용자가 다양한 각도에서 직접 대화식으로 정보를 분석하는 과정을 말한다.OLAP 시스템은 단독으로 존재하는 정보 시스템이 아니며, 데이터 웨어하우스나 데이터 마트와 같은 시스템과 상호 연관된다. 데이터 웨어하우스가 데이터를 저장하고 관리한다면, OLAP은 데이터 웨어하우스의 데이터를 전략적인 정보로 변환시키는 역할을 한다. OLAP은 기본적인 접근과 조회·계산·시계열·복잡한 모델링까지도 가능하다. OLAP은 최근의 정보 시스템과 같이 중간매개체 없이 이용자들이 직접 컴퓨터를 이용하여 데이터에 접근하는 데 있어 필수적인 시스템이라 할 수 있다.

## OLAP 와 OLTP 의 차이점

OLTP : 현재 업무의 효율적인 처리에만 관심이 있음

OLAP : 의사결정에 도움되는 데이터 분석에 관심이 있음