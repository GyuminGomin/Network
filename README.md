# Network Administrator

## TCP/IP

### - NAT - (Network Address Translation)
IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술  
  
<p style="color: yellow;"> NAT 사용 이유는 대개 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용해 인터넷에 접속하기 위함  </p>  

<strong>NAT은 IPv4의 주소 부족 문제를 해결하기 위한 방법으로 고려되었으며</strong>, 주로 비공인(사설, local) 네트워크 주소를 사용하는 망에서 외부의 공인망(public)과의 통신을 위해 네트워크 주소를 변환해줌.  
  
외부 침입자가 공격하기 위해선 사설망의 내부 IP주소를 알아야 하기 때문에 공격이 어려워지므로 내부 네트워크를 보호할 수 있는 장점 존재  
  
한정된 IPv4 공인 IP 주소 절약 가능  
  
> 예시
>> 1. 공유기 하나의 공인 IP를 사설 IP로 변환하여 여러 기기 사용
>> 2. 
  
> 참고 사항  
>> 1. 백본스위치와 방화벽 사이에 설치  
>> 2. 공개된 인터넷과 사설망 사이에 방화벽(Firewall)을 설치하여 외부 공격으로 부터 보호, 이때 인터넷망과 연결하는 장비인 라우터에 NAT을 설정할 경우 라우터는 자신에게 할당된 공인 IP주소만 외부로 알려지게 하고 내부에선 사설 IP 주소만 사용해 필요시 서로 변환
>> 3. 
  
### - 라우팅 경로 결정 순서 -
```
| 라우팅 최적 경로를 결정하는 순서 |
Longest Match Rule -> AD -> Metric
```

- Longest Match Rule  
  
> 라우팅 테이블에서 목적지 경로를 찾아 갈 때, 라우팅 테이블에 있는 네트워크 주소가 가장 길게 일치되는 경로를 먼저 선택하는 방식  

예를 들어, ping 192.128.100.100을 입력했을 때, 네트워크 장비가 다음의 라우팅 테이블 정보를 가진 라우터로 송신 요청을 한다면,
```
192.0.0.0/8
192.168.0.0/16
192.168.100.0/24
```
이 중 목적지 주소가 가장 긴 경로인 192.168.100.0/24를 선택하는 것이 Longest Match Rule  

- AD (Adminstrative Distance) 값
> 라우터 간의 약속된 "관리자 거리 값"을 나타냄. 이는 관리자가 라우팅 정보 소스의 신뢰성에 대해 정해 놓은 비율로서 여러 가지 라우팅 프로토콜을 운영할 경우 동일 목적지에 대한 여러 개의 경로 중 어떤 프로토콜에 의해 얻은 정보를 우선할 것인지 그 값을 정해놓은 것

- Metric
> AD 값을 몇 번 측정했는가를 나타냄 메트릭 값은 정해진 출발지에서 목적지까지 가는 임의의 단위이며 값의 단위는 프로토콜에 따라 달라짐. 쉽게 말해 목적지까지 가기 위한 '거리 비용'이라고 생각할 수 있음.
작을수록 송신하고자 하는 라우터와 가깝고, 클수록 멀다고 볼 수 있음.

ex. 동일 라우팅 프로토콜 내에서 목적지로 가는 경로가 여러 개 있을 경우, 메트릭 값이 작을 수록 우선순위가 높아짐.

### - ARP - (Address Resolution Protocol)
IP주소를 MAC 주소와 매칭 시키기 위한 프로토콜  
  
***로컬네트워크(LAN***)에서 단말과 단말 간 통신을 하기 위해선 IP 주소와 함께 MAC 주소를 사용하게 되는데, IP 주소를 MAC Address와 매칭하여 목적지 IP의 단말이 소유한 MAC 주소를 향해 제대로 찾아가기 위함임.

이더넷같은 네트워크가 제공하는 BroadCast 기능을 사용해
목적지 IP Address에 MAC(물리적 하드웨어 주소)을 매핑하는 것, IP->MAC 주소 매핑  
*RARP(Reverse Address Resolution Protocol) : MAC->IP 주소 매핑

