# E-Commerce Service

본 문서는 e-커머스 상품 주문 서비스의 요구사항, 아키텍처, 주요 설계 결정 사항을 기술하는 최상위 문서입니다.
(참고 링크 : https://github.com/elyo9381/hhplus-backend-study/blob/feature/server-java/server-java/README.md)

## 1. 개요 (Overview)

본 프로젝트는 사용자가 상품을 조회하고, 포인트를 이용해 주문 및 결제하는 e-커머스 백엔드 서비스를 구축하는 것을 목표로 합니다. 대용량 트래픽 환경에서도 안정적인 서비스를 제공하기 위해 동시성 제어와 확장성 있는 아키텍처 설계를 핵심 과제로 삼습니다.

## 2. 주요 기능 요구사항 (Key Features)

- **상품 조회**: 상품의 정보(가격, 재고 등)를 조회합니다.
- **주문 및 결제**: 여러 상품을 동시에 주문하고, 사용자의 포인트 잔액으로 결제합니다.
- **포인트 관리**: 사용자의 포인트를 충전하고 잔액을 조회합니다.
- **프로모션**: 선착순으로 쿠폰을 발급받을 수 있습니다.
- **데이터 분석**: 판매 실적을 기반으로 인기 상품을 조회할 수 있습니다.

> 상세 요구사항은 [요구사항.md](./요구사항.md) 문서에서 확인할 수 있습니다.

## 3. 설계 문서 (Design Documents)

본 프로젝트의 상세 설계 내용은 아래 문서에서 확인할 수 있습니다.

- **[API 명세서](./docs/api-specs.yaml)**: API 엔드포인트, 요청/응답 형식, 인증 및 에러 코드를 정의합니다.
- **[ERD (데이터 모델)](./docs/erd.md)**: 데이터베이스 엔티티와 관계, 제약 조건 등을 기술합니다.
- **[인프라 아키텍처](./docs/infrastructure.md)**: 시스템을 구성하는 인프라 요소와 그 역할, 선택 이유를 설명합니다.
- **[시퀀스 다이어그램](./docs/sequence-diagrams.md)**: 주요 기능의 처리 흐름을 시각적으로 표현합니다.
- **[아키텍처 결정 기록 (ADRs)](./docs/adr/README.md)**: 주요 설계 결정과 그 배경을 기록합니다.

## 4. 프로젝트 실행 및 테스트

### Prerequisites
- Java 17
- Gradle 8.x

### Build
```bash
./gradlew clean build
```

### Run Tests
```bash
./gradlew test
```

### Run Application
```bash
java -jar build/libs/server-java-0.0.1-SNAPSHOT.jar
```

