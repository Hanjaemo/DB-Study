# NoSQL

## RDB의 단점

### 경직된 스키마

- RDB같은 경우에는 기능이 추가될 때, DB스키마가 변경되는 경우가 많음

    - column이 하나 추가되던지, 테이블이 하나 생성되던지..

    - 데이터가 많은 테이블에서 column을 추가하는 것은 굉장히 부담됨.

=> 유연한 확장성이 부족함

### 과도한 조인과 성능 하락

- 보통 RDB는 정규화를 통해 중복을 최대한 제거하는 방향으로 사용함.

    - 이렇게 함으로써, 일관성과 무결성을 유지하는 것이 RDB의 컨셉

- 그런데 통합된 데이터를 한번에 읽어오려면 JOIN이 여러번 필요하게 됨.

- 이 과정에서 성능하락 발생

### Scale-Out 이 편하지 않음

- 서비스 운영중에 Replication을 하거나, 샤딩을 하게 될 때 데이터 옮기는 작업이 어려움

### ACID가 성능에 미치는 영향

- ACID를 보장하는 과정에서 DB서버의 Performance를 희생함

## NoSQL의 등장 배경

- 인터넷보급화에 따른 사용자 대폭 증가, SNS와 같은 글로벌서비스 등장

- high-throughput(높은 처리량)이 요구됨.

- low-latency 요구됨

- 비정형 데이터가 증가함. 스키마의 틀에 맞추어 저장하기 어려운 데이터들이 많아짐.

## NoSQL의 일반적인 특징

### Flexible Schema

**[ 예시 ]**

- RDB
    ```SQL
    create table student (
        id INT PRIMARY KEY,
        name VARCHAR(20),
        ...
    )
    ```

- mongoDB

    ```java
    db.createCollection("student")
    db.student.insertOne({
        name: "park"
    })
    db.student.find({name: "park"})
    db.student.find({})
    ```
    ```java
    name : "hope",
    address : {
        country : "Korea",
        state : "Seoul",
        city : "Gangnam-gu",
        street : "blurblur"
    },
    certificate : ["AWS solution architect"]
    ```

대신에 NoSQL은 application레벨에서 스키마 관리가 필요하다.

### 중복 허용

![alt text](img/8/image-7.png)

- 중복을 허용함으로써 join을 회피함.

- 대신 application 레벨에서 중복된 데이터들이 모두 최신 상태를 유지할 수 있도록 관리가 필요함

### Scale Out

- 스케일아웃이 쉬움

- 그래서 보통 서버 여러 대로 하나의 클러스터를 구성하여 사용함

### High-Throughput, Low-Latency

- 그러나 금융 시스템처럼 일관성이 중요한 환경에서는 사용하기가 조심스러움

