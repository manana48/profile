#### 🏗 인프라 운영

**외부 서비스 인프라**
- CloudFront, WAF, ALB, EC2 기반 서비스 아키텍처 설계 및 구축
- EC2(Amazon Linux 2023), RDS(Aurora MySQL, MySQL), ElastiCache(Redis OSS, Valkey 캐시), DocumentDB, DynamoDB 리소스 운영
- SES, Rekognition(이미지/영상 필터 목적), MSK(Kafka Cluster), OpenSearch 리소스 운영

**Web Server**
- Apache, Nginx 리소스 운영 및 튜닝 최적화
- Nginx Reverse Proxy 설정 및 관리(uWSGI, WebSocket, Java 등)

**백업 및 비용 최적화**
- AWS Backup 구성(S3, RDS, EC2) 및 증분, 소산 백업 적용(Osaka Region)
- EC2, RDS 스케줄러로 비용 절감(STAGE/QA 리소스)
  - AWS Instance Scheduler, CloudFormation, EventBridge, Lambda, DynamoDB
- CloudFront + Lambda 자동 퍼지 설정(캐시 무효화 자동화)
- (S3, ECR, SSM)VPC Endpoint 구성(보안 및 비용 절감)

---

#### 🔁 CI/CD

- **SpringBoot 배포 파이프라인**: GitLab → Gradle → Jenkins → CodeDeploy
- **컨테이너 배포 파이프라인**: GitLab → GitLab Runner(Docker/alpine) → ECR → EC2(Docker Swarm/ECS)
- **패키지 관리**: Verdaccio, CodeArtifact 운영
- GitLab, Jenkins 리소스 운영 및 마이그레이션
  - GitLab: Runner, Pages 리소스 운영, Export/Import 프로젝트 마이그레이션
  - Jenkins: Jobs, Credentials, Plugins, SSH, Verdaccio 리소스 운영

---

#### 🔒 보안

- **AL2023 OS 보안 강화**
  - CrowdStrike, HostIPS 설치
  - 커널 업데이트 및 튜닝, 계정 패스워드 유효기간 활성화, PAM 인증 제한(pam_faillock), SUID/SGID 조정, 서비스 배너, su 권한 관리
- **보안 AMI 자동 생성 파이프라인 구축** (커널 업데이트 목적)
  - EventBridge + Lambda + SSM Document
- AWS Site-to-Site VPN 구성(AWS ↔ Paloalto)
- WAF + Athena 기반 Block 리스트 조회 체계 구성

---

#### 📊 모니터링

- **Zabbix**
  - Slack 알람 연동(Amazon Q Developer in chat applications, Slack App 생성)
  - Custom Template 생성으로 세부 모니터링 강화
    - Top Process 수집, Top CPU/IO 수집
  - ALB, Target Group Unhealthy Host Count 알람 설정
- **Dynatrace** 모니터링 서비스 운영

---

#### ⚙️ 자동화

- **AWS SDK + Bedrock 기능 검증**
  - Claude 3.5 Sonnet(추론 모델) 구독
  - EC2 인스턴스 목록 조회(SDK) → 태그 추천(DynamoDB + Bedrock) → Slack 발송
- **EC2 인스턴스 자산 목록 수집** (OS, Package 정보)
  - EC2, SSM, S3, Athena, QuickSight
  - OS, Package 버전 정보 수집
  - OS, Package 버전 정보 기반 EOL 정보 수집(endoflife.date API 활용)
- **Ansible**: VMware 패스워드 변경, 보안 스크립트 실행 자동화
- **Terraform**: VPC, Subnet, Route Table, IGW, NGW, (S3, ECR, SSM)Endpoint 코드화
- **Systems Manager**: 패스워드 변경, 보안 스크립트 실행 자동화

---

#### ☸️ K8s 환경 구축, 리소스 운영 및 EKS 데모 환경 구성, 리서치

- **kOps 기반 Kubernetes 설치**(AWS EC2)
  - S3, Route 53 연동으로 Kubernetes Cluster 생성
  - External DNS, Cluster Autoscaler, Docker Secret, Dashboard, Prometheus & Grafana 모니터링
  - Ingress Controller(Kong, Nginx) 등 필요 서비스 추가 구성
  - CI/CD: GitLab + Jenkins(Kubectl)
- **EKS 환경 구성**(AWS Workshop 기준)
  - AWS Cloud9, (Custom)VPC, Add-ons: EC2 Image Builder, AWS Load Balancer Controller, Amazon CloudWatch, Cluster Autoscaler, Amazon EBS CSI Driver
  - CI/CD: GitLab + ArgoCD

---

## 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Cloud** | AWS (EC2, RDS, EKS, S3, CloudFront, WAF, ALB, VPC, Lambda, EventBridge, SSM, Bedrock, MSK, OpenSearch, SES, Rekognition) |
| **Infra 자동화** | Terraform, Ansible, AWS Instance Scheduler, CloudFormation |
| **Container** | Docker, Docker Swarm, ECS, Kubernetes (EKS, kOps) |
| **CI/CD** | GitLab, GitLab Runner, Jenkins, CodeDeploy, ArgoCD |
| **Web Server** | Apache, Nginx (Reverse Proxy, uWSGI, WebSocket) |
| **Monitoring** | Zabbix, Dynatrace, Prometheus, Grafana, Amazon CloudWatch |
| **Security** | CrowdStrike, HostIPS, Paloalto VPN, WAF, Athena |
| **Package 관리** | Verdaccio, CodeArtifact, ECR |
| **DB / Cache** | Aurora (MySQL), MySQL, Redis OSS, Valkey, DocumentDB |
