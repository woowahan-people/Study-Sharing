# DNS

## IP(Internet Protocol)란?

---

인터넷에 연결되어있는 모든 장치들(컴퓨터, 서버 장비, 스마트폰 등)을 식별할 수 있도록 각각의 장비에게 부여되는 고유주소.

### IPv4, IPv6

IP는 `IPv4` , `IPv6`2가지 종류가 있으며, 일반적으로 IP라고 하면 `IPv4` 주소를 말한다. 

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled.png)

`IPv4`는 IP version 4의 약자로 **전 세계적으로 사용된 첫 번째 인터넷 프로토콜**이다. 주소는 32비트 방식으로 8비트씩 4자리로 되어있으며 각 자리는 온점(.)으로 구분한다.

ex) 115.68.24.88

`IPv4` 는 0~2^32(약 42억 9천)개의 주소를 가질 수 있는데 전 세계적으로 인터넷 사용자 수가 급증하면서 IPv4주소가 고갈될 위기에 처해있다.

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%201.png)

이러한 문제를 해결하기 위해 등장한 주소가 `IPv6`이다.

`IPv6`는 IP version 6의 약자로, `IPv4`의 주소체계를 128비트 크기로 확장한 차세대 인터넷 프로토콜 주소이다. 16비트씩 8자리로 각 자리는 클론(:)으로 구분한다.

ex) 2001:0DB8:1000:0000:0000:0000:1111:2222

*네트워크 속도, 보안적인 부분뿐만 아니라 여러면에서 뛰어나지만 기존의 주소체계를 변경하는데 소모되는 비용이 너무 많이 들어서 아직 완전히 상용화되지 않음

### 고정 IP, 유동 IP

IP는 나누는 방식에 따라 `고정 IP`, `유동 IP` 와 `공인 IP`, `사설 IP` 로 나눌 수 있다. 

`고정 IP` 는 말 그대로 변하지 않고 컴퓨터에 고정적으로 부여된 IP이다. 한번 부여되면 IP를 반납하기 전까지는 다른 장비에 부여할 수 없는 IP로 보안성이 우수하기 때문에 보안이 필요한 업체나 기관에서 사용한다. 

`유동 IP`는 역시 변하는 IP이다.  인터넷 사용자 모두에게 `고정 IP`를 부여해주기 힘들기 때문에 일정한 주기 또는 사람들이 인터넷에 접속하는 매 순간마다 사용하고 있지 않은 IP주소를 임시로 발급해준 것이다. 

*대부분의 사용자는 유동 IP를 사용한다.

### 공인 IP, 사설 IP

IP주소는 임의로 우리나라가 부여하는 것이 아닌 ICANN이라는 기관이 국가별로 사용할 IP대역을 관리하고, 우리나라는 한국인터넷진흥원(KISA)에서 국내 IP주소들을 관리하고 있다. 

이것을 ISP(Internet Service Provider의 약자로 KT, LG, SKT와 같이 인터넷을 제공하는 통신업체) 가 부여받고 우리는 위 회사에 가입을 통해 IP를 제공받아 인터넷을 사용하게 되는 것이다. 이렇게 받은 IP를 `공인 IP` 라고 한다. 

공유기를 사용한 인터넷 접속 환경일 경우 공유기까지는 공인 IP를 할당하지만, 공유기에 연결되어있는 가정이나, 회사의 각 네트워크 기기에는 `사설 IP` 를 할당한다. `사설 IP`는 어떤 네트워크 안에서 내부적으로 사용되는 고유한 주소이다.

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%202.png)

즉, `공인 IP`는 전 세계에서 유일하지만, `사설 IP`는 하나의 네트워크 안에서 유일하다. `공인 IP`는 내부, 외부 상관없이 해당 IP에 접속할 수 있으나, `사설 IP`는 내부에서만 접근이 가능하다.

## Domain(도메인)이란?

---

- 웹 브라우저를 통해 특정 사이트에 진입할 때, **IP주소를 대신하여 사용하는 주소**.
- 도메인을 이용해서 한눈에 파악하기 힘든 IP주소를 보다 분명하기 나타낼 수 있다.
- 만약 IP주소가 **제주특별자치도 제주시 오라동 1770**라면, 도메인은 **더큰내일센터**로 볼 수 있다.
- 도로명 주소를 대신해서, 우리는 상호나 건물을 찾아갈 수 있는 것과 같다.

### 터미널에서 도메인의 IP주소를 확인하는 방법

```bash
$ nslookup naver.com

Server:		164.124.101.2
Address:	164.124.101.2 #53

Non-authoritative answer:
Name:	naver.com
Address: 223.130.200.107
Name:	naver.com
Address: 223.130.195.200
Name:	naver.com
Address: 223.130.200.104
Name:	naver.com
Address: 223.130.195.95
```

## 도메인과 URL

---

### 도메인의 구조

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%203.png)

1단계: 최상위 도메인(TLD, Top-Level Domain)

도메인 레벨중 가장 높은 단계에 있는 도메인, 도메인의 목적, 종류, 국가를 나타낸다.

2단계: 차상위 도메인(SLD, Second-Level Domain)

