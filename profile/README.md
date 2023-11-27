![image](https://github.com/every-task/.github/assets/71807768/a772dca9-1fe3-435c-93e6-dd1d7f563563)


## 1. 프로젝트 기획
![image](https://github.com/every-task/.github/assets/71807768/b3af9410-012a-4754-bb32-f394f837a81d)

## 2. 핵심 아이디어
1. **성공담, 질문글에 대한 태스크 매치**
2. **비정형 데이터의 가공 필요**
> **데이터 엔지니어링 과정을 생성형 AI를 접목하여 해결**

![image](https://github.com/every-task/.github/assets/71807768/f517d84a-c7bd-482f-9d65-48dcd3358f5e)

## 3. 사용 기술
- Back End : Java 17, Spring Boot 3.x.x, Spring Security, Spring Data JPA, Query DSL
- Front End : React, MUI
- DB : MySQL, Mongo DB
- Messaging : Kafka, Kafka Connect
- Infra : Docker, Kubernetes Nginx, GCP
- Storage : Firebase
- CI/CD : Github Action
- 3rd Party : Open AI API

## 4. 서비스 아키텍처
### 4.1. 서비스 구성
![image](https://github.com/every-task/.github/assets/71807768/836f6d2f-1ecd-41da-8e13-db3edb50c402)

> GPT와의 통신으로 병목 지점인 Task Service의 독립적인 확장의 어려움

![image](https://github.com/every-task/.github/assets/71807768/bcddb142-64cd-42ad-96ad-2b0038468d9f)

> MSA로 구성하여 병목지점인 GPT와의 통신 문제를 개선

### 4.2. 서비스 별 ERD

![image](https://github.com/every-task/.github/assets/71807768/ed2932f0-250c-4d85-9478-3b31bc3e772e)

### 4.3. 배포
![image](https://github.com/every-task/.github/assets/71807768/55b9eca0-9593-4311-bbc7-c31f188647e9)

## 5. 프로젝트 진행
![image](https://github.com/every-task/.github/assets/71807768/c667f0e7-afac-41df-9b85-7e3ef259ca32)
![image](https://github.com/every-task/.github/assets/71807768/e3085f8e-894c-4f23-b528-22028146d7e2)

## 6. 트러블 슈팅 & 개선사항

### 6.1. Kafka Timeout

#### 문제상황

kafka 메세지를 수신 후 GPT 통신이 오래 걸리는 경우, 통신에 성공하더라도 consume을 하지 못하고 timeout이 발생하였습니다.

#### 해결

GPT 통신 로직을 비동기로 전환하였습니다. 이 과정에서 메세지를 수신한 순간 정상처리로 commit 하기 때문에 비동기 처리 중 예외가 발생하면 실패 토픽 발행 및 로깅 등의 실패 처리 로직이 실행되도록 하였습니다.

### 6.2. Scale Out 환경에서의 QA

#### 문제상황

여러 개의 node에 올라간 서비스의 경우, 예외가 발생해도 바로 로그를 확인하기 어려웠습니다.

#### 해결
즉각적인 확인을 위해 Error 레벨의 로그는 슬랙 알림이 오도록 설정하여 원활한 QA가 가능하도록 하였습니다.
![image](https://github.com/every-task/.github/assets/71807768/4a4cab5b-532b-4a1a-b1de-be4e8c3c22f9)

### 6.3. Member 데이터 동기화

#### 문제상황 

여러 서비스에서 사용하는 Member 데이터는 Member Service에서 변경 이벤트가 발생하면 카프카로 변경 데이터를 전송하고, 각 Service 에서 수신하여 Member 데이터를 변경하는 방식으로 각 서비스에서  중복 로직이 매우 많았습니다.

#### 해결

`Kafka Connect`를 사용하여 Member 데이터가 변경되었을 경우 DB 간 동기화가 되도록 하였습니다.

### 6.4. Kafka Message에 State 추가

#### 문제상황

kafka 메세지를 발행할 때, create, update, delete 등 여러가지 상태에 따라 consumer 로직이 복잡 해지는 문제가 발생하였습니다.

#### 해결
kafka 필드에 state 타입을 추가하여 create, update, delete의 각 상황에 따라 topic을 추가 발행 하지 않고, 하나의 topic으로 처리 할 수 있게 하였습니다.

###  6.5. 조건에 따른 조회 로직 개선

#### 문제상황
여러 조건에 맞는 검색결과를 JPA 기반으로 데이터를 가져 오려면 코드 복잡도가 많이 증가 되는 문제가 발생했습니다.

#### 해결
확장가능한 동적 Query 를 작성하기 좋은 Query DSL 를 사용하여 BooleanExpression를 통해 다양한 조건을 동시에 처리해주는 검색 함수를 만들어
코드 복잡도를 낮출 수 있었습니다.


## 7. 향후 개선사항
1. 서비스 내 컨텐츠 부족
   - 보유한 Story, Question, Task 데이터를 활용하여 더욱 다양한  정보 제공
3. 프롬프트 엔지니어링
   - 현재 서비스의 핵심인 GPT Parsing의 성공확률이 다소 낮음
   - 프롬프트에 대한 개선이 필요
4. 부하 테스트
   - Ngrinder 등 테스트 도구를 통해 병목지점 색출 및 개선

## 8. 시연 및 발표 영상
- 시연 : 유튜브 링크
- 발표 : 유튜브 링크




