# CS 면접을 위한 네트워크 질문 리스트

<details>
  <summary>www.google.com 에 접속할 때 생기는 과정에 대해 설명해주세요 (웹 동작 중심)</summary>
  <ul>
    <li> 사용자가 브라우저에 URL입력하면 브라우저는 DNS를 통해 서버의 IP 주소를 찾습니다. client에서 http request로 메시지를 만들고 TCP/IP 패킷을 생성해 서버로 전송합니다. 서버에서는 그 요청에 대한 응답을 http response로 만들어 tcp/ip 패킷을 만들고 클라이언트에게 전송합니다. 도착한 HTTP response 메시지는 웹 브라우저에 의해 출력되게 됩니다.</li>
    <li> IP 주소를 알아야 전송이 가능하므로 DNS lookup을 통해 해당 도메인의 서버 IP 주소를 알아냅니다. 생성된 http 요청 메시지를 tcp/ip 계층에 전달하고 그것을 헤더에 추가해 TCP/IP 패킷을 생성합니다.해당 패킷은 전기 신호로 랜선을 통해 네트워크로 전송되고, 목적지 IP에 도달합니다. 구글 서버에 도착한 패킷은 언패킹을 통해 메시지를 복원하고 서버의 프로세스로 보냅니다. 서버의 프로세스는 응답을 다시 보내고, 웹 브라우저에서 html 렌더링을 하여 모니터에 화면을 보여줍니다.</li>
    <li> <img width="1000" alt="image" src="https://github.com/user-attachments/assets/c1eac91a-5fb6-4111-8f82-1b1e8bf87fbc" />
</li>
  </ul>
</details>

<details>
  <summary>DNS가 무엇인지에 대한 설명과 동작 과정을 설명해주세요</summary>
  <ul>
    <li> DNS는 도메인 이름을 IP 주소로 변환해주는 시스템입니다. 사용자가 특정 도메인에 접속하면 브라우저 캐시 -> OS 캐시 -> 로컬 DNS를 순서대로 확인하고 없으면 Root DNS, Tld(Top Level Domain) DNS, 최종 Authoritative(권한 서버) DNS 서버를 차례로 조회해서 IP 주소를 받아옵니다.</li>
  </ul>
</details>

<details>
  <summary>DNS의 종류에 대해 말해주세요</summary>
  <ul>
    <li> A 레코드: 도메인을 Ipv4 주소로 매핑해줍니다.</li>
    <li> AAAA 레코드: 도메인을 Ipv6 주소로 매핑해줍니다.</li>
    <li> CNAME: 도메인 -> 다른 도메인으로 매핑 (별칭 alias)</li>
    <li> MX(Mail Exchange): 메일 서버 주소</li>
    <li> NS(Name Server): 해당 도메인을 관리하는 권한 DNS 서버 정보</li>
    <li> PTR: IP -> 도메인 역방향 조회</li>
  </ul>
</details>

<details>
  <summary>CDN에 대해 설명해주세요</summary>
  <ul>
    <li> CDN은 전 세계에 분산된 캐시 서버에서 컨텐츠를 제공해 사용자에게 가장 가까운 곳에서 빠르게 파일을 전달하는 시스템입니다. 속도 향상, 트래픽 절감, 대규모 확장성 확보가 장점입니다.</li>
    <li> 사용자가 요청하면 DNS가 가까운 CDN 서버로 라우팅하며, 가까운 CDN 엣지 서버가 응답합니다. CDN 서버에 캐시가 없으면 Origin 서버에서 가져오며, 가져온 파일을 CDN이 캐싱합니다. 이후 요청부터는 CDN에서 바로 응답하게됩니다.</li>
  </ul>
</details>

<details>
  <summary>Ipv4와 Ipv6의 차이점은 뭔가요?</summary>
  <ul>
    <li> Ipv4는 32비트 주소 체계를 써서 약 43억개의 주소만 표현할 수 있고, 주소 부족 문제 때문에 NAT를 사용합니다. IPv6는 128비트라 사실상 무한한 주소를 표현할 수 있고, NAT가 필요없으며 패킷 헤더가 단순해서 라우팅 성능과 보안이 강화된 버전입니다.</li>
  </ul>
</details>


<details>
  <summary>NAT에 대해 설명해주세요</summary>
  <ul>
    <li> NAT는 사설 IP를 공인 IP로 변환하는 기술입니다. 내부에서는 수십대의 기기가 사설 IP로 사용해도 인터넷에서는 하나의 공인 IP로 보이게 하는 기술로, 43억개의 부족한 IP를 더 늘려서 사용할 수 있습니다. </li>
    <li> SNAT(Source NAT)은 출발지의 IP와 포트를 변환하는 NAT이며 주로 내부에서 외부로 나갈때 사용됩니다. </li>
    <li> DNAT(Destination NAT)은 목적지의 IP와 포트를 변환하는 NAT이며 외부에서 내부로 들어오는 요청을 특정 장비로 라우팅 합니다. </li>
    <li> PAT(Port Address Translation)는 IP는 하나인데 포트만 다르게 해서 여러 기기가 나가는 방식입니다.  </li>
    <li> NAT 때문에 p2p가 어려운 경우는 stun, turn 같은 nat traversal 기술을 사용합니다.  </li>
  </ul>
</details>
<details>
  
  <summary>CDN에 대해 설명해주세요</summary>
  <ul>
    <li> CDN은 전 세계에 분산된 캐시 서버에서 컨텐츠를 제공해 사용자에게 가장 가까운 곳에서 빠르게 파일을 전달하는 시스템입니다. 속도 향상, 트래픽 절감, 대규모 확장성 확보가 장점입니다.</li>
    <li> 사용자가 요청하면 DNS가 가까운 CDN 서버로 라우팅하며, 가까운 CDN 엣지 서버가 응답합니다. CDN 서버에 캐시가 없으면 Origin 서버에서 가져오며, 가져온 파일을 CDN이 캐싱합니다. 이후 요청부터는 CDN에서 바로 응답하게됩니다.</li>
  </ul>
</details>

