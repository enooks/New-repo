# Learn about Infrastructure as Code (IaC)

> Terraform은 사람과 기계가 읽을 수 있는 코드로 인프라를 정의할 수 있는 도구
> 

대규모 분산 시스템, 클라우드 네이티브 애플리케이션, 서비스 기반 아키텍처를 관리하기 위해

‘IaC’ (Infrastructure as Code)를 알아보세요.

코드로서의 Infrastructure는 그래픽 사용자 인터페이스나, 수동 명령줄 스크립트 시퀀스가 아닌

구성 파일로 관리를 한다. (main.tf?)

이 방식을 사용하면 버전관리, 재사용, 공유할 수 있는 리소스 구성을 정의하여 안전하고 일관성있고

추적 가능하며 반복 가능한 방식으로 인프라를 구축, 변경, 관리 할 수 있다.

<aside>
💡 인프라를 구성 파일로 관리를 하게되면 버전관리, 재사용, 공유를 할 수 있음
안전하고 일관성, 추적, 반복 가능. 인프라 구축, 변경, 관리 가능.

</aside>

## Infrastructure as Code

What is it? Why is it important?

인프라는 전통적으로 어떻게 관리되고 있나요?

프라이빗 데이터센터 내부에서 실행되는 VMware와 같은 기존 인프라를 살펴봅니다. 

고전적인 접근 방식은 다음과 같습니다: 

인프라 소비자인 제가 티켓을 제출하면 티켓 대기열의 다른 쪽 끝에 있는 누군가가 티켓을 가져와서 관리 포털이나 관리 콘솔에 로그인한 후 해당 인프라를 가리키고 클릭하여 프로비저닝하는 방식이었습니다.

이런 경우는 많은 인프라를 관리할 필요가 없는 경우에는 괜찮았습니다.

또는 인프라의 이탈이 상대적으로 미미한 경우였습니다.

그리고 이는 많은 프라이빗 데이터센터의 경우였습니다.

가상 머신은 수개월에서 수년 동안 사용되며, 배포 규모가 비교적 제한적이었기 때문에 수동으로 시스템을 가리키고 클릭하여 관리할 수 있었습니다.

이제 전환을 진행하면서 몇 가지 주요 변경 사항이 있습니다.

1. 주로 API를 기반으로 하는 클라우드 환경으로 전환하고있습니다.
2. 인프라의 탄력성이 훨씬 높아져 리소스의 수명이 몇 개월에서 몇 년이 아니라 며칠에서 몇 주가 될 수 있습니다.

인프라의 규모가 훨씬 더 큰 이유는 소수의 대규모 인스턴스 대신, 다수의 소규모 인스턴스가 있을 수 있기 때문에 프로비저닝해야 할 것이 훨씬 더 많으며, 이러한 인프라는 주기적인 경향이 있기 때문입니다.

트래픽이 많은 낮에는 부하를 처리하기 위해 확장했다가 밤에는 고정 비용이 아니기 때문에 비용을 절감하기 위해 축소할 수 있습니다. 감가상각이 가능한 하드웨어를 소유하는 것과 달리 이제는 시간 단위로 비용을 지불합니다. 따라서 필요한 인프라만 사용하는 것이 합리적이며, 이러한 탄력성을 확보해야 합니다.

따라서 이러한 변경을 시작하면 갑자기 모든 것이 달라집니다. "매일 아침 천 개의 티켓을 제출하여 최대 용량까지 돌린 다음 밤에 다시 천 개의 티켓을 제출하여 다시 돌린 다음 이 모든 것을 수동으로 관리해야 한다"는 생각은 분명 어려운 일이 되기 시작합니다: 어떻게 하면 안정적이고 견고하며 사람의 실수가 발생하지 않는 방식으로 이를 운영할 수 있을까요?

인프라의 역동성 측면에서 변화가 일어나고 있습니다. 따라서 코드로서의 인프라에 대한 진정한 아이디어는 다음과 같습니다: 어떤 의미에서 우리가 가리키고 클릭해서 달성하던 프로세스를 어떻게 하면 코드화된 방식으로 캡처할 수 있을까요? 그래서 이 작업을 한 번, 열 번, 천 번 반복해야 하는 경우 이를 자동화할 수 있습니다. 매일 아침에는 천 대의 머신을 불러오는 스크립트를 실행하고, 매일 저녁에는 같은 스크립트를 실행하여 저녁에 원하는 크기로 다시 축소할 수 있습니다.

