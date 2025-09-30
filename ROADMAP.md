# ROADMAP

**Stack**: .NET 10, Blazor WASM/MudBlazor, Angular/Material, SCSS/Tailwind, YARP, RabbitMQ, PostgreSQL (EF Core), Redis, MongoDB/Neo4j (opt.), Keycloak, Discord, Hangfire, QuestPDF, Shouldly, Moq, FluentResults, ProblemDetails, Prometheus, Grafana, Helm, OpenTelemetry, FastEndpoints.

**AI**: Python FastAPI worker (scikit-learn/PyTorch), Hugging Face fallback, Redis queue.

**KPIs**: Latency <150 ms (P95); 90% clean posts (AI); karma >20/week/user.

## Conventions
- **Epic**: Key feature.
- **Task**: Action with story points (SP): 1 (simple), 2 (medium), 3 (standard), 5 (complex), 8 (advanced).
- **Success**: xUnit tests >80% coverage with Shouldly/Moq.
- **Dependencies**: Prerequisites.
- **Risks**: Mitigations included.

## Roadmap Details

### PI-1: Foundations & Authentication (Weeks 1–6)
**Goal**: Set up Aspire, infrastructure (PostgreSQL, Redis, RabbitMQ, minimal PythonWorker), auto migrations in dev, Posts/Auth modules, test framework, Docker Compose prep.  
**Epics**: Posts, Auth modules; EF Core, FluentValidation, Mediator, Scalar, FluentResults, ProblemDetails, OpenTelemetry.  
**KPI**: CI <5 min; 100% auth cases; 1 signup/24h/Discord.  

#### Sprint 1 (Weeks 1–2)
- **Task 1.1: Monorepo + Aspire + Infrastructure** (5 SP)  
  - **Do**: Structure monolith (PostOuCancer.Infrastructure, empty PostOuCancer.Domain); Aspire.AppHost (Backend, PostgreSQL, Redis, RabbitMQ, PythonWorker with /health); OTEL; FastEndpoints (base); configure Redis (PING), PostgreSQL (postoucancerdb, empty auto migrations), RabbitMQ (UI), PythonWorker (/health); Dockerfiles.  
  - **Success**: DS220+ works; Aspire dashboard (`http://<ds220-ip>:18888`); API healthcheck (`/api/health`); PostgreSQL (`SELECT 1`); RabbitMQ UI (`http://<ds220-ip>:15672`); PythonWorker (`http://<ds220-ip>:8000/health`).  
  - **Dependencies**: .NET 10 RC1, PostgreSQL 16, Redis 7, RabbitMQ 3, Python 3.12, Docker.  
  - **Risks**: Aspire learning curve (use Microsoft samples).  

- **Task 1.2: CI/CD + Tests** (3 SP)  
  - **Do**: GitHub Actions (build/tests/push), xUnit with Shouldly/Moq for infrastructure (PostgreSQL/Redis/RabbitMQ connections).  
  - **Success**: Auto build; >80% coverage.  
  - **Dependencies**: Task 1.1.  

#### Sprint 2 (Weeks 3–4)
- **Task 2.1: Auth Module (Keycloak + Discord)** (5 SP)  
  - **Do**: Keycloak (JWT, refresh tokens), Discord (>7 days, Redis `SETEX`), hCaptcha, 100 req/min/IP rate-limit, FluentValidation, Mediator, FluentResults, ProblemDetails, FastEndpoints.  
  - **Success**: Secure signup (`/api/auth/login`).  
  - **Dependencies**: Task 1.1.  
  - **Risks**: Discord downtime (cache tokens in Redis).  

- **Task 2.2: Posts Module CQRS + EF Core** (5 SP)  
  - **Do**: `CreatePostCommand` (HEAD HttpClient + Polly, GZIP, EF Core, FluentValidation, FluentResults, Mediator, ProblemDetails, FastEndpoints); add Post/Community entities.  
  - **Success**: Post created (`/api/posts`); valid links.  
  - **Dependencies**: Task 1.1.  

- **Task 2.3: YARP + Middleware** (3 SP)  
  - **Do**: Route to modules, Redis rate-limits, exception middleware (ProblemDetails).  
  - **Success**: Requests routed (`http://<ds220-ip>:5000/api/*`).  
  - **Dependencies**: Task 2.2.  