<details>
  <summary>로드밸런서에 대해 설명해주세요</summary>
  <ul>
    <li>로드밸런서는 사용자 요청을 여러 서버로 분산해주는 장치입니다. 부하 분산/확장성/고가용성 등을 위해 많이 사용합니다. 대표적으로 L4, L7 로드밸런서가 있습니다. L4는 TCP/UDP 포트 기준으로 IP 기반 라우팅을 수행합니다. 요청 내용은 볼 수 없고 패킷 레벨에서 클라이언트 IP주소나 포트 번호 등을 기준으로 분산합니다. 하지만 HTTP를 활용한 라우팅은 불가능합니다. L7은 애플리케이션 계층에서 동작하며 HTTP/HTTPS 내용을 보고 라우팅을 수행합니다. </li>
    <li>헬스 체크를 통해 장애 서버를 자동으로 제외하고 서버 증설시에도 자동으로 트래픽을 분배할 수 있어 클라우드나 MSA 환경에서 필수적이라고 할 수 있습니다. 라운드 로빈, 해싱, 가중치, 최소 연결 등의 알고리즘을 사용하여 트래픽을 분산처리 하게됩니다.</li>
  
  </ul>
</details>


<details>
  <summary>Rest는 무엇이고, Restful API는 무엇인가요? </summary>
  <ul>
    <li> Rest는 리소스를 URI로 표현하고, 행동은 HTTP 메서드로 표현하는 아키텍처 스타일입니다. 서버는 무상태성을 유지하고 일관된 인터페이스를 따르는 것이 핵심입니다.</li>
    <li> 이 원칙을 지켜서 만든 API를 Restful API라고 하며, URL을 명사형으로 사용하고 GET/POST/PUT/DELETE 메서드를 적절히 사용해 CRUD를 표현합니다.</li>
  </ul>
</details>


## OSI 7계층 & TCP/IP 4계층

<details>
  <summary>OSI 7계층에 대해 설명해주세요</summary>
  <ul>
    <li> OSI 7계층은 네트워크 통신을 표준화한 모델로, 통신 시스템을 7단계로 나누어 설명한 것입니다. 이 모델은 프로토콜을 기능별로 나눴으며, 각 계층은 하위 계층의 기능만을 이용하고 상위 계층에게 기능을 제공합니다. 일반적으로 상위 계층의 프로토콜은 소프트웨어로, 하위 계층의 프로토콜은 하드웨어로 구현됩니다.</li>
    <li> 물리 계층 : 데이터를 비트(0과 1) 신호로 변환하여 통신 매체(케이블, 무선 등)를 통해 물리적으로 전송하는 역할을 담당합니다. 전송 단위는 비트입니다. </li>
    <li> 데이터 링크 계층 : 물리적 링크를 통해 인접한 두 장치 간의 오류 없는 데이터 전송을 담당합니다. 데이터의 시작과 끝을 구분하며, 흐름 제어 및 오류 검출/복구를 수행합니다. MAC 주소를 사용하여 통신하며, 물리적인 주소 지정(Addressing)을 합니다. 전송 단위는 프레임이며, 가장 잘 알려진 예로는 이더넷이 있습니다. </li>
    <li> 네트워크 계층 : 여러 개의 네트워크를 걸쳐 최적의 경로를 선택하고, 데이터를 최종 목적지까지 전달하는 책임을 집니다.IP 주소를 사용하여 논리적인 주소 지정 및 라우팅(Routing) 기능을 수행하며, 전송 단위는 datagram(packet)입니다. 장비로는 라우터가 있습니다. </li>
    <li> 전송 계층 : 발신지 프로세스와 목적지 프로세스 간의 신뢰성 있는 데이터 전송을 보장합니다. 전체 메시지의 에러 복구 및 흐름 제어를 담당하며, 세그먼트 순서 재조립을 합니다. 포트 번호를 사용하여 프로세스를 식별하며, TCP와 UDP 프로토콜을 사용합니다. 전송 단위는 segment입니다. </li>
    <li> 세션 계층 : 응용 프로그램 간의 통신 세션을 설정, 유지, 종료시키는 역할을 합니다. 동시 송수신 방식(duplex), 반이중 방식(half-duplex), 전이중 방식(Full Duplex)의 통신과 함께, 체크 포인팅과 유휴, 종료, 다시 시작 과정 등을 수행합니다. 이 계층은 TCP/IP 세션을 만들고 없애는 책임을 가집니다. </li>
    <li> 표현 계층 : 데이터를 응용 계층이 이해할 수 있는 형식으로 변환하거나 표현하는 기능을 담당합니다.데이터 암호화(Encryption) 및 복호화(Decryption), 데이터 압축 및 압축 해제, 다양한 데이터 형식(JPEG, MPEG 등)의 변환을 수행합니다. </li>
    <li> 응용 계층 : 사용자와 가장 가까운 계층으로, 네트워크 서비스를 제공합니다. 사용자가 직접 상호작용하는 응용 프로그램들이 이 계층에 속합니다. 사용자의 요청을 처리하는 프로토콜(HTTP, FTP, SMTP, DNS 등)을 제공합니다.</li>
  </ul>
</details>

<details>
  <summary>OSI 7계층과 TCP/IP 4계층을 비교하여 설명해주세요.</summary>
  <ul>
    <li> OSI 7계층은 네트워크 통신을 표준화한 모델로, 통신 시스템을 7단계로 나누어 설명한 것입니다. 하지만 OSI 모델이 실무적으로 이용하기에 복잡한 탓에 실제 인터넷에서는 이를 단순화한 TCP/IP 4계층이 사용되고 있습니다.</li>
    <li> OSI 4계층은 네트워크 인터페이스 계층, 인터넷 계층, 전송 계층, 응용 계층으로 나뉩니다. 응용 계층의 예시로는 HTTP, FTP, SMTP, SSH, Telnet, DNS 등의 프로토콜이 있습니다. 전송 계층은 TCP, UDP 프로토콜이 있으며 인터넷 계층은 IP, IMCP, AR 등이 있습니다. 네트워크 인터페이스 계층에는 이더넷 등이 있습니다.</li>
    <li> TCP/IP 4계층에서 시작한 네트워크 표준이 꾸준히 갱신되면서 하위 레이어가 다시 세분화 되었습니다. TCP/IP 4계층의 Network Interface Layer를 Data link Layer와 Physical Layer로 나뉘어져서 TCP/IP 5계층 모델이 되었습니다. </li>
  </ul>