### - SNMP - (Simple Network Management Protocol)  
IP 기반 네트워크상의 각 호스트로부터 정기적으로 여러 관리 정보를 자동으로 수집하거나 실시간으로 상태를 모니터링 및 설정할 수 있는 서비스  

시스템이나 네트워크 관리자로 하여금 원격으로 네트워크 장비를 모니터링하고 환경설정 등의 운영을 할 수 있도록 하는 네트워크 관리 프로토콜  

SNMP 구성 요소는 기본적으로 관리 시스템과 관리 대상으로 나뉘는데, 관리 시스템을 Manager, 관리 대상을 Agent라고 부름.  

SNMP Manager은 Agent에 필요한 정보를 요청하는 모듈, SNMP Agent는 관리 대상 시스템에 설치되어 필요한 정보를 수집하고 Manager에게 전달해주는 역할 수행하는 모듈  

SNMP는 OSI 7계층의 Application 계층 프로토콜이며, 메시지는 단순 요청과 응답 형식의 프로토콜에 의해 교환되기 때문에 전송계층 프로토콜로 UDP 프로토콜 사용  

> 단점 :
>> 관리의 편의성을 주지만, 여러 취약점들이 존재해 DoS, Buffer Overflow, 비인가 접속 등 여러가지 문제점이 발생 가능

> 참고 :
>> 1. SNMP는 프로토콜일 뿐이며, 이를 활용하여 실제 네트워크 관리정보를 얻기 위해서는 관련 프로그램이 준비되어야 함.  
>> 2. 네트워크 관리를 위한 목적으로 주로 서버나 네트워크 장비에서 SNMP를 설정한 MGRT 프로그램을 이용해 트래픽 관리 등을 위해 사용됨.  
***MRGT*** : (Multiple Router Traffic Grapher)의 약자로 SNMP기반의 장비 모니터링 프로그램으로 주 용도는 네트워크 트래픽 사용량 모니터링이지만 벤더에서 제공하는 SNMP MIB값을 사용해 다양한 정보를 수집 가능

### - HTTP 상태코드 -  
- 100번대 : 정보응답  
> 100 - continue : 이 임시적인 응답은 지금까지 상태가 괜찮으며 클라이언트가 계속해서 요청을 하거나 이미 요청을 완료한 경우 무시해도 되는 것을 알려줌.  
> 101 - switching Protocol : 클라이언트가 보낸 upgrade 요청 헤더에 대한 응답에 들어가며 서버에서 프로토콜을 변경할 것임을 알려줌.  
> 102 - Processing : 서버가 요청을 수신하였으며 이를 처리하고 있지만, 아직 제대로 된 응답을 알려줄 수 없음을 알려줌.  
> 103 - Early Hints : 주로 Link 헤더와 함께 사용되며 서버가 응답을 준비하는 동안 사용자 에이전트가(user agent) 사전 로딩을 시작할 수 있도록 함.  

