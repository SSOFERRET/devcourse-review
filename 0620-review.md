# :fire: 6/20 학습 내용

## CD 파이프라인

### IaC와 테라폼

#### IaC (Infrastructure as Code)

- 구성 관리
  - 구정은 의존성 때문에 코드에 못지 않게 소프트웨어 시스템에 큰 영향을 미침
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
