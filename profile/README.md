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
    <img src="https://go-skill-icons.vercel.app/api/icons?i=kafka"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=mysql,postgresql,redis"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=aws,docker,kubernetes"/>
  </a>
</p>
<p align="center">
  <a href="https://go-skill-icons.vercel.app/">
    <img src="https://go-skill-icons.vercel.app/api/icons?i=githubactions"/>
  </a>
</p>

## 💼 Introduction

- Spring Boot 기반의 Java/Kotlin 서비스를 개발하고 운영하는 백엔드 개발자입니다. 사내에서 여러 프로젝트를 진행하면서 성능 개선, 구조 리팩토링, 테스트 코드 도입을 맡아 왔습니다.
- 복잡한 비즈니스 요구사항을 누구나 같은 뜻으로 읽을 수 있게 정리하는 데 관심이 많습니다. 구현이 틀리는 비용보다 합의가 어긋나는 비용이 더 크다고 생각합니다.
- 오래 유지되고 신뢰할 수 있는 시스템을 만드는 개발자를 지향합니다.

## 🧭 Approach

- 정상 동작보다 먼저 실패했을 때 무엇이 깨지는지를 그려봅니다. 무엇을 희생하고 무엇을 지킬지 정한 다음 설계에 들어갑니다.
- 커버리지 숫자보다 "이 테스트가 실제로 회귀를 잡아내는가"를 먼저 묻습니다. 통과하는 테스트와 신뢰할 수 있는 테스트는 다르다고 생각합니다.
- 테스트가 어렵다면 테스트 기법보다 설계를 먼저 의심합니다. 결합이 심한 코드는 대체로 테스트에서 먼저 드러납니다.
- 기획을 그대로 구현하기 전에 이 기획이 풀려는 문제가 무엇이었는지 되짚고, 애매한 지점은 넘어가지 않고 먼저 확인합니다.
- 요구사항 하나가 코드 어디까지 닿는지, 그 요구사항이 바뀌면 어디가 흔들릴지를 먼저 그려봅니다.
- 선택지를 명시적으로 비교하고 "왜 이 방식을 택했는가"를 기록으로 남기는 습관을 들이고 있습니다.

## 🔭 Interests

- **실패의 전파** — 장애가 어디서 나는지보다 어디까지 번지는지. 재시도·폴백·격리를 실패를 없애는 장치가 아니라 반경을 줄이는 장치로 보는 관점.
- **정합성의 수준** — "완벽한 일관성"이 기본값이 아닌 영역들. 도메인이 실제로 요구하는 선이 어디까지이고, 그 선을 무엇을 근거로 정하는지.
- **변화에 견디는 구조** — 지금 필요하지 않은 유연성을 미리 만드는 쪽보다, 실제로 바뀔 때 바꿀 수 있는 구조.
- **문제의 본질** — 새로운 기술 자체보다 그 기술이 풀려는 문제. 도구는 계속 바뀌지만 문제는 잘 바뀌지 않아서, 특정 스택보다 원리를 이해하는 데 시간을 씁니다.

## 🏗️ Toy Projects

### [order-system](https://github.com/thought-corner/order-system) — RabbitMQ 비동기 주문 시스템

> **주문 API가 메시지 발행 즉시 응답하고, 컨슈머가 재고 차감·주문 생성을 처리하는 구조입니다.**
- MySQL Named Lock(주문 단위) + JPA 비관적 락(재고 단위) 이중 락으로 동시성 제어
- **k6 부하 테스트: 4,000 VUs에서 TPS 2.23k, 실패 0건, 재고 정합성 오차 0** 실측 기록
- Dead Letter Queue + 재발행으로 실패 메시지 유실 방지, prefetch·컨슈머 수 튜닝 과정 문서화

### [query-performance](https://github.com/thought-corner/query-performance) — 대용량 조회 성능 개선 (SQL 튜닝 → Redis 캐싱)

> **30만 건 데이터에서 풀 테이블 스캔으로 최대 14초 걸리던 필터링 조회를 단계적으로 개선했습니다.**
- EXPLAIN으로 풀 테이블 스캔(type: ALL) 확인 → 가격 인덱스 적용으로 조회 시간**70.5% 단축**(1s → 0.295s)
- **인덱스를 생성했는데도 동작하지 않던 원인 분석** — 낮은 카디널리티(LocalDate 일 단위 저장 + 중복도 높은 더미 데이터)로 옵티마이저가 인덱스를 버리는 상황을 규명하고, 타입·데이터 분포를 바로잡아 **82.25% 단축** (2s → 0.355s)
- 시스템 변경 없는 SQL 튜닝을 먼저, 그 위에 Redis Cache-Aside를 얹어 **추가 45.7% 개선**(1.01s → 0.548s) — 캐시-DB 정합성 한계까지 문서화

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-stats-extended.vercel.app/api?username=dnwls16071&show_icons=true&include_all_commits=true&rank_icon=github&hide_border=true&theme=github_dark">
  <img src="https://github-stats-extended.vercel.app/api?username=dnwls16071&show_icons=true&include_all_commits=true&rank_icon=github&hide_border=true&theme=default" alt="Jang Woo's GitHub stats">
</picture>