- 200번대 : 성공응답  
> 200 - OK : 요청이 성공적으로 되었습니다. 성공의 의미는 HTTP 메소드에 따라 달라짐.  
> ***GET*** : 리소스를 불러와서 메시지 바디에 전송되었습니다.  
> ***HEAD*** : 개체 헤더가 메시지 바디에 있습니다.  
> ***PUT or POST*** : 수행 결과에 대한 리소스가 메시지 바디에 전송되었습니다.  
> ***TRACE*** : 메시지 바디는 서버에서 수신한 요청 메시지를 포함하고 있습니다.  
> 201 - Created : 요청이 성공적이었으며, 그 결과로 새로운 리소스가 생성. 이 응답은 주로 POST 요청 또는 일부 PUT 요청 이후에 따라옴.  
> 202 - Accepted : 요청을 수신하였지만 그에 응하여 행동할 수 없음. 이 응답은 요청 처리에 대한 결과를 이후에 HTTP로 비동기 응답을 보내는 것에 대해 명확하게 명시하지 않는다. 이것은 다른 프로세스에서 처리 또는 서버가 요청을 다루고 있거나 배치 프로세스를 하고 있는 경우를 위해 만들어짐.  
> 203 - Non-Authoritative Information : 이 응답 코드는 돌려받은 메타 정보 세트가 오리진 서버의 것과 일치하지 않지만 로컬이나 서드 파티 복사본에서 모아졌음을 의미함. 이러한 조건에선 이 응답이 아니라 200 OK 응답을 반드시 우선됨.  
> 204 - No Content : 요청에 대해서 보내줄 수 있는 콘텐츠가 없지만, 헤더는 의미있을 수 있음. 사용자-에이전트는 리소스가 캐시된 헤더를 새로운 것으로 업데이트 할 수 있음.  
> 205 - Reset Content : 요청을 완수한 후 사용자 에이전트에게 이 요청을 보낸 문서 뷰를 리셋하라고 알려줌.  
> 206 - Partial Content : 클라이언트에서 복수의 스트림을 분할 다운로드 하고자 범위 헤더를 전송했기 때문에 사용됨.  
> 207 - Multi-Status : 멀티-상태 응답은 여러 리소스가 여러 상태 코드인 상황이 적절한 경우 해당되는 정보를 전달  
> 208 - Multi-Status : DAV에서 사용됨. propstat(Property와 status의 합성어) 응답 속성으로 동일 컬렉션으로 바인드된 복수의 내부 멤버를 반복적으로 열거하는 것을 피하기 위해 사용됨.  
> 226 - IM Used : 서버가 GET 요청에 대한 리소스의 의무를 다 했고, 응답이 하나 또는 그 이상의 인스턴스 조작이 현재 인스턴스에 적용 되었음을 알려줌.  

- 300번대 : 리다이렉션 메시지  
> 300 - Multiple Choice : 요청에 대해 하나 이상의 응답이 가능, 사용자 에이전트 또는 사용자는 그 중 하나를 반드시 선택해야 함. 응답 중 하나를 선택하는 방법에 대한 표준화 된 방법은 존재하지 않음.  
> 301 - Moved Permanently : 요청한 리소스의 URL가 변경되었음을 의미함. 새로운 URI가 응답에서 주어질 수 있음.  
> 302 - Found : 리소스의 URI가 일시적으로 변경되었음을 의미함. 새롭게 변경된 URI는 나중에 만들어질 수 있음. 그러므로, 클라이언트는 향후 요청도 반드시 동일한 URI로 해야함.  
> 303 - See Other : 클라이언트가 요청한 리소스를 다른 URI에서 GET 요청을 통해 얻어야 할 때, 서버가 클라이언트로 직접 보내는 응답임.  
> 304 - Not Modified : 캐시를 목적으로 사용됨. 이것은 클라이언트에게 응답이 수정되지 않았음을 알려주며, 클라이언트는 계속해서 응답의 캐시된 버전을 사용할 수 있음.  
> 307 - Temporary Redirect : 클라이언트가 요청한 리소스가 다른 URI에 있으며, 이전 요청과 동일한 메소드를 사용하여 요청해야할 때, 서버가 클라이언트에 이 응답을 직접 보냄. 이것은 302 Found HTTP 응답코드와 동일한 의미를 가지고 있으며, 사용자 에이전트가 반드시 사용된 HTTP 메소드를 변경하지 말아야 하는 점만 다름: 만약 첫 요청에 POST가 사용되었다면, 두 번째 요청도 반드시 POST를 사용.  
> 308 - Permanent Redirect : 리소스가 이제 HTTP 응답 헤더의 Location: 에 명시된 영구히 다른 URI에 위치하고 있음을 의미함. 이것은 301 Moved Permanently HTTP 응답 코드와 동일한 의미를 갖고 있으며, 사용자 에이전트가 반드시 HTTP 메소드를 변경하지 말아야 하는 점만 다름: 만약 첫 요청에 POST가 사용되었다면, 두 번째 요청도 반드시 POST 사용.  