#### Sprint 3 (Weeks 5–6)
- **Task 3.1: Auth UI Blazor WASM** (3 SP)  
  - **Do**: MudTextField/MudButton (Discord OAuth, hCaptcha), SCSS mobile-first.  
  - **Success**: Mobile-friendly login UI.  
  - **Dependencies**: Task 2.1.  

- **Task 3.2: Posts Read Models EF Core** (3 SP)  
  - **Do**: MassTransit projectors → PostgreSQL (GZIP bytea, EF Core migrations).  
  - **Success**: Posts readable <0.5s.  
  - **Dependencies**: Task 2.2.  

- **Task 3.3: CQRS Tests** (2 SP)  
  - **Do**: xUnit with Shouldly/Moq (mock HttpClient/Polly, FastEndpoints).  
  - **Success**: >80% coverage.  
  - **Dependencies**: Task 1.2.  

**Deliverables**: Aspire-orchestrated monolith, Posts/Auth modules, FastEndpoints, EF Core (auto migrations in dev), Mediator, Scalar, FluentResults, ProblemDetails, Shouldly, Moq, secure auth, Redis/PostgreSQL/RabbitMQ, OpenTelemetry, Docker Compose prep.

### PI-2: Communities & Votes (Weeks 7–12)
**Goal**: Communities, votes, manual moderation, email flows.  
**Epics**: Communities, Votes, Moderation modules; AutoMapper.  
**KPI**: R2 upload <2s; reports <10/day/user.  

#### Sprint 4 (Weeks 7–8)
- **Task 4.1: Communities Module CQRS + AutoMapper** (5 SP)  
  - **Do**: `CreateCommunityCommand` (R2 upload, RabbitMQ, AutoMapper DTOs, Mediator, FluentResults, FastEndpoints).  
  - **Success**: Community created.  
  - **Dependencies**: Task 2.3.  

- **Task 4.2: Communities UI MudBlazor** (3 SP)  
  - **Do**: MudCard for R2 banners, SCSS mobile-first.  
  - **Success**: Responsive display.  
  - **Dependencies**: Task 4.1.  

- **Task 4.3: Votes Module CQRS** (3 SP)  
  - **Do**: `CastVoteCommand` (Mediator, RabbitMQ, EF Core, FluentResults, FastEndpoints).  
  - **Success**: Async score updates.  
  - **Dependencies**: Task 2.2.  

#### Sprint 5 (Weeks 9–10)
- **Task 5.1: Votes UI MudBlazor + Cache** (3 SP)  
  - **Do**: MudChip up/down, Redis cache for scores, SCSS mobile-first.  
  - **Success**: Interactive voting.  
  - **Dependencies**: Task 4.3.  

- **Task 5.2: Moderation Module (Reports)** (3 SP)  
  - **Do**: `ReportPostCommand` (hCaptcha, 5/day, RabbitMQ, Mediator, FluentResults, FastEndpoints).  
  - **Success**: Secure reports.  
  - **Dependencies**: Task 2.3.  

- **Task 5.3: Admin Moderation Angular + Email** (5 SP)  
  - **Do**: MatTable for reports, SendGrid confirmation/reset, SCSS mobile-first.  
  - **Success**: Moderator actions; email flows.  
  - **Dependencies**: Task 5.2.  

#### Sprint 6 (Weeks 11–12)
- **Task 6.1: Keycloak Roles** (3 SP)  
  - **Do**: Admin/Moderator/User/Premium claims.  
  - **Success**: Permissions applied.  
  - **Dependencies**: Task 2.1.  

- **Task 6.2: Moderation Audit** (3 SP)  
  - **Do**: `ModerationAction` events (OTEL).  
  - **Success**: Traceability.  
  - **Dependencies**: Task 5.2.  

**Deliverables**: Communities, votes, moderation, email flows.

### PI-3: Threads & Search (Weeks 13–18)
**Goal**: Threads, paginated search, drafts, mobile-first PWA.  
**Epics**: Posts (threads), Search module; PWA setup.  
**KPI**: Thread <1s; search <200 ms; mobile-first UX.  

#### Sprint 7 (Weeks 13–14)
- **Task 7.1: Threads Module CQRS** (5 SP)  
  - **Do**: `CreateThreadCommand` (HEAD, GZIP, EF Core, Mediator, FluentResults, FastEndpoints).  
  - **Success**: Thread created.  
  - **Dependencies**: Task 4.1.  

