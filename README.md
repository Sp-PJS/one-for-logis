# MSA 기반 B2B 대상 물류 관리 및 배송 플랫폼 - One For logis 🚚
<img src="./assets/one-for-logis-banner.png" alt="one-for-logis" width="100%" />

<br>
<br>

# 📦 Project Overview (프로젝트 개요)
전국 허브 간 물류 이동 경로를 관리하고, 배송 상태를 실시간 추적할 수 있는 MSA 기반 물류 관리 플랫폼. <br>
허브 간 최단 경로 탐색(다익스트라 적용), Redis 캐싱 및 Kafka 이벤트 기반 구조를 통해 안정성과 확장성을 확보합니다.

## 🗓️ 개발 기간
2025.10.31 ~ 2025.11.13

</br>

# ⚒️ Technology Stack (기술 스택)

[![Java](https://img.shields.io/badge/Java%2017-007396?style=for-the-badge&logo=java&logoColor=white)](https://www.java.com/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot%203.3.2-6DB33F?style=for-the-badge&logo=spring&logoColor=white)](https://spring.io/projects/spring-boot)
[![Spring Data JPA](https://img.shields.io/badge/Spring%20Data%20JPA-228B22?style=for-the-badge&logo=java&logoColor=white)](https://spring.io/projects/spring-data-jpa)
[![Spring Security](https://img.shields.io/badge/Spring%20Security-6DB33F?style=for-the-badge&logo=spring&logoColor=white)](https://spring.io/projects/spring-security)
[![Kafka](https://img.shields.io/badge/Kafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white)](https://kafka.apache.org/)
[![Redis](https://img.shields.io/badge/Redis-DC382D?style=for-the-badge&logo=redis&logoColor=white)](https://redis.io/)

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL%2017-316192?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)

[![IntelliJ IDEA](https://img.shields.io/badge/IntelliJ%20IDEA-000000?style=for-the-badge&logo=intellijidea&logoColor=white)](https://www.jetbrains.com/idea/)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)
[![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)](https://swagger.io/)
[![Slack](https://img.shields.io/badge/Slack-4A154B?style=for-the-badge&logo=slack&logoColor=white)](https://slack.com/)
[![Notion](https://img.shields.io/badge/Notion-000000?style=for-the-badge&logo=notion&logoColor=white)](https://www.notion.so/)


<br>
<br>

# ⭐ Team Members (팀원)
| name | role |  담당 파트   |                                                                       Github                                                                        |
|:----:|:----:|:--------:|:---------------------------------------------------------------------------------------------------------------------------------------------------:|
| 김다윤  |  팀장  | 허브, 공통모듈 | <a href="https://github.com/dyun23"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a> |
| 박근용  |  멤버  | Slack 알림 | <a href="https://github.com/GoodNyong"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a>    |
| 박진성  |  멤버  |    회원    |  <a href="https://github.com/Sp-PJS"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a>  |
| 설정아  |  멤버  |    주문    |  <a href="https://github.com/AlkongDalkonge"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a>  |
| 안소나  |  멤버  |  업체, 상품  |  <a href="https://github.com/sonaanweb"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a>   |
| 이다인  |  멤버  |    배송    |  <a href="https://github.com/dain391"><img src="https://img.shields.io/badge/Github-181717?style=for-the-badge&logo=Github&logoColor=white"></a>    |

<br>
<br>

# 🗝️ Key Features (주요 기능)

- **회원**
    - 사용자는 회원 가입 및 로그인을 진행할 수 있습니다.
    - 회원가입이 성공하고 로그인 요청이 성공하면 서버에서 Access Token, Refresh Token을 발급하여 반환합니다.
    - 인증, 인가
      - AccessToken은 클라이언트 Header에 저장되고, Refresh Token은 Redis 및 HttpOnly 쿠키에 저장되어 관리되며, `게이트웨이의 GlobalFilter`를 통한 인증, 검증을 통해 통합 인증 절차를 수행합니다.
    - 관리자 전용 조회 기능은 Redis 캐싱을 활용해 성능을 최적화하였습니다.
    - Netty기반(비동기, 논블로킹)의 WebFlux & GlobalFilter와 Spring Security ThreadLocal(동기, 블로킹) 구조적인 차이로 인해 인증 정보가 유실되거나 오염되는 현상이 발생하여 GlobalFilter를 WebFilter로 변환하는 타입 브릿지를 통해 문제를 해결하였습니다.
    Spring Security는 기본적으로 ROLE_ 접두사를 항상 붙인 상태로 권한체크를 내부적으로 진행하므로 해당 Authentication 인증 객체를 생성할 때, ROLE_ 접두사를 제거하여 인증 문제를 해결했습니다.
    - 로깅이 중복으로 출력되는 현상을 ConcurrentHashMap에 로깅 정보를 저장하여 중복을 확인하여 1회만 로깅되도록 수정했습니다.

- **허브**
    - 전국 물류망을 구성하는 핵심 거점 정보를 관리합니다. (허브의 이름, 주소, 위도/경도 등 기본 정보 등록·수정·삭제)
    - 허브 간 이동 경로 관리
      - Hub to Hub Relay 구조로 이루어지기 때문에, 허브와 함께 허브 간 경로(Route) 도 통합 관리합니다.
      - 허브 경로에는 거리, 평균 소요 시간, 연결된 허브 ID 등이 포함되며 이 정보로 전체 허브 네트워크를 그래프 구조로 구성합니다.
      - 구성된 그래프는 Redis에 캐싱되어 `Dijkstra 알고리즘`을 통한 최단 거리·최소 시간 경로 탐색에 활용됩니다.
      - 허브 또는 경로 데이터가 변경되면 네트워크 그래프도 자동 반영되어, 이후 배송 경로 계산 시 항상 최신 상태를 기반으로 처리됩니다.

- **업체**
    - MASTER, HUB_MANAGER 권한을 가진 사용자는 업체 등록, 수정, 삭제, 조회 기능을 수행할 수 있습니다.
    - 업체 타입은 생산/공급 업체로 분리됩니다.
    - 사용자는 업체 조회 시 업체 이름을 기준으로 검색할 수 있습니다.

- **상품**
    - MASTER, HUB_MANAGER, COMPANY_MANAGER 권한 사용자는 상품 등록, 수정, 조회 기능을 사용할 수 있으며, 삭제는 COMPANY_MANAGER를 제외한 권한에서만 가능합니다.
      - 상품 조회 시 상품명을 기준으로 검색을 지원합니다.
      - 필수 속성 검증을 통해 상품 등록 시 데이터 정합성을 보장합니다.
    - 재고 연동
      - 주문 서비스와의 연동을 위해 재고 차감 및 증가 기능을 제공하는 internal API를 구성하여 안정적이고 일관성 있는 재고 관리를 지원하도록 구현하였습니다.

- **주문**
    - 주문 생성: 업체, 상품 정보로 주문을 생성하며 초기 상태는 요청(REQUESTED) 상태로 설정되며, 상태 이력(History)을 자동으로 기록합니다.
      - (REQUESTED → CONFIRMED → IN_PROGRESS → COMPLETED → CANCELED)
    - 주문 단건 조회 및 목록 조회로 주문 기본 정보를 확인할 수 있으며, 주문 수정은 변경 가능한 기본 정보, 주문 상태 변경을 분리해 관리하였습니다.
    - 주문 상태 이력 조회
      - 해당 주문의 모든 상태 변경 내역을 전체 조회할 수 있습니다.
    - 주문 취소 시에는 취소 사유와 함께, 상태 이력을 저장합니다.

- **배송**
    - 배송 생성: 주문 생성 `Kafka 이벤트`를 구독하여 배송 정보를 자동으로 생성합니다.
      - (출발/도착 허브 정보, 수령자 정보를 함께 저장하며 기본 상태는 허브 대기(WAITING_AT_HUB)로 설정됩니다.)
    - 배송 담당자 관리
      - 허브 별 배송 담당자 등록 및 조회 기능을 제공합니다. 배송 상태가 허브 대기일 때만 담당자 배정이 가능합니다.
    - 배송 상태 흐름은 허브 대기 -> 허브 이동 -> 목적지 도착 -> 배송 중 -> 완료로 이루어지며, 잘못된 상태 변경, 또는 비정상 요청 시 표준화된 예외를 반환합니다.

- **알림 서비스**
    - AI 자연어 분석을 통해 출발 시간을 계산하고, Gemini API를 이용해 사용자에게 도착 기한, 출발/목적지 허브 정보 기반으로 최적의 출발 시간을 산출하여 제공합니다.
    - 알림 발송
      - 자동 및 수동 알림 발송 지원, REST API, Kafka 이벤트를 통한 발송 처리
      - `Kafka`: 주문 생성 및 배송 상태 변경 이벤트를 구독하며 멱등성 보장 구현
    - 안정성 보장
      - FeignClient Fallback을 적용해 user-service 연동 시 발생 가능한 NPE 위험 제거
      - Circuit Breaker와 Fallback 패턴을 적용해 외부 API 장애 시 서비스 안정성을 확보하고, 스냅샷 패턴으로 발송 시점의 발신자 정보를 저장해 데이터 정합성 보장
    - 조회: 발송된 알림(Slack/Gemini API)의 상세 조회를 포함해 페이징, 필터링하여 조회할 수 있습니다.
    - 통계 및 모니터링: 알림 통계(일 별/기간 별 발송 성공, 실패율), API 통계(성공률, 평균 응답시간, 총 비용)

<br>
<br>

# 🌐 ERD 및 시스템 아키텍처
<img src="./assets/one-for-logis-erd.png" alt="one-for-logis" width="100%" />
<img src="./assets/one-for-logis-archi.png" alt="one-for-logis" width="100%" />


# 📂 Project Structure (프로젝트 구조)
```plaintext
각 서비스 모듈은 4계층 레이어드 아키텍처를 기본으로 합니다.
└─main
    ├─java
    │  └─com
    │      └─oneforlogis
    │          └─service-module
    │              ├─application
    │              │  └─dto
    │              │      ├─request
    │              │      └─response
    │              ├─domain
    │              │  ├─model
    │              │  └─repository
    │              ├─global
    │              ├─infrastructure
    │              │  ├─client
    │              │  │  └─dto
    │              │  └─persistence
    │              └─presentation
    │                  └─controller
    │                      └─internal
    │                          └─dto
    └─resources
```

<br>
<br>

# ✈️ Development Workflow (개발 워크플로우)

## 브랜치 전략, 커밋 컨벤션
Git Flow를 기반으로 하며, 다음과 같은 브랜치를 사용합니다.

- **Branch**
    - **전략**
  
      | Branch Type | Description                                       |
      |-------------|---------------------------------------------------|
      | `dev`       | 주요 개발 branch, `main`으로 merge 전 거치는 branch |
      | `feature`   | 각자 개발할 branch, 기능 단위로 생성하기, 할 일 issue 등록 후 branch 생성 및 작업 |

    - **네이밍**
        - `{header}/#{issue number}-feature`
        - 예) `feature/#issueNum-create-user`


- **커밋 메시지 규칙**
    ```bash
    > type: 기능 요약

      - chore: 내부 파일 수정
      - feat: 새로운 기능 구현
      - add: feat 이외의 부수적인 코드 추가, 라이브러리 추가, 새로운 파일 생성 시
      - fix: 코드 수정, 버그, 오류 해결
      - del: 쓸모없는 코드 삭제
      - docs: README나 WIKI 등의 문서 개정
      - move: 프로젝트 내 파일이나 코드의 이동
      - rename: 파일 이름의 변경
      - merge: 다른 브랜치를 merge하는 경우
      - style: 코드가 아닌 스타일 변경을 하는 경우
      - init: Initial commit을 하는 경우
      - refactor: 로직은 변경 없는 클린 코드를 위한 코드 수정

      ex) feat: 게시글 목록 조회 API 구현
    ```

<br>
<br>

# 📄 기술 문서
[API 명세서 보기](https://teamsparta.notion.site/29d2dc3ef5148147b0b4ddc502359f62?v=2a02dc3ef51480b183d6000c635a5567)

[테이블 명세서 보기](https://teamsparta.notion.site/2a12dc3ef51480609d8be79e3a0c82ad)
