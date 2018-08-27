*Read this in other languages: [English](README.md), [中国](README-cn.md).*
# BlockchainNetwork-CompositeJourney

## Build Your First Network (BYFN)

블록체인 애플리케이션을 빌드하는 시리즈 중 첫 번째 과정에 오신 것을 환영합니다. **1 단계**에서는 상품 거래를 위한 비즈니스 프로세스를 하이퍼레저 컴포저(Hyperledger Composer)를 사용하여 BNA (Business Network Archive) 파일을 생성, 생성된 파일을 하이퍼레저 패브릭(Hyperledger Fabric)에 배포하는 방법을 다룹니다. 이 과정은 하이퍼레저 컴포저 샘플버전의 "Hello World"이기 때문에 초급 개발자분들도 이 과정을 따라하실 수 있습니다. 이 코드 패턴은 하이퍼레저 컴포저 V0.19.5 와 하이퍼레저 패브릭 V1.1 을 지원하도록 업데이트 되었습니다.

하이퍼레저 패브릭은 리눅스 재단 (Linux Foundation®)에서 하이퍼레저 프로젝트 기반에 인큐베이션 프로젝트 중 하나인 표준 블록체인 플랫폼 구현 프로젝트입니다.하이퍼레저 패브릭은 모듈러 아키텍처로 애플리케이션이나 솔루션을 개발하기 위한 토대가 되며, 블록체인 구성 요소인 합의 및 멤버십 서비스 기능 등을 플러그 앤 플레이 방식으로 사용할 수 있습니다.

[2 단계](https://github.com/IBM/BlockchainBalanceTransfer-CompositeJourney)에서는 여러 참가자가 참여하는 복잡한 네트워크를 만들고 액세스 제어 리스트 (ACL)를 사용하여 네트워크 액세스 권한을 제공하는 방법에 대해 자세히 살펴봅니다. 이 과정에서는 하이퍼레저 패브릭을 로컬에서 실행합니다.

[하이퍼레저 컴포저](https://github.com/hyperledger/composer)를 사용하여 기존 자산과 이에 관련된 거래를 포함하는 현재의 비즈니스 네트워크를 신속하게 모델링할 수 있습니다. 여기서 자산(Asset)은 유형 또는 무형의 재화, 서비스 또는 소유물입니다. 비즈니스 네트워크 모델의 한 부분인, 자산을 기반으로 서비스 기능별로 트랜젝션(Transaction)을 정의합니다. 비즈니스 네트워크에 참여중인 참가자는 고유한 ID를 통해 하나의 비즈니스 네트워크나 여러 다른 비즈니스 네트워크에 참여할 수 있는 권한을 갖게 됩니다. 비즈니스 네트워크 정의하는 구성요소는 모델(.cto), 스크립트(.js) 및 ACL(.acl) 파일로 구성되어 아카이브(.bna 파일) 형태로 패키지화 되어 생성되게 됩니다. 그런 다음 아카이브 파일(BNA)을 하이퍼레저 패브릭 네트워크에 배포됩니다.

## 구성 요소

* 하이퍼레저 패브릭 (Hyperledger Fabric)
* 하이퍼레저 컴포저 (Hyperledger Composer)
* 도커 (Docker)

## 애플리케이션 워크플로우 다이아그램
![Application Workflow](images/arch-blockchain-network1.png)

1. 네트워크 구성파일 설치하기 a) cryptogen b) configtxgen c) configtxlator d) peer
2. 네트워크 설정하기 a) 네트워크 아티팩트 생성하기 b) 네트워크 시작하기