호스트 또는 서브 도메인으로도 불린다. 보조 도메인으로써, URL로 전송되거나 계정 내의 IP주소나 디렉토리로 *포워딩되는 도메인 이름의 확장자이다.

*포워딩: 사물을 다음 장소에 전달하거나 보내는 행위

3단계: 도메인 이름(Domain Name)

임의로 지정할 수 있는 사이트의 이름. google, naver, daum등 사용자에게 쉽게 기억될 수 있도록 보통 서비스명으로 도메인 명을 지정해 사용한다.

예시) `https://www.naver.com`

- www → 호스트명(서브도메인)
- naver → 도메인 명
- com → 최상위 도메인 명

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%204.png)

도메인 주소 맨 뒤에는 `.` 이 생략되어있다. 

### URL의 구조

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%205.png)

- path( / )
    - 파일의 경로를 가리키며 `/` 뒤에 나온다.
    - 폴더 내에 파일과 폴더를 계속 만들 수 있는 것처럼 컴퓨터의 폴더와 비슷한 개념이다.
- parameter( ? )
    - 쿼리 스트링이라고도 부르며 key(파라미터 이름) = value(파라미터의 값) 형태로 이루어진다. `?` 뒤에 나열되고 `&` 기호로 구분되어 여러 개가 존재할 수 있다.
- fragment ( # )
    - 해시태그(Hashtag), 앵커(Ancher)라고도 부른다.
    - 특정 요소를 지시할 수 있다.
    - 예를 들어 해시태그로 이동을 원하는 요소의 id를 링크로 연결하면, 스크롱하지 않고 바로 해당위치로 이동한다.

## DNS(Domain Name System)이란?

---

앞서 말했듯 네트워크 상에 존재하는 모든 통신기기(PC, 스마트폰 등등)는 IP주소가 있다. 그러나 모든 IP주소가 도메인 이름을 가지는 것은 아니다. 로컬 PC를 나타내는 127.0.0.1은 localhost로 사용할 수 있지만 그 외의 모든 도메인 이름은 일정기간동안 대여하여 사용한다.

### 도메인 이름과 IP주소 매칭 방법

브라우저의 검색창에 도메인 이름을 입력하여 해당 사이트로 이동하기 위해서는, 해당 도메인 이름과 매칭된 **IP주소를 확인하는 작업이 반드시 필요**하다.

네트워크에는 이 확인하는 작업을 위한 별도의 서버가 있는데 이것이 바로 **DNS서버**이다.

### DNS의 역할

DNS는 Domain Name System의 줄임말로, **호스트의 도메인 이름을 IP주소로 변환**하거나 **IP주소를 도메인이름으로 변환**하기 위해 개발된 데이터베이스 시스템이다.

이 DNS는 법국제적 단위로 웹사이트의 IP주소와 도메인 주소를 이어주는 환경혹은 시스템이다. 

DNS 시스템안에서 이여주는 역할을 하는 서버를 풀네임으로 DNS 서버라고 한다.

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%206.png)

Root DNS → Top-Level 서브 → 책임서버로 찾아 내려가는 계층 구조로 이루어져 있다. 

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%207.png)

### Root DNS

DNS와 특정 프로토콜들의 제한, 즉 파편화되지 않은 사용자 데이터그램 프로토콜(UDP) 패킷의 실질적인 크기 제한으로 인해 **루트 서버 수를 13개 서버 주소로 제한**하도록 결정됨(A~M).

### Top-level DNS

최상위 도메인(Top-level domain, TLD)은 인터넷에서 도메인 네임의 가장 마지막 부분을 말한다. 예를 들어 `ko.wikipedia.org`의 최상위 도메인은 `.org`가 된다. 최상위 도메인은 `.com`과 같은 **일반 최상위 도메인**과 `.kr` 같은 **국가 코드 최상위 도메인**으로 나뉜다.

가령, [베리사인](https://ko.wikipedia.org/wiki/%EB%B2%A0%EB%A6%AC%EC%82%AC%EC%9D%B8) 글로벌 레지스트리 서비스 사는 `com` TLD에 대한 TLD 서버를 담당하고 있다고 한다.

### Authorative Name Server

책임 DNS 서버. 조직 자체 DNS서버, 조직의 명명된 호스트에 대한 IP매핑에 권한있는 호스트 이름을 제공. 조직 또는 서비스 제공업체에 의해 유지보수가 가능

## DNS 구성

### 구성요소

- Registrant 등록자 : 일반인(고객). 대부분은 여기에 속함
- Registrar 등록대행자 : Cafe24, 닷홈 같은 도메인 대행 등록 업체들이 존재한다. 1년에 2~3만원이면 내아이피에 도메인을 등록할 수 있다.
- Registry 등록소 : 최상위 도메인을 관리하는 역할을 한다. 일례로 베리사인 글로벌 레지스트리 서비스 사가 `com`을 관리하고, 에듀코즈 사가 `edu`를 관리하고 있다.
- ICANN : 국제인터넷주소관리기구(Internet Corporation for Assigned Names and Numbers, ICANN)는 1998년에 설립된 인터넷의 비즈니스, 기술계, 학계 및 사용자 단체 등으로 구성된 기관으로 인터넷 DNS의 기술적 관리, IP 주소공간 할당, 프로토콜 파라미터 지정, 루트 서버 시스템 관리 등의 업무를 조정하는 역할을 한다.

### 등록과정

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%208.png)

