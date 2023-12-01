### Terraform 실행 단계
- 환경 초기화(terrafrom init) → 결과 미리보기(terraform plan) → 실행(terraform apply) → 삭제(terraform destroy) 단계로 이루어진다

> [!info] terraform init
> The `terraform init` command initializes a working directory containing Terraform configuration files. 
 This is the first command that should be run after writing a new Terraform configuration or cloning an existing one from version control. It is safe to run this command multiple times.

>`terraform init` 명령어는 Terraform 구성 파일이 포함된 작업 디렉터리를 초기화합니다.(환경 초기화)
>이 명령은 새 Terraform 구성을 작성하거나 버전 관리에서 기존 구성을 복제한 후 가장 먼저 실행해야 하는 명령입니다. 이 명령은 여러 번 실행해도 안전합니다.

- `terraform init` 명령어 실행 시 .terraform 디렉토리가 생성된다.
- main.tf에 설정하고 생성과 관련된 module, library, provider 등이 담기게된다. 그리고 .terraform.lock.hcl 파일도 동시에 생성된다.
```shell-session
e.g.) 
$ tree .terraform
.terraform
└── providers
    └── registry.terraform.io
        └── hashicorp
            └── aws
                └── 5.28.0
                    └── linux_amd64
                        └── terraform-provider-aws_v5.28.0_x5
```

>[!info] .terraform.lock.hcl
> - 0.14 version 이후부터 provider 종속성을 고정시키는 파일
> - 지속적으로 업그레이드되는 provider와 module로 인해 변경된 버전으로 설치될 가능성 있음
> - 따라서 작업당시 main.tf 파일에 버전 정보를 기입하고 `.terraform.lock.hcl` 파일이 있으면
>   해당 파일에 명시된 버전으로 `terraform init` 수행
> - 작업자가 버전을 변경하고싶다면 `terraform init -upgrade` 명령어로 버전 변경 가능
