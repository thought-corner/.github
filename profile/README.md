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
    <img src="https://go-skill-icons.vercel.app/api/icons?i=mysql,postgresql,redis,elasticsearch"/>
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
| Framework | [spring-batch-settlement](https://github.com/thought-corner/spring-batch-settlement) | 매일 쌓이는 대량 거래를 재처리 가능하고 재현 가능하게 정산하는 문제 — 청크 기반 Reader/Processor/Writer로 대량 데이터를 나눠 처리하고, Clock 주입으로 시간 의존성을 제거해 정산 결과를 테스트로 고정. Jenkins + Docker로 실행 |
| Framework | [spring-ai-chat-bot](https://github.com/thought-corner/spring-ai-chat-bot) | LLM이 모르는 사내 문서를 근거로 답하게(RAG) 하고 검색 품질을 끌어올리는 문제 - Tika 인제스천→청킹→BGE-M3 임베딩→Elasticsearch 검색 파이프라인에 다중 쿼리 확장으로 recall 보강, 대화 메모리 Advisor로 맥락 유지 (로컬 Ollama LLM) |
| OOP | [law-of-demeter](https://github.com/thought-corner/law-of-demeter) | 메시지를 보내되 묻지 않는다(Tell-Don't-Ask)를 getter 없는 도메인으로 강제하고 테스트 31개로 캡슐화 회귀 방지 |
| Architecture | [monolithic-architecture](https://github.com/thought-corner/monolithic-architecture) | 도메인 규칙을 어디에 둘지를 포트&어댑터·값 객체·애그리거트로 답한 헥사고날·DDD 실습 |
| Architecture | [service-communication-patterns-with-rest-api](https://github.com/thought-corner/service-communication-patterns-with-rest-api) | 동기 REST로 분리한 서비스 사이의 장애 전파(cascading failure)를 서킷 브레이커 격리 + 폴백 degrade로 끊어 다운스트림 장애가 상위 서비스 가용성을 끌어내리지 않도록 설계한 MSA 통신 패턴 |
| Architecture | [service-communication-patterns-with-graphql](https://github.com/thought-corner/service-communication-patterns-with-graphql) | 서비스 간 통신 계층만 REST → GraphQL로 교체하되 client-facing은 REST로 유지해 전송 계층의 경계를 나누고, 동기 결합은 그대로라 서킷 브레이커 격리를 존치하되 전송 방식 교체로 달라진 실패 판정 기준을 다시 세워 다운스트림 장애만 회로를 열도록 설계한 MSA 통신 패턴 |
| Architecture | [service-communication-patterns-with-grpc](https://github.com/thought-corner/service-communication-patterns-with-grpc.git) | 동기 통신을 REST에서 gRPC blocking stub으로 전환하고, gRPC status(UNAVAILABLE·RESOURCE_EXHAUSTED) 기반 재시도를 서킷 브레이커와 한 계층으로 묶어, 재시도 소진 시 부가 조회는 200 degrade·필수 조회는 503+Retry-After로 종착 분기를 갈라 설계한 MSA 통신 패턴 |

<p align="center">
<img src="https://github-readme-activity-graph.vercel.app/graph?username=dnwls16071&theme=github-light" alt="GitHub Activity Graph" />
</p>