1. 우리는 93.184.216.34라는 서버를 가지고 있다. 이 서버에다가 `example.com`이라는 도메인을 부여하고 싶다.
2. 우리는 `example.com`과 IP, 약간의 수수료를 Registrar 등록대행자에게 보내면서 등록 신청을 한다.
3. Registrar 등록대행자는 Authorative Name Server(a.iana-servers.net)에다가 example.com은 93.184.216.34라는 것을 등록한다. 이 Authorative Name Server는 자신이 직접 구축할 수도 있고 무료를 이용할 수 있고 지금처럼 Registrar 등록대행자를 이용할 수도 있다.
4. Registrar 등록대행자는 `example.com`의 NS(Name Server)는 a.iana-servers.net이라는 것을 Registry 등록소에게 알려준다.
5. Registry 등록소는 Top-level Domain Sever(a.gtId-servers.net)에다가 마찬가지로 example.com의 NS는 a.iana-servers.net라는 것을 등록한다.
6. ICANN이 관리하는 Root Name Server(a.root-servers.net)은 이미 com의 Top-level Domain Sever가 a.gtId-servers.net라는 것을 알고 있다.

### 도메인 탐색

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%209.png)

1. 이제 클라이언트가 여러분의 서버에 접속하려고 한다. 클라이언트는 이미 DNS Sever가 등록되어있다. 예를 들어 앞서 KT가 168.126.63.1인 것처럼.
2. DNS Sever는 Root Name Server가 무엇인지 이미 가지고 있다. 따라서 a.root-servers.net을 이미 알고 있다. 물론 여러개를 알고 있겠지?
3. 우리의 서버는 example.com이라고 등록되어있다. 따라서 example.com의 IP가 무엇인지 알아야 한다. DNS Server는 Root Name Server에게 물어본다. Root Name Server는 com을 보고 com을 관리하는 Top-level domain을 알려준다.
4. 다시 DNS Server는 Top-level domain을 관리하는 a.gtId-servers.net에 가서 물러본다. a.gtId-servers.net는 example.com이 a.iana-servers.net이 관리한다는 것을 알고 있다.
5. 다시 DNS는 Server는 a.iana-servers.net에 가서 example.com에 대해 물어본다. 드디어 93.184.216.34를 획득할 수 있는 것이다.
6. 최종적으로 클라이언트는 93.184.216.34로 서버에게 요청해서 웹 페이지를 받아볼 수 있는 것이다.

![Untitled](DNS%209a162d5c9f364b60b87c6b5a8c06cbc4/Untitled%2010.png)

1. 사용자가 웹 브라우저를 열어 주소 표시줄에 www.example.com을 입력하고 Enter 키를 입력
2. www.example.com에 대한 요청은 일반적으로 케이블 인터넷 공급업체, DSL 광대역 공급업체 또는 기업 네트워크 같은 인터넷 서비스 제공업체(ISP)가 관리하는 DNS 해석기로 라우팅
3. ISP의 DNS 해석기는 www.example.com에 대한 요청을 DNS 루트 이름 서버에 전달.
4. ISP의 DNS 해석기는 www.example.com에 대한 요청을 이번에는 .com 도메인의 TLD 이름 서버 중 하나에 다시 전달합니다. .com 도메인의 이름 서버는 example.com 도메인과 연관된 4개의 Amazon Route 53 이름 서버의 이름을 사용하여 요청에 응답.
5. ISP의 DNS 해석기는 Amazon Route 53 이름 서버 하나를 선택해 www.example.com에 대한 요청을 해당 이름 서버에 전달.
6. Amazon Route 53 이름 서버는 example.com 호스팅 영역에서 www.example.com 레코드를 찾아 웹 서버의 IP 주소 192.0.2.44 등 연관된 값을 받고 이 IP 주소를 DNS 해석기로 반환.
7. ISP의 DNS 해석기가 마침내 사용자에게 필요한 IP 주소를 확보하게 됩니다. 해석기는 이 값을 웹 브라우저로 반환합니다. 또한, DNS 해석기는 다음에 누군가가 example.com을 탐색할 때 좀 더 빠르게 응답할 수 있도록 사용자가 지정하는 일정 기간 example.com의 IP 주소를 캐싱(저장). 
8. 웹 브라우저는 DNS 해석기로부터 얻은 IP 주소로 www.example.com에 대한 요청을 전송합니다. 여기가 콘텐츠가 있는 곳으로, 예를 들어 웹 사이트 엔드포인트로 구성된 Amazon S3 버킷 또는 Amazon EC2 인스턴스에서 실행되는 웹 서버.
9. 192.0.2.44에 있는 웹 서버 또는 그 밖의 리소스는 www.example.com의 웹 페이지를 웹 브라우저로 반환하고, 웹 브라우저는 이 페이지를 표시.