</details>

<details>
  <summary>Encapsulation & Decapsulation에 대해 설명해주세요</summary>
  <ul>
    <li> 캡슐화란 통신 프로토콜의 특성을 포함한 정보를 Header에 포함시켜서 하위 계층에 전송하는 것을 말합니다. 통신 상대측에서 이러한 Header를 역순으로 제거하면서 원래의 Data를 얻는 과정을 역캡슐화라고 합니다 </li>
  </ul>
</details>

<details>
  <summary>PDU(Protocol Data Unit)에 대해 설명해주세요</summary>
  <ul>
    <li> PDU는 네트워크 계층별로 데이터를 주고받는 단위를 말합니다. 응용 계층은 메시지, 전송 계층은 세그먼트(TCP)/데이터그램(UDP), 네트워크 계층은 패킷, 데이터 링크 계층은 프레임, 물리 계층은 비트 형태로 전달됩니다.</li>
    <li> 계층별로 데이터를 표현하는 단위가 다르기 때문에 각 계층에서 어떤 구조의 데이터가 전달될지 표현하기 위해 PDU가 필요합니다.</li>
  </ul>
</details>

<details>
  <summary>TCP/UDP에 대해 설명해주세요</summary>
  <ul>
    <li> TCP는 연결형, 신뢰성 전송 프로토콜 입니다. 연결 지향적 서비스를 제공하기 위해 데이터를 제공하기 전에 3-way 핸드쉐이크을 하여 두 호스트의 전송계층 사이에 논리적 연결을 성립시킵니다. 신뢰성 있는 서비스를 제공하기 위해 오류제어, 흐름 제어, 혼잡 제어를 수행합니다. 신뢰성을 보장하기 위해 헤더가 더 크고 속도가 비교적 느리다는 단점이 있습니다.</li>
    <li>UDP는 비연결형 프로토콜로 3-way 핸드쉐이크 등 세션 연결 수립 과정이 없습니다. 또한 비신뢰성 프로토콜로 오류제어, 흐름제어, 혼잡제어를 수행하지 않습니다. 이런 단순한 흐름때문에 오버헤드가 적고 수신여부를 확인하지 않아 속도가 빠르다는 장점이 있습니다.</li>
    <li>TCP는 신뢰성이 중요한 통신(HTTP, File 전송 등)에 쓰이고, UDP는 실시간성이 중요한 통신(동영상 스트리밍 등)에 주로 사용됩니다.</li>
  </ul>
</details>

<details>
  <summary>TCP헤더와 UDP헤더는 어떻게 구성되어있나요?</summary>
  <ul>
    <li> TCP는 시퀀스 넘버, ack 넘버, syn/ack/fin과 같은 플래그, 윈도우 사이즈, 체크섬과 같은 다양한 필드가 있어 헤더가 20~60바이트로 크고 복잡합니다. UDP 헤더는 단순히 데이터만 보내기 위한 프로토콜이라 출발지/목적지 포트, 길이, 체크섬 등만 들어있는 8byte 고정 헤더입니다.</li>
  </ul>
</details>

<details>
  <summary>TCP/IP에 대해 설명해주세요</summary>
  <ul>
    <li> TCP/IP는 인터넷에서 사용하는 프로토콜 그룹을 칭합니다. TCP/IP는 Application layer(응용계층), Transport layer(전송계층), Network layer, Data link layer, Physical layer로 5개의 계층으로 나뉩니다. 그 중에 전송계층은 두 응용 계층 사이에서의 process-to-process 통신을 제공합니다. 전송계층은 응용계층으로부터 메시지를 받아 전송계층 패킷으로 캡슐화하여 전송합니다.(segment 또는 datagram으로 부릅니다.) 전송계층의 주된 프로토콜은 TCP, UDP입니다. TCP(Transmission Control Protocol)는 연결형, 신뢰성 전송 프로토콜입니다. TCP로 전송하는 패킷을 segment라고 부릅니다. UDP(User, Datagram Protocol)는 비연결형, 비신뢰성 전송 프로토콜입니다. UDP로 전송하는 패킷을 datagram이라고 합니다.</li>
  </ul>
</details>

<details>
  <summary>TCP의 연결과 해제 과정에 대해 설명해주세요</summary>
  <ul>
    <li> </li>
  </ul>
</details>

<details>
  <summary>3-way handshaking / 4-way handshaking에 대해 설명해주세요</summary>
  <ul>
    <li> 3-way handshake는 TCP/IP 프로토콜로 통신하기 전, 정확한 정보 전송을 위해 상대방 컴퓨터와 세션을 수립하는(연결을 하는) 과정입니다.(TCP 연결 초기화) 클라이언트가 서버에게 접속을 요청하는 SYN 패킷을 보내면, 서버는 요청을 수락하는 ACK를 포함하여 SYN+ACK 패킷을 클라이언트에게 발송합니다. 클라이언트가 이것을 수신한 후, 다시 ACK를 서버에게 발송하면 연결이 이루어지고, 이로써 데이터를 주고받을 수 있게됩니다.</li>
    <li> 4-way handshaking은 tcp의 연결을 종료하기 위한 과정입니다. TCP connection termination은 양방향으로 2개의 연결이 독립적으로 닫히기 때문에 4-way 단계를 밟게 됩니다. Client process에서 active close를 하면, client tcp에서 FIN 세그먼트를 보냅니다. Server는 FIN 세그먼트를 받았다는 응답에 대한 ACK를 client로 보냅니다. 이때, server 내의 process에게 EOF를 보내지만, 아직 process는 close되지 않을 수 있습니다. Server process로부터 passive close를 받으면 server tcp에서 FIN 세그먼트를 client TCP에게 보냅니다. Server tcp가 ACK를 받게 되면 연결이 종료됩니다.</li>
  </ul>
</details>

<details>
  <summary>4-way handshaking에서 바로 연결을 끊지 않고 time wait가 필요한 이유를 설명해주세요 </summary>
  <ul>
    <li> TCP는 신뢰성을 보장해야 하므로, 4-way 핸드셰이크 이후 바로 연결을 닫으면 지연 패킷이 다음 연결에 섞이거나, 마지막 ACK가 상대에게 도달하지 않았을 때 처리할 수 없습니다. TIME_WAIT은 지연 패킷이 모두 사라질 시간을 확보하고, 상대가 FIN을 재전송할 경우 ACK를 다시 보내기 위해 약 2MSL 동안 연결 정보를 유지하는 단계입니다.</li>
  
  </ul>
