# :fire: 6월 17일 학습 정리 (2)

## 젠킨스

### CI 도구로서의 젠킨스

- Java로 작성된 오픈 소스 자동화 서버
- 지속적 인도 프로세스를 구축하는데 널리 이용됨
- 장점: 유연성과 확장성
- 오픈소스 솔루션으로 개발을 지속하는 중.

### CI/CD 시나리오

- CI (Continuous Integration; 지속적 통합) 단계
  - 일반적으로 개발자가 소스 코드를 커밋하고 푸시하는 것으로 시작
  - 응용 소프트웨어를 자동으로 빌드, 통합
  - 자동 테스트를 통해 배포할 수 있는 상태임을 확인
- CD (Continuos Delivery/Deployment; 지속적 인도) 단계
  - CI 단계에서 소프트웨어가 배포 가능한 상태임을 확인하는 것으로 시작
  - 응용 소프트웨어를 컨테이너 이미지로 만들어냄
  - 포드, 디플로이먼트, 서비스 등 다양한 오브젝트 조건에 맞추어 배포
 
### 지속적 통합 파이프라인

- 리포지토리에 코드 커밋이 발생할 때마다 빌드, 단위테스트, 정적 분석 등을 행함
- CI 서버를 Jenkins가 담당함
![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/c6c540a4-2c9c-44dc-ab44-78ac86db3ca5)

### 쿠버네티스 클러스터에 구성한 젠킨스 CI

- k8s 클러스터 환경에 젠킨스에 의한 CI를 적용

### 젠킨스의 특징

- 다양한 프로그래밍 언어 지원
- 플러그인을 통한 확장
  - 사용자가 직접 플러그인을 작성해 젠킨스의 기능을 확장하는 것도 가능
- 이식성
  - 여러 종류의 컴퓨터에서 뿐만 아니라 컨테이너 및 클러스터 환경에도 부드럽게 적용
- 대부분의 소스 관리 시스템 지원
- 분산 처리 지원
  - 마스터/슬레이브 구조를 채택하여 여러 노드에서 작업 수행
- 코드로 파이프라인 구성
  - 프로세스 자동화에 적합
 
### 젠킨스 아키텍처

- 마스터 슬레이브 구조
  - 슬레이브는 에이전트라고 부르기도 한다.
  - 마스터
    - 빌드 시작 트리거 포착 (예: 코드 커밋)
    - 알림 (예: 빌드 실패를 사용자에게 전달)
    - 클라이언트와 통신하여 HTTP 요청 처리
    - 에이전트에서 실행 중인 작업의 우선순위 조정 등 빌드 환경 관리
  - 에이전트
    - 마스터에 의한 개시 후 모든 작업을 처리
   
### 수평적 확장

- 조직이 늘어날 때마다 마스터 인스턴스의 수를 늘려가는 방식
  - 비교: 수직적 확장- 마스터에 대한 부하가 증가함에 따라 마스터 시스템에 자원을 추가하는 방식
- 통합 자동화가 복잡해진다는 단점.
- 이점
  - 마스터 역할을 하는 컴퓨터의 하드웨어 사양에 대한 부담이 감소
  - 팀마다 다른 다른 설정이 가능
  - 팀 전용 마스터 인스턴스가 있으므로 팀워크와 업무 효율이 높아짐
  - 마스터 인스턴스 하나에 문제가 생겨도 다른 팀에 끼치는 영향이 최소화됨
 
### 테스트 인스턴스와 프로덕션 인스턴스

★ 중요! - 젠킨스 인스턴스는 항상 테스트용과 프로덕션용으로 분리 운용해야함

- 다음과 같은 시스템 설정 변경이 일어날 때 프로덕션에 적용하기 이전 철저한 검증이 필요
  - 젠킨스 소프트웨어의 업데이트
  - 신규 플러그인 적용
  - CI/CD 파이프라인의 변경 및 유지보수

---

## 젠킨스 설치