- 400번대 : 클라이언트 에러 응답  
> 400 - Bad Request : 잘못된 문법으로 인하여 서버가 요청을 이해할 수 없음을 의미함.  
> 401 - Unauthorized : 비록 HTTP 표준에선 "미승인(unauthorized)"를 명확히 하고 있지만, 의미상 이 응답은 "비인증(unauthenticated)"을 의미함. 클라이언트는 요청한 응답을 받기 위해 반드시 스스로를 인증해야 함.  
> 403 - Forbidden : 클라이언트는 콘텐츠에 접근할 권리를 가지고 있지 않음. 예를들어 그들은 미승인이어 서버는 거절을 위한 적절한 응답을 보내고, 401과 다른점은 서버가 클라이언트가 누구인지 알고 있음.  
> 404 - Not Found : 서버는 요청받은 리소스를 찾을 수 없음. 브라우저에선 알려지지 않은 URL을 의미함. 이것은 API에서 종점은 적절하지만, 리소스 자체는 존재하지 않음을 의미할 수도 있음. 서버들은 인증받지 않은 클라이언트로부터 리소스를 숨기기 위해 이 응답을 403 대신에 전송할 수도 있음.  
> 405 - Method Not Allowed : 요청한 메소드는 서버에서 알고 있지만, 제거되었고 사용할 수 없음. 예를 들어, 어떤 API에서 리소스를 삭제하는 것을 금지할 수 있음. 필수적인 메소드인 GET과 HEAD는 제거될 수 없으며 이 에러 코드를 리턴할 수 없음.  
> 406 - Not Acceptable : 서버가 <a href="https://developer.mozilla.org/ko/docs/Web/HTTP/Content_negotiation#%EC%84%9C%EB%B2%84_%EC%A3%BC%EB%8F%84_%EC%BB%A8%ED%85%90%EC%B8%A0_%ED%98%91%EC%83%81">서버 주도 콘텐츠 협상</a>을 수행한 이후, 사용자 에이전트에서 정해준 규격에 따른 어떠한 콘텐츠도 찾지 않았을 때, 웹서버가 보냄.  
> 407 - Proxy Authentication Required : 401 Unauthorized와 비슷하지만 프록시에 의해 완료된 인증이 필요함.  
> 408 - Request Timeout : 이 응답은 요청을 한지 시간이 오래된 연결에 일부 서버가 전송하며, 어떨때에는 이전에 클라이언트로부터 어떠한 요청이 없었다고 하더라도 보내지기도 함. 이것은 서버가 사용되지 않는 연결을 끊고 싶어한다는 것을 의미함.  
> 409 - Conflict : 요청이 현재 서버의 상태와 충돌될 때 보냄.  
> 410 - Gone : 요청한 콘텐츠가 서버에서 영구적으로 삭제되었으며, 전달해 줄 수 있는 주소 역시 존재하지 않을 때 보냄. 클라이언트가 그들의 캐쉬와 리소스에 대한 링크를 지우기를 기대함. HTTP 기술 사양은 이 상태 코드가 "일시적인, 홍보용 서비스"에 사용되기를 기대함. API는 알려진 리소스가 이 상태 코드와 함께 삭제되었다고 강요해선 안됨.  
> 411 - Length Required : 서버에서 필요로 하는 Content-Length 헤더 필드가 정의되지 않은 요청이 들어왔기 때문에 서버가 요청을 거절함.  
> 412 - Precondition Failed : 클라이언트의 헤더에 있는 전제조건은 서버의 전제조건에 적절하지 않음.  
> 413 - Payload Too Large : 요청 엔티티는 서버에서 정의한 한계보다 큼. 서버는 연결을 끊거나 혹은 Retry-After 헤더 필드로 돌려보낼 것.  
> 414 - URI Too Long : 클라이언트가 요청한 URI는 서버에서 처리하지 않기로 한 길이보다 김.  
> 415 - Unsupported Media Type : 요청한 미디어 포맷은 서버에서 지원하지 않음. 서버는 해당 요청 거부  
> 416 - Requested Range Not Satisfiable : Range 헤더 필드에 요청한 지정 범위를 만족시킬 수 없음. 범위가 타겟 URI 데이터의 크기를 벗어났을 가능성이 있음.  
> 417 - Expectation Failed : Expect 요청 헤더 필드로 요청한 예상이 서버에선 적당하지 않음을 알려줌.  
> 418 - I'm a teapot : 서버는 커피를 찻 주전자에 끓이는 것을 거절함.  
> 421 - Misdirected Request : 서버로 유도된 요청은 응답을 생성할 수 없음. 이것은 서버에서 요청한 URI와 연결된 스킴과 권한을 구성하여 응답을 생성할 수 없을 때 보내짐.  
> 422 - Unprocessable Entity : 요청은 잘 만들어졌지만, 문법 오류로 인해 따를 수 없음.  
> 423 - Locked : 리소스는 접근하는 것이 잠겨있음.  
> 424 - Failed Dependency : 이전 요청이 실패하였기 때문에 지금의 요청도 실패함.  
> 426 - Upgrade Required : 서버는 지금의 프로토콜을 사용하여 요청을 처리하는 것을 거절하였지만, 클라이언트가 다른 프로토콜로 업그레이드를 하면 처리를 할지도 모름. 서버는 Upgrade 헤더와 필요로 하는 프로토콜을 알려주기 위해 426 응답에 보냄.  
> 428 - Precondition Required : 오리진 서버는 요청이 조건적이어야 함. 클라이언트가 리소스를 GET해서, 수정하고 PUT으로 서버에 돌려놓는 동안 서드파티가 서버의 상태를 수정하여 발생하는 충돌인 '업데이트 상실'을 예방하기 위한 목적임.  
> 429 - Too Many Requests : 사용자가 지정된 시간에 너무 많은 요청을 보냄.  
> 431 - Request Header Fields Too Large : 요청한 헤더 필더가 너무 크기 때문에 서버는 요청을 처리하지 않을 것. 요청은 크기를 줄인 다음 다시 전송해야 함.  
451 - Unavailable For Legal Reasons : 사용자가 요청한 것은 정부에 의해 검열된 웹 페이지와 같은 불법적인 리소스임.  
- 500번대 : 서버 에러 응답  
> 500 - Internal Server Error : 서버가 처리 방법을 모르는 상황이 발생함. 서버는 아직 처리 방법을 알 수 없음.  
> 501 - Not Implemented : 요청 방법은 서버에서 지원되지 않으므로 처리할 수 없음. 서버가 지원해야 하는 유일한 방법은 GET과 HEAD임. 이 코드는 반환하면 안됨.  
> 502 - Bad Gateway : 서버가 요청을 처리하는 데 필요한 응답을 얻기 위해 게이트웨이로 작업하는 동안 잘못된 응답을 수신했음을 의미함.  
> 503 - Service Unavailable : 서버가 요청을 처리할 준비가 되지 않음. 일반적인 원인은 유지보수를 위해 작동이 중단되거나 과부하가 걸렸을 경우. 이 응답과 함께 문제를 설명하는 사용자 친화적인 페이지가 전송되어야 함. 이 응답은 임시 조건에 사용되어야 하며, Retry-After : HTTP헤더는 가능하면 서비스를 복구하기 전 예상 시간을 포함해야 함. 웹 마스터는 또한 이러한 일시적인 조건 응답을 캐시하지 않아야 하므로 이 응답과 함께 전송되는 캐싱 관련 헤더에 대해서도 주의해야 함.  
> 504 - Gateway Timeout : 서버가 게이트웨이 역할을 하고 있으며, 적시에 응답을 받을 수 없을 때 주어짐.  
> 505 - HTTP Version Not Supported : 요청에 사용된 HTTP 버전은 서버에서 지원되지 않음.  
> 506 - Variant Also Negotiates : 서버에 내부 구성 오류가 있음. 즉, 요청을 위한 투명한 컨텐츠 협상이 순환 참조로 이어짐.  
> 507 - Insufficient Storage : 서버에 내부 구성 오류가 있음. 즉, 선택한 가변 리소스는 투명한 콘텐츠 협상에 참여하도록 구성되므로 협상 프로세스의 적절한 종료 지점이 아님.  
> 508 - Loop Detected : 서버가 요청을 처리하는 동안 무한 루프를 감지함.  
> 510 - Not Extended : 서버가 요청을 이행하려면 요청에 대한 추가 확장이 필요함.  
> 511 - Network Authentication Required : 511 상태 코드는 클라이언트가 네트워크 액세스를 얻기 위해 인증을 받아야 할 필요가 있음을 나타냄.