</details>

<details>
  <summary>TCP에서 흐름제어와 혼잡제어를 설명해주세요 </summary>
  <ul>
    <li> 흐름제어란 endsytem과 endsystem 사이에서 송신측과 수신측의 데이터 처리 속도 차이를 해결하기 위한 기법입니다. flow control은 수신자가 패킷을 지나치게 많이 받지 않도록 조절하는 것입니다. 기본 개념은 수신자가 송신자에게 현재 자신의 상태를 피드백한다는 것입니다.</li>
    <li> 혼잡제어란 송신측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 기법입니다.</li>
  </ul>
</details>

<details>
  <summary>TCP에서 흐름제어는 어떤 방식으로 일어나나요?</summary>
  <ul>
    <li> 응용 계층에서 데이터를 전송할 때 송신측 애플리케이션은 소켓에 데이터를 쓰게됩니다. 이 데이터는 전송 계층으로 전달되어 세그먼트라는 작은 단위로 나누어집니다. 전송 계층은 이 세그먼트를 네트워크 계층에 넘겨줍니다. 전송된 데이터는 수신자쪽으로 전달되어 수신자 쪽에서는 수신 버퍼라는곳에 저장됩니다. 이때 수신자 쪽에서는 수신 버퍼의 용량을 넘치지게 하지 않도록 조절해야합니다. 만약 수신측에서 제한된 저장 용량을 초과한 이후 도착된 데이터는 손실될 수 있으며, 손실된다면 불필요한 응답과 전송이 빈번하게 발생할 수 있기 때문입니다.</li>
    <li> 이때 수신자쪽에서 자신의 수신 버퍼의 남은 용량을 송신자에게 알려주는데, 이를 수신 윈도우라고 합니다. 송신자는 수신자의 수신 윈도우를 확인해 수신자의 수신 버퍼 용량을 초과하지 않도록 데이터를 전송합니다. 이를 통해 데이터 전송 중에 수신 버퍼가 넘치는 현상을 방지하면서, 안정적인 데이터 전송을 보장합니다. 이것을 흐름제어라고 합니다.</li>
    <li> stop-and-wait 방식과 슬라이딩윈도우 방식이 있습니다. stop-and-wait는 매번 전송한 패킷에 대해 확인 응답을 받아야만 그 다음 패킷을 전송하는 방법이며, 슬라이딩 윈도우는 수신측에서 설정한 윈도우 크기만큼 송신측에서 확인응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 방식입니다.</li>
  </ul>
</details>

<details>
  <summary>TCP에서 혼잡제어는 어떤 방식으로 일어나나요?</summary>
  <ul>
    <li> 송신측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달됩니다. 만약 한 라우터에 데이터가 몰릴 경우 자신에게 온 데이터를 모두 처리할 수 없게 됩니다. 이럴 경우 호스트들은 또다시 재전송을 하게되고 결국 혼잡만 가중시키게 되어 오버플로우나 데이터 손실을 발생시키게 됩니다. 따라서 이러한 네트워크의 혼잡을 피하기 위해 송신측에서 보내는 데이터의 전송속도를 강제로 줄이게 되는데, 이러한 작업을 혼잡제어라고 합니다. 또한 네트워크 내 패킷의 수가 과도하게 증가하는 현상을 혼잡이라고 하며, 혼잡 현상을 방지하거나 제거하는 기술을 혼잡제어라고 합니다.</li>
    <li> 해결방법은 AIMD(Additive Increase/Multiple Decrease), Slow start, Fast Retransmit(빠른 재전송), Fast Recovery(빠른 회복)이 있습니다.</li>
    <li> AIMD는 처음에 패킷을 하나씩 보내고 이것이 문제없으면 윈도우의 크기를 1씩 증가시키며 전송하는 방법입니다. 전송 실패하거나 일정 시간을 넘으면 패킷 보내는 속도를 절반으로 줄입니다. 공평한 방식으로, 나중에 진입한 쪽이 처음엔 불리하지만 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있습니다. 문제점은 초기에 네트워크의 높은 대역폭을 사용하지 못해 시간이 오래걸리고, 혼잡해지는 상황을 미리 감지하지 못하고 혼잡해지고나서야 대역폭을 줄이는 점이 있습니다.</li>
    <li> Slow start는 AIMD와 마찬가지로 패킷을 하나씩 보내면서 시작하고, 문제없이 도착하면 각각의 ack 패킷마다 윈도우 사이즈를 1씩 늘려줍니다. 즉, 한 주기가 지나면 윈도우 사이즈가 2배가 됩니다. AIMD에 반해 지수 함수 꼴로 증가하게 되며, 대신 혼잡 상황이 발생하면 1로 떨어뜨리게 됩니다. 처음에는 네트워크의 수용량을 예상할 수 없지만 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량을 어느정도 예상할 수 있습니다. 혼잡 현상이 발생하였던 윈도우 사이즈의 절반까지는 이전처럼 지수 함수 꼴로 창 크기를 증가시키고 그 이후부터는 완만하게 1씩 증가시킵니다.</li>
    <li> 빠른 재전송은 TCP 혼잡 조절에 추가된 정책입니다. 패킷을 받는 쪽에서 먼저 도착해야할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ack 패킷을 보내게 됩니다. 순서대로 잘 도착한 마지막 패킷의 다음 패킷의 순번을 ack 패킷에 실어보내게 되므로, 중간에 하나가 손실되면 송신측에서는 순번이 중복된 ack 패킷을 받게 됩니다. 이것을 감지하는 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있습니다. 중복된 순번의 패킷을 3개 받으면 재전송을 하게 됩니다. 이때 혼잡이 일어났다고 갑지하고 윈도우 사이즈를 줄이게 됩니다.</li>
    <li> 빠른 회복은 혼잡한 상태가 되면 윈도우 사이즈를 1로 줄이지 않고 반으로 줄이고 선형 증가시키는 방법입니다. 이 정책까지 적용하면 혼잡 상황을 한번 겪고 나서는 순수한 AIMD 방식으로 동작하게 됩니다.</li>
  </ul>
