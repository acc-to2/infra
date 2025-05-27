# 🌩️ To2 인프라 레포지토리

이 저장소는 **AWS 인프라를 코드로 관리**하기 위한 CloudFormation 템플릿을 정의하고 문서화합니다.  
반복 가능하고 안정적인 배포를 위해 모든 리소스를 선언적 방식으로 구성하며, 팀 간 협업 및 인프라 변경 이력을 명확하게 추적할 수 있도록 지원합니다.

<br>

## 📐 인프라 아키텍처 다이어그램

![To2 Architecture](https://github.com/acc-to2/infra/blob/main/to2-architecture.jpg)

<br>

## 🧱 주요 모듈 설명

### 🔹 네트워킹 구성
- **VPC**: 퍼블릭/프라이빗 서브넷 분리
- **Security Group**: 서비스별 최소 권한 설정
- **NAT Gateway**: 프라이빗 서브넷의 인터넷 접근
- **VPC Endpoint**: S3, DynamoDB, MQ, DDB, CloudWatch, ECR 등 서비스 연결
- **ALB**: HTTP 요청 라우팅

### 🔹 서비스 실행
- **AWS ECS Fargate**: 백엔드 컨테이너 기반 서비스
- **AWS Lambda**: 서버리스 유틸리티 함수 처리

### 🔹 배포 및 CI/CD
- **Github Actions**: CI/CD 자동화
- **Docker & ECR**: 컨테이너 이미지 빌드 및 저장
- **Amazon S3, CloudFront**: 프론트엔드 정적 파일 배포
- **Amazon Route53**: 사용자 도메인 관리

### 🔹 데이터 및 메시징
- **Amazon DynamoDB**: 서버리스 NoSQL DB
- **Amazon MQ (ActiveMQ)**: 메시징 브로커

### 🔹 로깅 및 모니터링
- **Amazon CloudWatch**: 로그 수집 및 지표 모니터링

<br>

## ✅ 인프라 요구사항 검증


<br>

## 🧯 트러블슈팅 히스토리

### ECR Pull 인증 오류
- 문제: ECR API 호출 중 인증 오류 발생
- 조치: STS 엔드포인트 누락 확인 → 추가 후 해결

### Cognito Access Token 누락 필드 오류
- 문제: 토큰에 `email` 필드 없음 → 인증 로직 오류
- 조치: `email` 필드 삽입 Lambda 함수 생성 후  
  `pre-token-generation trigger`로 연결하여 해결

<br>

## 💡 개선 및 비용 절감 계획

- **Amazon MQ → Redis ElastiCache Pub/Sub**
  - 현재는 구현 용이성을 고려해 ActiveMQ 사용
  - 추후 Redis Pub/Sub로 전환하여 비용 절감 예정

- **NAT Gateway 제거**
  - Lambda 프록시 + API Gateway 구성으로 대체 가능
  - 현재 외부 호출 시 403 오류 발생 → API Gateway 리소스 정책 수정 필요