- **Task 7.2: Threads UI MudBlazor + PWA** (3 SP)  
  - **Do**: MudExpansionPanel, Redis drafts, SCSS mobile-first, PWA manifest.  
  - **Success**: Responsive threads, installable PWA.  
  - **Dependencies**: Task 7.1.  

#### Sprint 8 (Weeks 15–16)
- **Task 8.1: Comments Module CQRS** (5 SP)  
  - **Do**: `AddCommentCommand` (nested, GZIP, EF Core, Mediator, FluentResults, FastEndpoints).  
  - **Success**: Nested comments.  
  - **Dependencies**: Task 7.1.  

- **Task 8.2: Comments UI MudBlazor** (3 SP)  
  - **Do**: MudTextField for replies, SCSS mobile-first.  
  - **Success**: Fluid interactions.  
  - **Dependencies**: Task 8.1.  

#### Sprint 9 (Weeks 17–18)
- **Task 9.1: Search Module (FTS + Elasticsearch)** (5 SP)  
  - **Do**: `SearchPostsQuery` (tsvector + ES, pagination, Mediator, FluentResults, FastEndpoints).  
  - **Success**: Results <200 ms.  
  - **Dependencies**: Task 7.1.  

- **Task 9.2: Search UI MudBlazor** (2 SP)  
  - **Do**: MudAutocomplete with pagination, SCSS mobile-first, PWA cache.  
  - **Success**: Intuitive search.  
  - **Dependencies**: Task 9.1.  

- **Task 9.3: Admin Search Angular** (3 SP)  
  - **Do**: MatPaginator for results, SCSS mobile-first, Angular PWA.  
  - **Success**: Unified dashboard.  
  - **Dependencies**: Task 5.3.  

**Deliverables**: Threads, paginated search, drafts, mobile-first PWA.

### PI-4: AI & Gamification (Weeks 19–24)
**Goal**: Custom AI moderation, gamification with image uploads, analytics.  
**Epics**: Moderation (AI), Gamification modules; MongoDB/Neo4j (opt.).  
**KPI**: AI <5s/post; karma >20/week.  

#### Sprint 10 (Weeks 19–20)
- **Task 10.1: Custom AI Moderation (Python + FastAPI)** (8 SP)  
  - **Do**: Develop scikit-learn (text) + PyTorch MobileNetV3 (image) models, FastAPI `/analyze`, Redis queue, Hugging Face fallback (Toxic-BERT/CLIP).  
  - **Success**: Analysis <5s, 90% clean posts.  
  - **Dependencies**: Task 2.2.  
  - **Risks**: Dataset quality (use Discord/Reddit, manual labeling).  

- **Task 10.2: Pending Posts UI MudBlazor** (2 SP)  
  - **Do**: Poll ghost posts, SCSS mobile-first.  
  - **Success**: Author-only visibility.  
  - **Dependencies**: Task 10.1.  

#### Sprint 11 (Weeks 21–22)
- **Task 11.1: Gamification Module CQRS** (5 SP)  
  - **Do**: `KarmaChangedCommand` (MongoDB/Neo4j opt., EF Core, Mediator, FluentResults, FastEndpoints).  
  - **Success**: JVC levels, stored analytics.  
  - **Dependencies**: Task 10.1.  

- **Task 11.2: Badges UI MudBlazor** (3 SP)  
  - **Do**: MudBadge for levels/badges, SCSS mobile-first.  
  - **Success**: Engaging visuals.  
  - **Dependencies**: Task 11.1.  

#### Sprint 12 (Weeks 23–24)
- **Task 12.1: Keycloak Privileges** (5 SP)  
  - **Do**: VIP/Premium claims (skip AI).  
  - **Success**: Unlocked access.  
  - **Dependencies**: Task 11.1.  

- **Task 12.2: k6 Tests** (3 SP)  
  - **Do**: 100 users (posts, rate-limits), Shouldly/Moq.  
  - **Success**: Latency <200 ms.  
  - **Dependencies**: Task 10.1.  

**Deliverables**: Custom AI moderation (text/image), gamification, MongoDB/Neo4j analytics.

### PI-5: Polling & Scalability (Weeks 25–30)
**Goal**: Notifications, webhooks, scalability, social features.  
**Epics**: Notifications module, Hangfire, Neo4j (opt.), Prometheus/Grafana, Helm.  
**KPI**: Polling <2s; 1000 users.  

