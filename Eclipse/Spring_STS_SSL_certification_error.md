# [Spring Tool Suite 4.0.1][ssl] certification error

<br/>

## 1. Download Source

### 1.1. curl 을 사용한 방법

curl -O https://gist.githubusercontent.com/lesstif/cd26f57b7cfd2cd55241b20e05b5cd93/raw/InstallCert.java

### 1.2. Web Browser 를 사용한 방법

https://gist.githubusercontent.com/lesstif/cd26f57b7cfd2cd55241b20e05b5cd93/raw/InstallCert.java

Save As... InstallCert.java

<br/>

## 2. Compiling

C:\DevConfigProject\OpenJDK> javac InstallCert.java

<br/>

## 3. Run

C:\DevConfigProject\OpenJDK> java -cp . InstallCert start.spring.io

```
C:\DevConfigProject\OpenJDK> java -cp . InstallCert start.spring.io
Loading KeyStore C:\DevConfigProject\JDK\AdoptOpenJDK\jdk8u232-b09\jre\lib\security\cacerts...
Opening connection to start.spring.io:443...
Starting SSL handshake...

javax.net.ssl.SSLHandshakeException: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
        at sun.security.ssl.Alerts.getSSLException(Alerts.java:192)
        at sun.security.ssl.SSLSocketImpl.fatal(SSLSocketImpl.java:1946)
        at sun.security.ssl.Handshaker.fatalSE(Handshaker.java:316)
        at sun.security.ssl.Handshaker.fatalSE(Handshaker.java:310)
        at sun.security.ssl.ClientHandshaker.serverCertificate(ClientHandshaker.java:1639)
        at sun.security.ssl.ClientHandshaker.processMessage(ClientHandshaker.java:223)
        at sun.security.ssl.Handshaker.processLoop(Handshaker.java:1037)
        at sun.security.ssl.Handshaker.process_record(Handshaker.java:965)
        at sun.security.ssl.SSLSocketImpl.readRecord(SSLSocketImpl.java:1064)
        at sun.security.ssl.SSLSocketImpl.performInitialHandshake(SSLSocketImpl.java:1367)
        at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1395)
        at sun.security.ssl.SSLSocketImpl.startHandshake(SSLSocketImpl.java:1379)
        at InstallCert.main(InstallCert.java:116)
Caused by: sun.security.validator.ValidatorException: PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
        at sun.security.validator.PKIXValidator.doBuild(PKIXValidator.java:397)
        at sun.security.validator.PKIXValidator.engineValidate(PKIXValidator.java:302)
        at sun.security.validator.Validator.validate(Validator.java:262)
        at sun.security.ssl.X509TrustManagerImpl.validate(X509TrustManagerImpl.java:330)
        at sun.security.ssl.X509TrustManagerImpl.checkTrusted(X509TrustManagerImpl.java:237)
        at sun.security.ssl.X509TrustManagerImpl.checkServerTrusted(X509TrustManagerImpl.java:113)
        at InstallCert$SavingTrustManager.checkServerTrusted(InstallCert.java:196)
        at sun.security.ssl.AbstractTrustManagerWrapper.checkServerTrusted(SSLContextImpl.java:1099)
        at sun.security.ssl.ClientHandshaker.serverCertificate(ClientHandshaker.java:1621)
        ... 8 more
Caused by: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target
        at sun.security.provider.certpath.SunCertPathBuilder.build(SunCertPathBuilder.java:141)
        at sun.security.provider.certpath.SunCertPathBuilder.engineBuild(SunCertPathBuilder.java:126)
        at java.security.cert.CertPathBuilder.build(CertPathBuilder.java:280)
        at sun.security.validator.PKIXValidator.doBuild(PKIXValidator.java:392)
        ... 16 more

Server sent 2 certificate(s):

 1 Subject CN=*.spring.io, OU=Spring, O="Pivotal Software, Inc.", L=Palo Alto, ST=California, C=US
   Issuer  CN=Somansa Root CA, O=Somansa, C=KR
   sha1    e1 a8 ba a9 a7 e2 62 98 d0 e9 ec 7d b9 88 30 73 9b f1 3f 07
   md5     2d 9c b2 b4 9e 69 20 4f fb e2 0d 46 54 80 26 0b

 2 Subject CN=Somansa Root CA, O=Somansa, C=KR
   Issuer  CN=Somansa Root CA, O=Somansa, C=KR
   sha1    d4 d3 6f 13 c8 f1 b8 4b 29 d0 e9 c6 3b 47 34 6b 77 fd a6 aa
   md5     d7 bf 83 62 46 c3 6b 78 a7 d4 37 ee fd 03 d2 20

Enter certificate to add to trusted keystore or 'q' to quit: [1]
1
```