### - ICMP -  (Internet Control Message Protocol)  
<> 네트워크 내 장치가 데이터 전송과 관련된 문제를 전달하기 위해 사용하는 <strong>***3계층 네트워크***</strong> 프로토콜임.  
ICMP 정의에서 ICMP가 사용되는 주요 방법 중 하나는 데이터가 대상에 도달하는지와 도달 시간이 적절한지를 확인하는 것임. 따라서 ICMP는 네트워크가 데이터를 얼마나 잘 전송하는지 알 수 있는 오류 보고 프로세스 및 테스트의 중요한 측면임. 그러나 DDoS 공격을 실행하는데에도 사용할 수 있음.  

<> 또한 네트워크 성능을 평가하기 위한 진단 도구임.  
트레이스 라우트와 핑 모두 ICMP를 사용함. 트레이스 라우트와 핑은 데이터가 정상적으로 전송되었는가에 대해 전송되는 메시지로, 트레이스 라우트를 사용하면 데이터 패킷이 대상에 도달하기 위해 통과한 장치가 보고서에 표시됨. 여기엔 데이터를 처리하기 위한 물리적 라우터가 포함됨.  
트레이스 라우트는 또한 데이터가 한 장치에서 다른 장치로 이동하는 데 걸리는 시간을 알려줌. 라우터 간 데이터의 이동을 홉(hop)이라고 하며 트레이스 라우트에 의해 파악한 정보로 경로 중 지연을 일으키는 장치를 파악할 수 있음.  
핑은 트레이스 라우트와 비슷하지만, 더 간단함. 그것은 데이터가 두 지점 간을 이동하는 데 걸리는 시간을 알려줌. ICMP는 에코 요청 및 에코 응답이 핑 프로세스 중에 사용된다는 점에서 p핑을 용이하게 함.  