</details>


## HTTP/HTTPS
<details>
  <summary>HTTP와 HTTPS가 뭔가요?</summary>
  <ul>
    <li> HTTP는 HyperText Transfer Protocol의 약자로 서버-클라이언트 모델을 따르면서 request/response 구조로 웹 상에서 정보를 주고받을 수 있는 프로토콜입니다. TCP/IP 기반으로 작동하며,  HTTP의 가장 큰 특징은 Connectionless와 Stateless 입니다.</li>
    <li> Request 메시지는 start line(method, path, HTTP version), headers, body로 이루어져 있고 response message는 status line(HTTP version, status code, status message), headers, body로 이루어져 있습니다.</li>
    <li> HTTP는 서버에 연결 후 요청에 응답을 받으면 연결을 끊어버리는 Connectionless 특성을 갖습니다. 이로 인해 많은 사람이 웹을 이용하더라도 실제 동시 접속을 최소화하여 더 많은 유저의 요청을 처리할 수 있습니다. 하지만 연결을 끊었기 때문에, 클라이언트의 이전 상태(로그인 유무 등)를 알 수가 없다는 Stateless 특성이 생기게 됩니다. 정보를 유지할 수 없는 Connectionless, Stateless 특성을 가진 HTTP의 단점을 해결하기 위해, cookie, session, jwt 등이 도입되었습니다.</li>
    <li> HTTP는 정보를 text 형식으로 주고받기 때문에 중간에 인터셉트할 경우 데이터 유출이 발생할 수 있는 문제가 있어서 이를 해결하고자 HTTP에 암호화를 추가한 프로토콜이 바로 HTTPS입니다. SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을때 공개키 방식으로 암호화를 합니다.</li>
  </ul>
</details>

<details>
  <summary>TCP의 keep-alive와 HTTP의 keep-alive의 차이는 무엇인가요? </summary>
  <ul>
    <li> TCP의 keep-alive: TCP 수준에서의 연결이 여전히 활성 상태인지 확인하기 위해 사용되는 기능이다.일정 시간 동안 데이터 교환이 없을 경우, keep-alive 패킷을 송수신하여 연결이 유효한지 검사한다.</li>
    <li> HTTP의 keep-alive: HTTP 프로토콜 수준에서 클라이언트와 서버 간에 여러 HTTP 요청/응답을 동일한 TCP 연결을 재사용하여, TCP 연결의 설정과 종료에 따른 지연과 오버헤드를 줄인다.</li>
    <li> TCP Keep-Alive는 TCP 레벨에서 상대 노드가 살아있는지를 확인하는 Health-check 기능입니다. 반면 HTTP Keep-Alive는 애플리케이션 계층에서 TCP 연결을 재사용하여 여러 HTTP 요청을 처리하는 기능입니다. TCP Keep-Alive는 OS가 관리하는 low-level 기능이고, HTTP Keep-Alive는 HTTP 성능 최적화를 위한 connection reuse 기능이므로 목적과 레이어가 완전히 다릅니다.</li>
  </ul>
</details>

<details>
  <summary>HTTP와 장단점에 대해 설명해주세요 </summary>
  <ul>
    <li> HTTP는 단순하고 표준화되어 있어서 어디서든 사용할 수 있다는 장점이 있습니다. Stateless한 구조라 서버 확장성이 뛰어나고, 캐싱이나 CDN과 잘 연동됩니다. 반면 평문 기반이라 보안에 취약하고(HTTPS로 해결 가능), 상태를 유지하지 않기 때문에 세션 관리가 필요합니다. 또한 요청-응답 기반이라 실시간 푸시가 어렵고, 텍스트 기반이라 오버헤드가 크다는 단점이 있습니다.</li>
  
  </ul>
</details>

<details>
  <summary>HTTP의 버전별 특징을 설명해주세요 </summary>
  <ul>
    <li> HTTP1.1은 가장 오래된 구조이며 요청 1개당 연결 1개라는 특징이 있습니다. 하나의 요청이 느리면 뒤의 요청도 기다려야하고, 브라우저가 해결하려고 동시에 6개 정도 커넥션을 열어 요청을 처리합니다. HOL Blocking(헤드라인 블로킹) 문제가 발생할 수 있습니다. 또한 그전 HTTP 1.0에 비해 keep-alive로 연결 재사용이 가능해졌습니다.</li>
    <li> HTTP2.0은 멀티플렉싱이라는 기법이 도입되었습니다. 하나의 TCP 연결에서 여러 요청을 동시에 병렬 처리할 수 있게 되었습니다. HTTP 1.1의 가장 큰 문제였던 HOL Blocking을 해결했고, HPACK을 통해 헤더를 압축해 오버헤드를 줄여 성능을 개선시켰고, 서버가 클라이언트 요청 없이 리소스를 먼저 push할 수 있게 되었습니다.또한 텍스트 기반이 아니라 바이너리 프레이밍 구조를 사용해 파싱속도와 오류를 감소시켰습니다. 하지만 TCP 기반이라 패킷 유실 시 재전송을 위해 전체 스트림이 잠시 멈춰버리는 문제가 발생했습니다.</li>
    <li> HTTP3.0은 TCP에서 QUIC(UDP기반)으로 변경해 패킷 단위로 독립적 처리를 하도록 해서 HOL Blocking 문제를 완전히 해결했으며 모바일환경(와이파이와 데이터 전환)에 매우 유리합니다. 또한 핸드쉐이크 횟수도 줄여 연결 수립을 더 빠르게 만들었습니다. </li>
  
  </ul>
</details>

<details>
  <summary>멀티 플렉싱에 대해 조금 더 자세히 설명해주세요 </summary>
  <ul>
    <li> 멀티플렉싱은 하나의 물리적인 연결 위에서 여러 논리적인 스트림을 동시에 처리하는 기술입니다. HTTP/2에서는 하나의 tcp 연결 안에 stream이라는 단위를 두고 각 요청/응답을 프레임으로 쪼갠 뒤 stream ID를 붙여서 섞어 보내고 다시 수신측에서는 스트림 아이디 기준으로 재조립합니다.</li>
  </ul>
</details>

