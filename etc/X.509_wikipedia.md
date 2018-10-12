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

## 인증서

###	인증서 구조
- 인증서
  - 버전 번호
  - 시리얼 번호
  - 서명 알고리즘 ID
  - 발급자 성명
  - 유효기간
    - 시작일시
    - 종료일시
  - 소유자 성명
  - 소유자 공개키 정보
    - 공개키 알고리즘
    - 소유자 공개키
  - 발행자 고유 식별자 (선택사항)
  - 소유자 고유 식별자 (선택사항)
  - 확장 (선택사항)
    - ...
- 인증서 서명 알고리즘
- 인증서 서명

###	인증서의 특정 사용법을 알려주는 확장
###	인증서 파일 확장자
## 인증서 체인과 상호인증
###	예시 1: 두 PKI간에 최상위 인증기관(CA) 수준의 상호인증
###	예시 2: CA 인증서 갱신
## 샘플 X.509 인증서
###	최종 엔티티 인증서
###	중간 인증서
###	루트 인증서
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
## 참고 문헌
##### 1. <sup id="footnote1">_[a](#footkey1-1) [b]()_</sup> [RFC 4158](https://tools.ietf.org/html/rfc4158)
## 외부 링크