<> 또한 ICMP는 네트워크 성능을 저하시키는 데도 사용함. 이 작업은 ICMP 서비스 장애, 스머프 공격 및 네트워크 상의 장치를 압도하고 정상적인 기능을 방해하는 PoD 공격으로 수행됨.  

> 메시지 타입
>> 0 - Echo reply : Echo 메시지의 응답 (ping의 응답)  
>> 3 - Destination unreachable : 도달 불가 에러 (Code에 더 자세한 에러사유)  
>> 4 - Source quench : congestion control의 용도  
>> 5 - Redirect : 더 빠른 경로가 있다고 알려줄 때  
>> 8 - Echo request : Echo 메시지의 요청 (ping의 요청)  
>> 11 - Time exceeded : TTL이 초과된 경우  
>> 13 - Timestamp Request
>> 30 - Trace route : 해당 라우터까지 가는 경로 체크용  
>> 15~18 - 정보 주고받기 관련 : DHCP가 이제 이 역할을 대신함.  
>>> 17 - Address Mask Request

### - CSMA/CD - (Carrier Sense Multiple Access/Collision Detection)
LAN의 통신 프로토콜 종류중 하나이며, 이더넷 환경에서 사용  
***IEEE 802.3 규격***

> 방식
>> 이더넷 환경에서 통신하고 싶은 PC나 서버는 먼저 네트워크 상에 통신이 일어나는지 확인을 함. (캐리어가 있는지 검사)
*캐리어 : 네트워크 상에 나타나는 신호  
>> 네트워크 통신이 일어나고 있으면 (캐리어가 감지되면) 데이터를 보내지 않고 기다림.  
>> 네트워크 통신이 일어나고 있지 않으면 (캐리어가 감지되지 않으면) 데이터를 네트워크 상에 보냄.  
>> 만약, 캐리어가 감지되지 않았을 때, 두 PC나 서버가 데이터를 동시에 보내면 이 경우를 Multiple Access(다중 접근)이라고 함.  
>> 위 처럼 두 PC나 서버가 데이터를 동시에 보내려다 부딪치는 경우 충돌(Collision)이 발생했다고 함.  
>> 만약, 충돌이 일어나게 된다면 데이터를 전송했던 두 PC나 서버들은 랜덤한 시간 동안 기다린 다음 다시 데이터를 전송함.  
>> 이런 충돌이 계속해서 ***15***번 일어나면 통신을 끊음.  