#### Sprint 13 (Weeks 25–26)
- **Task 13.1: Notifications Module Polling** (5 SP)  
  - **Do**: `PollStatusQuery` (Mediator, EF Core, FluentResults, FastEndpoints).  
  - **Success**: Fluid UI updates.  
  - **Dependencies**: Task 10.1.  

- **Task 13.2: Notifications UI + Webhooks** (3 SP)  
  - **Do**: MudSnackbar, Discord webhooks, SCSS mobile-first.  
  - **Success**: Community alerts.  
  - **Dependencies**: Task 13.1.  

#### Sprint 14 (Weeks 27–28)
- **Task 14.1: Search Filters + Soundex + Neo4j** (5 SP)  
  - **Do**: Date/author filters (ES), Soundex (PostgreSQL), Neo4j recommendations (opt.).  
  - **Success**: Results <200 ms.  
  - **Dependencies**: Task 9.1.  

- **Task 14.2: Filters UI Angular** (2 SP)  
  - **Do**: MatSelect for admin, SCSS mobile-first.  
  - **Success**: Intuitive interface.  
  - **Dependencies**: Task 14.1.  

#### Sprint 15 (Weeks 29–30)
- **Task 15.1: Kubernetes Helm + Hangfire** (5 SP)  
  - **Do**: Helm charts, Hangfire (cleanup, notifications).  
  - **Success**: Scalable cluster.  
  - **Dependencies**: Task 1.1.  

- **Task 15.2: Prometheus/Grafana** (3 SP)  
  - **Do**: OTEL (queues, latency), Prometheus/Grafana via Helm.  
  - **Success**: Dashboards/alerts.  
  - **Dependencies**: Task 3.2.  

**Deliverables**: Notifications, webhooks, Hangfire, Neo4j recommendations (opt.), prod monitoring.

### PI-6: GDPR & Monetization (Weeks 31–36)
**Goal**: Compliance, Premium subscriptions, Dapr/Go/React Native evaluation.  
**Epics**: GDPR, Monetization modules.  
**KPI**: Export <72h; Premium conversion >2%.  

#### Sprint 16 (Weeks 31–32)
- **Task 16.1: GDPR Export Module + QuestPDF** (5 SP)  
  - **Do**: JSON/PDF export (posts, karma), FastEndpoints.  
  - **Success**: Secure download.  
  - **Dependencies**: Task 11.1.  

- **Task 16.2: Export UI MudBlazor** (3 SP)  
  - **Do**: MudDialog for GDPR, SCSS mobile-first.  
  - **Success**: User-friendly.  
  - **Dependencies**: Task 16.1.  

#### Sprint 17 (Weeks 33–34)
- **Task 17.1: GDPR Deletion** (5 SP)  
  - **Do**: Anonymize events (keep links), FastEndpoints.  
  - **Success**: PII erased.  
  - **Dependencies**: Task 16.1.  

- **Task 17.2: Unified Themes** (3 SP)  
  - **Do**: SCSS/Tailwind for Material/MudBlazor consistency.  
  - **Success**: Coherent UI.  
  - **Dependencies**: Task 9.3.  

#### Sprint 18 (Weeks 35–36)
- **Task 18.1: Monetization Module Stripe + Google OAuth** (5 SP)  
  - **Do**: Stripe handler, Google OAuth (opt.), Premium claims, Dapr/Go/React Native evaluation, FastEndpoints.  
  - **Success**: Active subscriptions.  
  - **Dependencies**: Task 12.1.  

- **Task 18.2: Premium UI MudBlazor** (3 SP)  
  - **Do**: MudButton for upgrade, SCSS mobile-first.  
  - **Success**: Fluid payment.  
  - **Dependencies**: Task 18.1.  

**Deliverables**: GDPR compliance, Premium subscriptions, Google OAuth, Dapr/Go/React Native evaluation.

## Deployment Plan
- **Alpha (PI-3)**: DS220+ (posts, search, Discord, PWA, Docker Compose).
- **Closed Beta (PI-4)**: K8s staging (AI, gamification, analytics).
- **Public Beta (PI-5)**: Open signups, webhooks, recommendations, prod monitoring.
- **GA (PI-6)**: Live prod (Premium).

## Tracking
- **Tools**: GitHub Projects (CSV import via github-csv-tools), issues.
- **Review**: KPI checks at PI end (e.g., AI false positives <10%).
