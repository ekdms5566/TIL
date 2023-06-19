# Nginx HTTP redirect 및 포트포워딩

목차  
[Nginx란?](#📍-nginx란)  
[포트포워딩(Port Forwarding) 설정](#📍-포트포워딩port-forwarding-설정)  
[HTTP redirect to HTTPS](#📍-http-redirect-to-https)

<hr>

## 📍 Nginx란?

[Nginx](https://www.nginx.com/)란 웹사이트의 서버를 도와주는 경량화 웹 서버이다. 클라이언트로부터 요청을 받았을 때 요청에 맞는 정적 파일을 전달하는 HTTP Web Server 또는 reverse proxy server로 활용하여 WAS의 부하를 줄일 수 있는 로드 밸런서 역할을 하기도 한다.

Nginx는 요청에 응답하기 위해 비동기 이벤트 기반 구조를 가진다. 이것은 아파치 HTTP 서버의 스레드/프로세스 기반 구조를 가지는 것과는 대조적이다. 이러한 구조는 서버에 많은 부하가 생길 경우의 성능을 예측하기 쉽게 해준다.

Apache의 C10K(동접자 수가 만 명이 넘어갈 때 효율적 방안) 문제점 해결을 위해 만들어진 Event-Driven 구조의 웹 서버로, 프로그램의 흐름이 이벤트에 의해 결정되는 방식이다.

_🐮 https까지 적용할 것이기 때문에 쉽게 리버스 프록시 서버를 설정할 수 있는 nginx를 사용할 것이다 ㅎ\_ㅎ_

**Event-Driven 처리 기반 구조**

<img src="https://user-images.githubusercontent.com/78911818/246707511-914dae10-4b27-4108-a6f4-330da8ad2e6f.png" width="300px">

- 한 개 또는 고정된 프로세스만 생성하고, 여러 개의 connection을 모두 Event-handler를 통해 비동기 방식으로 처리한다.
- 적은 양의 스레드만 사용하기 때문에 Context Swiching 비용이 적고, CPU 소모가 적다.
- Apache와 달리 동시 접속자 수가 많아져도 추가적인 생성 비용이 들지 않는다.
- CPU와 관계없이 모두 비동기 I/O 처리 방식을 사용하기 때문에 높은 성능을 제공한다

> Context Switching : 현재 진행하고 있는 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정

### Apache

**스레드/프로세스 기반 구조**

- 클라이언트 요청 하나 당 하나의 스레드가 처리하는 구조로, 사용자가 많을 경우 스레드 생성, 메모리, CPU 낭비가 심하다.

**MPM(Multi-Process Module)**

- MPM은 클라이언트의 요청을 여러 프로세스에 분배해주는 모듈로 `prefork`, `worker` 등의 모듈이 있다.

- prefork(다중 프로세스 처리 방식)

<img src="https://user-images.githubusercontent.com/78911818/246706426-46c62ad8-6876-4464-aa71-3e355e9d89b2.png" width="300px" >

prefork는 부모 프로세스 하나가 여러 개의 자식 프로세스를 갖는 것을 의미한다. 부모 프로세스는 클라이언트의 요청이 들어올 때 마다 자식 프로세스를 생성해서 관리한다. 자식 프로세스는 1024개까지 생성될 수 있고, 서로 메모리를 공유하지 않는다. 하지만 여러 개의 프로세스를 생성하기 때문에 메모리 사용량이 많다.

- worker(멀티 프로세스/스레드 방식)

<img src="https://user-images.githubusercontent.com/78911818/246706477-f6c2333e-312f-4ca8-a69a-167d484e9242.png" width="300px" >

초기에 정해진 개수대로 프로세스를 생성하며 요청이 들어오면 스레드를 늘려 처리하기 때문에 prefork보다 메모리 사용량이 적다. 한 프로세스 당 64개의 스레드를 사용할 수 있고 서로 메로리를 공유한다. prefork보다 요청량이 많은 서버에서 사용하기 적절하고 정해진 스레드를 초과하면 새로운 자식 프로세스를 생성하는 방식이다.

## 📍 포트포워딩(Port Forwarding) 설정

### 포트포워딩(Port Forwarding) 이란?

- 특정 포트로 들어오는 데이터 패킷을 다른 포트로 변경하여 다시 전송하는 작업을 의미한다. 일반적으로 인터넷 공유기 또는 방화벽이 네트워크에서 내부 IP 주소로 들어오는 트래픽을 특정 컴퓨터로 전달하기 위해 사용된다. 포트 포워딩은 공인 IP 주소와 포트 번호를 통해 트래픽을 내부 IP 주소와 포트 번호로 전달한다. 예를 들어 외부에서 공인 IP 주소와 80번 포트로 들어오는 HTTP 요청을 내부 IP 주소와 8080 포트로 전달하는 경우, 포트 포워딩을 설정하여 이를 가능하게 할 수 있다.

### 포트포워딩 설정

- [EC2로 배포한 프로젝트](AWS%20EC2%20React%20%EB%B0%B0%ED%8F%AC.md#aws-ec2에-react-배포하기)에 `IPv4 주소:port번호`로 접속하면 배포가 잘 된 것을 확인할 수 있다. 하지만 웹 사이트 중 포트를 입력하여 들어가는 경우는 거의 없기 때문에 포트번호 없이도 웹 사이트에 접근할 수 있도록 포트 번호를 없애는 작업을 할 것이다.

<img src="https://user-images.githubusercontent.com/78911818/246713653-22361a03-9473-4240-a848-d76f1428a138.png" >

- 먼저 처음에 지정한 보안그룹 규칙에서 위 이미지와 같이 포트가 잘 지정되어 있는지 확인해줘야 한다. http는 기본적으로 80포트를 사용하고 https 기본포트가 443이다. 그리고 사용할 8080 포트를 추가로 열어주었다.

1. Nginx 설치

```bash
ssh -i [pem 파일명].pem ubuntu@[인스턴스 퍼블릭 IPv4 주소]
```

pem 파일이 있는 폴더에서 위 명령어로 먼저 우분투 환경에 들어간 후 Nginx를 설치한다.

```bash
sudo apt-get install nginx
# APT(Advanced Package Tool) 패키지 관리자를 사용하여 Nginx 웹 서버를 설치하는 명령어

sudo su
# 현재 사용자 세션은 root로 변경되며, 이후에 실행되는 명령어는 root 권한으로 실행한다.
```

2. `nginx.conf` 파일 설정

```bash
vim /etc/nginx/nginx.conf
# Nginx의 주요 설정 파일인 nginx.conf 파일 수정
```

`nginx.conf` 파일은 Nginx 웹 서버의 전체 구성을 정의하고 설정하는데 사용하는 중요한 파일이다. 해당 파일에서 Nginx 서버의 동작 방식, 가상 호스트 설정, 로깅, 프록시 설정 등을 정의할 수 있다.

```bash
##
# Virtual Host Configs
##

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;

# include 바로 밑에 이렇게 작성하면 된다.
server {
  server_name 구매한 도메인 주소;
  listen 80;
  # 포트 80으로 HTTP 요청을 받는다는 의미

  location / {
    #  / 경로에 대한 설정을 의미하며, 모든 요청에 적용된다.

    proxy_set_header HOST $host;
    # HOST 헤더 값을 원본 호스트로 설정

    proxy_pass http://127.0.0.1:8080;
    # http://127.0.0.1:8080으로 요청을 전달

    proxy_redirect off;
    # 리다이렉션 해제
  }
}
```

위 코드는 `nginx.conf` 파일에 서버 블록을 설정하는 것이다. Nginx 서버가 `http://구매한도메인주소`로 들어오는 요청을 받아서 `http://127.0.0.1:8080`로 프록시하는 역할을 한다. 따라서 클라이언트가 Nginx 서버에 요청을 보내면 Nginx는 해당 요청을 프록시 서버로 전달하고, 프록시 서버의 응답을 클라이언트에게 반환한다. 이것이 리버스 프록시 서버를 구성하는 방법이다! 위에서 사용한 각각의 명령어의 역할은 아래와 같다.

- server_name: 해당 서버 블록이 적용될 도메인 주소를 입력한다.
- listen: 서버가 클라이언트로부터 요청을 수신할 포트를 지정한다.
- location: 특정 경로에 대한 설정을 정의. 위의 설정에서는
- proxy_set_header: 프록시 서버에 전달되는 HTTP 헤더 값을 설정한다.
- proxy_pass: 프록시 서버에 전달할 요청을 지정한다.
- proxy_redirect: 프록시 서버에서의 리다이렉션 동작을 설정한다.

```bash
#  nginx 재실행
sudo service nginx restart
```

이렇게 해주면 도메인 뒤에 포트 번호를 입력하지 않아도 접속되는 것을 확인할 수 있다!!!

## 📍 HTTP redirect to HTTPS

- https로 설정하기 위해서는 인증서가 필요하다. `Certbot`을 사용할 예정이다. `Certbot`은 무료로 제공되는 오픈 소스 도구로, `Let's Encrypt` 인증 기관을 통해 SSL/TLS 인증서를 자동으로 발급하고 갱신해주는 역할이다. 또한, 웹 서버 구성 파일을 자동으로 업데이트하고, HTTPS 리디렉션을 설정하는 등의 작업을 수행하는 것이 가능하다.

> SSL/TLS 인증서는 웹사이트와 사용자 간의 통신을 암호화하여 보안성을 강화하는 데 필요한 요소

1. certbort 설치

```bash
sudo snap install --classic certbot
```

2. Nginx 자동 설정

```bash
sudo certbot --nginx
```

위 명령어는 certbot에게 Nginx 웹 서버와의 통합을 요청하는 것을 의미한다. certbot은 Nginx 서버 설정을 자동으로 감지하고 Let's Encrypt 인증 기관과 통신하여 도메인의 소유권을 확인한 후, 적절한 설정을 적용하여 SSL/TLS 인증서를 발급 및 설치한다. 또한, Nginx 서버의 구성 파일을 확인하고 기존의 HTTP 연결을 HTTPS로 리디렉션하는 설정을 자동으로 추가한다.

<img src="https://user-images.githubusercontent.com/78911818/246722750-1e1085aa-fcf7-4d72-9e18-7f5d31c53dbf.png" width="400px" >

이렇게 실행하고 나면 email을 입력하고 동의 여부를 묻는 과정을 거치는데 각각 맞게 대답해주면 된다. 그리고 이제 마지막으로 도메인을 선택하는 과정을 거치는데 위에서 서버 블록에 도메인을 등록해놓았기 때문에 우리가 등록한 도메인이 넘버링되어 출력될 것이다. 그럼 도메인 번호를 적어주면 끝난다!!

이제 도메인 주소를 입력하고 들어가보면 https 설정이 잘 된 것을 확인할 수 있다!

<hr>

참고

[Nginx란 무엇인가?](https://dkswnkk.tistory.com/513)  
[Apache 웹서버](http://wiki.hash.kr/index.php/%EC%95%84%ED%8C%8C%EC%B9%98_%EC%9B%B9%EC%84%9C%EB%B2%84)  
[Apache 웹서버의 기본 상식](https://wiseworld.tistory.com/entry/%E3%85%87%E3%85%87)  
[Apache와 Nginx의 차이](https://velog.io/@deannn/Apache%EC%99%80-NginX-%EB%B9%84%EA%B5%90-%EC%B0%A8%EC%9D%B4%EC%A0%90)
