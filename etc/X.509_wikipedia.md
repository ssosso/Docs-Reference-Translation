# X.509
[암호학](https://en.wikipedia.org/wiki/Cryptography)에서 **X.509**는 [공개키 인증서](https://en.wikipedia.org/wiki/Public_key_certificate)의 형식을 정의하는 표준이다. X.509 인증서는 [웹](https://en.wikipedia.org/wiki/World_Wide_Web) 탐색을 위한 보안 프로토콜 HTTPS<sup id="footkey1-1">[[1]](#footnote1)</sup>의 기반인 [TLS/SSL](https://en.wikipedia.org/wiki/Transport_Layer_Security)을 포함한 많은 인터넷 프로토콜에서 사용한다. [전자 서명](https://en.wikipedia.org/wiki/Electronic_signature)과 같은 오프라인 어플리케이션에서도 사용한다. X.509 인증서는 공개키와 ID(호스트명, 조직, 개인)를 포함하며 [인증기관](https://en.wikipedia.org/wiki/Certificate_authority)에서 서명하거나 자체적으로 서명한다. 신뢰할 수 있는 인증기관이 인증서에 서명하거나 다른 방법으로 유효성 검사를 한 경우, 해당 인증서를 보유한 사용자는 인증서에 포함된 공개키를 사용하여 다른 사람과 보안 통신을 설정할 수도 있고 해당 [개인키](https://en.wikipedia.org/wiki/Private_key)로 [디지털 서명한](https://en.wikipedia.org/wiki/Digital_signature) 문서를 검증할 수도 있다.

인증서 자체 형식 외에도 X.509는 더 이상 유효하지 않은 인증서 정보를 배포하는 수단인 인증서 [폐기 목록](https://en.wikipedia.org/wiki/Revocation_list)과 중간 CA 인증서로 인증서에 서명할 수 있도록 하는 [인증서 경로 유효성 알고리즘](https://en.wikipedia.org/wiki/Certification_path_validation_algorithm)을 명시하고 있다. 다른 인증서에 의해 차례로 서명되어 결국 [트러스트 앵커<sup>trust anchor</sup>](https://en.wikipedia.org/wiki/Trust_anchor)에 도달할 수 있도록 하는 것이다.

X.509는 [국제 통신 연합](https://en.wikipedia.org/wiki/International_Telecommunication_Union)의 표준화 부문(ITU-T)에서 정의히며, 또 다른 ITU-T 표준인 [ASN.1](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One)를 기반으로 한다.

#### Contents
1. [역사와 사용법](#역사와-사용법)
2. [인증서](#인증서)
    1. [인증서 구조](#인증서-구조)
    2. [인증서의 특정 사용법을 알려주는 확장](#인증서의-특정-사용법을-알려주는-확장)
    3. [인증서 파일 확장자](#인증서-파일-확장자)
3. [인증서 체인과 상호인증](#인증서-체인과-상호인증)
    1. [예시 1: 두 PKI간에 최상위 인증기관(CA) 수준의 상호인증](#예시-1-두-pki간에-최상위-인증기관ca-수준의-상호인증)
    2. [예시 2: CA 인증서 갱신](#예시-2-ca-인증서-갱신)
4. [샘플 X.509 인증서](#샘플-x509-인증서)
    1. [최종 엔티티 인증서](#최종-엔티티-인증서)
    2. [중간 인증서](#중간-인증서)
    3. [루트 인증서](#루트-인증서)
5. [보안](보안)
    1. [아키텍처 취약점](#아키텍처-취약점)
  	2. [인증기관 관련 문제](#인증기관-관련-문제)
    3. [구현 이슈](#구현-이슈)
    4. [암호화 취약점](#암호화-취약점)
        1. [암호화 취약점에 대한 완화책](#암호화-취약점에-대한-완화책)
6. [X.509에 대한 PKI 표준](#x509에-대한-pki-표준)
7. [PKIX 실무 그룹](#pkix-실무-그룹)
8. [X.509 인증서를 사용하는 주요 프로토콜과 표준](#x509-인증서를-사용하는-주요-프로토콜과-표준)
9. [기타 참고](#기타-참고)
10. [참고 문헌](#참고-문헌)
11. [외부 링크](#외부-링크)

## 역사와 사용법
X.509는 1988년 7월 3일에 최초로 발표되었으며, [X.500](https://en.wikipedia.org/wiki/X.500) 표준과 연계되어 시작하였다.


## 인증서

###	인증서 구조
- **인증서** (Certificate)
  - **버전 번호** (Version Number)
  - **시리얼 번호** (Serial Number)
  - **서명 알고리즘 ID** (Signature Algorithm ID)
  - **발급자 성명** (Issuer Name)
  - **유효기간** (Validity period)
    - **시작일시** (Not Before)
    - **종료일시** (Not After)
  - **소유자 성명** (Subject name)
  - **소유자 공개키 정보** (Subject Public Key Info)
    - **공개키 알고리즘** (Public Key Algorithm)
    - **소유자 공개키** (Subject Public Key)
  - **발행자 고유 식별자** (Issuer Unique Identifier) _(선택사항)_
  - **소유자 고유 식별자** (Subject Unique Identifier) _(선택사항)_
  - **확장** (Extensions) _(선택사항)_
    - ...
- **인증서 서명 알고리즘** (Certificate Signature Algorithm)
- **인증서 서명** (Certificate Signature)

###	인증서의 특정 사용법을 알려주는 확장
###	인증서 파일 확장자
## 인증서 체인과 상호인증
###	예시 1: 두 PKI간에 최상위 인증기관(CA) 수준의 상호인증
<center>
  <img src=".images/350px-Cross-certification_diagram.svg.png"/>
  <br/>
  <b>예시 1: 두 PKI간의 상호인증</b>
</center>
###	예시 2: CA 인증서 갱신
<center>
  <img src=".images/CA_certificate_renewal.png"/>
  <br/>
  <b>예시 2: CA 인증서 갱신</b>
</center>
## 샘플 X.509 인증서
###	최종 엔티티 인증서
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            10:e6:fc:62:b7:41:8a:d5:00:5e:45:b6
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=BE, O=GlobalSign nv-sa, CN=GlobalSign Organization Validation CA - SHA256 - G2
        Validity
            Not Before: Nov 21 08:00:00 2016 GMT
            Not After : Nov 22 07:59:59 2017 GMT
        Subject: C=US, ST=California, L=San Francisco, O=Wikimedia Foundation, Inc., CN=*.wikipedia.org
        Subject Public Key Info:
            Public Key Algorithm: id-ecPublicKey
                Public-Key: (256 bit)
                pub:
                    04:c9:22:69:31:8a:d6:6c:ea:da:c3:7f:2c:ac:a5:
                    af:c0:02:ea:81:cb:65:b9:fd:0c:6d:46:5b:c9:1e:
                    ed:b2:ac:2a:1b:4a:ec:80:7b:e7:1a:51:e0:df:f7:
                    c7:4a:20:7b:91:4b:20:07:21:ce:cf:68:65:8c:c6:
                    9d:3b:ef:d5:c1
                ASN1 OID: prime256v1
                NIST CURVE: P-256
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature, Key Agreement
            Authority Information Access:
                CA Issuers - URI:http://secure.globalsign.com/cacert/gsorganizationvalsha2g2r1.crt
                OCSP - URI:http://ocsp2.globalsign.com/gsorganizationvalsha2g2

            X509v3 Certificate Policies:
                Policy: 1.3.6.1.4.1.4146.1.20
                  CPS: https://www.globalsign.com/repository/
                Policy: 2.23.140.1.2.2

            X509v3 Basic Constraints:
                CA:FALSE
            X509v3 CRL Distribution Points:

                Full Name:
                  URI:http://crl.globalsign.com/gs/gsorganizationvalsha2g2.crl

            X509v3 Subject Alternative Name:
                DNS:*.wikipedia.org, DNS:*.m.mediawiki.org, DNS:*.m.wikibooks.org, DNS:*.m.wikidata.org, DNS:*.m.wikimedia.org, DNS:*.m.wikimediafoundation.org, DNS:*.m.wikinews.org, DNS:*.m.wikipedia.org, DNS:*.m.wikiquote.org, DNS:*.m.wikisource.org, DNS:*.m.wikiversity.org, DNS:*.m.wikivoyage.org, DNS:*.m.wiktionary.org, DNS:*.mediawiki.org, DNS:*.planet.wikimedia.org, DNS:*.wikibooks.org, DNS:*.wikidata.org, DNS:*.wikimedia.org, DNS:*.wikimediafoundation.org, DNS:*.wikinews.org, DNS:*.wikiquote.org, DNS:*.wikisource.org, DNS:*.wikiversity.org, DNS:*.wikivoyage.org, DNS:*.wiktionary.org, DNS:*.wmfusercontent.org, DNS:*.zero.wikipedia.org, DNS:mediawiki.org, DNS:w.wiki, DNS:wikibooks.org, DNS:wikidata.org, DNS:wikimedia.org, DNS:wikimediafoundation.org, DNS:wikinews.org, DNS:wikiquote.org, DNS:wikisource.org, DNS:wikiversity.org, DNS:wikivoyage.org, DNS:wiktionary.org, DNS:wmfusercontent.org, DNS:wikipedia.org
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 Subject Key Identifier:
                28:2A:26:2A:57:8B:3B:CE:B4:D6:AB:54:EF:D7:38:21:2C:49:5C:36
            X509v3 Authority Key Identifier:
                keyid:96:DE:61:F1:BD:1C:16:29:53:1C:C0:CC:7D:3B:83:00:40:E6:1A:7C

    Signature Algorithm: sha256WithRSAEncryption
         8b:c3:ed:d1:9d:39:6f:af:40:72:bd:1e:18:5e:30:54:23:35:
         ...
```


Issuer | C=BE, O=GlobalSign nv-sa, CN=GlobalSign Organization Validation CA - SHA256 - G2
--- | ---
**Authority Key Identifier** | **96:DE:61:F1:BD:1C:16:29:53:1C:C0:CC:7D:3B:83:00:40:E6:1A:7C**

###	중간 인증서
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            04:00:00:00:00:01:44:4e:f0:42:47
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA
        Validity
            Not Before: Feb 20 10:00:00 2014 GMT
            Not After : Feb 20 10:00:00 2024 GMT
        Subject: C=BE, O=GlobalSign nv-sa, CN=GlobalSign Organization Validation CA - SHA256 - G2
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:c7:0e:6c:3f:23:93:7f:cc:70:a5:9d:20:c3:0e:
                    ...
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Basic Constraints: critical
                CA:TRUE, pathlen:0
            X509v3 Subject Key Identifier:
                96:DE:61:F1:BD:1C:16:29:53:1C:C0:CC:7D:3B:83:00:40:E6:1A:7C
            X509v3 Certificate Policies:
                Policy: X509v3 Any Policy
                  CPS: https://www.globalsign.com/repository/

            X509v3 CRL Distribution Points:

                Full Name:
                  URI:http://crl.globalsign.net/root.crl

            Authority Information Access:
                OCSP - URI:http://ocsp.globalsign.com/rootr1

            X509v3 Authority Key Identifier:
                keyid:60:7B:66:1A:45:0D:97:CA:89:50:2F:7D:04:CD:34:A8:FF:FC:FD:4B

    Signature Algorithm: sha256WithRSAEncryption
         46:2a:ee:5e:bd:ae:01:60:37:31:11:86:71:74:b6:46:49:c8:
         ...
```
###	루트 인증서
```
Certificate:[14]
    Data:
        Version: 3 (0x2)
        Serial Number:
            04:00:00:00:00:01:15:4b:5a:c3:94
    Signature Algorithm: sha1WithRSAEncryption
        Issuer: C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA
        Validity
            Not Before: Sep  1 12:00:00 1998 GMT
            Not After : Jan 28 12:00:00 2028 GMT
        Subject: C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:da:0e:e6:99:8d:ce:a3:e3:4f:8a:7e:fb:f1:8b:
                    ...
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Key Usage: critical
                Certificate Sign, CRL Sign
            X509v3 Basic Constraints: critical
                CA:TRUE
            X509v3 Subject Key Identifier:
                60:7B:66:1A:45:0D:97:CA:89:50:2F:7D:04:CD:34:A8:FF:FC:FD:4B
    Signature Algorithm: sha1WithRSAEncryption
         d6:73:e7:7c:4f:76:d0:8d:bf:ec:ba:a2:be:34:c5:28:32:b5:
         ...
```
## 보안
###	아키텍처 취약점
###	인증기관 관련 문제
###	구현 이슈
###	암호화 취약점
#### 암호화 취약점에 대한 완화책
## X.509에 대한 PKI 표준
## PKIX 실무 그룹
## X.509 인증서를 사용하는 주요 프로토콜과 표준
## 기타 참고
- [Abstract Syntax Notation One](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One)
- [Certificate policy](https://en.wikipedia.org/wiki/Certificate_policy)
- [Code Access Security](https://en.wikipedia.org/wiki/Code_Access_Security)
- [Communications security](https://en.wikipedia.org/wiki/Communications_security)
- [Information security](https://en.wikipedia.org/wiki/Information_security)
- [ISO/IEC JTC 1](https://en.wikipedia.org/wiki/ISO/IEC_JTC_1)
- [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography)
- [Time stamp protocol](https://en.wikipedia.org/wiki/Time_stamp_protocol)
- [Trusted timestamping](https://en.wikipedia.org/wiki/Trusted_timestamping)
## 참고 문헌
##### 1. <sup id="footnote1">_[a](#footkey1-1) [b]()_</sup> [RFC 4158](https://tools.ietf.org/html/rfc4158)
## 외부 링크
