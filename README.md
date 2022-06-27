![image](https://user-images.githubusercontent.com/487999/79708354-29074a80-82fa-11ea-80df-0db3962fb453.png)

# Site Reliability Engineering(SRE) PreLab 예제 - 음식배달

- **개요**
  - 본 예제는 GitOps기반 SRE과정을 위한 클라우드 플랫폼 환경구축과 마이크로서비스 전 라이프사이클(분석/설계, 구현, DevOps 툴체인(파이프라인)을 통한 배포, 운영/모니터링)을 커버하도록 구성된 예제입니다. 
  - 분석/설계/구현을 포함한 클라우드 플랫폼기반 운영(컨테이너 오케스트레이션, 모니터링, 로깅, etc) 중심으로 예제 프로젝트는 전개됩니다.
  - PreLab 기간동안 주어진 마이크로서비스 리소스를 사용하거나, 새로운 도메인 모델을 선정 후 설계/구현/배포/운영/모니터링을 적용해도 됩니다.

- **PreLab 진행일정**
  - 분석/설계 : (1일차) 
    - ~~주제 도메인에 대한 분석/설계~~
    - ~~분석/설계 모델에 대한 마이크로서비스 구현~~
    - MSA 아키텍처 구성 및 Cloud Platform 프로비저닝
    - 산출물 : ~~기능 목록서, 이벤트스토밍 모델, 헥사고날 아키텍처,~~ MSA 아키텍처 구성도 
  
  - 배포/운영/모니터링 구현: (2~3일차) 
    - DevOps Toolchain 구축
    - EKS Cluster에 통합 메시징 인프라 구축 및 테스팅
    - 마이크로서비스 오케스트레이션 (오토 스케일아웃, 무정지 배포)
    - 서비스 메쉬 구성 및 마이크로서비스에 SideCar Injection
    - 통합 모니터링, 로깅, 메시징 플랫폼 모니터링 적용
    - 산출물 : 파이프라인 구성도, 수행 증적(Microservice 오케스트레이션, 서비스 메시, 모니터링/로깅)  
 
  - 산출물 리뷰 : (4일차)
    - MSA PaaS Platform 최종 산출물 통합
    - 오후(13:00)부터 최종 산출물을 통한 개별 검토회 진행
   
  - Phase별 산출물
    - MSA PaaS Platform 산출물은 해당 README.md 화일을 활용하거나 free format으로 제출
    - 제출 시, 반드시 소속과 이름을 기재
 
- **체크포인트** 
  - ~~Domain 주제영역 및 시나리오 이해~~
  - ~~이벤트스토밍 모델 이해~~
  - ~~폴리글랏 프로그래밍 코드(동기호출, 비동기 호출) 이해~~
  - MSA 아키텍처 구성
  - Cloud Platform 프로비저닝 
  - DevOps Toolchain 구축 
  - 분산 메시징 인프라 구축 
  - SLA 운영 - 오토 스케일아웃 (Auto Scale-out) 
  - SLA 운영 - 무정지 배포 (Zero downtime Deploy) 
  - Service Mesh 인프라 구축
  - Service Mesh 기반 마이크로서비스 Resilience 적용
  - 마이크로서비스 통합 모니터링
  - 마이크로서비스 통합 로깅
  - 분산 메시징 플랫폼 모니터링

- **PreLab 참고자료**
  - [핵심요소 기술 교재](https://drive.google.com/drive/folders/1atL7qAz4zPLcMiQvW-3PvrU1qmggdziD) (Ctrl + 클릭)
  - [Mini Shopping mall 이벤트스토밍 모델](https://labs.msaez.io/#/storming/C7pO0ZuWtXXxIKenocD9EMPYrxw2/98f0cfeee84ff3ded1a1b00f9cd38ac3) (Ctrl + 클릭) 
  - Microservice Code 리파지토리 (각 Github에 접속후 'fork'를 눌러 복제)
    - [Gateway] : https://github.com/acmexii/gateway
    - [Order] : https://github.com/acmexii/order
    - [Delivery] : https://github.com/acmexii/delivery
    - [Product] : https://github.com/acmexii/product
    
# Table of contents

- [예제 - 음식배달](#---)
  - [서비스 시나리오](#서비스-시나리오)
  - [체크포인트](#체크포인트)
  - [분석/설계](#분석설계)
  - [구현:](#구현)
    - [DDD 의 적용](#ddd-의-적용)
    - [폴리글랏 퍼시스턴스](#폴리글랏-퍼시스턴스)
    - [폴리글랏 프로그래밍](#폴리글랏-프로그래밍)
    - [동기식 호출과 Fallback 처리](#동기식-호출-과-Fallback-처리)
    - [비동기식 호출과 Eventual Consistency](#비동기식-호출과-Eventual-Consistency)
  - [Cloud Services Provisioning](#Cloud-Services-Provisioning)
  - [배포:](#배포)
    - [파이프라인 생성](#파이프라인-생성)  
  - [운영](#운영)
    - ~~[동기식 호출/서킷 브레이킹/장애격리](#동기식호출-서킷브레이킹-장애격리)~~
    - [SLA 준수 - 오토스케일 아웃](#오토스케일-아웃)
    - [SLA 준수 - 무정지 재배포](#무정지-재배포)
    - [Service Mesh 인프라구축](#Service-Mesh)
    - [마이크로서비스 통합 Monitoring](#통합-Monitoring)
    - [마이크로서비스 통합 Logging](#통합-Logging)
    - [이벤트 스트리밍 플랫폼 Monitoring](#이벤트-스트리밍-플랫폼-Install-및-Monitoring)
  - [신규 개발 조직의 추가](#신규-개발-조직의-추가)

# 서비스 시나리오

배달의 민족 커버하기 - https://1sung.tistory.com/106

**기능적 요구사항**
1. 고객이 메뉴를 선택하여 주문한다
1. 고객이 결제한다
1. 주문이 되면 주문 내역이 입점상점주인에게 전달된다
1. 상점주인이 확인하여 요리해서 배달 출발한다
1. 고객이 주문을 취소할 수 있다
1. 주문이 취소되면 배달이 취소된다
1. 고객이 주문상태를 중간중간 조회한다
1. 주문상태가 바뀔 때 마다 카톡으로 알림을 보낸다

**비기능적 요구사항**
1. 트랜잭션
    1. 결제가 되지 않은 주문건은 아예 거래가 성립되지 않아야 한다  Sync 호출 
1. 장애격리
    1. 상점관리 기능이 수행되지 않더라도 주문은 365일 24시간 받을 수 있어야 한다  Async (event-driven), Eventual Consistency
    1. 결제시스템이 과중되면 사용자를 잠시동안 받지 않고 결제를 잠시후에 하도록 유도한다  Circuit breaker, fallback
1. 성능
    1. 고객이 자주 상점관리에서 확인할 수 있는 배달상태를 주문시스템(프론트엔드)에서 확인할 수 있어야 한다  CQRS
    1. 배달상태가 바뀔때마다 카톡 등으로 알림을 줄 수 있어야 한다  Event driven


# 체크포인트

- **분석 설계**
  - **이벤트스토밍** 
    - 스티커 색상별 객체의 의미를 제대로 이해하여 헥사고날 아키텍처와의 연계 설계에 적절히 반영하고 있는가?
    - 각 도메인 이벤트가 의미있는 수준으로 정의되었는가?
    - 어그리게잇: Command와 Event 들을 ACID 트랜잭션 단위의 Aggregate 로 제대로 묶었는가?
    - 기능적 요구사항과 비기능적 요구사항을 누락 없이 반영하였는가?    

  - **서브 도메인, 바운디드 컨텍스트 분리**
    - 팀별 KPI 와 관심사, 상이한 배포주기 등에 따른  Sub-domain 이나 Bounded Context 를 적절히 분리하였고 그 분리 기준의 합리성이 충분히 설명되는가?
      - 적어도 3개 이상 서비스 분리
    - 폴리글랏 설계: 각 마이크로 서비스들의 구현 목표와 기능 특성에 따른 각자의 기술 Stack 과 저장소 구조를 다양하게 채택하여 설계하였는가?
    - 서비스 시나리오 중 ACID 트랜잭션이 크리티컬한 Use 케이스에 대하여 무리하게 서비스가 과다하게 조밀히 분리되지 않았는가?
  - **컨텍스트 매핑 / 이벤트 드리븐 아키텍처**
    - 업무 중요성과  도메인간 서열을 구분할 수 있는가? (Core, Supporting, General Domain)
    - Request-Response 방식과 이벤트 드리븐 방식을 구분하여 설계할 수 있는가?
    - 장애격리: 서포팅 서비스를 제거 하여도 기존 서비스에 영향이 없도록 설계하였는가?
    - 신규 서비스를 추가 하였을때 기존 서비스의 데이터베이스에 영향이 없도록 설계(열려있는 아키택처)할 수 있는가?
    - 이벤트와 폴리시를 연결하기 위한 Correlation-key 연결을 제대로 설계하였는가?

  - **헥사고날 아키텍처**
    - 설계 결과에 따른 헥사고날 아키텍처 다이어그램을 제대로 그렸는가?
    
- **구현**
  - **[DDD] 분석단계에서의 스티커별 색상과 헥사고날 아키텍처에 따라 구현체가 매핑되게 개발되었는가?**
    - Entity Pattern 과 Repository Pattern 을 적용하여 JPA 를 통하여 데이터 접근 어댑터를 개발하였는가
    - [헥사고날 아키텍처] REST Inbound adaptor 이외에 gRPC 등의 Inbound Adaptor 를 추가함에 있어서 도메인 모델의 손상을 주지 않고 새로운 프로토콜에 기존 구현체를 적응시킬 수 있는가?
    - 분석단계에서의 유비쿼터스 랭귀지 (업무현장에서 쓰는 용어) 를 사용하여 소스코드가 서술되었는가?
  - **Request-Response 아키텍처 구현**
    - 마이크로 서비스간 Request-Response 호출에 있어 대상 서비스를 어떠한 방식으로 찾아서 호출 하였는가? (Service Discovery, REST, FeignClient)
    - 서킷브레이커를 통하여  장애를 격리시킬 수 있는가?
  - **이벤트 드리븐 아키텍처의 구현**
    - 카프카를 이용하여 PubSub 으로 하나 이상의 서비스가 연동되었는가?
    - Correlation-key:  각 이벤트 건 (메시지)가 어떠한 폴리시를 처리할때 어떤 건에 연결된 처리건인지를 구별하기 위한 Correlation-key 연결을 제대로 구현 하였는가?
    - Message Consumer 마이크로서비스가 장애상황에서 수신받지 못했던 기존 이벤트들을 다시 수신받아 처리하는가?
    - Scaling-out: Message Consumer 마이크로서비스의 Replica 를 추가했을때 중복없이 이벤트를 수신할 수 있는가
    - CQRS: Materialized View 를 구현하여, 타 마이크로서비스의 데이터 원본에 접근없이(Composite 서비스나 조인SQL 등 없이) 도 내 서비스의 화면 구성과 잦은 조회가 가능한가?

  - **폴리글랏 플로그래밍**
    - 각 마이크로 서비스들이 하나이상의 각자의 기술 Stack 으로 구성되었는가?
    - 각 마이크로 서비스들이 각자의 저장소 구조를 자율적으로 채택하고 각자의 저장소 유형 (RDB, NoSQL, File System 등)을 선택하여 구현하였는가?
  - **API 게이트웨이**
    - API GW를 통하여 마이크로 서비스들의 집입점을 통일할 수 있는가?
    - 게이트웨이와 인증서버(OAuth), JWT 토큰 인증을 통하여 마이크로서비스들을 보호할 수 있는가?
    - 
- **배포**
  - 각 마이크로 서비스별로 파이프라인이 구성되어 있는가?
  - 각 DevOps팀에서 테스트가 끝난 마이크로서비스는 Continuous Integration과 Continuous Deployment를 거쳐 최종 고객에게까지 자동으로 딜리버리 되는가? 

- **운영**
  - **SLA 준수**
    - 셀프힐링: Liveness Probe 를 통하여 어떠한 서비스의 health 상태가 지속적으로 저하됨에 따라 어떠한 임계치에서 pod 가 재생되는 것을 증명할 수 있는가?
    - 서비스 메시: 마이크로서비스간 커뮤니케이션 기능을 인프라레이어로 분리하여 운영할 수 있는가? 
    - 서킷브레이커, 레이트리밋 등을 통한 장애격리와 성능효율을 높힐 수 있는가?
    - 오토스케일러 (HPA) 를 설정하여 확장적 운영이 가능한가?
    - 모니터링, 통합 로깅을 구성하여 시각화된 모니터링 환경구성이 가능한가? 
  - **무정지 운영**
    - Readiness Probe 의 설정과 Rolling update을 통하여 신규 버전이 완전히 서비스를 받을 수 있는 상태일때 신규버전의 서비스로 전환됨을 siege 등으로 증명 



# 분석/설계


### AS-IS 조직 (Horizontally-Aligned)
  ![image](https://user-images.githubusercontent.com/487999/79684144-2a893200-826a-11ea-9a01-79927d3a0107.png)

### TO-BE 조직 (Vertically-Aligned)
  ![image](https://user-images.githubusercontent.com/487999/79684159-3543c700-826a-11ea-8d5f-a3fc0c4cad87.png)


### Event Storming 결과
* MSAEz 로 모델링한 이벤트스토밍 결과:  http://msaez.io/#/storming/nZJ2QhwVc4NlVJPbtTkZ8x9jclF2/every/a77281d704710b0c2e6a823b6e6d973a/-M5AV2z--su_i4BfQfeF


#### 도메인 시나리오가 반영된 이벤트스토밍 1차 모형

![image](https://user-images.githubusercontent.com/487999/79683646-63bfa300-8266-11ea-9bc5-c0b650507ac8.png)

    - View Model 추가

#### 1차 정제된 모델에 대한 기능적/비기능적 요구사항 커버리지 검증

![image](https://user-images.githubusercontent.com/487999/79684167-3ecd2f00-826a-11ea-806a-957362d197e3.png)

    - 고객이 메뉴를 선택하여 주문한다 (ok)
    - 고객이 결제한다 (ok)
    - 주문이 되면 주문 내역이 입점상점주인에게 전달된다 (ok)
    - 상점주인이 확인하여 요리해서 배달 출발한다 (ok)

![image](https://user-images.githubusercontent.com/487999/79684170-47256a00-826a-11ea-9777-e16fafff519a.png)

    - 고객이 주문을 취소할 수 있다 (ok)
    - 주문이 취소되면 배달이 취소된다 (ok)
    - 고객이 주문상태를 중간중간 조회한다 (View-green sticker 의 추가로 ok) 
    - 주문상태가 바뀔 때 마다 카톡으로 알림을 보낸다 (?)


#### 모델수정을 통한 이벤트스토밍 완성

![image](https://user-images.githubusercontent.com/487999/79684176-4e4c7800-826a-11ea-8deb-b7b053e5d7c6.png)
    
    - 수정된 모델은 모든 요구사항을 커버함

#### 비기능 요구사항에 대한 검증

![image](https://user-images.githubusercontent.com/487999/79684184-5c9a9400-826a-11ea-8d87-2ed1e44f4562.png)

    - 마이크로 서비스를 넘나드는 시나리오에 대한 트랜잭션 처리
        - 고객 주문시 결제처리:  결제가 완료되지 않은 주문은 절대 받지 않는다는 경영자의 오랜 신념(?) 에 따라, ACID 트랜잭션 적용. 주문와료시 결제처리에 대해서는 Request-Response 방식 처리
        - 결제 완료시 점주연결 및 배송처리:  App(front) 에서 Store 마이크로서비스로 주문요청이 전달되는 과정에 있어서 Store 마이크로 서비스가 별도의 배포주기를 가지기 때문에 Eventual Consistency 방식으로 트랜잭션 처리함.
        - 나머지 모든 inter-microservice 트랜잭션: 주문상태, 배달상태 등 모든 이벤트에 대해 카톡을 처리하는 등, 데이터 일관성의 시점이 크리티컬하지 않은 모든 경우가 대부분이라 판단, Eventual Consistency 를 기본으로 채택함.

 

### 헥사고날 아키텍처 다이어그램 도출
    
![image](https://user-images.githubusercontent.com/487999/79684772-eba9ab00-826e-11ea-9405-17e2bf39ec76.png)


    - Chris Richardson, MSA Patterns 참고하여 Inbound adaptor와 Outbound adaptor를 구분함
    - 호출관계에서 PubSub 과 Req/Resp 를 구분함
    - 서브 도메인과 바운디드 컨텍스트의 분리:  각 팀의 KPI 별로 아래와 같이 관심 구현 스토리를 나눠가짐


# 구현

분석/설계 단계에서 도출된 헥사고날 아키텍처에 따라, 각 BC별로 대변되는 마이크로 서비스들을 스프링부트와 파이선으로 구현하였다. 구현한 각 서비스를 로컬에서 실행하는 방법은 아래와 같다 (각자의 포트넘버는 8081 ~ 808n 이다)

```
cd app
mvn spring-boot:run

cd pay
mvn spring-boot:run 

cd store
mvn spring-boot:run  

cd customer
python policy-handler.py 
```

### DDD 의 적용

- 각 서비스내에 도출된 핵심 Aggregate Root 객체를 Entity 로 선언하였다: (예시는 pay 마이크로 서비스). 이때 가능한 현업에서 사용하는 언어 (유비쿼터스 랭귀지)를 그대로 사용하려고 노력했다. 하지만, 일부 구현에 있어서 영문이 아닌 경우는 실행이 불가능한 경우가 있기 때문에 계속 사용할 방법은 아닌것 같다. (Maven pom.xml, Kafka의 topic id, FeignClient 의 서비스 id 등은 한글로 식별자를 사용하는 경우 오류가 발생하는 것을 확인하였다)

```
package fooddelivery;

import javax.persistence.*;
import org.springframework.beans.BeanUtils;
import java.util.List;

@Entity
@Table(name="결제이력_table")
public class 결제이력 {

    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    private String orderId;
    private Double 금액;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }
    public String getOrderId() {
        return orderId;
    }

    public void setOrderId(String orderId) {
        this.orderId = orderId;
    }
    public Double get금액() {
        return 금액;
    }

    public void set금액(Double 금액) {
        this.금액 = 금액;
    }

}

```
- Entity Pattern 과 Repository Pattern 을 적용하여 JPA 를 통하여 다양한 데이터소스 유형 (RDB or NoSQL) 에 대한 별도의 처리가 없도록 데이터 접근 어댑터를 자동 생성하기 위하여 Spring Data REST 의 RestRepository 를 적용하였다
```
package fooddelivery;

import org.springframework.data.repository.PagingAndSortingRepository;

public interface 결제이력Repository extends PagingAndSortingRepository<결제이력, Long>{
}
```
- 적용 후 REST API 의 테스트
```
# app 서비스의 주문처리
http localhost:8081/orders item="통닭"

# store 서비스의 배달처리
http localhost:8083/주문처리s orderId=1

# 주문 상태 확인
http localhost:8081/orders/1

```


### 폴리글랏 퍼시스턴스

앱프런트 (app) 는 서비스 특성상 많은 사용자의 유입과 상품 정보의 다양한 콘텐츠를 저장해야 하는 특징으로 인해 RDB 보다는 Document DB / NoSQL 계열의 데이터베이스인 Mongo DB 를 사용하기로 하였다. 이를 위해 order 의 선언에는 @Entity 가 아닌 @Document 로 마킹되었으며, 별다른 작업없이 기존의 Entity Pattern 과 Repository Pattern 적용과 데이터베이스 제품의 설정 (application.yml) 만으로 MongoDB 에 부착시켰다

```
# Order.java

package fooddelivery;

@Document
public class Order {

    private String id; // mongo db 적용시엔 id 는 고정값으로 key가 자동 발급되는 필드기 때문에 @Id 나 @GeneratedValue 를 주지 않아도 된다.
    private String item;
    private Integer 수량;

}


# 주문Repository.java
package fooddelivery;

public interface 주문Repository extends JpaRepository<Order, UUID>{
}

# application.yml

  data:
    mongodb:
      host: mongodb.default.svc.cluster.local
    database: mongo-example

```

### 폴리글랏 프로그래밍

고객관리 서비스(customer)의 시나리오인 주문상태, 배달상태 변경에 따라 고객에게 카톡메시지 보내는 기능의 구현 파트는 해당 팀이 python 을 이용하여 구현하기로 하였다. 해당 파이썬 구현체는 각 이벤트를 수신하여 처리하는 Kafka consumer 로 구현되었고 코드는 다음과 같다:
```
from flask import Flask
from redis import Redis, RedisError
from kafka import KafkaConsumer
import os
import socket


# To consume latest messages and auto-commit offsets
consumer = KafkaConsumer('fooddelivery',
                         group_id='',
                         bootstrap_servers=['localhost:9092'])
for message in consumer:
    print ("%s:%d:%d: key=%s value=%s" % (message.topic, message.partition,
                                          message.offset, message.key,
                                          message.value))

    # 카톡호출 API
```

파이선 애플리케이션을 컴파일하고 실행하기 위한 도커파일은 아래와 같다 (운영단계에서 할일인가? 아니다 여기 까지가 개발자가 할일이다. Immutable Image):
```
FROM python:2.7-slim
WORKDIR /app
ADD . /app
RUN pip install --trusted-host pypi.python.org -r requirements.txt
ENV NAME World
EXPOSE 8090
CMD ["python", "policy-handler.py"]
```


### 동기식 호출 과 Fallback 처리

분석단계에서의 조건 중 하나로 주문(app)->결제(pay) 간의 호출은 동기식 일관성을 유지하는 트랜잭션으로 처리하기로 하였다. 호출 프로토콜은 이미 앞서 Rest Repository 에 의해 노출되어있는 REST 서비스를 FeignClient 를 이용하여 호출하도록 한다. 

- 결제서비스를 호출하기 위하여 Stub과 (FeignClient) 를 이용하여 Service 대행 인터페이스 (Proxy) 를 구현 

```
# (app) 결제이력Service.java

package fooddelivery.external;

@FeignClient(name="pay", url="http://localhost:8082")//, fallback = 결제이력ServiceFallback.class)
public interface 결제이력Service {

    @RequestMapping(method= RequestMethod.POST, path="/결제이력s")
    public void 결제(@RequestBody 결제이력 pay);

}
```

- 주문을 받은 직후(@PostPersist) 결제를 요청하도록 처리
```
# Order.java (Entity)

    @PostPersist
    public void onPostPersist(){

        fooddelivery.external.결제이력 pay = new fooddelivery.external.결제이력();
        pay.setOrderId(getOrderId());
        
        Application.applicationContext.getBean(fooddelivery.external.결제이력Service.class)
                .결제(pay);
    }
```

- 동기식 호출에서는 호출 시간에 따른 타임 커플링이 발생하며, 결제 시스템이 장애가 나면 주문도 못받는다는 것을 확인:


```
# 결제 (pay) 서비스를 잠시 내려놓음 (ctrl+c)

#주문처리
http localhost:8081/orders item=통닭 storeId=1   #Fail
http localhost:8081/orders item=피자 storeId=2   #Fail

#결제서비스 재기동
cd 결제
mvn spring-boot:run

#주문처리
http localhost:8081/orders item=통닭 storeId=1   #Success
http localhost:8081/orders item=피자 storeId=2   #Success
```

- 또한 과도한 요청시에 서비스 장애가 도미노 처럼 벌어질 수 있다. (서킷브레이커, 폴백 처리는 운영단계에서 설명한다.)



### 비동기식 호출과 Eventual Consistency


결제가 이루어진 후에 상점시스템으로 이를 알려주는 행위는 동기식이 아니라 비 동기식으로 처리하여 상점 시스템의 처리를 위하여 결제주문이 블로킹 되지 않아도록 처리한다.
 
- 이를 위하여 결제이력에 기록을 남긴 후에 곧바로 결제승인이 되었다는 도메인 이벤트를 카프카로 송출한다(Publish)
 
```
package fooddelivery;

@Entity
@Table(name="결제이력_table")
public class 결제이력 {

 ...
    @PrePersist
    public void onPrePersist(){
        결제승인됨 결제승인됨 = new 결제승인됨();
        BeanUtils.copyProperties(this, 결제승인됨);
        결제승인됨.publish();
    }

}
```
- 상점 서비스에서는 결제승인 이벤트에 대해서 이를 수신하여 자신의 정책을 처리하도록 PolicyHandler 를 구현한다:

```
package fooddelivery;

...

@Service
public class PolicyHandler{

    @StreamListener(KafkaProcessor.INPUT)
    public void whenever결제승인됨_주문정보받음(@Payload 결제승인됨 결제승인됨){

        if(결제승인됨.isMe()){
            System.out.println("##### listener 주문정보받음 : " + 결제승인됨.toJson());
            // 주문 정보를 받았으니, 요리를 슬슬 시작해야지..
            
        }
    }

}

```
실제 구현을 하자면, 카톡 등으로 점주는 노티를 받고, 요리를 마친후, 주문 상태를 UI에 입력할테니, 우선 주문정보를 DB에 받아놓은 후, 이후 처리는 해당 Aggregate 내에서 하면 되겠다.:
  
```
  @Autowired 주문관리Repository 주문관리Repository;
  
  @StreamListener(KafkaProcessor.INPUT)
  public void whenever결제승인됨_주문정보받음(@Payload 결제승인됨 결제승인됨){

      if(결제승인됨.isMe()){
          카톡전송(" 주문이 왔어요! : " + 결제승인됨.toString(), 주문.getStoreId());

          주문관리 주문 = new 주문관리();
          주문.setId(결제승인됨.getOrderId());
          주문관리Repository.save(주문);
      }
  }

```

상점 시스템은 주문/결제와 완전히 분리되어있으며, 이벤트 수신에 따라 처리되기 때문에, 상점시스템이 유지보수로 인해 잠시 내려간 상태라도 주문을 받는데 문제가 없다:
```
# 상점 서비스 (store) 를 잠시 내려놓음 (ctrl+c)

#주문처리
http localhost:8081/orders item=통닭 storeId=1   #Success
http localhost:8081/orders item=피자 storeId=2   #Success

#주문상태 확인
http localhost:8080/orders     # 주문상태 안바뀜 확인

#상점 서비스 기동
cd 상점
mvn spring-boot:run

#주문상태 확인
http localhost:8080/orders     # 모든 주문의 상태가 "배송됨"으로 확인
```


# Cloud Services Provisioning

### 적용 아키텍처 : AWS, EKS(Elastic Kubernetes Service), ECR(Elastic Container Registry), Kafka

### Control Tower 환경설정
- 생성한 MSAEz 환경을 이용하여 컨트롤타워 역할의 클라이언트 환경을 구성한다.
  - 부여받은 AWS 테넌트 계정으로 로컬에 설치된 클라이언트의 Configuration 설정 (관련 Lab 참조)
```
aws configure
AWS Access Key ID [None]:
AWS Secret Access Key [None]:
Default region name [None]: 
Default output format [None]: 
```

### 쿠버네티스 클러스터 구성
- AWS 리전에 쿠버네티스 클러스터를 생성하고, 오케스트레이션에 필요한 서버들을 초기화 한다.  (관련 Lab 참조)
- AWS 리전에 마이크로서비스별 컨테이너 레지스터리를 생성하고 DevOps Toolchain에서 이를 활용한다.
```
eksctl create cluster --name (Cluster-Name) --version 1.21 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 1 --nodes-max 3

aws eks --region (Region-Code) update-kubeconfig --name (Cluster-Name)
kubectl get all
# 클러스터 설정확인
kubectl config current-context

aws ecr create-repository \
    --repository-name (Repository-Name) \
    --image-scanning-configuration scanOnPush=true \
    --region (Region-Code)
    
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
# 설치 확인
kubectl top node
kubectl top pod    
```

### 통합 메시징 인프라 설치
- 구성된 쿠버네티스 클러스터에 통합 메시징 인프라를 아래 Commands 순서로 실행하여 설치한다.
```
helm repo add incubator https://charts.helm.sh/incubator 
helm repo update 
kubectl create ns kafka 
helm install my-kafka --namespace kafka incubator/kafka 
```

# 배포

### 적용 아키텍처 : AWS CodeBuild

### 파이프라인 생성
- 음식배달 도메인의 각 마이크로서비스의 자동 배포를 위한 DevOps Toolchain을 생성한다. 
- 또는, Mini Shopping Mall을 활용하여 각 서브 도메인의 자동 배포를 위한 DevOps Toolchain을 생성한다. 
- DevOps Toolchain구성에 사용한 CI/CD 플랫폼은 AWS Codebuild로 Pipeline build script 는 각 프로젝트 Root 폴더 buildspec.yml 리소스에 포함되었다.
* 각 마이크로서비스들은 배포된 Github의 master 브랜치에 리소스 커밋이 발생한 경우, Trigger가 동작하여 AWS Codebuild를 실행하여 무정지 배포하여 준다.
* AWS기 제공하는 캐쉬기능을 활용하여 마이크로서비스 배포에 소요되는 Lead time을 줄인다.
![image](https://user-images.githubusercontent.com/35618409/174682937-ce037bce-1851-451d-add0-00ecb04a64dc.png)

- 이때, Gateway 마이크로서비스는 외부로부터의 단일진입점 역할을 하므로, 클라우드 외부에서도 접근이 가능한 LoadBalancer 타입으로 Buildspec을 수정한다.
- Gateway 파이프라인을 수정하여 55라인 Service Buildspec에 아래처럼 type: LoadBalancer를 추가해 준다. 
```
  apiVersion: v1
  kind: Service
  metadata:
    name: $_PROJECT_NAME
    labels:
      app: $_PROJECT_NAME
  spec:
    ports:
      - port: 8080
        targetPort: 8080
    selector:
      app: $_PROJECT_NAME
    type: LoadBalancer  
```           

### 배포된 마이크로서비스 테스팅
- 테스트 전, 배포된 Gateway가 라우팅할 마이크로서비스의 Endpoint를 확인합니다.
```
kubectl get service
```
- 위 명령으로 조회된 서비스이름이 Gateway Github상에 resouces폴더 하위에 있는 application.yml에 해당되는 서비스별로 수정합니다.
```
spring:
  profiles: docker
  cloud:
    gateway:
      routes:
        - id: order
          uri: http://user##-order:8080
          predicates:
            - Path=/orders/** 
        - id: delivery
          uri: http://user##-delivery:8080
          predicates:
            - Path=/deliveries/** 
        - id: product
          uri: http://user##-product:8080
          predicates:
            - Path=/inventories/** 
        - id: frontend
          uri: http://frontend:8080
          predicates:
```
- Gateway 마이크로서비스의 배포가 다시 실행되고, 라우팅할 서비스정보가 적용됩니다.

- 배포된 마이크로서비스를 테스트하기 전, 통합 메시징 인프라(Kafka)가 설치되어 있어야 한다.
- 마이크로서비스 Test Commands (Cloud)
  - 상품등록 : http POST http://GATEWAY-EXTERNAL-IP:8080/inventories productId=1001 productName=TV stock=100 
  - 주문생성 : http POST http://GATEWAY-EXTERNAL-IP:8080/orders productId=1001 productName=TV qty=5 customerId=100
  - 주문취소 : http DELETE http://GATEWAY-EXTERNAL-IP:8080/orders/1
  - Kafka 모니터링 : kubectl -n kafka exec -ti my-kafka-0 -- /usr/bin/kafka-console-consumer --bootstrap-server my-kafka:9092 --topic mall --from-beginning
    
    
# 운영

### 동기식호출 서킷브레이킹 장애격리

* 서킷 브레이킹 프레임워크의 선택: Spring FeignClient + Hystrix 옵션을 사용하여 구현함

시나리오는 단말앱(app)-->결제(pay) 시의 연결을 RESTful Request/Response 로 연동하여 구현이 되어있고, 결제 요청이 과도할 경우 CB 를 통하여 장애격리.

- Hystrix 를 설정:  요청처리 쓰레드에서 처리시간이 610 밀리가 넘어서기 시작하여 어느정도 유지되면 CB 회로가 닫히도록 (요청을 빠르게 실패처리, 차단) 설정
```
# application.yml
feign:
  hystrix:
    enabled: true
    
hystrix:
  command:
    # 전역설정
    default:
      execution.isolation.thread.timeoutInMilliseconds: 610

```

- 피호출 서비스(결제:pay) 의 임의 부하 처리 - 400 밀리에서 증감 220 밀리 정도 왔다갔다 하게
```
# (pay) 결제이력.java (Entity)

    @PrePersist
    public void onPrePersist(){  //결제이력을 저장한 후 적당한 시간 끌기

        ...
        
        try {
            Thread.currentThread().sleep((long) (400 + Math.random() * 220));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
```

* 부하테스터 siege 툴을 통한 서킷 브레이커 동작 확인:
- 동시사용자 100명
- 60초 동안 실시

```
$ siege -c100 -t60S -r10 --content-type "application/json" 'http://localhost:8081/orders POST {"item": "chicken"}'

** SIEGE 4.0.5
** Preparing 100 concurrent users for battle.
The server is now under siege...

HTTP/1.1 201     0.68 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.68 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.70 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.70 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.73 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.75 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.77 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.97 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.81 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     0.87 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.12 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.16 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.17 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.26 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.25 secs:     207 bytes ==> POST http://localhost:8081/orders

* 요청이 과도하여 CB를 동작함 요청을 차단

HTTP/1.1 500     1.29 secs:     248 bytes ==> POST http://localhost:8081/orders   
HTTP/1.1 500     1.24 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     1.23 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     1.42 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     2.08 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.29 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     1.24 secs:     248 bytes ==> POST http://localhost:8081/orders

* 요청을 어느정도 돌려보내고나니, 기존에 밀린 일들이 처리되었고, 회로를 닫아 요청을 다시 받기 시작

HTTP/1.1 201     1.46 secs:     207 bytes ==> POST http://localhost:8081/orders  
HTTP/1.1 201     1.33 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.36 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.63 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.65 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.68 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.69 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.71 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.71 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.74 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.76 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     1.79 secs:     207 bytes ==> POST http://localhost:8081/orders

* 다시 요청이 쌓이기 시작하여 건당 처리시간이 610 밀리를 살짝 넘기기 시작 => 회로 열기 => 요청 실패처리

HTTP/1.1 500     1.93 secs:     248 bytes ==> POST http://localhost:8081/orders    
HTTP/1.1 500     1.92 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     1.93 secs:     248 bytes ==> POST http://localhost:8081/orders

* 생각보다 빨리 상태 호전됨 - (건당 (쓰레드당) 처리시간이 610 밀리 미만으로 회복) => 요청 수락

HTTP/1.1 201     2.24 secs:     207 bytes ==> POST http://localhost:8081/orders  
HTTP/1.1 201     2.32 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.16 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.19 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.19 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.19 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.21 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.29 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.30 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.38 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.59 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.61 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.62 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     2.64 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.01 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.27 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.33 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.45 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.52 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.57 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.69 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.70 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.69 secs:     207 bytes ==> POST http://localhost:8081/orders

* 이후 이러한 패턴이 계속 반복되면서 시스템은 도미노 현상이나 자원 소모의 폭주 없이 잘 운영됨


HTTP/1.1 500     4.76 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.23 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.76 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.74 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.82 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.82 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.84 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.66 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     5.03 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.22 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.19 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.18 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.69 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.65 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     5.13 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.84 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.25 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.25 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.80 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.87 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.33 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.86 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.96 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.34 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 500     4.04 secs:     248 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.50 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.95 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.54 secs:     207 bytes ==> POST http://localhost:8081/orders
HTTP/1.1 201     4.65 secs:     207 bytes ==> POST http://localhost:8081/orders


:
:

Transactions:		        1025 hits
Availability:		       63.55 %
Elapsed time:		       59.78 secs
Data transferred:	        0.34 MB
Response time:		        5.60 secs
Transaction rate:	       17.15 trans/sec
Throughput:		        0.01 MB/sec
Concurrency:		       96.02
Successful transactions:        1025
Failed transactions:	         588
Longest transaction:	        9.20
Shortest transaction:	        0.00

```
- 운영시스템은 죽지 않고 지속적으로 CB 에 의하여 적절히 회로가 열림과 닫힘이 벌어지면서 자원을 보호하고 있음을 보여줌. 하지만, 63.55% 가 성공하였고, 46%가 실패했다는 것은 고객 사용성에 있어 좋지 않기 때문에 Retry 설정과 동적 Scale out (replica의 자동적 추가,HPA) 을 통하여 시스템을 확장 해주는 후속처리가 필요.

- Retry 의 설정 (istio)
- Availability 가 높아진 것을 확인 (siege)

#### 오토스케일 아웃
- 클러스터에 Metric Server가 설치되어 있어야 한다.
- 또한, Auto Scaling 되기 위해서는 주문서비스의 배포스펙(Buildspec.yml)에 Resource Spec.이 추가되어야 한다.
- 주문 서비스 Code 저장소에서 buildspec.yml 배포스펙에 resources (맨아래에서 5줄)을 추가한다.
```
  readinessProbe:
    httpGet:
      path: /actuator/health
      port: 8080
    initialDelaySeconds: 10
    timeoutSeconds: 2
    periodSeconds: 5
    failureThreshold: 10
  livenessProbe:
    httpGet:
      path: /actuator/health
      port: 8080
    initialDelaySeconds: 120
    timeoutSeconds: 2
    periodSeconds: 5
    failureThreshold: 5
  resources:
    limits:
      cpu: 500m
    requests:
      cpu: 200m
```

- 아래 스크립트를 terminal 에 복사하여 siege 라는 Load Generator를 생성한다.
```
kubectl apply -f - <<EOF
apiVersion: v1
kind: Pod
metadata:
  name: siege
spec:
  containers:
  - name: siege
    image: apexacme/siege-nginx
EOF
```
- 생성된 siege Pod 안쪽에서 Load Generator가 정상작동하는지 확인한다.
```
kubectl exec -it siege -- /bin/bash
siege -c1 -t2S -v http://ORDER-SERVICE-NAME:8080/orders
exit
```

- 주문서비스에 대한 replica를 동적으로 늘려주도록 HPA를 설정한다. 설정은 CPU 사용량이 15프로를 넘어서면 replica 를 10개까지 늘려준다:
```
kubectl autoscale deploy ORDER-DEPLOY-NAME --min=1 --max=10 --cpu-percent=15
```

- 생성된 siege Pod 안쪽에서 주문서비스에 대해 워크로드를 30초 동안 걸어준다.
```
kubectl exec -it siege -- /bin/bash
siege -c30 -t30S -r10 --content-type "application/json" 'http://ORDER-SERVICE-NAME:8080/orders POST {"productId": "1001", "productName": "TV", "qty": "5", "customerId": "100"}'
```

- 어느정도 시간이 흐른 후 (약 30초) 스케일 아웃이 벌어지는 것을 확인할 수 있다:
```
NAME    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
order     1         1         1            1           17s
order     1         2         1            1           45s
order     1         4         1            1           1m
:
```



### 무정지 재배포

- 먼저 무정지 재배포가 100% 되는 것인지 확인하기 위해서 Autoscaler 설정을 제거함
- 테스트 개념
  - 주문 서비스에 부하량이 적고 오랫동안 테스트를 실행하는 Siege 명령을 실행한다.
  - 주문 서비스의 파이프라인이 동작하도록 한다. (새버전 배포)
  - 파이프라인 동작이 끝나면 새버전으로 배포가 이루졌음을 의미한다.
  - Siege 결과화면에서 Availability가 100% 임을 확인한다. (배포중 오류가 없었음을 증명)

- 테스트 방법
  - 배포된 Siege 컨테이너에 접속한 후, 주문서비스에 3분동안 요청을 보낸다.
```
kubectl exec -it siege -- /bin/bash
siege -c1 -t180S -v http://ORDER-SERVICE-NAME:8080/orders

** SIEGE 4.0.5
** Preparing 100 concurrent users for battle.
The server is now under siege...

HTTP/1.1 201     0.68 secs:     207 bytes ==> GET http://ORDER-SERVICE-NAME:8080/orders
HTTP/1.1 201     0.68 secs:     207 bytes ==> GET http://ORDER-SERVICE-NAME:8080/orders
HTTP/1.1 201     0.70 secs:     207 bytes ==> GET http://ORDER-SERVICE-NAME:8080/orders
HTTP/1.1 201     0.70 secs:     207 bytes ==> GET http://ORDER-SERVICE-NAME:8080/orders
:

```
  - 주문 서비스 파이프라인이 동작하도록 한다. (Git 상에서 리소스 중 일부를 수정하고 Commit 한다.)

  - 주문 서비스의 파이프라인이 동작 완료되었다. 

  - Seige 의 화면으로 넘어가서 Availability 가 100% 으로 떨어졌는지 확인
```
Transactions:		        3078 hits
Availability:		       100 %
Elapsed time:		       120 secs
Data transferred:	        0.34 MB
Response time:		        5.60 secs
Transaction rate:	       17.15 trans/sec
Throughput:		        0.01 MB/sec
Concurrency:		       96.02

```

배포기간 동안 클라이언트의 Siege Availability 가 100% 수치는 무정지 재배포가 성공적으로 이루어졌음을 암시한다.

### Service Mesh 

#### 적용 아키텍처 : Istio 1.11.3

* Service Mesh(Istio)를 클러스터에 설치하여 Food Delivery 마이크로서비스간 Service Mesh를 오케스트레이션 한다.
```
curl -L https://istio.io/downloadIstio | ISTIO_VERSION=1.11.3 TARGET_ARCH=x86_64 sh -
export PATH=$PWD/bin:$PATH
istioctl install --set profile=demo -y
✔ Istio core installed
✔ Istiod installed
✔ Egress gateways installed
✔ Ingress gateways installed
✔ Installation complete
...
```

* Envoy 사이드카를 생성하는 Pod 들에 자동적으로 주입하게 하기 위해 다음의 설정을 추가한다
```
kubectl label namespace default istio-injection=enabled
```

* Istio Sidecar Injection이 설정된 네임스페이스에 마이크로 서비스들을 재배포하여 Service Mesh를 적용한다.
* 재배포가 완료된 후, 리소스를 확인하여 Envoy Proxy인 사이드카가 적용되었음을 확인한다.
```
kubectl get all
```

### 통합 Monitoring

#### 적용 아키텍처 : Prometheus, Grafana

- Istio Addon서버 중 모니터링 스텍을 활용, 쿠버네티스 클러스터 및 음식배달 마이크로서비스 통합 모니터링이 가능한 환경을 구축한다. 
- Istio 버전 '1.11.3'이 설치된 경우,
```
cd istio-1.11.3
kubectl apply -f samples/addons
kubectl get svc -n istio-system
```
* Grafana Service(On istio-system namespace)를 외부접근 가능하게 설정 후 Endpoint에 접속하여 Dashboard를 설정하고 확인

![image](https://user-images.githubusercontent.com/35618409/171767588-2db72ec3-8515-4fa5-989c-9b04ad667a87.png)

* 쿠버네티스 클러스터 모니터링 설정
  * (Left Import 메뉴에서)
  * uid 에 315번 입력 후 Load 클릭
  * 하단 prometheus 에서 prometheus 선택
  * 선택 후 import 클릭


* 마이크로서비스 모니터링 설정
  * (Left Import 메뉴에서)
  * uid 에 7636번 입력 후 Load 클릭
  * 하단 prometheus 에서 prometheus 선택
  * 선택 후 import 클릭

### 통합 Logging

#### 적용 아키텍처 : Elastic Search, Fluentd, Kibana

* 음식배달 마이크로서비스들의 로그를 중앙에서 통합 조회하기 위해 EFK(Elasticsearch, Fluentd, Kibana) 스텍을 클러스터에 설치한다. 
```
helm repo add elastic https://helm.elastic.co
helm repo update
kubectl create namespace elastic
helm install elasticsearch elastic/elasticsearch -n elastic
helm install kibana elastic/kibana -n elastic
```

* 음식배달 마이크로서비스가 설치된 네임스페이스 정보(ex. shop)를 활용하여 Fluentd 데몬의 환경설정에 반영한다.

* kibana Service(On elastic namespace)를 외부접근 가능하게 설정 후 Endpoint에 접속하여 Logging filter를 설정하고 확인

- Kibana 접속 후, Management > Stack Management를 선택한다.
![image](https://user-images.githubusercontent.com/35618409/160071078-bd7caa6f-3532-45ba-bb13-a05198aac002.png)

- Kibana > Index Patterns 화면의 Search 필드에 'fluent-shop*'을 입력하고 Time field 에 @timestamp 를 선택하여 수집된 데이터를 인덱싱한다.
![image](https://user-images.githubusercontent.com/35618409/160071303-0b7b9e35-9f2a-490f-9368-fe5e2312c4b8.png)

- Analytics > Discover 를 눌러 조회페이지를 오픈한다.
![image](https://user-images.githubusercontent.com/35618409/160072101-a5fb8e02-913a-4cbb-bbc5-1ba2fd6a97c7.png)

- 'Add filter' 에서 'kubernetes.namespace.name is shop'으로 조건을 지정한다.
![image](https://user-images.githubusercontent.com/35618409/160072511-b79a1933-ff0d-4476-a1d3-4cf5bf081a24.png)

- 조회할 Date Range에 인덱싱된 shop 네임스페이스 data가 존재하면  아래처럼 로그가 나타난다.
![image](https://user-images.githubusercontent.com/35618409/160073584-24ab9fb6-b341-46e1-b7f3-0f5b8dce2761.png)


### 이벤트 스트리밍 플랫폼 Install 및 Monitoring

#### 적용 아키텍처 : Kafka, Kafka UI

* 음식배달 마이크로서비스들간 런타임-디커플링을 위한 비동기통신에 사용될 Kafka 설치 및 모니터링을 위해 GUI기반 Kafka Dashboard를 설치한다.
* Install Kafka
  - kafka를 default 네임스페이스에 설치할 경우,
```
helm repo update
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-kafka bitnami/kafka
```

  - Install - kafka를 kafka 네임스페이스에 설치할 경우,
```
helm repo update
helm repo add bitnami https://charts.bitnami.com/bitnami
kubectl create ns kafka
helm install my-kafka bitnami/kafka --namespace kafka
``` 

* Install Kafka Monitor
```
helm repo add kafka-ui https://provectus.github.io/kafka-ui
helm repo update
helm install kafka-ui kafka-ui/kafka-ui  --namespace=kafka \
--set envs.config.KAFKA_CLUSTERS_0_NAME=shop-Kafka \
--set envs.config.KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS=my-kafka:9092 \
--set envs.config.KAFKA_CLUSTERS_0_ZOOKEEPER=my-kafka-zookeeper:2181
```

* 설치 결과, kafka-ui 서비스와 Pod가 설치한 네임스페이스에서 조회된다.
 ![image](https://user-images.githubusercontent.com/35618409/171811015-a18184b3-df38-4ced-be1e-82ee0105fc44.png)
 
* kafka-ui Service를 외부접근 가능하게 설정 후 Endpoint에 접속하여 Kafka Dashboard를 확인
![image](https://user-images.githubusercontent.com/35618409/171811367-7b25445b-4afc-4908-a811-4ad6f536ebf8.png)

## 신규 개발 조직의 추가

  ![image](https://user-images.githubusercontent.com/487999/79684133-1d6c4300-826a-11ea-94a2-602e61814ebf.png)


### 마케팅팀의 추가
    - KPI: 신규 고객의 유입률 증대와 기존 고객의 충성도 향상
    - 구현계획 마이크로 서비스: 기존 customer 마이크로 서비스를 인수하며, 고객에 음식 및 맛집 추천 서비스 등을 제공할 예정

### 이벤트 스토밍 
    ![image](https://user-images.githubusercontent.com/487999/79685356-2b729180-8273-11ea-9361-a434065f2249.png)


### 헥사고날 아키텍처 변화 

![image](https://user-images.githubusercontent.com/487999/79685243-1d704100-8272-11ea-8ef6-f4869c509996.png)

### 구현  

기존의 마이크로 서비스에 수정을 발생시키지 않도록 Inbund 요청을 REST 가 아닌 Event 를 Subscribe 하는 방식으로 구현. 기존 마이크로 서비스에 대하여 아키텍처나 기존 마이크로 서비스들의 데이터베이스 구조와 관계없이 추가됨. 

### 운영과 Retirement

Request/Response 방식으로 구현하지 않았기 때문에 서비스가 더이상 불필요해져도 Deployment 에서 제거되면 기존 마이크로 서비스에 어떤 영향도 주지 않음.

* [비교] 결제 (pay) 마이크로서비스의 경우 API 변화나 Retire 시에 app(주문) 마이크로 서비스의 변경을 초래함:

예) API 변화시
```
# Order.java (Entity)

    @PostPersist
    public void onPostPersist(){

        fooddelivery.external.결제이력 pay = new fooddelivery.external.결제이력();
        pay.setOrderId(getOrderId());
        
        Application.applicationContext.getBean(fooddelivery.external.결제이력Service.class)
                .결제(pay);

                --> 

        Application.applicationContext.getBean(fooddelivery.external.결제이력Service.class)
                .결제2(pay);

    }
```

예) Retire 시
```
# Order.java (Entity)

    @PostPersist
    public void onPostPersist(){

        /**
        fooddelivery.external.결제이력 pay = new fooddelivery.external.결제이력();
        pay.setOrderId(getOrderId());
        
        Application.applicationContext.getBean(fooddelivery.external.결제이력Service.class)
                .결제(pay);

        **/
    }
```