이 작업을 자동화할 수 있을 뿐만 아니라 코드화된 양식으로 캡처했으니 버전 관리에서 확인할 수 있고 버전 관리도 할 수 있습니다. 이제 누가 무엇을 변경했는지, 특정 시점에 인프라가 실제로 어떻게 정의되었는지 점진적인 이력을 확인할 수 있으며, 기존의 포인트 앤 클릭 환경에서는 부족했던 문서화 투명성을 확보할 수 있게 되었습니다. 정말 구전으로 내려오는 전통이죠: 구성은 어떻게 되나요? 무엇을 가리키고 클릭해서 설정해야 하는가?

## Introduce to IaC

How does Terraform work?

Terraform은 애플리케이션 프로그래밍 인터페이스(API)를 통해 클라우드 플랫폼 및 기타 서비스에서 리소스를 생성하고 관리합니다. 제공업체는 접근 가능한 API를 통해 거의 모든 플랫폼 또는 서비스에서 Terraform이 작동할 수 있도록 지원합니다.

![image](https://github.com/enooks/new-repo/assets/104637797/c76bf111-60a5-4206-8e5a-dae9c69776d5)

Hashicorp와 테라폼 커뮤니티는 이미 다양한 유형의 리소스와 서비스를 관리하기 위해 수천 개의 공급자를 작성해왔습니다. 아마존 웹 서비스(AWS), 애저, 구글 클라우드 플랫폼(GCP), 쿠버네티스, 헬름, 깃허브, 스플렁크, 데이터독 등 공개적으로 사용 가능한 모든 공급자를 테라폼 레지스트리에서 찾을 수 있습니다.

Terraform의 핵심 워크플로우는 세 단계로 구성됩니다.

- **Write:** 여러 클라우드 제공업체 및 서비스에 걸쳐 있을 수 있는 리소스를 정의합니다. 예를 들어, 보안 그룹과 부하 분산 장치가 있는 가상 사설 클라우드(VPC) 네트워크의 가상 머신에 애플리케이션을 배포하는 구성을 만들 수 있습니다.
- **Plan:** Terraform은 기존 인프라와 사용자 구성을 기반으로 생성, 업데이트 또는 파기할 인프라를 설명하는 실행 계획을 생성합니다.
- **Apply:** 승인을 받으면 Terraform은 모든 리소스 종속성을 고려하여 제안된 작업을 올바른 순서로 수행합니다. 예를 들어, VPC의 속성을 업데이트하고 해당 VPC의 가상 머신 수를 변경하는 경우, Terraform은 가상 머신을 확장하기 전에 VPC를 다시 생성합니다.

[assets.avif](Learn%20about%20Infrastructure%20as%20Code%20(IaC)%2012452e103af8426eb9633fde6531d694/assets%201.avif)

### 모든 인프라 관리

이미 사용 중인 많은 플랫폼과 서비스에 대한 제공자를 Terraform 레지스트리에서 찾을 수 있습니다. 직접 작성할 수도 있습니다. Terraform은 인프라에 대해 불변의 접근 방식을 취하므로 서비스 및 인프라를 업그레이드하거나 수정하는 데 따르는 복잡성을 줄여줍니다.

### 인프라 추적

Terraform은 인프라를 수정하기 전에 계획을 생성하고 사용자에게 승인을 요청하는 메시지를 표시합니다. 또한 실제 인프라를 상태 파일에 추적하여 사용자 환경에 대한 신뢰할 수 있는 소스 역할을 합니다. Terraform은 상태 파일을 사용하여 인프라가 사용자의 구성과 일치하도록 변경할 사항을 결정합니다.

### 변경 사항 자동화

Terraform 구성 파일은 선언적 파일로, 인프라의 최종 상태를 설명합니다. Terraform이 기본 로직을 처리하므로 리소스를 생성하기 위해 단계별 지침을 작성할 필요가 없습니다. Terraform은 리소스 그래프를 구축하여 리소스 종속성을 결정하고 종속되지 않은 리소스를 병렬로 생성하거나 수정합니다. 이를 통해 Terraform은 리소스를 효율적으로 프로비저닝할 수 있습니다.

### 구성 표준화

Terraform은 구성 가능한 인프라 모음을 정의하는 모듈이라고 하는 재사용 가능한 구성 구성 요소를 지원하여 시간을 절약하고 모범 사례를 장려합니다. Terraform 레지스트리([registry.terraform.io](https://registry.terraform.io/))에서 공개적으로 사용 가능한 모듈을 사용하거나 직접 작성할 수 있습니다.

### 공동 작업

구성이 파일로 작성되므로 버전 관리 시스템(VCS)에 커밋하고 Terraform Cloud를 사용하여 팀 전체에서 Terraform 워크플로를 효율적으로 관리할 수 있습니다. Terraform Cloud는 일관되고 안정적인 환경에서 Terraform을 실행하며 공유 상태 및 비밀 데이터에 대한 안전한 액세스, 역할 기반 액세스 제어, 모듈과 공급자를 모두 공유하기 위한 비공개 레지스트리 등을 제공합니다.

### Introduction to Infrastructure as Code with Terraform

![Untitled](Learn%20about%20Infrastructure%20as%20Code%20(IaC)%2012452e103af8426eb9633fde6531d694/Untitled.png)

Terraform으로 인프라를 배포:

- **Scope:** 프로젝트의 인프라를 식별합니다.
- **Author:** 인프라에 대한 구성을 작성합니다.
- **Initialize:** Terraform이 인프라를 관리하는 데 필요한 플러그인을 설치합니다.
- **Plan:** 구성에 맞게 Terraform이 수행할 변경 사항을 미리 봅니다.
- **Apply:** 계획한 변경 사항을 적용합니다.

### Terraform Use Cases

해시코프 테라폼은 사람이 읽을 수 있는 구성 파일로 인프라 리소스를 정의하여 버전 관리, 재사용, 공유할 수 있는 코드형 인프라 도구입니다. 그런 다음 일관된 워크플로를 사용해 인프라의 수명 주기 내내 인프라를 안전하고 효율적으로 프로비저닝하고 관리할 수 있습니다.

### Multi-Cloud Deployment

여러 클라우드에 걸쳐 인프라를 프로비저닝하면 ***내결함성(fault-tolerance)**이 향상되어 클라우드 서비스 제공업체의 중단으로부터 보다 원활하게 복구할 수 있습니다. 하지만 멀티클라우드 배포는 각 제공업체마다 고유한 인터페이스, 도구, 워크플로우를 가지고 있기 때문에 복잡성이 증가합니다. Terraform을 사용하면 동일한 워크플로를 사용하여 여러 공급자를 관리하고 클라우드 간 종속성을 처리할 수 있습니다. 따라서 대규모 멀티클라우드 인프라에 대한 관리와 오케스트레이션이 간소화됩니다.

<aside>
💡 ***시스템이 하드웨어 또는 소프트웨어의 오류, 장애 또는 고장에도 계속해서 정상적으로 동작하는 능력을 의미한다. 즉, 시스템이 일시적인 장애나 오류를 감지하고 복구할 수 있는 능력을 가지고있다. 고가용성(High-Availability)과 비슷?
예시) 클러스터링, RAID**

</aside>

### **Application Infrastructure Deployment, Scaling, and Monitoring Tools**

Terraform을 사용하면 멀티 티어 애플리케이션을 위한 인프라를 효율적으로 배포, 릴리스, 확장, 모니터링할 수 있습니다. N-티어 애플리케이션 아키텍처를 사용하면 애플리케이션 구성 요소를 독립적으로 확장할 수 있으며 우려 사항을 분리할 수 있습니다. 애플리케이션은 데이터베이스 티어를 사용하는 웹 서버 풀과 API 서버, 캐싱 서버, 라우팅 메시를 위한 추가 티어로 구성될 수 있습니다. Terraform을 사용하면 각 계층의 리소스를 함께 관리할 수 있으며, 계층 간의 종속성을 자동으로 처리할 수 있습니다. 예를 들어, Terraform은 데이터베이스 티어를 배포한 후 이에 의존하는 웹 서버를 프로비저닝합니다.

### **Use Application Load Balancers for blue-green and canary deployments**

## Terraform Tutorial (Use case)

블루그린 배포와 롤링 배포(카나리아 테스트)를 사용하면 새 소프트웨어를 점진적으로 릴리스하여 소프트웨어 릴리스 실패로 인한 잠재적 영향 반경을 줄일 수 있습니다. 이 워크플로를 사용하면 다운타임이 거의 없이 소프트웨어 업데이트를 게시할 수 있습니다. (무중단배포)

- Blue-Green Deplyment Background
    
    
    In a blue-green deployment, the current service deployment acts as the **blue** environment. When you are ready to release an update, you deploy the new service version and underlying infrastructure into a new **green** environment. After verifying the green deployment, you redirect traffic from the blue environment to the green one.
    
    This workflow lets you:
    
    1. Test the green environment and identify any errors before promoting it. Your configuration still routes traffic to the blue environment while you test, ensuring near-zero downtime.
    2. Easily roll back to the previous deployment in the event of errors by redirecting all traffic back to the blue environment.
    
    ![Untitled](Learn%20about%20Infrastructure%20as%20Code%20(IaC)%2012452e103af8426eb9633fde6531d694/Untitled%201.png)
    
    기존 운영하던 환경(Blue)에서 업그레이드 환경(Green)으로 버전업 할때
    Load Balancer에서 Blue로 향하는 트래픽을 Green으로 변경해주는 작업을 블루-그린 배포라한다.
    

In this tutorial, you will use Terraform to:

1. Provision networking resources (VPC, security groups, load balancers) and a set of web servers to serve as the blue environment.
2. Provision a second set of web servers to serve as the green environment.
3. Add feature toggles to your Terraform configuration to define a list of potential deployment strategies.
4. Use feature toggles to conduct a canary test and incrementally promote your green environment.

### Prerequisites (전제 조건)

- Terraform 1.3+ installed locally
- an AWS account

### **Review example configuration (구성 예시 검토)**

Clone the [Learn Terraform Advanced Deployment Strategies](https://github.com/hashicorp/learn-terraform-advanced-deployments) repository

```bash
git clone https://github.com/hashicorp/learn-terraform-advanced-deployments.git
```

Navigate to the repository directory in your terminal.

```bash
cd learn-terraform-advanced-deployments
```

This repository contains multiple Terraform configuration files:

1. `main.tf` defines the VPC, security groups, and load balancers.
2. `variables.tf` defines variables used by the configuration such as region, CIDR blocks, number of subnets, etc.
3. `blue.tf` defines 2 AWS instances that run a user data script to start a web server. These instances represent "version 1.0" of the example service.
4. `init-script.sh` contains the script to start the web server.
5. `terraform.tf` defines the `terraform` block, which specifies the Terraform binary and AWS provider versions.
6. `.terraform.lock.hcl` is the Terraform [dependency lock file](https://developer.hashicorp.com/terraform/tutorials/configuration-language/provider-versioning).

### Review `main.tf`

이 파일에는 AWS provider를 사용하여 VPC, subnet, application security group, load balancer security group을 배포 한다.

이 구성은 ALB를 나타내는 aws_lb 리소스를 정의합니다. 로드 밸런서가 요청을 받으면 aws_lb_listener.app에 정의된 리스너 규칙을 평가하고 트래픽을 적절한 대상 그룹으로 라우팅합니다.

Error

```bash
[root@Terraform-test learn-terraform-advanced-deployments]# terraform init

Initializing the backend...
Initializing modules...
╷
│ Error: Unsupported Terraform Core version
│
│   on terraform.tf line 17, in terraform:
│   17:   required_version = "~> 1.3.0"
│
│ This configuration does not support Terraform version 1.6.4. To proceed, either choose another supported Terraform version or update this
│ version constraint. Version constraints are normally set for good reason, so updating the constraint may lead to other errors or unexpected
│ behavior.
╵
```

현재 버전은 v1.6.4 버전이어서 버전 문제인가해서 terraform 삭제 후 재설치 해보았지만 같은 오류 발생.

[terraform.tf](http://terraform.tf) 파일의 required_version = “~> 1.3.0” 을 “~> 1.6.0”으로 변경 후 정상 확인.

의문점은 ~> 1.3.0은 1.3.0 이상 2.0.0 미만의 버전을 포함시키는 건데 왜 1.6.4 버전이 안됐는지 의문.. 찾아봐야겠다.
