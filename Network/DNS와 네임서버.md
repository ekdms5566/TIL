# DNS(Domain Name System)와 네임 서버(Name Server)

목차

[DNS(Domain Name System)](#📍-dnsdomain-name-system)  
[네임 서버(Name server)](#📍-네임-서버name-server)  
[DNS 구성요소](#📍-dns-구성요소)  
[DNS Query](#📍-dns-query)

<hr>

- DNS(Domain Name System)와 네임 서버(Name Server)는 웹사이트나 인터넷 서비스를 식별하기 위해 사용되는 요소다.

## 📍 DNS(Domain Name System)

DNS(Domain Name System)은 인터넷에서 도메인 이름과 IP 주소를 연결하는 시스템이다. 인터넷 사용자는 도메인 이름을 통해 웹사이트에 접근하고, 이를 IP 주소로 변환해야 하는데, DNS는 이러한 도메인 이름과 IP 주소 간의 매핑을 관리하고, 도메인 이름에 대한 IP 주소를 찾아주는 역할을 한다. DNS는 계층 구조로 구성되어 있으며, 도메인 이름을 계층적으로 구성된 네임 서버에 질의하여 해당 도메인에 대한 IP 주소를 받아온다.

<img src="https://user-images.githubusercontent.com/78911818/247471416-295d6da2-1cb8-4a28-b72e-4ddfd42e343b.png" >

크게 보면 위 그림과 같이 이해하면 되지만, 사실상 DNS 서버가 하나만 존재하지는 않는다. 만약 서버가 하나만 있을 경우 오랜 시간이 걸리기 때문에 도메인을 계층적으로 구분하는 정보(도메인과 IP주소)를 분산하는 구조를 사용한다.

## 📍 네임 서버(Name server)

네임 서버(Name Server)는 DNS에서 도메인 이름에 대한 IP 주소 정보를 저장하고 관리하는 서버로, 네임 서버는 도메인 이름에 대한 IP 주소 정보를 요청받으면 해당 도메인에 대한 IP 주소를 제공한다. 이를 위해 네임 서버는 도메인 이름과 그에 대응하는 IP 주소를 저장하는 DNS 레코드를 관리하고, 도메인 이름 질의에 대한 응답을 처리한다. 네임 서버는 도메인 이름의 계층 구조를 기반으로 동작하며, 최상위 도메인 서버, 지역 도메인 서버, 국가 도메인 서버 등 다양한 수준으로 구성될 수 있다.

DNS는 도메인 이름과 IP 주소 간의 매핑을 관리하는 시스템 전체를 의미하며, 네임 서버는 실제로 도메인 이름에 대한 IP 주소 정보를 저장하고 제공하는 서버를 의미하며, 네임 서버는 DNS 시스템의 핵심 요소 중 하나로 도메인 이름의 IP 주소 변환을 가능하게 한다.

## 📍 DNS 구성요소

<img src="https://user-images.githubusercontent.com/78911818/247482253-d653e13b-418a-4934-b145-e0ce5fed3e9f.png" >

도메인의 총 관리는 ICANN에서 하며 DNS 서버도 최상위 도메인에서 개인 도메인의 서브 도메인까지 도메인 이름의 분류와 마찬가지로 디렉토리/계층 형태로 구분된다.

**Resolver(권한 없는 DNS 서버)**

- 리졸버는 웹 브라우저와 같은 DNS 클라이언트의 요청을 네임 서버로 전달하고 네임 서버로부터 정보(도메인 이름과 IP 주소)를 받아 클라이언트에게 제공하는 기능을 수행한다.
- 클라이언트 호스트에서 설정하는 DNS 서버(Recursive DNS Server)는 이와 같은 서버를 의미하는 것으로, 도메인에 대한 질의를 받은 스터브 리졸버는 설정된 DNS 서버로 DNS Query(질의)를 전달하고 DNS 서버로부터 최종 결과를 응답 받아 웹 브라우저로 전달하는 인터페이스 기능만을 수행한다.
- 인터넷 사용자가 가장 먼저 접근하는 DNS 서버
- 한 번 DNS 서버에서 받은 데이터가 있다면 일정기간 동안 캐시 형태로 저장해놓는 역할
- 직접 도메인과 IP 주소의 관계를 기록, 저장, 변경 없이 캐시만을 보관
- 대표적으로 ISP(SKT, KT, LG 통신사) 서버가 있고, 브라우저 우회 용도로 사용하는 구글 DNS, 클라우드 플레어의 Public DNS 서버가 있다.

**Root DNS Server**

- ICANN이 직접 관리하는 서버로, TLD DNS 서버 IP를 저장하고 안내하는 역할

**TLD(최상위 도메인) DNS Server**

- 도메인 등록 기관이 관리하는 서버로, Authoritative DNS 서버 주소를 저장해두고 안내하는 역할
- 어떤 도메인 묶음이 어떤 Authoritative DNS Server에 속하는지 아는 이유는 도메인 판매 업체(Registrar)의 DNS 설정이 변경되면 도메인 등록 기관(Registry)으로 전달이 되기 때문

**Authoritative DNS Server(Second-Level Domain(SLD) Server)**

- 실제 개인 도메인과 IP 주소의 관계가 기록, 저장, 변경되는 서버
- 도메인/호스팅 업체의 '네임서버' 또는 개인이 구축한 DNS 서버

## 📍 DNS Query

- DNS Query란 클라이언트와 DNS 서버가 교환하는 것으로 Recursive(재귀적) 또는 Iterative(반복적)으로 구분된다.

**Recursive Query (재귀적 질의)**

- Recursive 서버가 Recursive 쿼리(IP 주소)를 클라이언트에게 반환해주는 역할이다.
- Recursive 쿼리를 받은 Recursive 서버는 Iterative 하게 권한 있는 네임 서버로 Iterative 쿼리를 보내서 결과적으로 IP 주소를 찾게 되고 해당 결과물을 응답한다.

**Iterative Query (반복적 질의)**

- Recursive DNS 서버가 다른 DNS 서버에게 쿼리를 보내어 응답을 요청하는 작업이다. Recursive 서버가 권한 있는 네임 서버들에게 반복적으로 쿼리를 보내서 IP 주소를 알아낸다. Recursive 서버에 이미 IP 주소가 캐시 되어있다면 이 과정은 건너뛴다.

## 📍 DNS 레코드

도메인 이름과 관련된 정보를 저장하는 DNS(Domain Name System)의 데이터베이스 항목이다. DNS 레코드는 도메인 이름을 IP 주소로 매핑하거나, 이메일 서버, 웹 서버, 네임서버 등과 관련된 정보를 포함하며, DNS 서버에 저장되어 클라이언트가 도메인 이름을 해석하는 데 사용된다.

- **일반적으로 사용되는 DNS 레코드 유형**

```
A 레코드 (Address Record): 도메인 이름을 IPv4 주소로 매핑한다. 예를 들어, example.com의 A 레코드는 해당 도메인의 IPv4 주소를 지정한다.

AAAA 레코드 (IPv6 Address Record): 도메인 이름을 IPv6 주소로 매핑한다. AAAA 레코드는 IPv6 주소 체계에서 사용된다.

CNAME 레코드 (Canonical Name Record): 도메인 이름을 다른 도메인 이름에 별칭으로 매핑한다. CNAME 레코드를 사용하면 한 도메인의 IP 주소 변경 시 다른 도메인에도 영향을 주지 않고 IP 주소를 업데이트할 수 있다.

MX 레코드 (Mail Exchanger Record): 도메인 이름과 관련된 이메일 서버의 우선순위와 호스트명을 지정한다. MX 레코드는 이메일 전송을 처리하는 메일 서버를 식별하는 데 사용된다.

NS 레코드 (Name Server Record): 도메인의 네임서버를 지정한다. NS 레코드는 해당 도메인의 DNS 쿼리를 처리하는 네임서버의 호스트명을 제공한다.

TXT 레코드 (Text Record): 도메인과 관련된 임의의 텍스트 정보를 저장한다. TXT 레코드는 SPF(Sender Policy Framework) 등과 같은 이메일 인증 시스템에 사용되기도 한다.
```

<hr>

참고

[AWS DNS](https://aws.amazon.com/ko/route53/what-is-dns/)  
[IBM DNS](https://www.ibm.com/kr-ko/topics/dns)  
[cloudflare DNS](https://www.cloudflare.com/ko-kr/learning/dns/what-is-dns/)  
[DNS란](https://hanamon.kr/dns%EB%9E%80-%EB%8F%84%EB%A9%94%EC%9D%B8-%EB%84%A4%EC%9E%84-%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B0%9C%EB%85%90%EB%B6%80%ED%84%B0-%EC%9E%91%EB%8F%99-%EB%B0%A9%EC%8B%9D%EA%B9%8C%EC%A7%80/)