1. helm 설치
https://may9noy.tistory.com/1113
```
// 2. 
helm repo add jenkinsci https://charts.jenkins.io

// 3.
helm repo update

// 4.
helm install jenkins jenkinsci/jenkins

// 5.
kubectl logs sts/jenkins jenkins
```

### Jenkins 관리자 비밀번호 알아내기

```
// 비밀번호 알아내기
kubectl get secret jenkins -o jsonpath="{.data.jenkins-admin-password}"

// 비밀번호 바꾸기
kubectl edit secrets jenkins
// 그러면 관련 파일이 팝업되는데, 사용하고자하는 비밀번호의 base64 인코딩 내용을 복붙한다.

// 젠킨스 접속
kubectl port-forward svc/jenkins 8080:8080
// 여기서 로그인하면 된다.
```
### Jenkins 기초 설정

- 언어 설정
  - 사용자 인터페이스에 기본적으로 적용되는 언어설정을 영어로 고정
    - 메뉴명 등이 달라서 겪을 수 있는 혼란을 피하려는 목적
  - 플러그인 'Locale'을 설치
  - Manage Jenkins > System > Locale
- 시간대 설정
  - 자신이 살고 있는 지역의 시간대로 서버 시간대를 설정
    - 빌드 시작 및 종료 시각 등의 기록을 해석하는 데 혼란을 피하려는 목적
  - People > 계정 선택 > Configure > User Defined Time Zone
 
---

## 젠킨스 기본 사용법

- Hello, world!를 콘솔에 찍는 작업을 할 때 벌어지는 일
  - Jenkins 마스터는 k8s 클러스터 내에 동적으로 에이전트를 생성
  - 에이전트에서는 지정한 작업을 수행
  - 에이전트의 작업 수행 결과는 마스터에게 보고

### 사용법

- 젠킨스 페이지에서 New Item 추가
- 이름 기재하고 pipeline 선택
- 들어온 페이지의 Pipeline 란에 아래 코드 기재
```
  pipeline {
    agent any
    stages {
        stage("Hello") {
            steps {
                echo 'Hello, world!'
            }
        }
    }
}
```
- 저장하고 지금 빌드를 클릭한다.

### 클러스터 내 에이전트 동작 관찰

- k8s 클러스터에서 Jenkins는 에이전트를 동적으로 프로비저닝
  - Kubernetes 플러그인이 설치되어 있어야하며, 적절한 설정도 되어 있어야함.

---

## 젠킨스 프로젝트 설정

- 프로젝트 소스가 담긴 GitHub 리포지토리 준비
- 젠킨스의 빌드 환경을 설정
- Docker를 이용하여 이미지를 빌드하고 DockerHub 레지스트리에 푸시
- kubectl을 이용하여 레지스트리로부터 이미지를 가져다가 같은 클러스터에 배포

### 과정

- new item에서 freestyle 설정해서 생성
- Build Step 항목에 아래처럼 적는다
![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/06b4aefa-27d8-4fc4-8a99-0d640bf7475c)

위는 Fail하는데, 설정된 docker image에 docker CLI가 내포되어있지 않기 때문이다.
따라서, 이를 내포한 이미지로 설정된 에이전트를 따로 생성할 필요가 있다.

### 컨테이너 스펙을 작성해서 포드 템플릿의 설정을 완성

- 신규 추가하는 포드 템플릿에는 두 개의 컨테이너 스펙이 포함되도록 할 예정
  - 빌드 작업을 실행할 컨테이너(jnlp)
    - JNLP(Java Network Launch Protocol)을 따라 Jenkins 마스터와 작업 조율하면서 빌드 작업 실행
    - docker CLI와 kubectl CLI 를 갖추고 있다.
    - 미리 만들어진 이미지 이용
  - 도커 데몬을 실행하는 컨테이너 (dind)
    - 어느 클러스터에 있더라도 통일된 docker build 환경을 제공하기 위해 독립된 컨테이너로 제공
    - 이미지는 docker:latest 이용
   
  ![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/10ae3771-c21e-49fc-add3-60ef8a5739e5)

  ![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/d9143b0c-61f1-481b-ba23-caf7ff397383)
