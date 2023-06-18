# AWS EC2에 React 배포하기

목차  
[AWS EC2(Elastic Compute Cloud)란?](#📍-aws-ec2elastic-compute-cloud란)  
[EC2 인스턴스 생성하기](#📍-ec2-인스턴스-생성하기)  
[EC2로 React 프로젝트 배포하기](#📍-ec2로-react-프로젝트-배포하기)  
[EC2 배포 시, react build 안되는 이유](#📍-ec2-배포-시-react-build-안되는-이유)

<hr />

## 📍 AWS EC2(Elastic Compute Cloud)란?

- EC2는 AWS에서 제공하는 클라우드 컴퓨팅 서비스이다. 클라우드 컴퓨팅은 인터넷(클라우드)를 통해 서버, 스토리지, 데이터베이스 등의 컴퓨팅 서버를 제공하는 것을 말한다. 즉, 독립된 컴퓨터를 임대해주는 서비스다.

- EC2는 서버를 직접 구축하는 것보다 쉽게 서버를 구축할 수 있다. 초기 구입이나 세팅비가 필요없고 사용한만큼 결제하는 것이 가능하다.

**용어 정리**

```
인스턴스 : 가상 컴퓨팅 환경

AMI(Amazon Machine Image) : 서버에 필요한 운영체제와 여러 소프트웨어들이 적절히 구성된 상태로 제공되는 템플릿

인스턴스 유형: 인스턴스를 위한 CPU, 메모리, 스토리지, 네트워킹 용량의 여러 가지 구성 제공

탄력적 IP 주소(EIP): 동적 클라우드 컴퓨팅을 위한 고정 IPv4 주소
```

<img src="https://user-images.githubusercontent.com/78911818/244575631-d079e63b-0faf-45c7-bb34-5ee33ad71912.png" width="400px" >

EC2는 인스턴스를 생성함으로써 가상 컴퓨팅 서비스를 사용할 수 있게 되는데 이것은 AMI를 토대로 운영체제, CPU, RAM 혹은 런타임 등이 구성된 컴퓨터를 빌리는 것을 의미한다. EC2의 인스턴스에는 다양한 유형이 있기 때문에 본인의 사용 목적(서버용, 머신러닝용, 게임용 등)에 따라 사용할 유형을 선택하면 된다.

[AWS 인스턴스 유형](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/instance-types.html)

## 📍 EC2 인스턴스 생성하기

EC2 대시보드 페이지에서 인스턴스 시작을 누르면 아래와 같이 인스턴스 생성 화면을 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/78911818/244578763-fe0d03db-93dc-409c-9a92-f7d60fda9693.png" width="400px" >

이름을 지정하고 사용할 OS를 선택해준다.

<img src="https://user-images.githubusercontent.com/78911818/244578853-db86450c-1d49-4c8e-8aaa-9b6125839d75.png" width="400px">

그리고 인스턴스 유형을 선택한다. 아마 기본적으로 프리티어 사용 가능한 `t2.micro`로 설정이 되어있을 것이다. 프리티어를 사용하지 않을 것이라면 본인의 목적에 따라 인스턴스 유형을 선택하면 된다.

<img src="https://user-images.githubusercontent.com/78911818/244578883-0cfde35a-1dda-4797-9c1d-e4de1ec6df8f.png" width="400px" >

그리고 키 페어를 설정해야 한다. 이미 생성한 키 페어가 있다면 해당 키 페어를 사용하면 되지만 없다면 `새 키 페어 생성`을 통해 만들어주면 된다. 이 때 생성되는 키 페어는 한 번만 다운로드 받을 수 있기 때문에 제대로 저장해야 한다!

<img src="https://user-images.githubusercontent.com/78911818/244581592-a7d0a478-5dbf-40c5-9d68-cc80842b4f22.png" width="400px" >

서브넷은 VPC의 IP 주소를 나누어 리소스가 배치되는 물리적인 주소(즉, 실제로 리소스가 생성될 수 있는 네트워크 영역) 범위를 의미한다. 그렇기 때문에 위 서브넷 설정에서는 IP가 사용 가능한 곳으로 설정해주면 된다.

<img src="https://user-images.githubusercontent.com/78911818/244583370-8756c2fa-cf84-4589-9d8b-54bb43a1fdf3.png" width="400px" >

다음으로 보안 그룹 규칙을 설정해야 하는데 이 부분은 배포 후에 다른 사람들도 이에 접속할 수 있도록 해주는 부분이다. 웹 서버의 포트 번호에 따라 포트 범위를 설정하고 소스 유형을 위치 무관으로 설정한다. 나는 8000번 포트로 웹 서버를 열 것이기 때문에 8000으로 설정하여 보안 그룹 규칙을 추가해주었다.

여기까지 하고 인스턴스를 생성하면 대시보드에서 인스턴스를 확인할 수 있다. 이제 생성한 인스턴스를 활용하여 프로젝트를 배포하면 된다.

## 📍 EC2로 React 프로젝트 배포하기

이제 아까 다운로드 받은 키 페어를 활용해야 한다. 먼저 다운로드 받은 키 페어가 있는 폴더의 터미널을 열어서 아래와 같이 입력한다. 해당 명령어는 리눅스 명령어로 파일 소유자에 읽기 권한을 부여하는 명령어다.

> 여기서 개인키 파일에 chmod 400 하는 이유는 개인키는 권한이 너무 open 되어있어도 경고 문구가 뜨면서 permission deny가 뜨기 때문에 권한을 축소시켜서(chmod 400) 사용한다고 한다.

```bash
sudo chmod 400 [pem 파일명].pem
```

그리고 다시 EC2 대시보드에 들어가 생성한 인스턴스를 눌러 인스턴스의 퍼블릭 IPv4 주소를 복사한다. 이제 터미널에 아래와 같이 입력하면 된다.

```bash
ssh -i [pem 파일명].pem ubuntu@[인스턴스 퍼블릭 IPv4 주소]
```

여기까지 하게 되면 만든 인스턴스의 우분투 환경에 접속할 수 있다. 이제 여기서 배포를 진행해주면 되는데 접속한 우분투 환경은 우리의 로컬이 아니기 때문에 `ls` 명령어를 통해 확인해보면 아무 파일도 없는 것을 확인할 수 있을 것이다. 그렇기 때문에 프로젝트를 배포하기 전에 배포가 가능한 환경을 만들어줘야 한다. 당연히 npm도 설치가 안되어 있는 상태이기 때문에 먼저 node.js를 설치해준다.

```bash
curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -
sudo apt-get install nodejs

# yarn 사용 시
sudo npm install yarn
```

설치를 했다면 이제 github에 있는 프로젝트를 clone하여 프로젝트 내 필요한 모듈을 설치하고 빌드까지 진행해준다.

```bash
git clone [repo 주소]
cd [repo 이름]
npm i
npm run build

# 위에서 지정해준 포트 번호로 start 해야한다.
npm start
```

여기까지 하고 `퍼블릭 IPv4 주소:port 번호`로 접속해보면 프로젝트가 잘 열려있는 것을 확인할 수 있다!

하지만 현재는 터미널에서 foreground로 실행되고 있기 때문에 터미널 창을 종료할 경우 앱이 종료되어 버리는데 이 때 사용할 수 있는 것이 `pm2`이다.

```bash
# 전역적으로 pm2 설치
sudo npm install pm2 -g

# pm2로 무중단 배포
pm2 start npm -- start
```

먼저 pm2를 설치하고 pm2 start 명령어로 실행하면 터미널을 종료하고도 접속이 가능한 것을 확인할 수도 있다. 이렇게 무중단 배포까지 완.

**pm2 기능**

```bash
# 실행중인 app 리스트
pm2 list

# pm2 종료
pm2 kill

# app 종료
pm2 stop appName

# app 재실행
pm2 restart appName

# 리스트에서 app 제거
pm2 delete appName

# 앱 설명 확인
pm2 describe appName
```

## 📍 express.js로 웹 서버 만들어 배포하기

위에서 한 것처럼 설정하는 것도 좋지만 실제 프로덕션 환경에서는 보안 및 성능을 고려하여 웹 서버를 설정하는 것이 좋다고 한다. 그렇기 때문에 이번에는 express.js를 활용하여 웹 서버를 구축하여 배포를 해보고자 한다.

```bash
npm i express
```

먼저 express를 설치한 후, 간단한 서버 코드를 작성하면 된다. 서버 코드는 root 경로에 작성하면 된다.

```bash
# server.js 파일 생성 명령어
vi server.js
```

위 명령어를 통해 server.js 파일을 생성하면 바로 server.js 파일 안으로 들어가진다. `i` 키를 통해 INSERT 모드로 변경하고 아래의 코드를 작성해주면 된다.

```js
// server.js
const http = require("http");
const express = require("express");
const path = require("path");

const app = express(); // express app 생성

const port = 8000; // 사용할 port 번호 정의

app.use(express.static(path.join(__dirname, "build"))); // build 폴더 내 정적 파일 서빙

app.get("/*", (req, res) => {
  // "/*"를 통해 모든 요청에 대해 라우팅 처리
  res.set({
    "Cache-Control": "no-cache, no-store, must-revalidate",
    Pragma: "no-cache",
    Date: Date.now(),
  });
  res.sendFile(path.join(__dirname, "build", "index.html"));
  // index.html 파일을 클라이언트에 전송
});

// 웹 서버 생성
http.createServer(app).listen(port, () => {
  console.log(`app listening at ${port}`);
});

// 지정된 포트에서의 클라이언트 요청 수신
```

해당 코드는 `npm run build` 명령어를 통해 생성된 build 폴더 내 정적 파일을 서빙하는 간단한 웹 서버 구축 코드이다.

이제 `server.js` 파일을 실행하면 된다. 이 때, build가 되어있는 상태여야 한다!!!

위에서 했던 것처럼 pm2를 통해 무중단 배포를 하기 위해 간단하게 아래 명령어를 실행하면 끝!

```bash
pm2 start server.js
```

<img src="https://user-images.githubusercontent.com/78911818/244695379-b4bea23d-f772-43d5-8dac-07f27180abfa.png" width='400px'>

`pm2 list`로 확인해보면 아주 잘 돌아가고 있는 것을 확인할 수 있다!

## 📍 EC2 배포 시, react build 안되는 이유

만약 몇 분이 지나도 빌드가 되지 않는다면...? 나는 `Creating an optimized production build...` 이 문구만 십 몇분을 보고만 있었던...

그렇다면 왜 빌드가 안되는 것인가... 그 이유인 즉슨 빌드 규모 대비 램 크기가 작아서 그랬던 것이다... 램 크기가 2기가인 인스턴스 유형으로 사용할 때는 잘되던 빌드가 램 크기가 1기가인 인스턴스로 변경 하니까 안되는... 하지만 인스턴스를 변경하지 않아도 해결할 수 있다!

### Swap 메모리 활용하기

부족한 메모리를 임시로 Swap 메모리를 설정하게 되면 EC2의 메모리가 부족하더라도 Swap 메모리로 잠시 대체할 수 있다.

> Swap 메모리란?
>
> - 실제 메모리 램이 가득 찼지만 더 많은 메모리가 필요할 때 디스크 공간을 이용하여 부족한 메모리를 대체할 수 있는 공간을 의미한다. 실제 디스크 공간을 메모리처럼 사용하는 개념이기 때문에 가상 메모리라고 생각할 수 있다.

우분투 환경에서 `free -h`명령어를 통해 현재 swap 메모리를 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/78911818/244642912-bcbbab40-ccde-459a-8213-c45385f6d15b.png" width="300px" >

total은 전체 설치된 메모리, used는 사용 중인 메모리, free는 사용 가능한 메모리를 의미하며, available은 swap 메모리의 사용 없이 새로운 프로세스에서 할당 가능한 메모리의 예상 크기를 의미한다.

또한, 현재 두 번째 줄을 보면 현재 할당된 Swap 메모리는 0인 것을 볼 수 있다. 몇 가지 명령어를 통해 Swap 메모리를 할당하여 빌드를 해보자!

```bash
# Swap 메모리 추가

sudo dd if=/dev/zero of=/mnt/swapfile bs=1M count=2048
```

먼저 Swap 메모리를 설정해야 한다. bs는 포맷의 단위로 뒤에 단위를 붙이지 않고 1024로 설정할 경우 1KB로 설정되며 1M으로 설정할 경우 MB 단위로 설정된다. 또한 count는 횟수를 의미한다. 위 코드는 1MB만큼 2048번 포맷을 하여 약 2기가 정도의 공간을 Swap 메모리를 추가한 것이다.

```bash
# Swap 메모리를 Swap 파일로 포멧
sudo mkswap /mnt/swapfile

# Swap 메모리 활성화
sudo swapon /mnt/swapfile
```

각각의 명령어를 실행하고 `free -h` 명령어로 확인해보면 아래와 같이 Swap 메모리가 할당된 것을 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/78911818/244656518-461f9593-3146-449d-9c6e-3ddfc7fb8b17.png" width="400px">

이제 가상 메모리 공간을 만들었으니 다시 `npm run build` 명령어를 실행하고 약 50초 정도 기다렸더니 build가 잘 되는 것을 확인할 수 있다.

```bash
# Swap 메모리 비활성화
sudo swapoff -v /mnt/swapfile

# Swap 메모리 삭제
sudo rm /mnt/swapfile
```

이제 swapoff 명령어를 사용하여 Swap 메모리를 비활성화하고 삭제해주면 된다! 이제 다시 배포를 해주면 끝이다!

<hr />

참고  
[AWS EC2란 무엇인가요?](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/concepts.html)  
[리눅스 chmod 명령 사용 방법](https://bimmermac.com/2789)  
[AWS EC2에 웹 프로젝트 배포하기 (React)](https://3d-yeju.tistory.com/63)
