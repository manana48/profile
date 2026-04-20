# 인프라 운영
- 외부 서비스 인프라 아키텍처 구성(CloudFront, WAF, ALB, EC2)
- EC2 리소스 생성 및 관리(Amazon Linux 2023)
- RDS 리소스 생성 및 관리(Aurora(MySQL Compatible), MySQL)
- ElastiCache(Redis OSS 캐시, Valkey 캐시), DocumentDB 리소스 생성 및 관리
- 그 외 필요 리소스 생성 및 관리
  - SES(반송률 관리 목적), Rekognition(부적절한 이미지/영상 필터 목적), MSK(Kafka Cluster), OpenSearch
- Apache, Nginx 리소스 생성 및 관리  
  - Apache, Nginx 튜닝 최적화
  - Nginx Reverse Proxy 설정 및 관리(uWSGI, Websocket 등)
- 백업 서비스 구성
  - AWS Backup(S3, RDS, EC2)
  - 증분 및 소산 백업 적용(Oscaka Region)
- EC2, RDS 스케줄러 적용(비용 절감 목적)
  - AWS Instance Scheduler/CloudFormation/EventBridge/Lambda/DynamoDB
- CloudFront, S3 구성 자동 퍼지 설정(캐싱 퍼지 목적)
  - 기존 단순 Lambda 자동 퍼지 구성에서 SQS(DLQ), CloudWatch, Lambda 구성하여 퍼지 누락 개선(알람 수신 및 DLQ 드라이브) 
- Endpoint 구성(보안 및 비용 절감 목적)
  - S3, ECR, SSM Endpoint

# CI/CD 리소스 생성 및 관리
- Code(GitLab), Framework(SpringBoot), Build(Gradle), Deploy(Jenkins&CodeDeploy)
- Code(GitLab), GitLab(Runner/Docker(alpine:latest)), ECR, EC2(Docker Swarm/ECS), Verdaccio, CodeArtifact
- GitLab, Jenkins 운영, 관리, 마이그레이션
  - (GitLab)Migration(Export/Import Projects)
  - (GitLab)Runner, Pages 적용
  - (Jenkins)Jobs, Credentials, Plugins, SSH, Verdaccio 등

# 보안
- AL2023 OS 보안 조치
  - 백신 설치(CrowdStrike, HostIPS)
  - 커널 업데이트, 커널 튜닝, 계정 패스워드 유효기간 활성화, 계정 인증 제한(pam_faillock.so), SUID, SGID 조정, 서비스 배너 관리, su 권한 제한
- 보안 AMI 자동 생성(커널 업데이트 목적)
  - EventBridge, Lambda, SSM Document
- AWS Site-to-Site VPN 구성(AWS ↔ Paloalto)
- VPC 피어링, Transit Gateway 구성(VPC ↔ VPC)
- WAF, S3, Athena 구성(WAF 로그 조회 목적)
- CloudTrail, S3, Athena 구성(IAM 권한 제어 목적)
- CVE 보안 취약점 조치(OpenSSL 등)

# 모니터링
- (Zabbix)Slack 알람 연동
  - AWS Amazon Q Developer in chat applications
  - Slack APP 생성, Zabbix Template, Trigger 적용, Zabbix Slack APP 추가
- (Zabbix)모니터링 수집 개선
  - Custom Template 생성(세부 모니터링 목적)
    - UserParameter=topprocesses,ps -eo user,pid,comm,state,etime,%mem,%cpu --sort=-%cpu | head -10
    - UserParameter=topcpuio,pidstat -ud 1 1 | grep -vE '^Linux' | head -10
  - ALB, TG 알람 설정(Unhealthy Host Count)
- (Dynatrace)모니터링 서비스 운영

# 자동화
- AWS SDK/Bedrock 기능 검증
  - Bedrock (추론)모델 구독(Claude 3.5 Sonnet)
  - EC2 인스턴스 목록 조회(SDK) →  추천 태그(DynamoDB/Bedrock) → Slack 발송
- AWS EC2 인스턴스 자산 목록 수집(OS/Package 등)
  - EC2, SSM, S3, Athena, Quick Suite
  - OS, Package 버전 정보 수집 기반 EOL, CVE 정보 수집(endoflife API 등)
- Ansible(VMWare/패스워드 변경, 보안 스크립트 실행)
- Terraform(AWS/VPC, SN, RT, IGW, NGW, Endpoint)
- Systems Manager(AWS/패스워드 변경, 보안 스크립트 실행)
- 사내 VM 리소스(VMWare) 인증서 자동 갱신 적용(Let's Encrypt)

# 컨테이너 환경 운영
- kOps 활용하여 AWS 리소스 기반 K8s 설치 및 운영
  - AWS 연동 및 S3, Route 53 를 통한 Kubernetes Cluster 생성
    - External DNS, Cluster Autoscaler, Docker Secret, Dashboard, Monitoring(Prometheus&Grafana), Ingress-controller(Kong, Nginx) 등 필요에 따른 서비스 추가 설치
    - CI/CD(GitLab, Jenkins)
- ECS 리소스 생성 및 운영
  - CI/CD(GitLab, Jenkins)
  - 시스템 리소스, 트래픽 대비 AutoScale 구성
- Docker Swarm 리소스 생성 및 운영
  - CI/CD(GitLab, Jenkins or GitLab CI)
  - 시스템 리소스, 트래픽 대비 AutoScale 구성
- EKS 리소스 구성 검증(AWS Workshop 기준)
  - Cloud9, VPC(Custom), Add-ons(Image Builder, ALBIngress, Cloudwatch, Cluster Autoscaler, EBS), Dashboard
  - CI/CD(GitLab, ArgoCD)