1. CSMA/CD 충돌 x  
    1. 전송을 원하는 호스트는 네트워크에서 캐리어를 감지해 전송이 가능한지 검사.  
    1. 전송이 가능할 경우, 브로드캐스트를 해 목적지 Address를 찾아냄. (여기서 목적지 Address는 유니캐스트로 응답)  
    1. 그 후 전송

1. CSMA/CD 충돌 o
    1. 전송을 원하는 호스트는 네트워크에서 캐리어를 감지해 전송이 가능한지 검사
    1. 다른 PC에서 발생한 프레임이 공유매체에서 충돌이 발생함.
    1. 충돌이 발생하면 Jam Signal을 모든 호스트로 전송해 충돌 발생에 대해 알리고, Jam Signal을 받으면 일정 시간 뒤 다시 전송을 함.

### - OSPF - (Open Shortest Path First)  
링크상태 라우팅 프로토콜에 기반하여, 자치 시스템(AS) 내부의 라우터들끼리(IGP) 라우팅 정보를 교환하는 라우팅 프로토콜

> 특징
>> Interior Gateway Protocol(IGP)에 속함 : 동일 자치시스템(AS) 내에 있는 라우터끼리만 라우팅  
>> Link State 기술에 의한 최단경로 선택 라우팅 알고리즘 : 최단 경로를 선택하기 위해 Dijkstra의 SPF(Shortest Path First) 알고리즘 사용  
>> 빠른 재수렴 (Fast Reconvergence) 및 부분 갱신 (Partial Update) : OSPF 라우터 각각이 전체 네트워크 토폴로지 정보를 가지므로, 토폴로지 변화에 빠른 대처 가능 및 네트워크가 안정되면 라우팅 갱신 정보 만 전달  
>> 라우팅 메트릭으로 링크 비용 사용 : 목적지까지의 최적 경로 선택을 위한 라우팅 메트릭으로는 Link Cost 사용  
>> 라우터 인터페이스에 접속된 OSPF 네트워크 종류에 따라 동작 방식이 달라짐  
>> VLSM(Variable Length Subnet Mask) 및 CIDR(Classless InterDomain Routing) 지원  

### - IGMP -  (Internet Group Management Protocol)
호스트(컴퓨터)가 멀티캐스트 그룹 구성원을 인접한 라우터에게 알리는 프로토콜  
TCP/IP 프로토콜 집합이 동적 멀티캐스팅을 수행하기 위해 사용하는 표준 프로토콜 (멀티 캐스트 : 한 소스에서 선택된 대상 그룹의 통신을 지원하는 네트워크 전송의 유형)  
***IGMP, ICMP는 데이터 전송용 프로토콜이 아니고 이벤트 또는 변화를 알리는데 사용되는 프로토콜***  

> IGMP 동작  
>> 1. 그룹 멤버쉽 조사 (Monitoring) : 멤버쉽 질의 메시지를 보내 응답을 기다림. 일정 횟수 이상 응답 없으면 라우터는 해당 호스트를 그룹에서 탈퇴 시킴.  
>> 2. 그룹 가입 (Joining) : 그룹에 가입하고자 하는 요청을 라우터에 보고  
>> 3. 멤버쉽 연속 (Member continuation) : 계속해서 유지하기 원하는 보고 메시지  
>> 4. 그룹 탈퇴 (Leaving) : 호스트가 해당 그룹의 멀티캐스트 트래픽을 원치 않으면 leave 메시지 전송

