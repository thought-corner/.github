<h1 align="center">🧩 Thought Corner</h1>
<p align="center">생각을 코드로 정리하는 백엔드 개발자입니다.</p>
<p align="center">"왜 이렇게 동작하는가"를 직접 구현해보며 이해하고,<br>
부분이 아닌 전체 시스템의 상호작용 관점에서 문제를 바라봅니다.</p>

<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=java,kotlin"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=spring,springsecurity,springdatajpa,springbatch,junit"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=kafka,rabbitmq,grpc,graphql"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=mysql,postgresql,redis"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=aws,docker,kubernetes,terraform"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=jenkins,githubactions"/>
  </a>
</p>

---

## 🔭 Interests

**테스트와 코드 품질**

- 커버리지 숫자보다 "이 테스트가 실제로 회귀를 잡아내는가"를 먼저 묻습니다. 통과하는 테스트와 신뢰할 수 있는 테스트는 다르다고 생각합니다.
- 좋은 코드는 결국 테스트하기 쉬운 코드라고 봅니다. 테스트가 어렵다면 설계에 원인이 있는 건 아닌지 먼저 의심합니다.
- 모든 상황에 통용되는 정답은 없다고 생각해서, 트레이드오프를 명시적으로 비교하고 "왜 이 방식을 선택했는가"를 남기는 습관을 들이고 있습니다.
- 지금 당장 필요하지 않은 유연성을 미리 설계하기보다, 실제로 바뀔 때 바꿀 수 있는 구조를 만드는 쪽에 더 관심이 있습니다.

**기술보다 본질**

- 새로운 기술 자체보다, 그 기술이 해결하려는 문제의 본질에 더 관심이 있습니다.
- 도구는 계속 바뀌지만 문제의 본질은 잘 바뀌지 않는다고 생각해서, 특정 스택이나 트렌드보다 원리를 이해하는 데 더 시간을 씁니다.

**기획을 설계로 옮기는 과정**

- 주어진 기획을 그대로 구현하기보다, 기획이 어떤 문제를 풀려는 건지부터 되짚어보고 그 의도가 설계에 잘 반영됐는지 확인합니다.
- 요구사항 하나가 코드 어디까지 영향을 미치는지, 나중에 요구사항이 바뀌면 어디가 흔들릴지를 먼저 그려보는 편입니다.
- 기획과 구현 사이의 간극은 결국 커뮤니케이션의 문제라고 생각해서, 애매한 지점은 넘어가지 않고 먼저 확인하려고 합니다.

**실패를 가정한 설계**

- 정상 동작보다 실패했을 때 무엇이 깨지는지를 먼저 그려봅니다. 장애가 어디서 나는지보다, 그 장애가 어디까지 전파되는지가 더 중요하다고 생각합니다.
- 정합성을 얼마나 강하게 보장할지는 항상 트레이드오프라, "완벽한 일관성"을 기본값으로 두기보다 도메인이 실제로 요구하는 수준이 어디인지부터 확인합니다.
- 재시도, 폴백, 격리 같은 대응은 결국 실패를 없애는 게 아니라 실패의 반경을 줄이는 일이라고 생각해서, 무엇을 희생하고 무엇을 지킬지 먼저 정하고 설계합니다.

## 🏗️ Featured

### [distributed-transactions](https://github.com/thought-corner/distributed-transactions) — 분산 트랜잭션 패턴 4종 비교 구현

**단일 ACID 트랜잭션 → TCC → Saga Choreography(Kafka) → Saga Orchestration을 같은 주문 도메인으로 구현하고 정합성·결합도 트레이드오프를 문서화했습니다.**
- requestId 중복 체크 + Redis 분산락 + 주문 상태머신으로 **"동일 주문 정확히 1회 실행" 보장**
  — Testcontainers 기반 E2E·동시성 테스트로 검증
- TCC는 가예약/확정 2단계 분리로 Try 실패 시 자원 소비 0,
  `@Version` 낙관적 잠금 + `@Retryable` 재시도까지 구현
- Kafka 이벤트는 트랜잭션 `afterCommit()` 이후에만 발행해 유령 이벤트 방지

### [order-system](https://github.com/thought-corner/order-system) — RabbitMQ 비동기 주문 시스템

**주문 API가 메시지 발행 즉시 응답하고, 컨슈머가 재고 차감·주문 생성을 처리하는 구조입니다.**
- MySQL Named Lock(주문 단위) + JPA 비관적 락(재고 단위) 이중 락으로 동시성 제어
- **k6 부하 테스트: 4,000 VUs에서 TPS 2.23k, 실패 0건, 재고 정합성 오차 0** 실측 기록
- Dead Letter Queue + 재발행으로 실패 메시지 유실 방지, prefetch·컨슈머 수 튜닝 과정 문서화

