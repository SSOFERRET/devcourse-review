# :fire: 6월 17일 학습 내용 (1)

## 쿠버네티스 소개

- 마이크로서비스 아키텍처 = MSA ↔ monolithic 아키텍처(전통적 방식)
  - 응용 시스템 개발 및 구성을 위한 아키텍처 스타일
  - 애플리케이션이 서비스 모음으로 개발되어 각 마이크로 서비스는 특정한 기능을 수용하고 개별 작업을 처리, 이 서비스들이 서로 연결되어 전체 응용을 구성
 
### 컨테이너 인프라 환경의 적용

- 마이크로서비스 아키넥처를 컨테이너 모델로 구현함이 적합하다.

![asdf](https://github.com/SSOFERRET/devcourse-review/assets/148465774/611de797-0209-4f8a-833e-03cd457b0ee4)

### 쿠버네티스

- = k8s
- 컨테이너 오케스트레이션 솔루션
  - 다수의 컨테이너를 관리
  - 자동 배포, 배포된 컨테이너의 동작 보증, 부하에 따른 동적 확장 등의 기능을 담당
- 도커와 잘 어울리는 실행 환경 구성 도구
  - 도커 컨테이너들을 클러스터 내에 실행하고 관리하는데 적합
  - 지속 통합과 인도(CI/CD)에 유효하게 적용할 수 있다.
  - 컨테이너는 포드라고 불리는 k8s 오브젝트와 연관하여 실행 (포드 위에서 실행한다고 대강 표현한다.)
 
### k8s 클러스터

https://kubernetes.io/docs/concepts/overview/components/


### k8s 클러스터의 구성 요소

- 클러스터는 하나 이상의 노드로 구성됨
  - 마스터 노드 (컨트롤 플레인)
    - kubectl
    - API 서버, etcd - 클러스터의 중심 역할을 하는 구성 요소들
    - 컨트롤러 매니저, 스케줄러
  - 워커 노드
    - 컨테이너 런타임 - 포드를 이루는 컨테이너의 실행을 담당
    - kubelet - 포드의 구성 내용을 받아 CRI에 전달하고 컨테이너들의 동작 상태를 모니터링
- 마스터-슬레이브 구조로 되어있다.

### k8s가 제공하는 기능

- 컨테이너 밸런싱
  - 포드 부하 균등화를 수행.
- 트래픽 로드 밸런싱
  - 응용의 복제본이 둘 이상 있다면 k8s가 트래픽 부하 균등화를 수행하여 클러스터 내부에 적절히 분배
- 동적 수평 스케일링
  - 인스턴스 수를 동적으로 확장하거나 감축하여 동적 요구사항에 대응하면서 시스템 자원을 효율적으로 활용
- 오류 복구
  - 포드와 노드를 지속적으로 모니터링하고 장애가 발생하면 새 포드를 실행하여 지정된 복제본 수를 유지
- 롤링 업데이트
  - 지연 시간을 저굥ㅇ하고 순차적으로 업데이트 배포함으로써 문제가 발생하더라도 서비스를 정상 유지할 수 있다.
- 스토리지 오케스트레이션
  - 원하는 응용에 다양한 스토리지 시스템을 마운트 할 수 있다.
- 서비스 디스커버리
  - 태생적으로 수명이 짧은 포드의 동적 성질을 관리하기 위하여 자체 DNS 기반으로 서비스를 동적 바인딩할 수 있는 기능을 제공
 
### k8s pod의 생명 주기

- kubectl을 통해서 API 서버에 포드의 생성을 요청
  - 업데이트가 있을 때마다 API 서버는 etcd에 기록하고 클러스터의 상태를 최신으로 유지하려고 함
- 컨트롤러 매니저는 포드를 생성하고, 이 상태를 API 서버에 전달
  - 아직 어떤 워커 노드에 포드를 적용할지는 결정하지 않은 상태
- 스케줄러는 포드가 생성되었다는 정보를 인지하고, 이 포드를 어떤 워커 노드를 적용할지를 결정해서 해당 노드에 포드의 실행을 요청
- 해당 노드의 kubelet이 CRI에 요청하여 포드가 만들어지고 사용가능한 상태가 됨
- k8s는 **절차적인 구조가 아닌 선언적인 구조**를 가지고 있음.
  - 각 요소가 추구하는 상태를 선언하면 현재 상태와 비교하고 지속적으로 맞추어 가려고 노력하는 구조
 
### k8s 오브젝트

- 기본 오브젝트
  - Pod - 한 개 이상의 컨테이너로 단일 목적의 일을 하기 위해서 모인 단위
    - 독립적인 공간과 사용 가능한 IP를 가지고 있다. 언제든지 죽을 수 있다.
  - Namespace - k8s 클러스터에서 사용되는 리소스들을 구분해 관리하는 그룹
  - Volume - 포드가 생성될 때 포드에서 사용할 수 있는 디렉토리를 제공
  - Service - 유동적인 포드에 대한 접속을 안정적으로 유지하도록 클러스터 내/외부에 연결하는 역할
- 디플로이먼트(Deployment)
  - 기본 오브젝트들을 보다 효율적으로 작동할 수 있도록 조합하고 추가로 구현한 것
  - 레플리카셋(replicaset) 오브젝트를 합쳐 놓은 형태로 단순하게 생각할 수 있다.
 
### k8s 인프라 구축

- 로컬 환경
  - kubeadm, docker desktop 등을 설치, 운용함으로써 로컬 환경에 간단한 클러스터 구성 가능
  - 개발 단계에서의 테스트 등에 이용
- Public clouds
  - Amazon의 AWS EKS
  - GCP의 GKE
  - Microsoft의 AKS
- On-prem 설치
  - SUSE의 Rancher
  - ReadHat의 OpenShift

---

## 쿠버네티스 기본 사용법

- k8s 클러스터에 어떤 것을 지시할 수 있는 가
  - 포드의 생성과 컨테이너의 실행
  - 디플로이먼트를 이용하여 동일한 기능을 하는 포드의 레플리카셋을 실행
  - 클러스터 외부로부터의 접근이 가능하도록 서비스를 실행
  - 생성된 k8s오브젝트들을 삭제
- k8s 클러스터의 현재 상태를 조회하는 방법을 알아본다.
- 오브젝트 스펙을 작성하는 기초를 익히고 실습해본다.

### 노드와 포드의 정보를 조회하는 법

```
kubectl get nodes
kubectl get pods
```

![17-1](https://github.com/SSOFERRET/devcourse-review/assets/148465774/d51d407f-f49c-416f-b8b7-1e16425edcef)

```
kubectl get nodes -o wide
```
'-o wide' 옵션을 추가하면 아래 정보가 더 나온다

![17-2](https://github.com/SSOFERRET/devcourse-review/assets/148465774/c0d1dcba-5493-43a8-87dd-630f609f5b84)

```
kubectl get pods --all-namespaces
```

![17-3](https://github.com/SSOFERRET/devcourse-review/assets/148465774/0b97a1ed-2d42-45d4-be8d-63b75e52af7a)

옵션 없이 CLI를 입력했을 때엔 default namespace에 포드가 존재하지 않는다고 떴지만, --all-namespaces 옵션을 붙이니 kube-system namespace의 포드가 존재함을 알 수 있다.

### 포드 생성

```
kubectl run nginx-pod --image=nginx
//nginx-pod는 포드 이름이다.
```

### 쿠버네티스 디플로이먼트

- Deployment
  - 응용의 배포를 위해 많이 이용되는 k8s의 오브젝트 형태
  - 동일한 모습의 포드들의 복제본 모음인 replicaset을 이용하는 것이 일반적이다.
  - 단순한 레플리카셋에 비하여 동적 업데이트 및 롤백, 배포 버전 관리등이 유연하여 응용의 배포에 널리 이용됨.
  - 보통은 상태가 없는 stateless 응용의 배포에 이용
    - 포드는 언제라도 사멸할 수 있음을 기억하라
- 동작 방식
  - 디플로이먼트의 상태를 선언하면 k8s가 동적으로 의도된 상태가 되도록 레플리카셋을 관리한다.

```
// 디플로이먼트 생성
kubectl create deployment dpy-nginx --image=nginx

// 디플로이먼트 조회
kubectl get deployment

// 디플로이먼트 상세 조회
kubectl describe deployment dpy-nginx
```

```
// 포드 복제
kubectl scale deployment dpy-nginx --replicas=3
```

### 클러스터 바깥으로 응용을 노출해보자

- k8s 서비스: 클러스터 내부의 포드에 의하여 실행되는 응용을 외부에 접근 가능하도록 노출하는 기능을 하는 오브젝트
- 노출하는 대상: 특정 포터에서 실행하는 컨테이너의 특정 포트

- NodePort 형태의 실습
```
kubectl expose pod nginx-pod --type=NodePort --name=pod-svc --port=80
```
nginx-pod의 80번 포트를 모든 노드의 특정 포트로 노출

```
kubectl get svc
```
![17-4](https://github.com/SSOFERRET/devcourse-review/assets/148465774/90da599b-6415-44e3-9abe-81b19b0abe95)

![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/0218e9e3-171e-43ca-a347-21387383f9d2)

### 오브젝트 삭제

```
kubectl delete service pod-svc
```

복제된 오브젝트를 삭제하면 다음과 같은 일이 생긴다
![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/5ae10bec-b8a2-461c-81d7-cf00e65d790c)

- 디플로이먼트 개수를 3개로 설정했기 때문에, 쿠버네티스는 그 개수를 유지하고자한다.
- 디플로이먼트 자체를 삭제하면 그에 할당된 포드도 한꺼번에 삭제된다.

### 매니페스트

- k8s 오브젝트에 대한 명세를 파일로 기록한 것.
  - YAML 의 형태를 이용
- 파일에 각 오브젝트에 의도하는 상태를 기술
  - 이것을 오브젝트 스펙이라고 부름
- 이 파일을 이용하여
  - 오브젝트를 생성할 수 있음
  - 오브젝트의 상태를 변경할 수 있음
- 자동화가 필요한 환경에서 명령어 라인에 일일이 입력하는 것보다 많이 이용됨

```
kubectl apply -f
// kubectl create -f 와의 차이
```

---

## 쿠버네티스를 이용한 서비스 운용

- 매니페스트를 이용하여 디플로이먼트 생성

```python
// app.py
from flask import Flask
import socket

app = Flask(__name__)

hostname = socket.gethostname()
ipv4 = socket.gethostbyname(hostname)

message = f'<p>Hostname: {hostname}</p><p>IPv4 Address: {ipv4}</p>\n'

@app.route("/")
def root():
    return message
```

```yaml
// deployment.yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: dpy-hname
  labels:
    app: hostname
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hostname
  template:
    metadata:
      labels:
        app: hostname
    spec:
      containers:
      - name: hname
        image: {-}/hostname:latest //{-} 위치에 실제 docker hub 이름을 기입한다.
        ports:
        - containerPort: 80
```

```yaml
// service.yaml
apiVersion: v1
kind: Service

metadata:
    name: svc-hname
spec:
    type: NodePort
    selector:
        app: hostname
    ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30000
```

위의 파일들을 이미지 파일과 같은 디렉토리에 묶고 다음 CLI를 입력한다.

```cmd
// cmd prompt
docker build -t hostname:latest .
docker tag hostname:latest {-}/hostname:latest
docker push {-}/hostname:latest
```

이로써 디플로이먼트와 서비스가 생성되어 동작 상태가 된다.

### 쿠버네티스 포드의 동작 보증

- 디플로이먼트를 구성하는 포드를 1개로 만든 후, 1초에 한 번씩 curl port를 실행하는 스크립트 파일을 power shell로 만든다.
  
```powershell
$i=0
while($true)
{
    % { $1++; write-host -NoNewline "$i $_" }
    (Invoke-RestMethod "http://localhost:30000")-replace '\n', " "
    Start-Sleep -Seconds 1
}
```

- 이를 실행한 후, 고의로 포드를 죽인다

```
// 해당 디플로이먼트의 컨테이너에 들어간다
kubectl exec -it dpy-hname-d46897857-kwgx2 -- /bin/bash 

// 해당 컨테이너에서 실행 중인 프로세스 중 flask와 관련된 프로세스를 찾기 위한 코드이다. 이 코드로 디플로이먼트가 어디서 동작하는 중인지 알 수 있다.
ps ax | grep flask

// 해당 flask를 죽인다. 20은 해당 디플로이먼트가 동작하는 위치이다.
kill -9 20
```
위 코드로써 확인된 포드의 동작은, 처음에 멈췄다가 다시 재실행되었다.
이로써 다음과 같은 결론을 내릴 수 있다.

- 디플로이먼트에 의하여 배포된 포드의 컨테이너 실행에 문제가 생기면 k8s는 이 포드에서 실행하던 컨테이너를 삭제하고, 다시 컨테이너를 생성해 응용을 지속 실행한다.\
- k8s에 의해 관할되는 포드는 복구할 수 없는 오류를 맞닥뜨리는 경우에도 재시작에 의하여 정해진 동작을 지속하도록 보장된다. 컨테이너에 기반한 응용은 상태를 띠지 않는 (stateless) 방식으로 구현되어야 한다.

---

## 포드의 업데이트와 복구

- 소프트웨어 업데이트가 발생함에 따라 동적 업데이트가 필요한데, k8s는 어떻게 업데이트를 수행하는가
- 소프트웨어 업데이트는 실패할 수 있으므로, 빠르고 유연한 복구는 필수 기능이다. 롤백하는 방법은 무엇인가.

```
//rollout.yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: dpy-nginx
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16.0
        ports:
        - containerPort: 80
```

```
kubectl apply -f rollout.yaml
kubectl get deploy
kubectl rollout status deployment dpy-nginx // 디플로이먼트 배포 상태를 조회
kubectl rollout history deployment dpy-nginx // 디플로이먼트 배포 이력을 조회
```

- kubectl annotate를 적용해보자
```
// rollout.yaml로 인해 1.16.0 ver로 설치된 nginx를 1.17.0으로 재설치한다.
kubectl set image deployment dpy-nginx nginx=nginx:1.17.0

// annotate를 사용하여 변경 사유에 내용을 기입할 수 있다.
kubectl annotate deployment dpy-nginx kubernetes.io/change-cause="update nginx
```
---

## 쿠버네티스 서비스와 볼륨

### 서비스

- 클러스터 외부에서 클러스터에 접속하는 방법
  - 클로스터 내부에서 동작하는 기능을 외부로 노출하는 것을 서비스라고 부른다
- 서로 다른 서비스의 종류
  - 클러스터 IP
    - 클러스터 내부에서만 접근할 수 있는 IP를 할당
    - 포트 포워딩 또는 프록시를 통해 클러스터 외부로부터 접근 가능
    - 테스트, 디버깅 등의 목적에 제한적으로 이용
  - 노드포트
    - 모든 워커 노드의 특정 포트를 열고 여기로 오는 모든 요청을 노드포트 서비스에 전달
    - 노드포트 서비스는 해당 요청을 처리할 수 있는 포드로 요청을 전달
  - 로드밸런서
    - 클로스터 외부의 로드밸런허를 이용하여 부하 균등화를 수행한다
    - 노드포트와 달리 특정 노드가 접근 불가한 경우에도 서비스 제공이 가능
  - 인그레스
    - 복수의 서비스에 대해 목적에 따라 트래픽을 연결하는 도구
   
### 동적 수평 오토스케일링

- HPA (Horizontal Pod Autoscaler)
  - 부하량에 따라 디플로이먼트의 포드 수를 유동적으로 관리하는 k8s의 기능
    - 보통의 경우, 고려되는 부하량: CPU 및 메모리 사용량
  - 메트릭스 서버로부터 부하 계측값을 전달받아 동일한 기능을 제공하는 포드의 수를 동적으로 조절
  - 스케일링 기준이 되는 값과 최소/최대 포드의 수를 지정
