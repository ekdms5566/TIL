## AWS S3 React 배포

[S3(Simple Storage Service)란?](#s3simple-storage-service)  
[S3로 React 배포하기](#s3로-배포하기)  
[1) IAM 사용자 만들기](#1.-iam-사용자-만들기)  
[2) S3 버킷 만들기](#2.-s3-버킷-만들기)  
[3) 웹사이트 호스팅 설정 변경](#3-웹사이트-호스팅-설정을-변경)  
[4) 배포하기](#4.-배포하기)

프로젝트를 진행하면서 AWS S3로 배포한 과정을 정리한 글이다.

### S3(Simple Storage Service)

- [AWS S3](https://docs.aws.amazon.com/ko_kr/AmazonS3/latest/userguide/Welcome.html)는 데이터를 버킷 내의 객체로 저장하는 객체 스토리지(객체로 된 파일을 다루는 저장소) 서비스로 다양한 사용 사례에서 원하는 양의 데이터를 저장하고 보호할 수 있다.

#### **버킷**

- 버킷은 Amazon S3에 저장된 객체에 대한 컨테이너로, 버킷에 저장할 수 있는 객체 수에는 제한이 없다.
- Amazon S3 리소스에 대한 액세스를 관리하는 데 사용할 수 있는 버킷 정책, 액세스 제어 목록(ACL), S3 액세스 포인트와 같은 제어 옵션을 제공한다.

#### **객체**

- 객체는 Amazon S3에 저장되는 기본 개체로, 객체 데이터와 메타데이터(객체를 설명하는 이름-값 페어의 집합)로 구성된다.

#### **키**

- 버킷 내 객체에 대한 고유한 식별자. 버킷 내 모든 객체는 하나의 키를 가진다.
- 버킷, 객체 키, 버전 ID(선택 사항)으로 각 객체를 고유하게 식별하는 것이 가능하다.

#### **ACL(액세스 제어 목록)**

- ACL을 사용하여 권한이 부여된 사용자에게 개별 버킷 및 객체에 대한 읽기 및 쓰기 권한을 부여한다.
- ACL은 IAM보다 먼저 적용되는 액세스 제어 메커니즘이다.
- 기본적으로 다른 AWS 계정이 S3 버킷에 객체를 업로드하면 해당 계정(객체 작성자)이 객체를 소유하고 객체에 액세스할 수 있으며 ACL을 통해 다른 사용자에게 객체에 대한 액세스 권한을 부여할 수 있다.

#### **리전**

- 버킷을 저장할 지리적 AWS 리전
- 지연 시간 최적화, 비용 최소화, 규정 요구사항 준수 등 다양한 필요에 따라 리전을 선택할 수 있다.

#### **S3를 사용하는 이유**

- 저장 용량이 무한대로 파일 저장에 최적화.
- EC2와 EBS로 구축하는 것보다 저렴한 비용
- S3 자체가 수천 대 이상의 매우 성능이 좋은 웹 서버로 구성되어 있어서 EC2와 EBS로 구축했을 때 처럼 Auto Scaling이나 Load Balancing에 신경쓰지 않아도 된다.
- 정적 웹 서비스 가능

### **S3로 배포하기**

### **1. IAM 사용자 만들기**

- S3 버킷을 만들기 전, 먼저 IAM 사용자를 만들어준다.

#### IAM(AWS Identity and Access Management)

- IAM은 AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 서비스로, IAM을 사용하여 리소스를 사용하도록 인증(로그인) 및 권한 부여된 대상을 제어한다.
- 루트 사용자 보안 인증은 AWS에서 권장하지 않는다. 루트 사용자일 경우 AWS의 모든 리소스에 무제한 접근이 가능하기 때문에 IAM 사용자를 만들어 관리 권한을 분리하는 것을 권장하고 있다.

먼저 IAM 대시보드에서 사용자를 추가헌 휴ㅡ 보안 자격 증명 탭에서 액세스 키를 발급받는다. 이는 나중에 사용할 것이기 때문에 키를 복사해서 저장하거나 .csv②③ 파일을 받아 저장해놓는 것이 좋다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbcFIaI%2FbtsguCiLXkT%2FONPBJ6tibqD2pGA7GC8UtK%2Fimg.png" >

#### **2. S3 버킷 만들기**

**① 버킷 생성**  
S3 탭으로 들어와 본인이 원하는 버킷 이름을 지정해주고 배포하려는 서비스의 주 사용자의 거주 지역을 리전으로 설정한다.

<img src="https://user-images.githubusercontent.com/78911818/241365774-8e23ad78-fc84-41cd-a1c2-525ccfa1f456.png" >

**② 객체 소유권 설정**

객체 소유권은 ACL을 사용 중지하고 버킷에 있는 모든 객체의 소유권을 가져오는 데 사용할 수 있는 S3 버킷 수준 설정으로, S3에 저장된 데이터에 대한 액세스 관리를 간소화한다.

S3의 최신 사용 사례 대부분은 더 이상 ACL을 사용할 필요가 없으며, 각 객체에 대해 액세스를 개별적으로 제어해야 하는 비정상적인 상황을 제외하고는 ACL 사용을 중지하는 것이 좋다.

> **ACL 사용 중지됨**  
> 버킷 소유자 시행(Bucket owner enforced)(권장) – ACL이 사용 중지되고 버킷 소유자는 버킷의 모든 객체를 자동으로 소유하고 완전히 제어합니다. ACL은 더 이상 S3 버킷의 데이터에 대한 권한에 영향을 주지 않습니다. 버킷은 정책을 사용하여 액세스 제어를 정의합니다.
>
> **ACL 사용됨**  
> 버킷 소유자 기본(Bucket owner preferred) – 버킷 소유자가 bucket-owner-full-control 미리 제공 ACL을 사용하여 다른 계정이 버킷에 작성하는 새 객체를 소유하고 완전히 제어합니다.
>
> 객체 작성자(Object writer)(기본값) – 객체를 업로드하는 AWS 계정은 객체를 소유하고 완전히 제어하며 ACL을 통해 다른 사용자에게 이에 대한 액세스 권한을 부여할 수 있습니다.

**③ 퍼블릭 액세스 설정**

<img src="https://user-images.githubusercontent.com/78911818/241366394-ced42e63-e18f-4a88-8a4e-640bf8178b7b.png" width="400px">

퍼블릭 액세스 차단 기능은 액세스 포인트, 버킷 및 계정에 대한 설정을 제공하여 Amazon S3 리소스에 대한 퍼블릭 액세스를 관리하는 기능이다.

퍼블릭 액세스를 설정할 경우 S3 버킷에 퍼블릭 액세스를 설정하면 해당 버킷의 객체(파일)에 대한 읽기 또는 쓰기 액세스가 모든 사용자에게 허용된다. 또한, 해당 버킷의 객체에 대한 URL을 공개하고, 누구나 이 URL을 통해 객체에 액세스할 수 있다. 퍼블릭 액세스를 설정하는 것은 주로 웹 사이트 호스팅, 공개적으로 공유되는 파일 등의 용도로 사용된다.

만약 퍼블릭 액세스를 차단한다면 해당 버킷의 객체에 대한 액세스가 제한되며, 기본적으로 객체에 대한 액세스는 해당 버킷의 소유자 및 AWS 계정에서만 허용되며, 이는 보안을 강화하고 데이터 노출을 방지하는 데 도움이 된다.

나는 웹 사이트를 호스팅하는 것임 목적이며 사용자들이 직접적으로 객체에 액세스할 수 있어야 하기 때문에 퍼블릭 액세스를 설정하였다.

**④ 버킷 생성**

위에서 언급한 내용 제외하고는 기본 설정을 그대로 설정하였다.

#### **3. 웹사이트 호스팅 설정을 변경**

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FFrJWI%2Fbtsg9Np1xLe%2FLuOMwfVDzsAYUAVyZOkprk%2Fimg.png" width="400px" >

이제 만들어진 버킷에서 웹사이트 호스팅 설정을 변경한다. 정적 웹사이트 호스팅 편집 탭에서 인덱스 문서에 index.html을 작성해준다.

그리고 퍼블릭 권한을 설정하기 위해 버킷 정책을 설정해야 한다. '객체를 퍼블릭으로 설정할 수 있음'이라는 문구만 출력되고, 퍼블릭으로 설정되지 않았다면 버킷 정책을 확인해보는 것이 좋다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbfrd2g%2Fbtsg9Dg1LxG%2Fx1doeBV0kNK83hmFyCFKkK%2Fimg.png" width="400px">

버킷 정책은 권한 탭의 버킷 정책 부분에서 편집할 수 있다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbD7398%2Fbtshcfzj0rS%2FA2A37bGj9yKJxO9UM3pqQk%2Fimg.png" width="400px">

정책을 생성하기 위해서는 정책 생성기를 눌러주면 되는데, 이를 눌러주면 해당 웹 서비스로 들어오게 된다.

Select Policy Type을 S3 Bucket Policy를 설정해주고, Principal은 \*(전체), Actions는 필요한 것들만 설정하면 된다. 그리고, Add Statement를 눌러서 정책을 생성하면 JSON 파일을 확인할 수 있는데 이를 복사해서 버킷 정책에 넣어주면 된다.

#### 4. 배포하기

그럼 이제 파일을 업로드하거나 터미널을 통해 파일을 업로드 하는 것이 가능한데, 터미널로 배포하는 방법을 사용해 볼 것이다.

먼저 AWS CLI를 설치해야 하는데 [AWS 공식 문서](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)를 참고해서 설치하면 된다.

터미널을 키고 아래의 명령어를 작성하며 사용 권한을 확인해야 한다. 해당 명령어를 작성하면 access key로 인증을 받는 과정을 거치는데 이 때, IAM 사용자를 만들며 받았던 csv 파일을 사용하게 된다.

```
aws configure --profile 'IAM 사용자 이름'
```

```
AWS Access Key ID (csv 파일 내 Access key Id)
AWS Secret Access Key (csv 파일 내 Secret access key)
Default region name (ap-northeast-2)
Default output format (json)
```

이렇게 IAM 사용자 권한을 로컬에서 인증받았다면 이제 프로젝트 폴더의 터미널에서 build 후, 아래와 같이 입력해주면 배포가 된다. 이는 AWS S3의 정적 웹 사이트 호스팅의 엔드포인트 url로 접속하면 결과를 확인할 수 있다.

```
aws s3 sync ./build s3://[S3 버킷 이름] --profile=[IAM 사용자 이름]
```

마지막으로 재배포를 할 경우가 있다면 package.json에 아래와 같은 명령어를 작성해놓고 npm run deploy를 입력하면 쉽게 재배포할 수 있다.

```
"deploy" : "aws s3 sync ./build s3://[S3 버킷 이름] --profile=[사용자 아이디]"
```