스프링 서버에서 2 개의 인증서를 받았고 1번째가 spring.io 의 CA 인증서이므로 1번을 선택.

<br/>

## 4. save peer's ssl cert to keystore(name is jssecacerts)

인증서를 [jssecacerts]란 이름으로 저장한다. 단, [start.spring.io-1]란 별명을 사용한다.

```
[
[
  Version: V3
  Subject: CN=*.spring.io, OU=Spring, O="Pivotal Software, Inc.", L=Palo Alto, ST=California, C=US
  Signature Algorithm: SHA256withRSA, OID = 1.2.840.113549.1.1.11

  Key:  Sun RSA public key, 2048 bits
  modulus: 21985961396988956654773584893375938021551821951816183960864612807643488553921859908145192167238695445623989893713781900098096609502280605616086574877902535072474272580958223962762280325965640255389258671944494385666009443592354479571282922713303581769892257304089131179008812001572508196882901407403862504366603650994469195700951932977594998947691924833574048764081025537813848071792090940732138223111601628755603144143222128206341044675466329890928391812275478599144984199332034885072762018746865818908825568911585069395331538496241341963852929902007580351618389640035063402207895016481810467287307602213142604701779
  public exponent: 65537
  Validity: [From: Fri Mar 15 09:00:00 KST 2019,
               To: Wed Apr 01 21:00:00 KST 2020]
  Issuer: CN=Somansa Root CA, O=Somansa, C=KR
  SerialNumber: [    3ea15fbb 7c5b8064]

Certificate Extensions: 7
[1]: ObjectId: 1.3.6.1.4.1.11129.2.4.2 Criticality=false
Extension unknown: DER encoded OCTET string =
0000: 04 81 F4 04 81 F1 00 EF   00 75 00 EE 4B BD B7 75  .........u..K..u
0010: CE 60 BA E1 42 69 1F AB   E1 9E 66 A3 0F 7E 5F B0  .`..Bi....f..._.
0020: 72 D8 83 00 C4 7B 89 7A   A8 FD CB 00 00 01 69 83  r......z......i.
0030: B4 8E 8C 00 00 04 03 00   46 30 44 02 20 22 07 B1  ........F0D. "..
0040: 31 12 66 54 0D F5 83 83   9B 51 8A 9B 00 F7 A2 83  1.fT.....Q......
0050: EE 15 1A 45 36 4A 1E 91   71 87 B6 FB 74 02 20 75  ...E6J..q...t. u
0060: BF C8 F4 84 70 EB 03 78   FC C6 71 B4 00 92 0D EB  ....p..x..q.....
0070: 3E D5 36 07 ED 7E 28 5C   B4 5A 67 5C 7B B6 6B 00  >.6...(\.Zg\..k.
0080: 76 00 87 75 BF E7 59 7C   F8 8C 43 99 5F BD F3 6E  v..u..Y...C._..n
0090: FF 56 8D 47 56 36 FF 4A   B5 60 C1 B4 EA FF 5E A0  .V.GV6.J.`....^.
00A0: 83 0F 00 00 01 69 83 B4   8F AA 00 00 04 03 00 47  .....i.........G
00B0: 30 45 02 20 40 D0 FA 9B   DD AC 3B 93 DC AA B2 E9  0E. @.....;.....
00C0: B0 96 07 CA 68 40 8F C3   4B 79 01 81 59 7A 17 3D  ....h@..Ky..Yz.=
00D0: 7A D0 29 73 02 21 00 B7   E3 F9 0A FC 54 B2 31 B5  z.)s.!......T.1.
00E0: 7D 41 31 AC 04 BF 34 E9   62 2B 48 FE B2 EA 0A C8  .A1...4.b+H.....
00F0: D3 28 4F A7 04 A8 5C                               .(O...\


[2]: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: B2 83 2C 50 73 E9 90 A2   CD 6C E8 C2 2D 60 25 EE  ..,Ps....l..-`%.
0010: A1 64 65 46                                        .deF
]
]