<details>
  <summary>그냥 알아두면 좋은거 </summary>
  <ul>
    <li> 소켓은 OS가 생성하는 네트워크 통신 엔드포인트다.</li>
    <li> 소켓은 OS 커널 내부에 만들어집니다. 애플리케이션은 소켓 핸들(fd)만 받아서 파일처럼 read/write 합니다.</li>
    <li> TCP 커넥션은 클라이언트 소켓과 서버 소켓의 1:1 연결이다.</li>
    <li>서버는 포트는 하나지만 접속자마다 소켓은 여러 개 생긴다.</li>
    <li>소켓 유지(Keep-Alive)는 스레드를 점유하지 않는다.</li>
    <li>소켓은 가볍다. 매번 연결을 끊고 생성하는게 더 비싸다.</li>
    <li>각 TCP 소켓은 (서버IP, 서버Port, 클라이언트IP, 클라이언트Port) 조합으로 구별되어 서로 다른 연결로 관리할 수 있다.</li>
  </ul>
</details>

<details>
  <summary>HTTP와 HTTPS의 통신 흐름을 설명해주세요 </summary>
  <ul>
    <li> HTTP는 TCP 연결 후 평문으로 요청/응답을 주고받는 구조입니다.</li>
    <li> HTTPS는 SSL 인증이 추가됩니다. 먼저 앱 서버를 만드는 기업은 HTTPS를 만들기 위해 공개키/개인키를 만듭니다. 신뢰할 수 있는 CA기업을 선택하고, 그 기업에 공개키 관리를 부탁하며 계약합니다. (CA는 Certification Authority로, 공개키를 저장해주는 신뢰성이 검증된 민간기업) CA기업은 해당 기업의 이름, A서버 공개키, 공개키 암호화 방법을 담은 인증서를 만들고, 해당 인증서를 CA기업의 개인키로 암호화해서 A서버에 제공합니다.이제 서버는 암호화된 인증서를 가지고 A서버에 공개키로 암호화되지 않은 HTTPS가 아닌 요청이 오면 이 암호화된 인증서를 클라이언트에게 건네줍니다. CA기업의 공개키는 클라이언트가 이미 알고 있습니다.(세계적으로 신뢰할 수 있는 기업으로 등록되어 있기 때문에 브라우저가 인증서를 탐색하여 해독 가능). 브라우저는 해동한 뒤 A서버의 공개키를 얻게 되었습니다. 클라이언트는 A서버와 핸드쉐이킹 과정에서 주고 받은 난수롤 조합해 pre-master-scret-key를 생성해 A 서버의 공개키로 해당 대칭키를 암호화 하여 서버로 보냅니다. A서버는 암호화된 대칭키를 자신의 개인키로 복호화 하여 클라이언트와 동일한 대칭키를 획득합니다. 클라이언트 서버는 각각 pre-master-secret-key를 master-secret-key로 만들고, 이 키를 통해 session-key를 생성하고 이를 이용해 대칭키 방식으로 통신하게 됩니다. 각 통신이 종료되면 해당 세션키는 파기됩니다.</li>
  </ul>
</details>

<details>
  <summary>HTTP의 Method와 각각의 사용되는 경우를 설명해주세요 </summary>
  <ul>
    <li> GET은 method는 클라이언트가 서버에게 정보를 요청할 때 사용하는 method 입니다. GET을 통한 요청은 URL 주소 끝에 key-value 쌍으로 parameter를 포함하여 전송을 하는데, 이 부분을 Query String을 이라고 부릅니다. GET의 주요한 특징중 하나는 캐시가 가능하다는 것입니다. 한 번 서버에 GET요청을 한적이 있다면 브라우저가 그 결과를 저장해 둡니다. 이후 동일한 요청은 브라우저에 저장된 값으로 가져올 수 있습니다.</li>
    <li> POST method는 클라이언트가 body를 통해 전달한 데이터를 서버가 처리하도록 요청하는 method 입니다. 서버는 POST 메시지를 받으면 꼭 리소스를 등록하는 것만 아니라, 리소스마다 다양하게 처리를 합니다. 데이터를 생성하거나 변경하기도 하지만 특정 프로세스를 처리하기도 합니다.</li>
    <li> PUT메서드는 리소스를 대체(덮어쓰기)하며 해당 리소스가 없으면 생성합니다.</li>
    <li> PATCH메서드는 리소스의 일부분을 수정할때 사용합니다.</li>
  </ul>
</details>

<details>
  <summary>GET,POST의 차이점에 대해 설명해주세요 </summary>
  <ul>
    <li> GET 메소드는 클라이언트가 서버에게 `리소스를 요청`할 때 사용하는 메소드이고, POST 메소드는 서버에게 데이터 처리(주로 `생성`)를 요청할 때 사용하는 메소드입니다. GET 요청의 경우 필요한 정보를 특정하기 위해 URL 뒤에 Query String을 추가하여 정보를 조회하고, POST 요청의 경우 전달할 데이터를 Body 부분에 포함하여 통신합니다. GET 요청의 경우 URL 뒤의 Query String까지 포함해서 브라우저 히스토리에 남게 되고 캐시가 가능하지만, POST 요청의 경우 브라우저 히스토리에 남지 않고 캐시도 불가능합니다.</li>
  
  </ul>
</details>

<details>
  <summary>PUT,PATCH에 대해 설명해주세요 </summary>
  <ul>
    <li> PUT 메소드와 PATCH 메소드는 모두 서버의 리소스를 업데이트하는 메소드라는 공통점이 있습니다. 하지만 PUT 요청의 경우 모든 리소스를 수정,대체하고, PATCH 요청의 경우 일부 리소스만 수정하게 됩니다.</li>
  </ul>
</details>

<details>
  <summary>대칭키/비대칭키 암호화 방식에 대해 설명해주세요 </summary>
  <ul>
    <li> 대칭키 암호화는 하나의 같은 키로 암/복호화를 수행해 빠르지만 안전한 키 전달이 어렵습니다. 비대칭키 암호화는 공개키와 개인키를 사용해 키 교환은 안전하게 수행되지만 속도가 느립니다. 그래서 HTTPS는 비대칭키로 세션키(대칭키)를 교환한 뒤, 실제 데이터 통신은 빠른 대칭키로 암호화하는 하이브리드 구조를 사용합니다.”</li>
  
  </ul>
