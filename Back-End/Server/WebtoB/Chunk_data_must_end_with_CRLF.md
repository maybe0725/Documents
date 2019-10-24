# WebtoB error : Chunk data must end with CRLF

<br/>

안녕하세요. 티맥스소프트 엔지니어입니다.

WebtoB error 로그 중 Chunk data must end with CRLF. 메시지 발생으로 문의주신 내용 가이드 드립니다.

[매뉴얼]

MaxDechunkSize

- 종류: Numeric
- 단위: bytes
- 범위: 0 ~ INT_MAX
- 기본값: 10485760
- Chunked 요청을 처리하는 과정에서 de-chunk할 때 허용할 body의 최대 크기를 설정한다.
- 설정된 값보다 chunk body가 큰 요청은 "500 Internal Server Error"로 응답한다.

WebtoB의 http.m(환경파일)에서 \*NODE 절의 MaxDechunkSize 값을 조정해 주시기 바랍니다.

위 매뉴얼에서 확인할 수 있듯이 default는 MB이며 단위는 bytes입니다.

[ex]

```
*NODE
tmax
.....
MaxDechunkSize = 10485760,
.....
```

10MB 이상으로 설정해보시기 바랍니다.
