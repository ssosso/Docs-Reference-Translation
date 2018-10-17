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

이를 위해서는 일단 한나의 [키 쌍](https://en.wikipedia.org/wiki/Key_pair)을 생성하고 [개인키](https://en.wikipedia.org/wiki/Private-key_cryptography)는 공개하지 않고 이것으로 CSR을 서명한다. 이 CSR은 신청자를 식별할 수 있는 정보와 CSR의 서명을 검증하는 데 사용되는 신청자의 [공개키](https://en.wikipedia.org/wiki/Public-key_cryptography)를 포함한다. 인증서라고 할 수 있는 [고유 이름(DN, Distinguished Name)](https://en.wikipedia.org/wiki/Distinguished_Name)도 포함한다. CSR은 인증기관이 요구하는 다른 자격증명 또는 신원 증명을 수반할 수 있다.

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

각 확장(Extension)은 중요 표시와 함께 [객체 식별자](https://en.wikipedia.org/wiki/Object_identifier)로 표현된 고유한 ID를 가지며, 이는 값 집합이다. 인증서를 사용하는 시스템은 중요 표시가 되어 있으나 식별할 수 없는 확장이 있는 경우 또는 중요 표기가 되어 있으나 처리할 수 없는 정보를 포함한 경우 해당 인증서를 거부해야 한다. 중요하지 않음 표시가 되어 있는데 식별할 수 없는 확장이라면 이는 무시할 수 있으며, 식별할 수 있다면 처리해야 한다.<sup>[[4]](#footnote4)</sup>

버전 1의 구조는 RFC 1422에 나와 있다.<sup>[[5]](#footnote5)</sup>

ITU-T는 시간이 얼마간 흐른 뒤에도 발급자와 소유자의 이름을 재사용할 수 있게 하기 위해서 버전 2에 발급자와 소유자의 고유 식별자를 도입하였다. 재사용의 예로는 [CA](https://en.wikipedia.org/wiki/Certificate_authority)가 파산하여 그 CA의 이름이 해당 국가의 공개 목록에서 삭제될 경우가 있다. 일정기간 후에 동일한 이름의 다른 CA는 앞선 CA와 관련이 없더라도 등록할 수 있다. 그러나 [IETF](https://en.wikipedia.org/wiki/IETF)는 어떠한 발급자나 소유자의 이름도 재사용하지 않는 것을 권장한다. 그러므로 버전 2는 인터넷에 널리 보급되지 못했다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup>

확장은 버전 3에 도입되었다. CA는 특정한 용도로만, 이를테면 [전자 객체를 서명하는](https://en.wikipedia.org/wiki/Code_signing) 용도로만, 인증서를 발급하는 데 확장을 사용할 수 있다.

모든 버전에서 시리얼 번호는 특정CA가 발급한 각 인증서마다 고유해야 한다(RFC 5280에 언급되어 있는 바와 같이).

###	인증서의 특정 용법을 알려주는 확장(Extension)
RFC 5280(그리고 이것의 이전 모델)은 인증서 사용 방법을 나타내는 많은 인증서 확장(Extension)을 정의하고 있다. 대부분의 확장은 joint-iso-ccitt (2) ds (5) id-ce (29) OID의 일부분이다. 4.2.1절에 정의되어 있는 가장 일반적인 내용 중 몇 가지는 아래와 같다.

- 기본적인 제약 { id-ce 19 }<sup>[[6]](#footnote6)</sup>는 인증서가 CA에 속하는지 나타내는 데 사용한다.
- 키 사용법인 { id-ce 15 }<sup>[[7]](#footnote7)</sup>는 인증서에 포함된 공개키를 사용하여 수행할 수 있는 암호화 작업을 지정하는 비트맵을 제공한다. 예를 들어 해당 키는 서명에는 사용되지만 암호문에는 사용되지 않아야 함을 나타낼 수 있다.
- 확장한 키 사용법인 { id-ce 37 }<sup>[[8]](#footnote8)</sup>은 인증서가 포함하고 있는 공개키의 목적을 나타내기 위해 일반적으로 말단 인증서에서 사용된다. 이 확장(Extension)은 OID 목록을 포함하며 각 OID는 허용할 수 있는 사용을 나타낸다. 예를 들어 { id-pkix 3 1 }은 TLS/SSL 연결에서 서버측이 키를 사용할 수 있음을 나타내며, { id-pkix 3 4 }는 전자 메일 보안에 키를 사용할 수 있음을 나타낸다.

일반적으로 인증서에 사용을 제한하는 확장(Extension)이 여러 개 있는 경우, 적합하게 사용하도록 모든 제한을 만족해야 한다. RFC 5280에는 keyUsage와 extendedKeyUsage를 포함하는 인증서의 구체적인 예시가 나와 있다. 이런 경우, 두 확장은 모두 처리 되어야 하며, 인증서는 두 가지 확장을 만족하는 경우에만 사용할 수 있다. 예를 들어 [NSS](https://en.wikipedia.org/wiki/Network_Security_Services)는 두 가지 확장을 모두 사용하여 인증서의 용법을 지정하고 있다.<sup>[[9]](#footnote9)</sup>

###	인증서 파일 확장자
X.509 인증서에는 일반적으로 사용되는 몇가지 파일 확장자가 있다. 아쉽게도 이러한 확장자 중 일부는 개인키와 같은 다른 데이터에도 사용되고 있다.
- .pem - ([Privacy-enhanced Electronic Mail, 강화된 개인정보 보호 전자 메일](https://en.wikipedia.org/wiki/Privacy-enhanced_Electronic_Mail)) [Base64](https://en.wikipedia.org/wiki/Base64)로 인코딩된 [DER](https://en.wikipedia.org/wiki/Distinguished_Encoding_Rules) 인증서이며, "-----BEGIN CERTIFICATE-----"와 "-----END CERTIFICATE-----" 문자열로 앞뒤가 감싸져 있다.
- .cer, .crt, .der - 보통 사용되는 바이너리 DER형식, 그렇지만 Base64로 인코딩된 인증서도 일반적이다.
- .p7b, .p7c - 데이터 없이 인증서와 [CRL](https://en.wikipedia.org/wiki/Revocation_list)만 있는 [PKCS#7](https://en.wikipedia.org/wiki/PKCS7)의 SignedData 구조
- .p12 - [PKCS#12](https://en.wikipedia.org/wiki/PKCS12), 인증서(공개키)와 (암호로 보호된)[개인키](https://en.wikipedia.org/wiki/Private_key)를 포함할 수 있다.
- .pfx - PFX, PKCS#12의 이전 모델([IIS](https://en.wikipedia.org/wiki/Internet_Information_Services)에서 생성되는 PFX 파일과 같이 일반적으로 PKCS#12 형식의 데이터를 포함하고 있다.)

[PKCS#7](https://en.wikipedia.org/wiki/PKCS7)은 데이터를 서명 또는 암호화하는 것(공식적으로는 "감싸는 것(envelop)")에 대한 표준이다. 서명 데이터를 검증하기 위해서 인증서가 필요하기 때문에 SignedData 구조에서 이 둘을 포함하는 것이 가능하다. .p7c 파일은 서명할 데이터가 없는 SignedData 구조이다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup>

[PKCS#12](https://en.wikipedia.org/wiki/PKCS12)는 개인 정보 교환(PFX, personal information exchange) 표준에서 발전하여 하나의 파일로 공개 객체와 개인 객체를 교환하는 데 사용된다.<sup>[[인용문 필요]](https://en.wikipedia.org/wiki/Wikipedia:Citation_needed)</sup>

## 인증서 체인과 상호인증
**인증서 체인**([RFC 5280](https://tools.ietf.org/html/rfc5280)에서 정의하고 있는 "인증 경로(certification path)"와 동일한 개념)은 인증서 목록이며, 이 목록 끝에는 1개 이상의 [CA](https://en.wikipedia.org/wiki/Certificate_authority) 인증서가 있다(보통 최종 엔티티 인증서로 시작해서, 스스로를 서명한 CA 인증서로 끝난다). 인증서 체인은 아래와 같은 특징이 있다.

1. (마지막 인증서를 제외한) 각 인증서의 발급자는 목록의 다음 인증서 소유자와 일치한다.
2. (마지막 인증서를 제외한) 각 인증서는 체인의 다음 인증서 개인키로 서명되어야 한다. 즉, 어떤 인증서의 서명은 다음 인증서가 포함하는 공개키를 사용해서 검증할 수 있다.
3. 목록의 마지막 인증서는 [트러스트 앵커(trust anchor)](https://en.wikipedia.org/wiki/Trust_anchor)이다. 신뢰할 수 있는 절차를 통해 전달되었으므로 신뢰할 수 있는 인증서이다.

인증서 체인은 대상 인증서(체인의 첫 번째 인증서)에 포함된 공개키(PK)와 데이터가 실질적으로 인증서 소유자의 것이 맞는지 검증하기 위해 사용된다. 이를 명백하게 하기 위해서 대상 인증서의 서명은 다음 인증서가 포함하고 있는 PK를 사용하여 검증한다. 다음 인증서의 서명은 그 다음 인증서를 사용하여 인증하며 이를 체인의 마지막 인증서에 도달할 때까지 계속한다. 마지막 인증서는 트러스트 앵커이므로 이에 성공적으로 도달하였다는 것은 대상 인증서가 신뢰할 수 있다는 것을 증명하는 것이다.

앞 단락의 설명은 [RFC 5280](https://tools.ietf.org/html/rfc5280)<sup id="footkey10-1">[[10]](#footnote10)</sup>에 정의된 [인증 경로 유효성 검사 프로세스](https://en.wikipedia.org/wiki/Certification_path_validation_algorithm)를 단순화 한 것이다. RFC 5280은 인증서의 유효 기간 검증, [CRL](https://en.wikipedia.org/wiki/Revocation_list) 조회 등과 같은 추가적인 확인절차가 포함되어 있다.

인증서 체인을 만들고 검증하는 방법을 검토할 때 특정 인증서가 전혀 다른 인증서 체인에 일부로 속할 수 있다는 것을 유의해야 한다(여기에서 모든 인증서는 유효하다). 이는 동일한 주체와 그의 공개키에 대한 여러 CA 인증서가 생성될 수 있고 이 CA 인증서들이 서로 다른 CA의 서로 다른 개인키들로 서명되거나 동일한 CA의 서로 다른 개인키들로 서명될 수 있기 때문이다. 그래서 단일 X.509 인증서는 하나의 발급자와 하나의 CA 서명만을 가질 수 있음에도 불구하고 완전히 다른 인증서 체인을 만들어 2개 이상의 인증서에 유효하게 연결할 수 있다. 이는 PKI와 다른 어플리케이션 간의 상호인증에 매우 중요한 부분이다.<sup>[[11]](#footnote11)</sup> 아래 예시를 보자.

아래 다이어그램들에서,
 - 각 상자는 인증서를 나타낸다. 인증서의 소유자는 굵은 글씨로 표현된다.
 - A → B는 "B가 A를 서명하였다"(더 정확하게는 "B에 포함된 공개키에 대응하는 비밀키로 A를 서명하였다")는 것을 의미한다.
 - 동일한 색의 인증서는 동일한 공개키를 포함한다. (흰색이거나 투명한 색의 인증서 제외)

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
4. <sup id="footnote4"></sup>[RFC 5280 section 4.2, retrieved 12 February 2013](http://tools.ietf.org/html/rfc5280#section-4.2,)
5. <sup id="footnote5"></sup>[RFC 1422](http://www.ietf.org/rfc/rfc1422)
6. <sup id="footnote6"></sup>["RFC 5280, Section 'Basic Constraints'"](http://tools.ietf.org/html/rfc5280#section-4.2.1.9).
7. <sup id="footnote7"></sup>["'RFC 5280, Section 'Key Usage'"](http://tools.ietf.org/html/rfc5280#section-4.2.1.3).
8. <sup id="footnote8"></sup>["RFC 5280, Section 'Extended Key Usage'"](http://tools.ietf.org/html/rfc5280#section-4.2.1.12).
9. <sup id="footnote9"></sup>[All About Certificate Extensions](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/NSS/nss_tech_notes/nss_tech_note3)
10. <sup id="footnote10">_[a](#footkey10-1) [b](#footkey10-2)_</sup> "Certification Path Validation". [Internet X.509 Public Key Infrastructure Certificate and Certificate Revocation List (CRL) Profile](http://tools.ietf.org/html/rfc5280#page-71). Network Working Group. 2008.
11. <sup id="footnote11"></sup>Lloyd, Steve (September 2002). [Understanding Certification Path Construction](http://www.oasis-pki.org/pdfs/Understanding_Path_construction-DS2.pdf) (PDF). PKI Forum.

## 외부 링크