[3]: ObjectId: 2.5.29.19 Criticality=true
BasicConstraints:[
  CA:false
  PathLen: undefined
]

[4]: ObjectId: 2.5.29.37 Criticality=false
ExtendedKeyUsages [
  serverAuth
  clientAuth
]

[5]: ObjectId: 2.5.29.15 Criticality=true
KeyUsage [
  DigitalSignature
  Key_Encipherment
]

[6]: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
  DNSName: *.spring.io
  DNSName: spring.io
]

[7]: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: B3 75 F0 21 30 4F A3 F8   93 10 CD E9 B7 A4 79 23  .u.!0O........y#
0010: 5D DE F8 FA                                        ]...
]
]

]
  Algorithm: [SHA256withRSA]
  Signature:
0000: B0 1E 4A 63 AF ED 27 91   57 5B 6A DE EA B9 D4 35  ..Jc..'.W[j....5
0010: 43 30 B1 CB 4C 5B 3B 6F   03 22 5D 71 9D B2 60 C3  C0..L[;o."]q..`.
0020: 8B BD 8A 93 CE C4 E0 C0   DD 02 EE 03 E1 B5 AC 0F  ................
0030: B2 8A 0B C2 BC D2 F2 31   CA 4F 97 BF 3D E2 FA D4  .......1.O..=...
0040: 90 80 1F BE E4 6E 99 8A   65 6B 5A 98 9B 97 BC 11  .....n..ekZ.....
0050: 9F A1 AA EE 13 1A 45 8F   80 22 33 5A B5 74 E8 CE  ......E.."3Z.t..
0060: 46 3B 25 0A 4C 1E F4 CA   04 0D B2 BD 34 29 FB 91  F;%.L.......4)..
0070: 76 DA 24 51 27 96 8A 87   C8 7F 16 6C 30 E2 E0 F8  v.$Q'......l0...
0080: 34 3D 04 17 D1 99 5A C8   50 D9 00 2C 3B 68 CA 97  4=....Z.P..,;h..
0090: 86 16 DF 91 F5 16 12 28   8D 1E 9D 11 7B 89 76 11  .......(......v.
00A0: 9F 87 50 25 14 81 66 E0   26 03 6D BA D3 FB 3B AE  ..P%..f.&.m...;.
00B0: 2C 3C 6D 29 49 D2 74 E0   80 33 A0 31 20 C8 DD 6D  ,<m)I.t..3.1 ..m
00C0: 15 FF 4C 19 72 90 2C BE   BE DD 0B 63 76 6A 36 32  ..L.r.,....cvj62
00D0: C8 11 90 36 E8 BB 32 1E   33 B6 6B 04 3E 81 67 D7  ...6..2.3.k.>.g.
00E0: 39 3B D7 88 AC 8E 7D 13   4E 21 FB 5A EE F7 4E EF  9;......N!.Z..N.
00F0: AF 98 77 EA D8 CA E3 BA   0B 1C D9 51 DC 7F 7E 09  ..w........Q....

]

Added certificate to keystore 'jssecacerts' using alias 'start.spring.io-1'

C:\DevConfigProject\OpenJDK>
```

<br/>

## 5. extract cert from saved keystore

keytool 로 저장된 keystore에서 [output.cert]란 이름으로 인증서를 추출한다.

```
C:\DevConfigProject\OpenJDK>keytool -exportcert -keystore jssecacerts -storepass changeit -file output.cert -alias start.spring.io-1
인증서가 <output.cert> 파일에 저장되었습니다.
```

<br/>

## 6. import cert into JDK's keystore

설치된 JDK 의 keystore에 인증서를 추가한다.

```
C:\DevConfigProject\OpenJDK> keytool -importcert -keystore C:\DevConfigProject\OpenJDK\zulu11\lib\security\cacerts -storepass changeit -file output.cert -alias letsencrypt
소유자: CN=*.spring.io, OU=Spring, O="Pivotal Software, Inc.", L=Palo Alto, ST=California, C=US
발행자: CN=Somansa Root CA, O=Somansa, C=KR
일련 번호: 3ea15fbb7c5b8064
적합한 시작 날짜: Fri Mar 15 09:00:00 KST 2019 종료 날짜: Wed Apr 01 21:00:00 KST 2020
인증서 지문:
         MD5:  2D:9C:B2:B4:9E:69:20:4F:FB:E2:0D:46:54:80:26:0B
         SHA1: E1:A8:BA:A9:A7:E2:62:98:D0:E9:EC:7D:B9:88:30:73:9B:F1:3F:07
         SHA256: 79:15:B9:C9:D3:EF:53:E4:1A:FC:2D:88:97:46:1C:4E:4F:24:E7:78:34:8F:6A:BB:B5:2C:D9:CB:B6:13:77:14
