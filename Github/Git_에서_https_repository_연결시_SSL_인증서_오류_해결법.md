# git 에서 https repository 연결시 SSL 인증서 오류 해결법

<br/>

## 개요

git 은 https repository 연결시 curl 을 사용하며 curl은 기본적으로 SSL 인증서 검증을 수행하며 많이 발생하는 원인은 아래의 2 가지이다.

<br/>

## CA bundle 이 없음

git 이 사용하는 curl 에 등록된 인증기관 인증서(ca certificate) 정보는 다음 명령어로 확인할 수 있다. (참고 http://curl.haxx.se/docs/sslcerts.html)

```sh
$ git config --global http.sslCAInfo
/bin/curl-ca-bundle.crt
```

Windows 용 git 의 경우 http.sslCAInfo 이 없을 경우 git 이 설치된 경로에서 ca bundle 파일을 찾게 되는데 이 파일이 없는 경우 아래와 같이 "error setting certificate verify locations" 에러가 발생한다.

```sh
$ git clone https://mygit.example.com/lesstif/testprj.git/


fatal: unable to access 'https://mygit.example.com/lesstif/testprj.git/': error setting certificate verify locations:

  CAfile: D:/devel/Git-2.9/mingw64/libexec/ssl/certs/ca-bundle.crt
  CApath: none
```

이 경우 에러 메시지에 CAfile 경로를 확인후에 없을 경우 설정해 주고 다시 clone 을 실행하면 된다.

```sh
$ git config --global http.sslCAInfo d:/devel/my-ca-bundle.crt
```

<br/>

## Chain 없는 인증서

### 원인

Verisign, Comodo, Thawte 같이 유명한 CA에서 발급받은 SSL 인증서라면 괜찮지만 self singed certificate 나 모르는 CA 에서 발급한 인증서같이

기본 ca 정보 목록에 없는 인증서를 사용할 경우 에러를 발생시킨다.

- 셀프 사인 인증서 에러

  fatal: unable to access 'https://myserver/lesstif/util-script.git/': SSL certificate problem: self signed certificate

- ca-bundle 에 SSL 발급기관 인증서 정보가 없을 경우

  error: SSL certificate problem, verify that the CA cert is OK. Details:
  error:14090086:SSL routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed while accessing https://myhost/username/ExcelANT.git/info/refs

<details>
<summary>git-config 의 해당 항목 보기</summary>
<p>

**git-config man**

- **http.sslVerify**

  Whether to verify the SSL certificate when fetching or pushing over HTTPS. Can be overridden by the GIT_SSL_NO_VERIFY environment variable.

- **http.sslCert**

  File containing the SSL certificate when fetching or pushing over HTTPS. Can be overridden by the GIT_SSL_CERT environment variable.

- **http.sslKey**

  File containing the SSL private key when fetching or pushing over HTTPS. Can be overridden by the GIT_SSL_KEY environment variable.

- **http.sslCertPasswordProtected**

  Enable Git’s password prompt for the SSL certificate. Otherwise OpenSSL will prompt the user, possibly many times, if the certificate or private key is
  encrypted. Can be overridden by the GIT_SSL_CERT_PASSWORD_PROTECTED environment variable.

- **http.sslCAInfo**

  File containing the certificates to verify the peer with when fetching or pushing over HTTPS. Can be overridden by the GIT_SSL_CAINFO environment variable.

- **http.sslCAPath**

  Path containing files with the CA certificates to verify the peer with when fetching or pushing over HTTPS. Can be overridden by the GIT_SSL_CAPATH environment
  variable.

- **http.sslTry**

  Attempt to use AUTH SSL/TLS and encrypted data transfers when connecting via regular FTP protocol. This might be needed if the FTP server requires it for
  security reasons or you wish to connect securely whenever remote FTP server supports it. Default is false since it might trigger certificate verification
  errors on misconfigured servers.

</p>
</details>

처리 방법은 다음과 같다.

## 상용 SSL 인증서 발급

VeriSign 이나 Comodo, RapidSSL 등 유명한 SSL 인증서 발급 기관에서 돈을 주고 SSL 인증서를 발급받아서 git https 서버에 설치한다.

- 장점

  서버에 적용하므로 git client 마다 설정을 수정할 필요는 없다.

- 단점

  돈이 든다. 내부에서만 사용하는 서버라면 굳이 상용 SSL 인증서를 발급받을 필요는 없다고 본다.

<br/>

## SSL Verify 옵션 Off

git 의 ssl verify 옵션을 끈다. --global 을 주어 전역적으로 설정할 수 있고

```sh
[모든 https repository 연결시 ssl 검증 끔]

git config --global http.sslVerify false
## 또는 다음과 같이 환경 변수로 설정 가능
export GIT_SSL_NO_VERIFY=0
```

특정 repository 에서만 수행하려면 repository 가 있는 폴더에서 다음 명령어를 실행하거나 .git/config 의 [http] 섹션에 sslVerify = false를 추가한다.

```sh
[Visual Studio Code 에서 아래 명령어로 조치 했음.]

[https repository 연결시 ssl 검증 끔]

git config http.sslVerify false
```

내부적으로는 libcurl 호출시 다음과 같이 SSL 관련 검증을 하지 않게 된다.

```sh
[git 내부의 http 구현]

static CURL *get_curl_handle(void)
{
        CURL *result = curl_easy_init();
        if (!curl_ssl_verify) {
                curl_easy_setopt(result, CURLOPT_SSL_VERIFYPEER, 0);
                curl_easy_setopt(result, CURLOPT_SSL_VERIFYHOST, 0);
        } else {
                /* Verify authenticity of the peer's certificate */
                curl_easy_setopt(result, CURLOPT_SSL_VERIFYPEER, 1);
                /* The name in the cert must match whom we tried to connect */
                curl_easy_setopt(result, CURLOPT_SSL_VERIFYHOST, 2);
        }
        ...
}
```

- 장점

  돈이 안 든다.

- 단점

  1. 사용하는 git client 마다 설정을 수정해야 한다.
  2. git 을 upgrade 할때마다 설정을 반영해야 할수 있다.

<br/>

## curl 의 인증기관 목록에 SSL 인증서 추가

curl이 사용하는 인증기관 인증서 목록에 ca-bundle.crt 에 사용하는 인증서를 추가한다. (참고 [curl 에 신뢰하는 인증기관 인증서 추가하기](https://www.lesstif.com/pages/viewpage.action?pageId=15892500))

- 장점

  돈이 안 든다.

- 단점

  1. 사용하는 git client 마다 설정을 수정해야 한다.
  2. git 을 upgrade 할때마다 설정을 반영해야 할수 있다.
  3. SSL Verify off 보다 번거롭다.
