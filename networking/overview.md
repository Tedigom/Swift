# Networking Overview

네트워킹의 세계는 복잡합니다. 사용자는 케이블 모뎀, DSL, Wi-Fi, 셀룰러 연결, 위성 업링크, 이더넷 및 심지어 전통적인 음향 모뎀과 같은 광범위한 기술을 사용하여 인터넷에 연결할 수 있습니다. 이러한 연결은 각각 대역폭, 지연 시간, 패킷 손실 및 신뢰성의 차이를 포함하여 고유한 특성을 가집니다.

<img src="https://developer.apple.com/library/archive/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/art/AboutNetworking.png" width = 500>

네트워크는 본질적으로 신뢰할 수 없습니다. (셀룰러 네트워크는 두 배로 신뢰할 수 없습니다.) 결과적으로, 좋은 네트워킹 코드는 다소 복잡한 경향이 있습니다. 때문에 어플리케이션은 다음과 같은 사항을 따라야 합니다.

- 작업을 수행하는 데 필요한 만큼의 데이터만 전송합니다.

- 가능한 timeout을 피해야 합니다.

- 사용자가 완료에 너무 오래 걸리는 트랜잭션을 쉽게 취소할 수 있도록 사용자 인터페이스를 설계합니다.

- 네트워크 성능이 저하되면 우아하게(?) degrade 하십시오. (유튜브 비디오의 자동 품질 조정 등을 생각하면 될 듯합니다.)

- 소프트웨어를 신중하게 설계하여 보안 위험을 최소화하십시오.

장치의 네트워크 환경은 즉시 변경될 수 있습니다. 프로그램 메인 스레드에서 동기식 네트워킹 코드를 실행하거나 네트워크 변경을 적절하게 처리하지 못하는 등 앱의 성능과 사용성에 악영향을 미칠 수 있는 간단한(이러한 파괴적인) 네트워킹 오류가 많습니다. 나중에 디버깅하는 대신 이러한 문제가 발생하지 않도록 프로그램을 설계하면 많은 시간과 노력을 절약할 수 있습니다. **네트워킹은 반드시 Dynamic하고 Asynchronous해야 합니다.**


## Network Levels

일반적으로 네트워크가 이루어지는 계층을 다음과 같이 표현합니다. (7계층 or 4계층)

 <img src = "https://asecurity.so/wp-content/uploads/2017/05/052817_1040_1.png">

 1. Application Layer: 사용자와 네트워크 간의 연결, 데이터를 생성합니다. (HTTP, FTP 등)

 2. Presentation Layer: 데이터 형식을 규정합니다. (JPEG, MPEG 등)

 3. Session Layer: 인증 및 서비스를 제공합니다. (SSH, TLS 등)

 4. Transport Layer: 프로세스 간의 데이터를 전송합니다. (TCP, UDP 등)

 5. Network Layer: 스위칭, 라우팅 등 데이터 경로를 설정합니다. (IP 등)

 6. Link Layer: 네트워크 기기 간의 데이터를 전송합니다. (Ethernet, LAN, Wifi 등)

 7. Physical Layer: 시스템 간을 물리적으로 연결하고 전기적 신호를 변환합니다. (Modem, Cable 등)

이러한 네트워크 과정에서 Frontend 단에서 해주어야할 일은  DNS lookups, TCP connections, SSL negotiation, Send Data, Receive and Handle Data 등을 말할 수 있습니다.

<img src="http://blog.catchpoint.com/wp-content/uploads/2016/10/ComponentBreakdown-1.png" width = 600>


### Reference:

- http://blog.catchpoint.com/2016/11/03/naming-conventions/

- https://developer.apple.com/library/archive/documentation/NetworkingInternetWeb/Conceptual/NetworkingOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010220

- https://digitalleaves.com/complete-guide-networking-in-swift/