### [query-performance](https://github.com/thought-corner/query-performance) — 대용량 조회 성능 개선 (SQL 튜닝 → Redis 캐싱)

**30만 건 데이터에서 풀 테이블 스캔으로 최대 14초 걸리던 필터링 조회를 단계적으로 개선했습니다.**
- EXPLAIN으로 풀 테이블 스캔(type: ALL) 확인 → 가격 인덱스 적용으로 조회 시간
  **70.5% 단축** (1s → 0.295s)
- **인덱스를 생성했는데도 동작하지 않던 원인 분석** — 낮은 카디널리티(LocalDate 일 단위
  저장 + 중복도 높은 더미 데이터)로 옵티마이저가 인덱스를 버리는 상황을 규명하고,
  타입·데이터 분포를 바로잡아 **82.25% 단축** (2s → 0.355s)
- 시스템 변경 없는 SQL 튜닝을 먼저, 그 위에 Redis Cache-Aside를 얹어 **추가 45.7% 개선**
  (1.01s → 0.548s) — 캐시-DB 정합성 한계까지 문서화

### [realtime-chat-platform](https://github.com/thought-corner/realtime-chat-platform) — 다중 인스턴스 실시간 채팅

**Kotlin + STOMP WebSocket 채팅을 Redis Pub/Sub 브로커로 서버 여러 대에서도 동작하게 구성했습니다.**
- 로컬 SimpleBroker의 한계를 Redis 채널 릴레이로 해결 (인스턴스 간 메시지 라우팅)
- JWT를 STOMP 채널 인터셉터에서 검증 — HTTP 필터 체인 밖의 WebSocket 인가 처리
- 메시지별·참여자별 읽음 상태를 저장하고 실시간 전파 (카카오톡식 안 읽은 인원 표시)

## 🧪 Playground

| 분류 | 저장소 | 한 줄 요약 |
|--------|--------|-----------|
| Software Design | [law-of-demeter](https://github.com/thought-corner/law-of-demeter) | Tell-Don't-Ask로 getter를 제거한 캡슐화 도메인 설계 — 협력을 메시지로 강제하고 테스트 31개로 회귀 방지 |
| Software Design | [monolithic-architecture](https://github.com/thought-corner/monolithic-architecture) | 헥사고날·DDD로 도메인 경계를 분리한 설계 — 포트&어댑터·값 객체·애그리거트로 규칙의 위치를 명확화 |
| Software Architecture | [service-communication-patterns-with-rest-api](https://github.com/thought-corner/service-communication-patterns-with-rest-api) | 서킷 브레이커·폴백 degrade로 장애 전파를 격리한 MSA 통신 설계 — 다운스트림 장애가 상위 가용성을 침범하지 않도록 방어 |
| Software Architecture | [service-communication-patterns-with-graphql](https://github.com/thought-corner/service-communication-patterns-with-graphql) | client-facing REST + 서비스 간 GraphQL로 전송 계층을 분리 — 통신 방식 교체에도 서킷 브레이커로 실패 판정 기준을 일관 유지 |
| Software Architecture | [service-communication-patterns-with-grpc](https://github.com/thought-corner/service-communication-patterns-with-grpc.git) | gRPC blocking stub 전환과 status 기반 서킷 브레이킹 — 재시도 소진 시 부가 조회 200 degrade·필수 조회 503+Retry-After로 종착 분기 설계 |
| Data Engineering | [spring-batch-settlement](https://github.com/thought-corner/spring-batch-settlement) | 청크 기반 배치로 대량 거래를 재처리·재현 가능하게 정산 — Clock 주입으로 시간 의존성을 제거해 결과를 테스트로 고정 (Jenkins+Docker) |
| Machine Learning | [spring-ai-chat-bot](https://github.com/thought-corner/spring-ai-chat-bot) | RAG 파이프라인으로 사내 문서 기반 응답 구현 — Tika→청킹→BGE-M3→Elasticsearch에 다중 쿼리 확장으로 recall 보강, 대화 메모리로 맥락 유지 (로컬 Ollama) |
| Software Testing | [test-driven-development](https://github.com/thought-corner/test-driven-development.git) | outside-in TDD로 E-커머스 API 계약을 명세화 — 35개 통합 테스트로 회원가입·토큰·거래 입출력(상태코드·응답·JWT)을 @TddApiTest로 고정 |
| Release Engineering | [deploy-to-s3-with-jenkins](https://github.com/thought-corner/deploy-to-s3-with-jenkins.git) | Jenkins 선언형 파이프라인으로 정적 사이트 배포 자동화 — Docker agent 격리·credential 주입·IAM 최소권한으로 s3 sync --delete 무중단 배포 |

<img src="https://github-readme-activity-graph.vercel.app/graph?username=dnwls16071&theme=github-dark" alt="GitHub Activity Graph" />
