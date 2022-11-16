# HTTPS

HTTPS는 HTTP의 보안 측면을 강화한 웹 통신 프로토콜입니다. HTTP와 달리 소켓 통신에서 평문을 이용하는 대신, 웹 상에서 정보를 암호화하는 SSL이나 TLS 프로토콜을 통해 암호문을 주고 받습니다. 따라서 데이터의 적절한 보호를 보장합니다. 기본적으로 443번 포트를 사용합니다. 

HTTP에 비해 보안상의 이점을 가지고 있지만, 암호화 과정이 추가 돼 HTTP에 비해서는 느리다는 단점으 가집니다. 또한 인증서를 발급받고 유지하는 비용이 듭니다.

> 💡 인터넷 연결이 끊긴 경우 재인증 시간이 소요된다는데, 이는 추가로 학습해야 할 것 같다. 

## TLS(Transpot Layer Security), SSL(Secure Socket Layer)

TLS 프로토콜과 SSL 프로토콜은 모두 웹 상에서 정보를 암호화해 사용하는 프로토콜로, TLS는 SSL에서 발전한 형태입니다. 두 프로토콜의 주요 목표는 데이터의 기밀성, 무결성, 전자 서명입니다. 

- 기밀성: 데이터를 관련되지 않은 사람에게 노출하지 않는다.
- 무결성: 데이터의 변조를 막는다.
- 전자 서명: ID 및 디지털 인증서를 이용해 사용자를 인증한다. 

## HTTPS 동작 과정

공개키 암호화 방식과 대칭키 암호화 방식을 혼합한 하이브리드 방식을 사용합니다. 공개키 방식으로 대칭키를 서로 주고 받고, 대칭키를 이용해 데이터를 암호화 해 주고 받습니다.

<img width="1319" alt="image" src="https://user-images.githubusercontent.com/45311765/201861052-6e2f1aab-7476-4de2-aa8b-b457c57b44fb.png">

1. Client Hello: 클라이언트가 지원하는 TLS(SSL) 버전 지원되는 암호화 방식 목록, 랜덤 바이트 문자열(clinet random)을 전송한다. 
2. Server Hello: 서버의 SSL 인증서, 서버에서 선택한 암호화 방식, 랜덤 바이트 문자열(server random)
3. Verify Server Certificate: CA로 붙어 받은 목록을 확인해 서버가 인증서에 명시된 서버인지, 실제 해당 도메인의 소유자인지 등을 확인한다. 
4. Client Key Exchange: CA 인증서로 SSL 인증서를 복호화하고 얻은 서버 공개키로 랜덤 바이트 문자열(the premaster secret)을 암호화하여 전송한다. 
    - Send client certificate: 만약 서버가 클라이언트의 인증서를 요구한다면 서버의 인증서와 같은 방식으로 암호화를 진행하여 함께 전송한다.
5. Verify Client Certificate: 서버가 받은 the premaster secret을 서버 개인키로 복호화한다. 
6. Client Finished: 클라이언트가 `client random`, `server random`, `the premaster secret`을 이용해 대칭키로 활용할 `session key`를 생성한다.
7. Server Finished: 서버가 `client random`, `server random`, `the premaster secret`를 이용해 대칭키로 활용할 `session key`를 생성합니다. 서버가 세션 키로 암호화된 "finished" 메시지를 전송한다.
8. Exchange Messages: 핸드셰이크가 완료되고, 세션 키를 이용해 메세지를 주고 받는다.

## 참고 자료
- https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/