## 사전 준비
Node 설치 상태 따라 블록체인이 민감하게 동작할 수 있습니다. 다음 [스택오버플로우 답변](https://stackoverflow.com/questions/49744276/error-cannot-find-module-api-hyperledger-composer)을 참고하여 Node 버전이 맞지 않거나 적절하지 않은 설치에서 어떤 에러가 발생하는지 참고할 수 있습니다.

* [Docker](https://www.docker.com/products/overview) - v1.13 또는 그 이상
* [Docker Compose](https://docs.docker.com/compose/overview/) - v1.8 또는 그 이상 
* [nvm](https://github.com/creationix/nvm) - v0.33.11 이상 (nvm을 사용하면 여러 버전의 Node 를 설치하고 사용자별 의존성을 관리할 수 있습니다.)
* [Node.js](https://nodejs.org/en/download/) - node v8.11.3 (`nvm install 8` 명령어로 설치 가능합니다.)
* [NPM](https://www.npmjs.com/get-npm) - v5.6.0 이상 (nvm 에서 관리됩니다.)
* [Git client](https://git-scm.com/downloads) - v 2.9.x 이상
* [Python](https://www.python.org/downloads/) - 2.7.x

## Steps
1. [하이퍼레저 컴포저 개발 툴 설치하기](#1-하이퍼레저-컴포저-개발-툴-설치하기)
2. [하이퍼레저 패브릭 시작하기](#2-하이퍼레저-패브릭-시작하기)
3. [Business Network Archive (BNA) 생성하기](#3-business-network-archive-bna-생성하기)
4. [컴포저 플레이그라운드를 사용하여 Business Network Archive 배포하기](#4-컴포저-플레이그라운드를-사용하여-business-network-archive-배포하기)
5. [로컬에 있는 하이퍼레저 컴포저에 Business Network Archive 배포하기](#5-로컬에-있는-하이퍼레저-컴포저에-business-network-archive-배포하기-대체-설치-방안)

## 1. 하이퍼레저 컴포저 개발 툴 설치하기

**주의:** 수퍼유저 `sudo`모드에서 이 명령을 실행해야 할 수 있습니다. `sudo`를 사용하면 허용된 사용자가 보안 정책에 지정된대로 수퍼유저 또는 다른 사용자로 명령을 실행할 수 있습니다. 이번 과정에서는 가장 최신 버전의 composer-cli (0.19.5)을 사용합니다. 만약 이전 버전이 설치되어 있다면, 아래 명령을 사용하여 제거합니다:
```
npm uninstall -g composer-cli
```

* `composer-cli` 에는 비즈니스 네트워크 개발에 대한 모든 명령어를 가지고 있습니다. `composer-cli` 를 설치하려면 아래 명령어를 실행하십시오:
```
npm install -g composer-cli@0.19.5
```

* `generator-hyperledger-composer`는 Yeoman 플러그인으로 비즈니스 네트워크용 애플리케이션을 구성 및 생성하게 됩니다. Yeoman은 오픈 소스 클라이언트 측 개발 스택으로, 웹 애플리케이션을 제작하기 전에 필요한 디렉토리 구조 및 기본적인 파일을 생성해 주는 프레임워크입니다. `generator-hyperledger-composer`를 설치하려면 다음을 실행하십시오: 
```
npm install -g generator-hyperledger-composer@0.19.5
```

* `composer-rest-server`는 하이퍼레저 컴포저 루프백 커넥터를 사용하여 비즈니스 네트워크에 연결하고 모델을 추출한 다음, 모델용으로 생성된 REST API가 포함된 페이지를 보여줍니다.(* 루프백(Loopback) : 높은 확장성을 지닌 Open-source Node.js framework 이며, 간단한 코딩만으로 동적으로 end-to-end REST API를 생성해주는 기능을 제공합니다.)`composer-rest-server`를 설치하려면 다음을 실행합니다:
```
npm install -g composer-rest-server@0.19.5
```

* `Yeoman`을  `generator-hyperledger-composer` 구성 요소와 결합하게 되면, 비즈니스 네트워크를 해석하고 이를 기반으로 애플리케이션을 쉽게 생성할 수 있습니다. `Yeoman`을 설치하려면 다음을 실행합니다:
```
npm install -g yo@2.0.0
```

## 2. 하이퍼레저 패브릭 시작하기

먼저 컴포저 프로파일을 작성하기 위해 하이퍼레저 패브릭 관련 도커 파일을 다운로드합니다. 하이퍼레저 컴포저는 연결 프로파일(Connection Profiles)을 사용하여 런타임에 연결합니다. 연결 프로파일은 사용자의 홈 디렉토리에 있는 JSON 문서 형태이며(또는 환경 변수에서 올 수 있는) Composer API 또는 CLI(Command Line Interface)를 사용할 때 참조됩니다. 연결 프로파일을 사용하면 코드와 스크립트를 하나의 런타임 인스턴스에서 다른 런타임 인스턴스로 쉽게 이동할 수 있습니다.

PeerAdmin ID 카드는 로컬 하이퍼레저 패브릭을 관리하는데 사용되는 특수 ID입니다. 이 PeerAdmin ID 카드는 로컬 하이퍼레저 패브릭을 설치할 때 생성됩니다. 

하이퍼레저 패브릭 v1.0 네트워크의 PeerAdmin 카드 양식은 PeerAdmin@hlfv1 입니다. 대개 PeerAdmin은 아래와 같은 기능을 배치하기 위한 특별한 역할을 수행합니다: 

* 비즈니스 네트워크 배포하기
* 비즈니스 네트워크 관리자용 ID카드의 생성, 발급 및 해지하기 

먼저 다음 명령어로 이 저장소를 복제해 저장하고 프로젝트 폴더에 들어갑니다.
```bash
git clone https://github.com/jgkong/BlockchainNetwork-CompositeJourney.git
cd BlockchainNetwork-CompositeJourney
git checkout global-citizen
```

그리고 다음 명령어로 패브릭을 시작하고 사용하여 컴포저 프로파일을 만듭니다:
```bash
./downloadFabric.sh
./startFabric.sh
./createPeerAdminCard.sh
```  

지금 해볼 것은 아니지만 참고로 알아두십시오 - 아래 명령어를 사용하면 패브릭을 중단하거나 없앨 수 있습니다:
```
./stopFabric.sh
./teardownFabric.sh
```

## 3. Global Citizen 네트워크 준비하기

```bash
cd ..
git clone https://github.com/IBM/global-citizen.git
mkdir -p global-citizen/credentials
cp BlockchainNetwork-CompositeJourney/fabric-scripts/hlfv11/composer/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/keystore/114aab0e76bf0c78308f89efc4b8c9423e31568da0c340ca187a9b17aa9a4457_sk global-citizen/credentials/admin-priv.pem
cp BlockchainNetwork-CompositeJourney/fabric-scripts/hlfv11/composer/crypto-config/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp/signcerts/Admin@org1.example.com-cert.pem global-citizen/credentials/admin-pub.pem
cp BlockchainNetwork-CompositeJourney/connection.json global-citizen/connection-profile.json
```


## 추가 리소스
* [Hyperledger Fabric Docs](http://hyperledger-fabric.readthedocs.io/en/latest/)
* [Hyperledger Composer Docs](https://hyperledger.github.io/composer/introduction/introduction.html)

## License
[Apache 2.0](LICENSE)
