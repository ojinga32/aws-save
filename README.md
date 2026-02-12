정해진 틀 없이 내가 사용하고 있는 AWS Service 기록해두는 곳 
=============

gnews 환경 : nginx로 spring boot를 aws ubuntu에 배포중 redis, mysql은 docker로 띄워놓음

내가 하려고 하는 것
이렇게 배포된 웹을 https로 도메인 적용해서 배포하는 것

1. route 53 - hosted zone 생성
<img width="1289" height="543" alt="image" src="https://github.com/user-attachments/assets/2fa7c2a2-8bf1-469b-b733-c8fba0e89385" />

2. 도메인 구매처에서 네임서버(NS) 수정
<img width="1875" height="883" alt="image" src="https://github.com/user-attachments/assets/e4cc4519-6f27-4a11-9a2a-80f2bd36ca81" />

3. ACM 생성
<img width="1902" height="806" alt="image" src="https://github.com/user-attachments/assets/989a20ec-1084-4492-96bb-e28bdfbd3063" />

4. ACM 생성 후 create records in route 53(중요)
<img width="1883" height="775" alt="image" src="https://github.com/user-attachments/assets/2f0d84a9-13e2-44e2-bb88-af72d2f99db1" />

5. target group 생성
<img width="1885" height="797" alt="image" src="https://github.com/user-attachments/assets/34aa3818-9368-481d-8079-0f85e799ea6e" />

6. EC2 Security Group 생성
<img width="1323" height="455" alt="image" src="https://github.com/user-attachments/assets/841252a5-1a5b-4edd-9448-5b255d9ad753" />

7. EC2 Security Group 연결
<img width="1499" height="508" alt="image" src="https://github.com/user-attachments/assets/021754b1-8340-4266-b487-12a367bccb7e" />

8. application load balancer Security Group 생성
<img width="1416" height="592" alt="image" src="https://github.com/user-attachments/assets/ce746711-fdc0-4377-bc78-e6f3370909ea" />

9. Application Load Balancer 생성(중요)
<img width="1501" height="772" alt="image" src="https://github.com/user-attachments/assets/4171781a-a3c7-4033-880b-b400f2adab96" />
<img width="1534" height="745" alt="image" src="https://github.com/user-attachments/assets/ce33e16d-f83f-4f1b-97fe-ef15d37b916a" />
<img width="1515" height="793" alt="image" src="https://github.com/user-attachments/assets/8f09b6fb-71fc-451b-b5d4-0ec085ebe9a1" />
<img width="1509" height="783" alt="image" src="https://github.com/user-attachments/assets/f41052b3-186d-4723-8f37-21622d371172" />

10. Route 53 -> Aplication Load Balancer 연결(A/ALIAS 레코드)
<img width="1231" height="803" alt="image" src="https://github.com/user-attachments/assets/1a24cca6-3c0a-404d-aede-8afff8204ef9" />

11. https 배포 성공
<img width="1782" height="1022" alt="image" src="https://github.com/user-attachments/assets/a39aad9e-63d3-4474-8c38-5231ec2f0785" />