</details>

<details>
  <summary>멱등성이 무엇이고, 그것을 보장할 수 있는 메서드는 무엇인가요? </summary>
  <ul>
    <li> 멱등성은 같은 요청을 여러 번 보내도 서버의 상태가 변하지 않는 성질입니다. HTTP 메서드 중 GET, PUT, DELETE, HEAD, OPTIONS는 멱등성을 가지며, POST와 PATCH는 멱등성이 없습니다. 특히 PUT은 ‘자원을 특정 값으로 덮어쓰기’ 때문에 몇 번 호출하더라도 결과가 동일하고, POST는 새로운 자원을 생성하는 의미이므로 멱등성이 없습니다.</li>
  </ul>
</details>

<details>
  <summary>Connection Timeout과 Read TimeOut에 대해 설명해주세요 </summary>
  <ul>
    <li> Connection Timeout은 TCP 연결을 맺는 데 너무 오래 걸릴 때 발생하는 타임아웃이고, Read Timeout은 연결이 성립된 후 서버가 응답을 주지 않을 때 발생하는 타임아웃입니다. 즉, Connection Timeout은 네트워크 단절, 서버 문제, 방화벽 문제 등으로 연결 실패, Read Timeout은 서버 내부 처리 시간이 너무 길거나 서버가 응답 직전 멈춘 경우에 대한 응답 지연에 대한 타임아웃입니다.</li>
  </ul>
</details>

<details>
  <summary>http status code에 대해 설명해주세요 </summary>
  <ul>
    <li> 웹 개발시 서버와 클라이언트가 HTTP 통신할 때 주고받아야 할 값중에 하나입니다. 클라이언트로 부터 받은 request에 대한 서버의 response에 대한 간략할 설명 이라고 볼 수 있습니다. 상황에 알맞는 status code를 response에 담아서 클라이언트에 넘겨주면 이를 토대로 클라이언트는 알맞는 대응을 할 수 있습니다.</li>
    <li>1xx (정보): 요청을 받았으며 작업을 계속한다. 2xx (성공): 클라이언트가 요청한 동작을 성공적으로 수신하여 이해했고 성공적으로 처리하였다. 3xx (리다이렉션): 요청을 완료하기 위해 추가 작업 조치가 필요하다. 4xx (클라이언트 오류): 클라이언트의 요청에 문제가 있다. 5xx (서버 오류): 서버가 유효한 요청의 수행을 실패했다.</li>
    <li><img width="641" height="398" alt="image" src="https://github.com/user-attachments/assets/a5efb788-c977-4afb-90d5-20c928cfe03d" />
</li>
  
  </ul>
</details>


## 인증 및 인가
<details>
  <summary>쿠키와 세션의 차이를 설명해주세요 </summary>
  <ul>
    <li> 쿠키는 클라이언트(브라우저) 로컬에 key-value 쌍으로 저장되는 데이터 파일입니다. 유효시간 내에서는 브라우저가 종료되어도 계속 유지됩니다. 세션은 브라우저가 종료되거나, 서버에서 해당 세션을 삭제할 수 있기 때문에 쿠키보다 보안성이 좋습니다. 또한 서버에 데이터를 저장하므로 서버 용량이 허용하는 한에서 제한 없이 데이터를 저장할 수 있다는 장점이 있지만 서버의 부하가 커진다는 단점이 될 수 있습니다.</li>
    <li> 쿠키와 세션을 사용하는 이유는 HTTP의 connectionless(비연결성), stateless(비상태성)라는 특징 때문입니다</li>
  </ul>
</details>

<details>
  <summary> 세션도 보안이 좋은데 쿠키를 사용하는 이유는 뭔가요? </summary>
  <ul>
    <li> 세션은 사용자 인증 정보처럼 보안이 필요한 데이터를 서버에 저장하기 위한 기술이고, 쿠키는 클라이언트에서 유지해야 하는 간단한 데이터(개인 설정, 장바구니 등)를 저장하기 위한 기술입니다. 세션이 보안이 좋다고 해서 쿠키를 없앨 수는 없고, 역할 자체가 다르기 때문에 두 기술은 서로 대체 관계가 아닙니다</li>
  </ul>
</details>

<details>
  <summary> 쿠키와 세션의 사용 예시에 대해 말해주세요 </summary>
  <ul>
    <li> 쿠키는 쇼핑몰의 장바구니 기능, 로그인시 아이디와 비밀번호 저장(또는 자동로그인), 팝업에서 “오늘 더 이상 이 창을 보지 않음 체크 등이 있습니다.</li>
    <li> 세션은 로그인 유지 기능이 있습니다.</li>
  </ul>
</details>

<details>
  <summary> cors에 대해 아는대로 설명해주세요 </summary>
  <ul>
    <li> CORS는 브라우저의 보안 정책 때문에 서로 다른 Origin 간 요청이 차단되는 것을 해결하기 위한 메커니즘입니다. 서버는 Access-Control-Allow-Origin 등 여러 헤더를 통해 어떤 Origin을 허용할지 명시해주고,브라우저는 이를 확인해 요청을 허용합니다. 즉, CORS는 서버 문제가 아니라 브라우저의 보안 정책에 의한 것입니다.</li>
    <li> 브라우저는 특정 조건(Post, Put, Delete)에서는 브라우저가 바로 요청을 보내지 않고 Options 요청(preflight)를 먼저 보내고 성공 응답이 오면 요청을 보냅니다.</li>
  </ul>
</details>

<details>
  <summary> 헤더를 설정해도 코스 에러가 해결 안되는 경우가 뭐가있나요? </summary>
  <ul>
    <li> Preflight 요청을 서버가 처리하지 않는 경우가 있습니다. 게이트웨이가 옵션스를 프록시로 통과시키지 않는다거나, 스프링 시큐리티에서 차단됐거나, 엔진엑스와 같은곳에서 막았거나 하면 프리플라이트와 같은 단계에서 막습니다. 또한 Credentials true 사용시 Access-Control-Allow-Origin이 * 로 설정되는 경우 안될수 있습니다. 이외에도 프록시 단에서 코스 헤더를 지워버리는 경우가 있습니다.</li>
  </ul>
