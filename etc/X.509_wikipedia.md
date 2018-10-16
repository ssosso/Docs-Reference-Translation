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
X.509는 1988년 7월 3일에 최초로 발표되었으며, [X.500](https://en.wikipedia.org/wiki/X.500) 표준과 연계되어 시작하였다. 이는 인증서 발급에 대한 [인증기관](https://en.wikipedia.org/wiki/Certificate_authority)(CA)의 엄격한 계층구조의 시스템을 가정한다. 이것은 [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy)와 같은 [신뢰 웹<sup>web of trust</sup>](https://en.wikipedia.org/wiki/Web_of_trust) 모델과 대조된다. 신뢰 웹 모델의 경우 (특수한 CA뿐만 아니라) 누구든 서명할 수 있고 그에 따라 다른 사람의 중요 인증서의 유효성도 누구든 검증할 수 있다. X.509의 버전3은 [브릿지<sup>bridge</sup>](https://en.wikipedia.org/wiki/Network_bridge)와 [메시<sup>meshe</sup>](https://en.wikipedia.org/wiki/Mesh_network) <sup id="footkey1-2">[[1]](#footnote1)</sup> 같은 다른 토폴로지들을 지원하는 유연성을 가지고 있다. 이것은 피어 투 피어<sup>peer-to-peer</sup>에서나 [OpenPGP](https://en.wikipedia.org/wiki/OpenPGP) 같은 신뢰 웹에서 사용할 수 있지만<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup> 2004년 현재로써는 거의 사용되지 않고 있다. X.500 시스템은 국가 독자성 정보 공유 조약 이행 목적을 위해 독립 국가들에 의해서만 구현되었고, IETF의 공개키 기반 구조<sup>Public-key Infrastructure</sup>(X.509), 즉 PKIX, 실무 그룹은 이 표준을 더 유연한 인터넷 조직에 적용했다. 실제로 <i>X.509 인증서</i>라는 용어는 일반적으로 [RFC 5280](https://tools.ietf.org/html/rfc5280)에 기술된 일명 PKIX<sup>_Public Key Infrastructure (X.509)_</sup>라는 X.509 v3인증서 표준의 IETF의 PKIX 인증서와 [CRL](https://en.wikipedia.org/wiki/Revocation_list) 프로필을 말한다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup>

## 인증서
X.509 시스템에서 서명한 인증서를 원하는 조직은 [인증서 서명 요청(CSR, certificate signing request)](https://en.wikipedia.org/wiki/Certificate_signing_request)을 통해서 인증서를 요청할 수 있다.

이를 위해서는 일단 한나의 [키 쌍](https://en.wikipedia.org/wiki/Key_pair)을 생성하고 [개인키](https://en.wikipedia.org/wiki/Private-key_cryptography)는 공개하지 않고 이것으로 CSR을 서명한다. 이 CSR은 신청자를 식별할 수 있는 정보와 CSR의 서명을 검증하는 데 사용되는 신청자의 [공개키](https://en.wikipedia.org/wiki/Public-key_cryptography)를 포함한다. 인증서라고 할 수 있는 [고유 이름(DN, Distinguished Name)](https://en.wikipedia.org/wiki/Distinguished_Name)도 포함한다. CSR은 인증기관이 요구하는 다른 자격증명 또는 신원 증을 수반할 수 있다.

[인증기관](https://en.wikipedia.org/wiki/Certification_authority)은 공개키를 특정 [고유 이름(DN)](https://en.wikipedia.org/wiki/Distinguished_Name#Directory_structure)에 바인딩하는 인증서를 발급한다.

한 조직의 신뢰할 수 있는 [루트 인증서](https://en.wikipedia.org/wiki/Root_certificate)는 모든 임직원들에게 배포될 수 있으며 그러므로 그들은 회사 PKI시스템을 사용할 수 있게 된다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup> Internet Explorer, Firefox, Opera, Safari, Chrome과 같은 브라우저들은 사전 설정된 루트인증서 세트와 함께 제공되며, 주요 인증기관의 [SSL](https://en.wikipedia.org/wiki/Secure_Sockets_Layer) 인증서가 즉시 작동한다. 사실상 브라우저의 개발자들은 브라우저 사용자들을 위해 어떤 CA가 믿을 만한지 결정해야 한다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup> 예를 들어, Firefox는 포함하고 있는 CA의 목록을 CSV 또는 HTML 파일로 제공한다.<sup>[[2]](#footnote2)</sup>

또한 X.509와 [RFC 5280](https://tools.ietf.org/html/rfc5280)은 인증서 [폐기 목록](https://en.wikipedia.org/wiki/Revocation_list)(CRL, certificate revocation list) 구현을 위한 표준도 포함하고 있다. 인증서의 유효성을 검증하는 또 하나의 [IETF](https://en.wikipedia.org/wiki/IETF) 승인 방법은 [온라인 인증서 상태 프로토콜(OCSP, Online Certificate Status Protocol)](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol)이다. Firefox 3는 적어도 Vista이상의 Windows 버전에서 기본적으로 OCSP 검증을 진행한다.<sup>[[3]](#footnote3)</sup>

###	인증서 구조
표준에서 예상하는 구조는 [추상 구문 표기법(ASN.1, Abstract Syntax Notation One)](https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One)이라는 형식적인 언어로 표현되어 있다.

X.509 v3 [전자 인증서](https://en.wikipedia.org/wiki/Digital_certificate)의 구조는 아래와 같다.

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
<p align="center">
  <img src=".images/350px-Cross-certification_diagram.svg.png"/>
  <br/>
  <b>예시 1: 두 PKI간의 상호인증</b>
</p>

###	예시 2: CA 인증서 갱신
<p align="center">
  <img src=".images/CA_certificate_renewal.png"/>
  <br/>
  <b>예시 2: CA 인증서 갱신</b>
</p>

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

1. <sup id="footnote1">_[a](#footkey1-1) [b](#footkey1-2)_</sup> [RFC 4158](https://tools.ietf.org/html/rfc4158)
2. <sup id="footnote2"></sup>["CA:IncludedCAs - MozillaWiki"](https://wiki.mozilla.org/CA:IncludedCAs). wiki.mozilla.org. Retrieved 2017-01-17.
3. <sup id="footnote3"></sup>["Bug 110161 - (ocspdefault) enable OCSP by default"](https://bugzilla.mozilla.org/show_bug.cgi?id=110161). Retrieved 2016-03-17.

## 외부 링크
