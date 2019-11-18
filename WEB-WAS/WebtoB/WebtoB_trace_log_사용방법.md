# WebtoB trace log 사용방법

<br/>

WebtoB trace log 시작

- wsadmin -C "ll .hth -l TRACE”
- wsadmin -C "ll .hth -o dcR,dcW,dsR,dsW”

<br/>

trace log file 위치

- /sw/webtob/webtob4/log/trace

<br/>

WebtoB trace log 종료

- wsadmin -C "ll .hth -l INFO”
- wsadmin -C "ll .hth -o -dcR”
- wsadmin -C "ll .hth -o -dcW”
- wsadmin -C "ll .hth -o -dsR”
- wsadmin -C "ll .hth -o -dsW”