</details>

<details>
  <summary> 쿠키와 세션을 이용한 로그인 방식을 설명해주세요 </summary>
  <ul>
    <li> 클라이언트가 로그인을 하면 서버는 회원정보를 대조하여 인증을 합니다.(authentication) 회원 정보(클라이언트 정보)를 세션저장소에 생성하고 session ID를 발급합니다. http response header 쿠키에 발급한 session ID를 담아서 보냅니다. 클라이언트에서는 session ID를 쿠키 저장소에 저장하고 이후에 http request를 보낼 때마다 쿠키에 session ID를 담아서 보냅니다. 서버에서는 쿠키에 담겨져서 온 session ID에 해당하는 회원 정보를 세션 저장소에서 가져옵니다.(authorization) 응답 메시지에 회원 정보를 바탕으로 처리된 데이터를 담아서 클라이언트에 보냅니다.</li>
    <li> session ID만 쿠키에 담겨서 요청을 보내기 때문에 요청 때마다 사용자 정보를 쿠키에 담아서 전송하는 것보다 안전합니다. 하지만 session ID만 노출되어 악의를 가진 다른 사용자가 이를 이용해 서버에 요청하면 서버는 구별해낼 수 있는 방법이 없습니다. 이를 Session hijacking 이라고 합니다. 해결책으로는 HTTPS 를 사용하거나 session에 짧은 주기로 만료시간을 설정하는 방법이 있습니다. 또한 세션과 쿠키를 이용한 로그인 방식은 Load Balancing 및 서버 효율성 관리 및 확장이 어려워질 수 있다는 단점이 있습니다.</li>
  </ul>
</details>

<details>
  <summary> 세션도 보안이 좋은데 쿠키를 사용하는 이유는 뭔가요? </summary>
  <ul>
    <li> OSI 7계층은 네트워크 통신을 표준화한 모델로, 통신 시스템을 7단계로 나누어 설명한 것입니다. 이 모델은 프로토콜을 기능별로 나눴으며, 각 계층은 하위 계층의 기능만을 이용하고 상위 계층에게 기능을 제공합니다. 일반적으로 상위 계층의 프로토콜은 소프트웨어로, 하위 계층의 프로토콜은 하드웨어로 구현됩니다.</li>
  
  </ul>
</details>

<details>
  <summary> XSS, CSRF 공격에 대해 아는대로 설명해주세요 </summary>
  <ul>
    <li> XSS는 신뢰할 수 있는 웹 페이지 안에 공격자의 스크립트가 들어가서, 사용자의 브라우저에서 실행되는 공격입니다. 쿠키 탈취, DOM 변조, 악성 요청 트리거 등 브라우저 권한 내에서 거의 모든 것을 할 수 있고, 출력 시 이스케이프, CSP(Content-Security-Policy) 스크립트 삽입 자체를 막는 정책, HttpOnly(JS의 document.cookie를 이용해 쿠키에 직접 접근하여 탈취하는 것을 방지) 등으로 방어합니다</li>
    <li> CSRF는 사용자가 로그인해 있는 상태를 악용해서 그 사용자의 브라우저로부터 의도하지 않은 요청을 서버에 보내는 공격입니다. 쿠키가 자동으로 붙는 점을 이용하고, CSRF 토큰(시큐어 코딩), SameSite 쿠키, refrerer 검증 등으로 방어합니다.</li>
  </ul>
</details>

## Block,Nonblocking

<details>
  <summary> blocking/nonblocking의 차이점이 뭔지 설명해주세요 </summary>
  <ul>
    <li> Blocking은 I/O가 끝날 때까지 스레드가 대기하는 방식이고, Non-Blocking은 작업 여부와 관계없이 즉시 리턴하는 방식입니다. Blocking은 스레드가 낭비되는 반면, Non-Blocking은 스레드 효율이 높아 대규모 트래픽에서 유리합니다.</li>
  
  </ul>
</details>

<details>
  <summary> Synchronous/Asynchronous의 차이점이 뭔지 설명해주세요 </summary>
  <ul>
    <li> Synchronous는 함수가 결과를 직접 반환해야 다음 흐름으로 넘어가는 방식이고, Asynchronous는 결과는 나중에 시스템이 알려주는 방식입니다. 이는 스레드 대기 여부(Block/Non-block)와는 다른 개념으로, Sync/Async는 제어 흐름 관점, Blocking/Non-Blocking은 스레드 관점의 차이입니다.</li>
  
  </ul>
</details>

<details>
  <summary> blocking/nonblocking와 Synchronous/Asynchronous의 차이점이 뭔지 설명해주세요 </summary>
  <ul>
    <li> Blocking/Non-blocking은 스레드 관점입니다. Blocking은 작업이 끝날 때까지 스레드가 대기하고, Non-blocking은 결과가 준비되지 않아도 즉시 리턴합니다. 반대로 Synchronous/Asynchronous는 제어 흐름 관점입니다. 동기는 호출한 쪽이 직접 결과를 받아야 다음 로직으로 진행하고, 비동기는 결과를 콜백/이벤트로 나중에 전달받습니다. 두 개념은 다른 축이기 때문에 Async + Non-blocking 같은 고성능 모델도 가능합니다.</li>
    <li> Blocking/Non-blocking은 스레드가 대기하느냐 바로 리턴하느냐의 차이이고, Synchronous/Asynchronous는 결과를 호출한 쪽이 직접 받느냐, 아니면 콜백/이벤트로 나중에 받느냐의 차이입니다.</li>
  
  </ul>
</details>

<details>
  <summary> Blocking I/O & Non-Blocking I/O에 대해 설명해주세요 </summary>
  <ul>
    <li> Blocking I/O는 read/write 호출 시 데이터가 준비될 때까지 스레드가 대기하는 방식이고, Non-Blocking I/O는 준비되지 않아도 즉시 리턴해 스레드가 다른 작업을 수행할 수 있는 방식입니다. Blocking은 구현이 단순하지만 스레드를 많이 필요로 하고, Non-Blocking은 epoll 같은 이벤트 기반 처리 덕분에 적은 스레드로 많은 연결을 처리할 수 있어 Netty, WebFlux 같은 고성능 서버에서 사용됩니다.</li>
  </ul>
</details>



