#### 🏗 인프라 운영

**외부 서비스 인프라**
- CloudFront, WAF, ALB, EC2 기반 외부 서비스 아키텍처 설계 및 구축
- EC2 (Amazon Linux 2023), RDS (Aurora MySQL, MySQL), ElastiCache (Redis OSS, Valkey), DocumentDB 리소스 생성 및 관리
- SES (반송률 관리), Rekognition (이미지/영상 필터), MSK (Kafka Cluster), OpenSearch 운영

**Web Server**
- Apache, Nginx 리소스 생성·관리 및 튜닝 최적화
- Nginx Reverse Proxy 설정 및 관리 (uWSGI, WebSocket)

**백업 및 비용 최적화**
- AWS Backup 구성 (S3, RDS, EC2) — 증분 및 소산 백업 적용 (Osaka Region)
- EC2, RDS 스케줄러로 비용 절감
  - AWS Instance Scheduler, CloudFormation, EventBridge, Lambda, DynamoDB
- CloudFront + Lambda 자동 퍼지 설정 (캐시 무효화 자동화)
- S3, ECR, SSM VPC Endpoint 구성 (보안 및 비용 절감)

---

#### 🔁 CI/CD

- **SpringBoot 배포 파이프라인**: GitLab → Gradle → Jenkins → CodeDeploy
- **컨테이너 배포 파이프라인**: GitLab → GitLab Runner (Docker/alpine) → ECR → EC2 (Docker Swarm / ECS)
- **패키지 관리**: Verdaccio, CodeArtifact 운영
- GitLab, Jenkins 운영·관리·마이그레이션
  - GitLab: Export/Import 프로젝트 마이그레이션, Runner 및 Pages 구성
  - Jenkins: Jobs, Credentials, Plugins, SSH, Verdaccio 연동 관리

---

#### 🔒 보안

- **AL2023 OS 보안 강화**
  - CrowdStrike, HostIPS 설치
  - 커널 업데이트·튜닝, 계정 패스워드 유효기간 활성화, PAM 인증 제한 (`pam_faillock`), SUID/SGID 조정, 서비스 배너·su 권한 관리
- **보안 AMI 자동 생성 파이프라인 구축** (커널 업데이트 목적)
  - EventBridge + Lambda + SSM Document
- AWS Site-to-Site VPN 구성 (AWS ↔ Paloalto)
- WAF + Athena 기반 Block 리스트 조회 체계 구성

---

#### 📊 모니터링

- **Zabbix**
  - Slack 알람 연동 (Amazon Q Developer in chat applications, Slack App 생성)
  - Custom Template 생성으로 세부 모니터링 강화
    - Top Process 수집: `ps -eo user,pid,comm,state,etime,%mem,%cpu --sort=-%cpu | head -10`
    - Top CPU/IO 수집: `pidstat -ud 1 1 | grep -vE '^Linux' | head -10`
  - ALB, Target Group Unhealthy Host Count 알람 설정
- **Dynatrace** 모니터링 서비스 운영

---

#### ⚙️ 자동화

- **AWS SDK + Bedrock 기능 검증**
  - Claude 3.5 Sonnet (추론 모델) 구독
  - EC2 인스턴스 목록 조회 (SDK) → 태그 추천 (DynamoDB + Bedrock) → Slack 발송
- **EC2 인스턴스 자산 목록 수집** (OS, Package 정보)
  - EC2, SSM, S3, Athena, QuickSight
  - 패키지 버전 불일치 오류 개선, OS 정보 수집 기능 추가, S3 수명 주기 설정
- **Ansible**: VMware 패스워드 변경, 보안 스크립트 실행 자동화
- **Terraform**: VPC, Subnet, Route Table, IGW, NGW, Endpoint 코드화
- **Systems Manager**: 패스워드 변경, 보안 스크립트 실행 자동화

---

#### ☸️ K8s 리서치 및 데모 환경 구성

- **EKS 환경 구성** (AWS Workshop 기준)
  - AWS Cloud9, VPC (Custom), Add-ons: EC2 Image Builder, AWS Load Balancer Controller, Amazon CloudWatch, Cluster Autoscaler, Amazon EBS CSI Driver
  - CI/CD: GitLab + ArgoCD, Kubernetes Dashboard
- **kOps 기반 Kubernetes 설치** (AWS EC2)
  - S3, Route 53 연동으로 Kubernetes Cluster 생성
  - External DNS, Cluster Autoscaler, Docker Secret, Dashboard, Prometheus & Grafana 모니터링
  - Ingress Controller (Kong, Nginx) 등 필요 서비스 추가 구성

---

## 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Cloud** | AWS (EC2, RDS, EKS, S3, CloudFront, WAF, ALB, VPC, Lambda, EventBridge, SSM, Bedrock, MSK, OpenSearch, SES, Rekognition) |
| **Infra 자동화** | Terraform, Ansible, AWS Instance Scheduler, CloudFormation |
| **Container** | Docker, Docker Swarm, ECS, Kubernetes (EKS, kOps), Helm |
| **CI/CD** | GitLab, GitLab Runner, Jenkins, CodeDeploy, ArgoCD |
| **Web Server** | Apache, Nginx (Reverse Proxy, uWSGI, WebSocket) |
| **Monitoring** | Zabbix, Dynatrace, Prometheus, Grafana, Amazon CloudWatch |
| **Security** | CrowdStrike, HostIPS, Paloalto VPN, WAF, Athena |
| **Package 관리** | Verdaccio, CodeArtifact, ECR |
| **DB / Cache** | Aurora (MySQL), MySQL, Redis OSS, Valkey, DocumentDB |
