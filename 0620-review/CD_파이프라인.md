# :fire: 6/20 학습 내용

## CD 파이프라인

### IaC와 테라폼

#### IaC (Infrastructure as Code)

- 구성 관리
  - 구성은 의존성 때문에 코드에 못지 않게 소프트웨어 시스템에 큰 영향을 미침
  - 따라서 구성 관리는 작은 빌드, 통합, 릴리스로 이루어지는 CI/CD에 중요한 측면
  - 이를 체계적으로 관리하고 자동화할 수 있는 여러 종류의 도구가 만들어지고 이용되어 왔다.
 
- IaC
  - 인프라스트럭처(소프트웨어가 의도된 목적을 활용하기 위하여 이용하는 환경 구성)를 생성, 변경, 관리
  - 수작업에 의한 것보다 안정성, 일관성, 재현 가능성을 향상시킬 수 있다.
  - 버전 관리, 재사용, 공유 등에 유리
  - 코드를 이용하여 인프라 리소스를 정의하고 조합하는 형태로 관리: 다양한 형태의 리소스를 정의하고 조합하는 모듈들로 이루어짐
 
![image](https://github.com/SSOFERRET/devcourse-review/assets/148465774/150e7a99-61ab-438a-83ee-6cd5419ebc90)


#### 테라폼

- Hashicorp 사에서 제공하는 IaC 도구
- scope(범위 결정) → Author(코드 작성) → Initialize → Plan → Apply
<br />
- 설치: https://developer.hashicorp.com/terraform/install

---

### 실습

- 적당한 위치에 빈 디렉토리를 만들고, 아래와 같은 내용을 main.tf 파일로 작성
```
//main.tf

terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

// Terraform registry에서 설정을 가져옴. 버전은 3.0.1 이후로 지정
provider "docker" {}

// Docker 이미지를 지정하고
resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "sample"
// 이것을 이용한 컨테이너를 인프라로 생성 (포트 80을 열어 8000 포트로 노출)
  ports {
    internal = 80
    external = 8000
  }
}
```

##### 테라폼 명령어

```
terrafrom validate
terraform fmt // 파일의 형태를 통일하는 명령어
terraform apply // 생성. 플랜 단계로 넘어감.
terraform destroy // 삭제
```

---

#### 변수를 이용한 인프라 구성

```
// main.tf 수정

terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = var.container_name

  ports {
    internal = 80
    external = 8000
  }
}
```

```
// variables.tf

variable "container_name" {
  description = "Value of the name for the Docker container"
  type        = string
  default     = "ExampleNginxContainer"
}
```

##### 테라폼 명령어2

```
terraform apply --auto-approve -var "container_name=YetAnotherName" // 컨테이너명에 변수 설정
```

---

#### 출력 포맷 적용하기

```
//outputs.tf

output "container_id" {
  description = "ID of the Docker container"
  value       = docker_container.nginx.id
}

output "image_id" {
  description = "ID of the Docker image"
  value       = docker_image.nginx.id
}
```

---

### Terraform Cloud

- 인프라 상태 정보 유지
  - SCM에 저장하는 것은 바람직하지 않다
  - Jenkins agent 는 필요에 따라 생성되고 역할을 다하고 나면 사라지는 k8s 포드에 의해 실행되기 때문에 이 정보를 기록할 수 없다.
  - k8s persistent volume 을 이용하는 방법도 적합하지 않음
  - 보통은 이런 것을 저장하기 위한 vault 서비스 이용
- 설치: https://app.terraform.io

#### Terraform Cloud 설정

- Organization 신규 생성
  - User Settings > Organization
- 콘솔 로그인
  - 명령어: terraform login
  - 웹 화면으로 redirect 되고, 여기서 토큰 생성
  - 이 토큰을 입력하여 로그인 완료
  - 토큰은 비밀로 관리하고 잘 저장해 둘 것
 
---

### CD 파이프라인 설계

- 지금까지 개발한 CI 파이프라인의 상태
  - Code checkout → Build & test → Packaging & Registry push
  - Staging과 Acceptance test는 임시 상태
- CD 파이프라인 완성을 위해 남아 있는 일들
  - 스테이징 환경과 프로덕션 환경 구성 (Terraform IaC를 이용)
  - Acceptance test 와 smoke test 단계를 설정
  - 추가로 고려할 것: 빌드 버전 관리

#### 프로덕션 vs 스테이징

- 스테이징 환경은 프로덕션 환경을 가능한 한 그대로 모사하도록 설계되어야 함
  - (가상화된) 인프라의 논리적 구성뿐만 아니라 컴퓨팅 리소스의 물리적/지리적 배치 등도 고려
  