> IGMP 기타 기능
>> IGMP Snooping : 라우터와 호스트 사이에 있는 스위치가 IGMP 메시지들을 들을 수 있는 기능  
>> IGMP Querier Election : 동일 LAN에 여러 멀티캐스트 라우터가 있으면, IPv4 주소 중 가장 낮은 주소를 갖는 라우터가 Querier역할을 집중하게 함.

## 네트워크 일반

### - WMN - (Wireless Mesh Network)
무선 네트워크로 새로운 링크를 동적으로 설정하고 다른 노드와 연결하는 기술.  

Mesh는 네트워킹 주파수 대역에 따라 다중 주파수 다중 채널 네트워킹과 단일 주파수 네트워킹으로 나뉨. 단일 주파수 네트워크 송수신은 단일 주파수를 사용하며, 대역폭 용량은 절반으로 줄어듬. 다중 주파수 다중 채널 네트워킹에서 장치는 서로 다른 링크에 대해 다중 직교 주파수를 사용하므로 시스템 처리량이 증가할 수 있음.  

무선 주파수 기술은 전송률 및 성능 향상을 위해 OFDM, MIMO, 스마트 안테나 등의 기술이 널리 사용되고 있음.  

자원 스케줄링 측면에서 일반적으로 CSMA/CD 모드와 TDMA 모드로 구분됨. CSMA/CA는 자원 경쟁 모드를 채택하지만 노드와 홉의 수가 많고 네트워크 부하가 높을 때 실효 자원 활용률이 낮음. TDMA 모드는 시간 할당을 기반으로 하는 스케줄링 메커니즘으로 네트워크 부하가 높을 때 효율성이 높음.  

네트워크 라우팅 알고리즘 측면에서 고정 경로에서 사용되는 RIP 및 OSPF 라우팅 프로토콜과 달리 일반적인 메시 무선 라우팅 알고리즘에선 DSDV, DSR, AODV 등이 포함됨.

> 장점
>> 자체 구성 네트워크, 자체 복구, 다중 홉 계단식 및 노드 자체 관리의 장점이 있음.  
>> 장비 배치가 빠르고 설치가 쉬움.  
>> 가시선이 아닌 전송(NLOS) : 데이터 전송 지점의 경우 데이터는 전송 지점에 대한 직접적인 시선을 가진 장치 또는 사용자가 먼저 수락하고 수신 장치와 사용자는 AP이며, 직접 장치에 도달하기 위해 데이터를 계속 전달할 수 있음.  
>> 네트워크 안전성 : 다른 노드를 통해 데이터를 전송할 수 있음. 하나 또는 일부 노드에 장애가 발생하거나 간섭이 발생하면, 다른 작업 노드를 통해 데이터를 전송 가능.  
>> 고대역폭 : 무선 통신의 물리적 특성은 통신 전송 거리가 짧을수록 높은 대역폭을 얻기 쉽고 간섭이 적음을 결정함. 따라서 여러 개의 짧은 홉 전송 데이터를 선택하면 네트워크에서 더 높은 대역폭을 얻을 수 있으며 한 노드는 동시에 정보를 주고 받을 수 있을 뿐만 아니라 라우터 역할을 하여 다른 노드에 정보를 전달함. 노드의 수가 증가하고 전파 경로의 수가 증가함에 따라 총 대역폭이 크게 증가함.



## - 문제적 읽기 -  
- TCP, UDP port를 함께 사용하는 프로토콜은?  
-\<DNS>  

- 멀티캐스트(Multicast)에 사용되는 IP Class는?  
-\<D Class>

- 프로토콜의 기본적인 기능 중, 송신기에서 발생된 정보의 정확한 전송을 위해 사용자 정보의 앞, 뒤 부분에 헤더와 트레일러를 부가하는 과정은?  
-\<캡슐화(Encapsulation)>  

- 호스트의 IP Address가 '201.100.5.68/28' 일 때, Network ID로 올바른 것은?  
-\<201.100.5.64>