서명 알고리즘 이름: SHA256withRSA
주체 공용 키 알고리즘: 2048비트 RSA 키
버전: 3

확장:

#1: ObjectId: 1.3.6.1.4.1.11129.2.4.2 Criticality=false
0000: 04 81 F1 00 EF 00 75 00   EE 4B BD B7 75 CE 60 BA  ......u..K..u.`.
0010: E1 42 69 1F AB E1 9E 66   A3 0F 7E 5F B0 72 D8 83  .Bi....f..._.r..
0020: 00 C4 7B 89 7A A8 FD CB   00 00 01 69 83 B4 8E 8C  ....z......i....
0030: 00 00 04 03 00 46 30 44   02 20 22 07 B1 31 12 66  .....F0D. "..1.f
0040: 54 0D F5 83 83 9B 51 8A   9B 00 F7 A2 83 EE 15 1A  T.....Q.........
0050: 45 36 4A 1E 91 71 87 B6   FB 74 02 20 75 BF C8 F4  E6J..q...t. u...
0060: 84 70 EB 03 78 FC C6 71   B4 00 92 0D EB 3E D5 36  .p..x..q.....>.6
0070: 07 ED 7E 28 5C B4 5A 67   5C 7B B6 6B 00 76 00 87  ...(\.Zg\..k.v..
0080: 75 BF E7 59 7C F8 8C 43   99 5F BD F3 6E FF 56 8D  u..Y...C._..n.V.
0090: 47 56 36 FF 4A B5 60 C1   B4 EA FF 5E A0 83 0F 00  GV6.J.`....^....
00A0: 00 01 69 83 B4 8F AA 00   00 04 03 00 47 30 45 02  ..i.........G0E.
00B0: 20 40 D0 FA 9B DD AC 3B   93 DC AA B2 E9 B0 96 07   @.....;........
00C0: CA 68 40 8F C3 4B 79 01   81 59 7A 17 3D 7A D0 29  .h@..Ky..Yz.=z.)
00D0: 73 02 21 00 B7 E3 F9 0A   FC 54 B2 31 B5 7D 41 31  s.!......T.1..A1
00E0: AC 04 BF 34 E9 62 2B 48   FE B2 EA 0A C8 D3 28 4F  ...4.b+H......(O
00F0: A7 04 A8 5C                                        ...\


#2: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: B2 83 2C 50 73 E9 90 A2   CD 6C E8 C2 2D 60 25 EE  ..,Ps....l..-`%.
0010: A1 64 65 46                                        .deF
]
]

#3: ObjectId: 2.5.29.19 Criticality=true
BasicConstraints:[
  CA:false
  PathLen: undefined
]

#4: ObjectId: 2.5.29.37 Criticality=false
ExtendedKeyUsages [
  serverAuth
  clientAuth
]

#5: ObjectId: 2.5.29.15 Criticality=true
KeyUsage [
  DigitalSignature
  Key_Encipherment
]

#6: ObjectId: 2.5.29.17 Criticality=false
SubjectAlternativeName [
  DNSName: *.spring.io
  DNSName: spring.io
]

#7: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: B3 75 F0 21 30 4F A3 F8   93 10 CD E9 B7 A4 79 23  .u.!0O........y#
0010: 5D DE F8 FA                                        ]...
]
]

이 인증서를 신뢰합니까? [아니오]:  예
인증서가 키 저장소에 추가되었습니다.

C:\DevConfigProject\OpenJDK>
```
