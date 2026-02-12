# AWS HTTPS 배포 기록 (gnews)

> 정해진 틀 없이  
> **내가 사용하고 있는 AWS Service와 배포 과정을 기록해두는 README**

---

## 📌 gnews 환경 설명

- **OS**: AWS EC2 (Ubuntu)
- **Web Server**: Nginx
- **Application**: Spring Boot (8080)
- **Reverse Proxy**: Nginx → Spring Boot
- **DB / Cache**
  - MySQL (Docker)
  - Redis (Docker)

### 현재 구조 요약

```
Internet
↓
Route 53 (Domain)
↓
Application Load Balancer (HTTPS)
↓
EC2 (Nginx :80)
↓
Spring Boot (:8080)
```

---

## 🎯 내가 하려고 한 것

> **이미 AWS EC2에 배포된 Spring Boot 웹을**
>
> - 도메인 적용
> - HTTPS 적용
> - AWS 서비스(Route53 + ACM + ALB)를 사용해
> - 실서비스 구조로 배포하기

---

## 🚀 HTTPS 배포 전체 과정

### 1️⃣ Route 53 - Hosted Zone 생성
- 도메인 관리를 AWS Route 53에서 하기 위해 Hosted Zone 생성

<img width="1289" height="543" alt="image" src="https://github.com/user-attachments/assets/2fa7c2a2-8bf1-469b-b733-c8fba0e89385" />

---

### 2️⃣ 도메인 구매처에서 네임서버(NS) 수정
- Route 53에서 생성된 **NS 레코드 4개**
- 도메인 구매처에 그대로 등록

<img width="1875" height="883" alt="image" src="https://github.com/user-attachments/assets/e4cc4519-6f27-4a11-9a2a-80f2bd36ca81" />

> ⚠️ 이 과정을 하지 않으면  
> ACM 인증 / ALB 연결이 정상적으로 동작하지 않음

---

### 3️⃣ ACM(AWS Certificate Manager) 인증서 생성
- HTTPS 적용을 위한 SSL 인증서 생성
- **반드시 ALB와 같은 리전(us-east-1)** 에서 생성

<img width="1902" height="806" alt="image" src="https://github.com/user-attachments/assets/989a20ec-1084-4492-96bb-e28bdfbd3063" />

---

### 4️⃣ ACM 생성 후 Route 53에 검증 레코드 생성 (중요)
- **Create records in Route 53**
- DNS 검증 방식으로 인증서 Issued 상태로 변경

<img width="1883" height="775" alt="image" src="https://github.com/user-attachments/assets/2f0d84a9-13e2-44e2-bb88-af72d2f99db1" />

---

### 5️⃣ Target Group 생성
- ALB가 트래픽을 전달할 대상
- EC2 Instance + HTTP:80

<img width="1885" height="797" alt="image" src="https://github.com/user-attachments/assets/34aa3818-9368-481d-8079-0f85e799ea6e" />

---

### 6️⃣ EC2 Security Group 생성
- **SSH(22)** : 내 IP만 허용
- **HTTP(80)** : Application Load Balancer SG만 허용

<img width="1323" height="455" alt="image" src="https://github.com/user-attachments/assets/841252a5-1a5b-4edd-9448-5b255d9ad753" />

---

### 7️⃣ EC2 Security Group 연결
- 생성한 EC2 보안그룹을 실제 인스턴스에 적용

<img width="1499" height="508" alt="image" src="https://github.com/user-attachments/assets/021754b1-8340-4266-b487-12a367bccb7e" />

---

### 8️⃣ Application Load Balancer Security Group 생성
- **HTTP(80), HTTPS(443)** : 0.0.0.0/0 허용
- 외부 트래픽 수신용 SG

<img width="1416" height="592" alt="image" src="https://github.com/user-attachments/assets/ce746711-fdc0-4377-bc78-e6f3370909ea" />

---

### 9️⃣ Application Load Balancer 생성 (중요)
- Internet-facing ALB
- Listener 구성
  - **HTTP 80 → HTTPS 443 Redirect**
  - **HTTPS 443 → Target Group Forward**
- ACM 인증서 연결

<img width="1501" height="772" alt="image" src="https://github.com/user-attachments/assets/4171781a-a3c7-4033-880b-b400f2adab96" />
<img width="1534" height="745" alt="image" src="https://github.com/user-attachments/assets/ce33e16d-f83f-4f1b-97fe-ef15d37b916a" />
<img width="1515" height="793" alt="image" src="https://github.com/user-attachments/assets/8f09b6fb-71fc-451b-b5d4-0ec085ebe9a1" />
<img width="1509" height="783" alt="image" src="https://github.com/user-attachments/assets/f41052b3-186d-4723-8f37-21622d371172" />

---

### 🔍 Target Group 상태 확인
- 초기 Unhealthy → 보안그룹 연결 후 **Healthy 상태 확인**
- ALB ↔ EC2 통신 정상

---

### 🔟 Route 53 → Application Load Balancer 연결
- **A / ALIAS 레코드**
- 루트 도메인 + www 도메인 모두 ALB로 연결

<img width="1231" height="803" alt="image" src="https://github.com/user-attachments/assets/1a24cca6-3c0a-404d-aede-8afff8204ef9" />

---

### 1️⃣1️⃣ HTTPS 배포 성공 🎉
- 도메인 접속 시 HTTPS 적용
- HTTP 접속 시 HTTPS로 리다이렉트

<img width="1782" height="1022" alt="image" src="https://github.com/user-attachments/assets/a39aad9e-63d3-4474-8c38-5231ec2f0785" />

---

## ✅ 결과 정리

- AWS 표준 아키텍처 기반 HTTPS 배포 성공
- Route 53 + ACM + ALB + EC2 연동 이해
- 실서비스 운영 가능한 구조 완성

---

## 📎 참고
이 문서는 **gnews 프로젝트 HTTPS 배포 과정을 기록한 개인 정리 문서**입니다.
