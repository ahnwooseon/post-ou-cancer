# ROADMAP.md

## Conventions

**Universe**: Each space (public feed, forum, micro-world) has its own rules, identity, and moderation.
**Identity**: Users can choose real name (optional), pseudonym per community, or ephemeral anonymous identity for micro-worlds.
**Thread**: Can migrate between universes (public → micro-world), with user-selected anonymity and visibility.
**Moderation**: Granular, adapted to each universe (community-driven, automated, or strict).
**PI (Program Increment)**: Major delivery cycle with business value
**Sprint**: 2-week development cycle within a PI
**Milestone**: Major deliverable with specific due date and success criteria
**Task**: Action with story points (SP): 1 (simple), 2 (medium), 3 (standard), 5 (complex), 8 (advanced)
**Success**: xUnit tests >80% coverage with Shouldly/Moq
**Dependencies**: Prerequisites
**Risks**: Mitigations included

---

## PI-1: Foundation & Infrastructure

**Duration**: 8 weeks (Oct 25, 2025 → Dec 19, 2026)  
**Goal**: Establish solid technical foundation with authentication and basic functionality

### Sprint 1.1 (Oct 25 - Nov 7, 2025)

- Milestone 0: Architecture Blueprint
- Milestone 1: Local Dev Environment Ready

### Sprint 1.2 (Nov 8 - Nov 21, 2025)

- Milestone 2: Automated Testing Pipeline

### Sprint 1.3 (Nov 22 - Dec 5, 2025)

- Milestone 3: Auth System Functional

### Sprint 1.4 (Dec 6-19, 2025)

- Milestone 4: Login Flow Complete

---

## PI-2: Core Social Features

**Duration**: 8 weeks (Dec 20, 2025 → Mar 27, 2026)  
**Goal**: Build core forum functionality (posts, communities, voting, discussions, moderation)

### Sprint 2.1 (Dec 20, 2025 - Jan 2, 2026)

- Milestone ALPHA: Posts MVP + Performance Baseline 🚀

### Sprint 2.2 (Jan 3-16, 2026)

- Milestone 5: Multi-Community Support

### Sprint 2.3 (Jan 17-30, 2026)

- Milestone 6: Voting + Score Aggregation

### Sprint 2.4 (Jan 31 - Feb 13, 2026)

- Milestone 7: Nested Discussion Trees

### Sprint 2.5 (Feb 14-27, 2026)

- Milestone 8: Report System + Audit Trail

---

## PI-3: Intelligence & Premium

**Duration**: 8 weeks (Mar 28 → May 22, 2026)  
**Goal**: Add AI moderation, gamification, notifications, and premium features

### Sprint 3.1 (Feb 28 - Mar 13, 2026)

- Milestone BETA: Full-Text Search Live 🚀

### Sprint 3.2 (Mar 14-27, 2026)

- Milestone 9: Moderation Tools

### Sprint 3.3 (Mar 28 - Apr 10, 2026)

- Milestone 10: Consistent Brand Across UIs

### Sprint 3.4 (Apr 11-24, 2026)

- Milestone 11: Automated Content Moderation

### Sprint 3.5 (Apr 25 - May 8, 2026)

- Milestone 12: Karma & Progression Visible

---

## PI-4: Production & Compliance

**Duration**: 14 weeks (Jun 5 → Sep 11, 2026)  
**Goal**: Scale to production, ensure compliance, and launch monetization

### Sprint 4.1 (May 9-22, 2026)

- Milestone 13: User Alerts System

### Sprint 4.2 (May 23, Jun 5, 2026)

- Milestone 14: Premium Features Ready

### Sprint 4.3 (Jun 6-19, 2026)

- Milestone V1.0: Full System Visibility 🚀

### Sprint 4.4 (Jun 20 - Jul 3, 2026)

- Milestone 15: Production-Ready Deployment

### Sprint 4.5 (Jul 4-17, 2026)

- Milestone 16: Data Privacy Complete (GDPR)

### Sprint 4.6 (Jul 18-31, 2026)

- Milestone 17: Monetization Live

### Sprint 4.7 (Aug 1-14, 2026)

- Milestone 18: Security Audit Complete

### Sprint 4.8 (Aug 15-28, 2026)

- Milestone PRODUCTION: Ready for Production 🚀

---

## MILESTONE 0: Architecture Blueprint

**Due Date**: 2025-10-31

### 📦 Deliverables

- ADR documentation (8 ADRs minimum)
- OWASP/GDPR baseline security guidelines
- Docker Compose + Aspire setup
- Environment configuration documentation

### Tasks

- **Task 0.1: Architecture Decision Records** (8 SP)

  - Create `/docs/adr/` folder structure
  - ADR-001: .NET 10 Preview choice and justification
  - ADR-002: Modular monolith vs microservices architecture
  - ADR-003: Aspire for orchestration and observability
  - ADR-004: PostgreSQL 18 as primary database
  - ADR-005: Elasticsearch vs PostgreSQL FTS for search
  - ADR-006: Blazor WASM + MudBlazor for public UI
  - ADR-007: Angular + Material for admin interface
  - ADR-008: Deployment strategy (Docker Compose → k3s → Kubernetes)
  - Document trade-offs and alternatives considered

- **Task 0.2: Security & Compliance Baseline** (7 SP)
  - Create OWASP Top 10 checklist
  - Document GDPR requirements (export, deletion, consent)
  - Define secrets management strategy
  - Plan OpenTelemetry instrumentation (logs, traces, metrics)
  - Rate-limiting and DDoS protection policy
  - User input validation strategy
  - Backup and disaster recovery plan

### ✅ Success Criteria

- At least 8 ADRs documented with standardized format
- OWASP checklist complete and GDPR scope documented
- Complete developer onboarding documentation

**⏱️ Estimated effort**: 15h (AI scaffolding 8h + integration 7h)

---

## MILESTONE 1: Local Dev Environment Ready

**Due Date**: 2025-11-07

### 📦 Deliverables

- Monorepo + Aspire orchestration
- PostgreSQL/Redis/RabbitMQ health checks
- EF Core migrations with auto-migration
- OpenTelemetry logging and tracing
- Docker Compose environment

### Tasks

- **Task 1.1: Monorepo + Aspire AppHost** (12 SP)

  - Create solution structure: `src/`, `tests/`, `docs/`, `infrastructure/`
  - `PostOuCancer.Domain` project (entities, interfaces)
  - `PostOuCancer.Infrastructure` project (EF Core, repositories)
  - `PostOuCancer.Api` project (FastEndpoints)
  - `PostOuCancer.Web` project (Blazor WASM)
  - `PostOuCancer.Admin` project (Angular)
  - `Aspire.AppHost` project for orchestration
  - Configuration `appsettings.Development.json`
  - Dockerfiles for each service
  - Validate all projects compile

- **Task 1.2: Health Checks Infrastructure** (9 SP)

  - Health check PostgreSQL (`SELECT 1`)
  - Health check Redis (`PING`)
  - Health check RabbitMQ (management API connection)
  - Health check Python Worker (`GET /health`)
  - Main API endpoint `/api/health`
  - Aspire dashboard integration for visualization
  - Automated health check tests
  - Endpoint documentation

- **Task 1.3: EF Core Migrations Framework** (10 SP)

  - Install EF Core + Npgsql packages
  - Create `ApplicationDbContext` with configuration
  - Initial migration `InitialCreate`
  - Auto-migration on startup in development
  - Seed data script for local environment
  - Integration tests with Testcontainers
  - Migration commands documentation

- **Task 1.4: OpenTelemetry Setup** (9 SP)

  - Install OpenTelemetry SDK
  - Configure structured logging (console + OTLP)
  - Enable distributed tracing
  - Custom metrics (requests, latency, errors)
  - Aspire dashboard integration
  - Test trace propagation between services
  - Logging conventions documentation

- **Task 1.5: Docker Compose Environment** (6 SP)
  - Create `docker-compose.yml` with all services
  - PostgreSQL 18 with persistent volumes
  - Redis 8 for caching and rate-limiting
  - RabbitMQ 3 for messaging
  - Python FastAPI worker for AI
  - Keycloak for authentication
  - Network configuration between containers
  - Startup and shutdown scripts
  - Developer setup documentation in README

### ✅ Success Criteria

- All services respond as healthy
- Aspire dashboard displays service status
- Migrations created and applied automatically
- Seed data injected in development
- Structured logs visible in console
- Traces visible in Aspire dashboard
- Metrics collected and displayed
- `docker-compose up` starts all services successfully

**⏱️ Estimated effort**: 20h (AI 12h + integration 8h)

---

## MILESTONE 2: Automated Testing Pipeline

**Due Date**: 2025-11-21

### 📦 Deliverables

- GitHub Actions CI/CD pipeline
- xUnit + Shouldly/Moq framework
- Testcontainers integration
- SonarQube quality gates

### Tasks

- **Task 2.1: GitHub Actions Pipeline** (11 SP)

  - Workflow `.github/workflows/ci.yml`
  - Job `build`: restore + build solution
  - Job `test`: run xUnit with coverage
  - Job `lint`: StyleCop for C# conventions
  - Upload coverage report (codecov.io or Coveralls)
  - NuGet package caching for performance
  - Failure notifications
  - Build status badge in README.md

- **Task 2.2: Testcontainers Framework** (14 SP)

  - Install Testcontainers package
  - PostgreSQL container fixture
  - Redis container fixture
  - RabbitMQ container fixture
  - Base class for integration tests
  -
  - Test examples with containers
  - Parallel execution configuration
  - Test pattern documentation

- **Task 2.3: xUnit + Shouldly + Moq** (8 SP)

  - `PostOuCancer.Tests.Shared` project
  - Test entity builders (Builder pattern)
  - Custom Shouldly extensions
  - Reusable Moq mocks (repositories, services)
  - Test data factories
  - Unit and integration test examples
  - Test style guide
  - Visual Studio/Rider test template

- **Task 2.4: SonarQube Integration** (6 SP)
  - Create SonarCloud account or self-hosted instance
  - Configure `sonar-project.properties`
  - Add to GitHub Actions workflow
  - Define quality gate: coverage > 80%, 0 critical code smells
  - Configure exclusions (tests, migrations)
  - SonarQube badge in README
  - Quality threshold documentation

### ✅ Success Criteria

- Push to `main` triggers pipeline
- All jobs pass successfully
- Coverage > 80% required
- Status badge visible
- Integration tests start containers
- Test isolation maintained
- Execution time < 5 minutes
- SonarQube analyzes each push
- Quality gate applied

**⏱️ Estimated effort**: 15h (AI 8h + integration 7h)

---

## MILESTONE 3: Auth System Functional

**Due Date**: 2025-12-05

### 📦 Deliverables

- Keycloak + Discord OAuth integration
- JWT validation middleware
- Redis rate-limiting
- YARP API Gateway
- **Selective end-to-end encryption for DMs, private circles, and drafts (Zero Knowledge Graph)**
- **Local-first storage for user data, with optional encrypted cloud sync**

### Tasks

- **Task 3.1: Keycloak + Discord OAuth & Selective Encryption** (22 SP)
- Deploy Keycloak in Docker Compose
- Implement client-side encryption for DMs, private circles, and drafts
- Ensure public content remains readable and transparent
- Local-first storage logic (offline-first, explicit cloud sync)
- Create realm `postoucancer`
- Configure OAuth client (confidential)
- Add Discord as Identity Provider
- Map Discord claims → Keycloak
- Configure JWT (RS256, 15 min duration)
- Refresh tokens (7 days duration)
- JWT validation middleware in API
- End-to-end OAuth flow tests
- Authentication flow documentation

- **Task 3.2: Redis Rate Limiting** (12 SP)

  - ASP.NET Core rate-limiting middleware
  - Redis counting logic (sliding window)
  - Per-endpoint configuration (ex: 100 req/min/IP)
  - Per-authenticated-user configuration (ex: 200 req/min)
  - Response headers (X-RateLimit-\*)
  - 429 Too Many Requests with ProblemDetails
  - Middleware unit tests
  - Load tests to validate limits
  - Threshold documentation

- **Task 3.3: YARP API Gateway** (9 SP)
  - Install YARP package
  - Route configuration (`/api/*`, `/auth/*`)
  - Load balancing (round-robin)
  - Backend health checks
  - Header transformation (add correlation ID)
  - Request logging
  - Routing tests
  - Gateway architecture documentation

### ✅ Success Criteria

- Discord login functional
- Valid JWT issued by Keycloak
- Refresh token flow operational
- Rate-limiting active on all endpoints
- Configurable limits per environment
- Informative headers in responses
- Requests correctly routed
- Health checks functional
- Structured request logs

**⏱️ Estimated effort**: 22h (AI 14h + integration 8h)

---

## MILESTONE 4: Login Flow Complete

**Due Date**: 2025-12-19

### 📦 Deliverables

- Blazor WASM Discord login UI (MudBlazor)
- Session management (JWT refresh + auto-renewal)
- Global exception handler

### Tasks

- **Task 4.1: Auth UI Blazor WASM** (18 SP)

  - `Login.razor` page with MudBlazor
  - Styled "Login with Discord" button
  - OAuth callback handler
  - OAuth error handling
  - Loading states and spinners
  - SCSS mobile-first (320px → desktop)
  - Responsive tests (320px, 768px, 1024px)
  - End-to-end tests with bUnit
  - Component documentation

- **Task 4.2: Session Management** (15 SP)

  - `AuthenticationService` Blazor service
  - Secure JWT storage (encrypted LocalStorage)
  - Auto-refresh timer (5 min before expiration)
  - `/api/auth/refresh` endpoint
  - Refresh failure handling
  - Auto-logout if refresh fails
  - "Logout" button with confirmation
  - Token lifecycle tests
  - Flow documentation

- **Task 4.3: Global Exception Handler** (9 SP)
  - `GlobalExceptionMiddleware`
  - Exception mapping → ProblemDetails (RFC 7807)
  - Structured logging with OpenTelemetry
  - Dev vs production differentiation (stack traces)
  - Standardized error codes
  - All error path tests
  - Error code documentation

### ✅ Success Criteria

- Mobile-friendly and accessible UI
- Complete OAuth flow functional
- bUnit tests passing
- Design consistent with brand
- Token auto-renewed before expiration
- Clean logout (clear storage + redirect)
- Service unit tests
- Edge case handling
- All exceptions logged
- ProblemDetails responses RFC 7807 compliant
- No stack traces in production
- All error scenarios covered

**⏱️ Estimated effort**: 20h (AI 12h + integration 8h)

---

## MILESTONE ALPHA: Posts MVP + Performance Baseline

**Due Date**: 2026-01-02

### 📦 Deliverables

- **Replay Social: ability to revisit feed as it was at a past date (feed historization)**
- Implement feed historization and replay feature
- CreatePostCommand (CQRS + FluentValidation)
- MassTransit projections
- Posts Feed UI (pagination + SCSS)
- k6 baseline tests (10 users)

### Tasks

- **Task ALPHA.1: Posts Module CQRS** (17 SP)

  - `Post` entity (Domain)
  - `CreatePostCommand` with FluentValidation
  - `CreatePostCommandHandler` with Mediator
  - Link validation (HttpClient + Polly retry)
  - GZIP content compression
  - `PostCreated` event to RabbitMQ
  - FastEndpoints `/api/posts` endpoint
  - Unit tests (mock HttpClient)
  - Integration tests with Testcontainers
  - API documentation with Swagger

- **Task ALPHA.2: Posts Read Model Projections** (16 SP)

  - MassTransit consumer for `PostCreated`
  - `PostReadModel` table (denormalized)
  - GZIP storage in PostgreSQL `bytea`
  - EF Core migration for table
  - `GetPostsQuery` with pagination
  - GZIP decompression on read
  - Consumer tests
  - Performance tests (read time < 500ms)
  - Projection pattern documentation

- **Task ALPHA.3: Posts Feed UI MudBlazor** (22 SP)

  - `PostCard.razor` component (MudCard)
  - `Feed.razor` page with post list
  - Pagination with MudPagination
  - Edit/Delete buttons for author
  - Post creation modal (MudDialog)
  - Client-side validation (FluentValidation.Blazor)
  - SCSS responsive mobile-first
  - Skeleton loaders during loading
  - bUnit component tests
  - UX documentation

- **Task ALPHA.4: k6 Baseline Performance Tests** (8 SP)
  - k6 script: login scenario
  - k6 script: create post scenario
  - k6 script: read feed scenario
  - Mixed scenario (10 virtual users)
  - Metrics collection (P95 latency, throughput, errors)
  - Grafana dashboard for visualization
  - Baseline results documentation
  - Alert thresholds (latency > 150ms)

### ✅ Success Criteria

- Post created with complete validation
- Links verified before publication
- Event published to RabbitMQ
- Test coverage > 80%
- Event consumed and projected correctly
- Read time < 500ms for 100 posts
- Pagination functional
- Tests validate integrity
- Feed displays paginated posts
- Create/edit/delete functional
- UI responsive (mobile → desktop)
- bUnit tests passing
- k6 tests executable locally and CI
- Baseline metrics recorded
- Performance expectations documented
- Grafana dashboard functional

**⏱️ Estimated effort**: 25h (AI 14h + integration 11h)

**🚀 Launch**: ALPHA MVP

---

## MILESTONE 5: Multi-Community Support

**Due Date**: 2026-01-16

### 📦 Deliverables

- CreateCommunity API (CQRS + R2 storage)
- Community Management UI (MudBlazor)
- Keycloak roles (Admin/Mod/User/Premium)

### Tasks

- **Task 5.1: Community Module CQRS** (17 SP)

  - `Community` entity (Domain)
  - `CreateCommunityCommand` with validation
  - Cloudflare R2 upload for banners/icons
  - AutoMapper DTOs (CreateCommunityDto, CommunityDto)
  - `CommunityCreated` event
  - `GetCommunitiesQuery` with pagination
  - `/api/communities` endpoint
  - Tests with mock R2
  - API documentation

- **Task 5.2: Community Management UI** (18 SP)

  - `Communities.razor` page (list)
  - `CommunityCard.razor` component
  - Creation/edition modal (MudDialog)
  - Image upload with preview
  - Community detail page with tabs (Posts, Members, Settings)
  - SCSS responsive
  - bUnit tests

- **Task 5.3: Keycloak Roles Setup** (7 SP)
  - Create roles: Admin, Moderator, User, Premium
  - Configure custom claims
  - Map roles → JWT claims
  - Role-based authorization middleware
  - `[Authorize(Roles = "Admin")]` attributes
  - Authorization tests
  - Permission documentation

### ✅ Success Criteria

- Community created with R2 upload
- Event published
- Tests > 80% coverage
- Community CRUD functional
- Image upload working
- Mobile-friendly UI
- Roles propagated in JWT
- Role-based authorization functional
- Tests validate permissions

**⏱️ Estimated effort**: 20h (AI 12h + integration 8h)

---

## MILESTONE 6: Voting + Score Aggregation

**Due Date**: 2026-01-30

### 📦 Deliverables

- **Attention Limiting: daily cap on likes/reactions to encourage thoughtful engagement**
- Implement logic to limit number of likes/reactions per user per day
- CastVoteCommand (CQRS + validation)
- Vote Counter UI (MudChip + Redis cache)
- Vote Aggregation Worker (background service)

### Tasks

- **Task 6.1: Votes Module CQRS** (15 SP)

  - `Vote` entity (Domain)
  - `CastVoteCommand` (Up/Down/Remove)
  - Validation: 1 vote/user/post
  - Permission verification (banned users)
  - `VoteCast` event to RabbitMQ
  - `/api/votes` endpoint
  - Idempotence tests
  - Documentation

- **Task 6.2: Vote Counter UI + Redis Cache** (16 SP)

  - `VoteButtons.razor` component (MudChip)
  - Optimistic update (immediate UI)
  - Redis cache for scores (TTL 5 min)
  - Refresh if backend fails
  - Vote animations
  - bUnit tests

- **Task 6.3: Vote Aggregation Worker** (13 SP)
  - MassTransit consumer for `VoteCast`
  - Recalculate post score (upvotes - downvotes)
  - Invalidate Redis cache
  - Concurrency handling (locks)
  - Consumer tests
  - OTEL monitoring

### ✅ Success Criteria

- Unique vote per user/post
- Event published correctly
- Tests > 80%
- Instant vote in UI
- Fallback if error
- Efficient caching
- Scores recalculated correctly
- Cache invalidated
- Tests validate aggregation

**⏱️ Estimated effort**: 22h (AI 12h + integration 10h)

---

## MILESTONE 7: Nested Discussion Trees

**Due Date**: 2026-02-13

### 📦 Deliverables

- **Multilingual Threads: automatic translation and semantic coherence across languages**
- Implement thread translation and synchronization logic
- CreateThread endpoint (HEAD caching + GZIP)
- Threads UI (MudExpansionPanel + Redis drafts + PWA)
- AddComment endpoint (nested validation)
- Comments UI (inline replies + indentation)

### Tasks

- **Task 7.1: Threads Module CQRS** (12 SP)

  - `Thread` entity (Domain)
  - `CreateThreadCommand`
  - HTTP HEAD check for links
  - GZIP compression
  - `ThreadCreated` event
  - `/api/threads` endpoint
  - Tests

- **Task 7.2: Threads UI + Drafts** (18 SP)

  - `Thread.razor` component
  - MudExpansionPanel for hierarchy
  - Autosave drafts Redis (10s)
  - PWA manifest
  - Offline detection
  - Tests

- **Task 7.3: Comments Module CQRS** (15 SP)

  - `Comment` entity (recursive parent)
  - `AddCommentCommand`
  - Max depth validation (5 levels)
  - Circular reference guard
  - Recursive tree construction
  - Edge case tests

- **Task 7.4: Comments UI** (18 SP)
  - `Comment.razor` component (recursive)
  - Inline MudTextField replies
  - Visual indentation (CSS)
  - "Load more" button if too many replies
  - Performance tests (1000 comments)

### ✅ Success Criteria

- Thread created with validation
- Optimal performance
- Drafts auto-saved
- PWA installable
- Nesting up to 5 levels
- No circularity
- Tree displayed correctly
- Acceptable performance

**⏱️ Estimated effort**: 26h (AI 14h + integration 12h)

---

## MILESTONE 8: Report System + Audit Trail

**Due Date**: 2026-02-27

### 📦 Deliverables

- ReportPost endpoint (CQRS + hCaptcha + rate limits)
- Moderation Queue (RabbitMQ events)
- Moderation Audit Trail (OpenTelemetry instrumentation)

### Tasks

- **Task 8.1: Report Post Module** (14 SP)

  - `Report` entity (Domain)
  - `ReportPostCommand`
  - hCaptcha validation
  - Rate limit 5 reports/day/user
  - `PostReported` event → RabbitMQ
  - `/api/reports` endpoint
  - Tests

- **Task 8.2: Moderation Queue** (9 SP)

  - MassTransit consumer for `PostReported`
  - Report aggregation per post
  - Auto-flagging threshold (5 reports)
  - Moderator notifications
  - Consumer tests

- **Task 8.3: Moderation Audit Trail** (12 SP)
  - `ModerationAction` entity
  - EF Core event store
  - OpenTelemetry instrumentation
  - Audit log queries
  - Tests

### ✅ Success Criteria

- Report created with captcha
- Limits respected
- Reports aggregated correctly
- Notifications sent
- All actions logged
- OTEL traces visible

**⏱️ Estimated effort**: 24h (AI 12h + integration 12h)

---

## MILESTONE BETA: Full-Text Search Live

**Due Date**: 2026-03-13

### 📦 Deliverables

- Elasticsearch integration (indexing pipeline + relevance scoring)
- SearchPosts Query (CQRS + pagination)
- Search UI (MudAutocomplete + debounced suggestions)
- Advanced filters (date/author/community + Soundex fuzzy matching)

### Tasks

- **Task BETA.1: Elasticsearch Integration** (18 SP)

  - Add Elasticsearch to Docker Compose
  - NEST client for .NET
  - `PostCreated` consumer → ES index
  - Schema mapping (full-text, facets)
  - Relevance scoring
  - Index lag tests
  - Index health monitoring

- **Task BETA.2: Search Posts Query** (12 SP)

  - `SearchPostsQuery` with pagination
  - Filters: date, author, community
  - PostgreSQL FTS fallback if ES down
  - `/api/search` endpoint
  - Precision tests

- **Task BETA.3: Search UI MudBlazor** (18 SP)

  - `SearchBar.razor` component
  - MudAutocomplete with debounce (300ms)
  - Real-time suggestions
  - Results page with pagination
  - SCSS mobile-first
  - bUnit tests

- **Task BETA.4: Search Filters + Soundex** (15 SP)
  - Filter components (date, author, community)
  - Backend Soundex for fuzzy search
  - Soundex precision tests
  - Algorithm documentation

### ✅ Success Criteria

- Posts indexed < 1s
- Relevant search results
- Paginated search functional
- Fallback operational
- Instant suggestions
- Responsive UI
- Functional filters
- Soundex improves results

**⏱️ Estimated effort**: 28h (AI 16h + integration 12h)

**🚀 Launch**: BETA

---

## MILESTONE 9: Moderation Tools

**Due Date**: 2026-03-27

### 📦 Deliverables

- **Ethical Humor Lab: community space for co-creating inclusive, intelligent humor**
- Implement local learning of user humor style (no data leaves device)
- Build collaborative humor tools and moderation
- Admin Moderation UI (Angular MatTable + bulk ops)
- Admin Search Dashboard (MatPaginator + CSV export)
- Pending Posts Queue UI (ghost posts)
- SendGrid email integration (moderation notifications)

### Tasks

- **Task 9.1: Admin Moderation UI Angular** (20 SP)

  - Standalone Angular admin project
  - MatTable with sorting and filters
  - Approve/reject actions
  - Bulk operations (multi-selection)
  - Report detail modal
  - SCSS responsive
  - Jasmine/Karma tests

- **Task 9.2: Admin Search Dashboard** (14 SP)

  - Search component with MatPaginator
  - Advanced filters
  - CSV export (QuestPDF backend)
  - Tests

- **Task 9.3: Pending Posts Queue UI** (12 SP)

  - `PendingPosts.razor` component
  - Author-only visibility
  - Queue status
  - Visibility tests

- **Task 9.4: SendGrid Email Integration** (9 SP)
  - SendGrid client
  - Email templates (approved, rejected)
  - Moderation event consumer
  - Tests (mock SendGrid)

### ✅ Success Criteria

- Reports list functional
- Bulk actions working
- Admin search functional
- CSV export working
- Pending posts visible only to author and mods
- Emails sent on decisions
- HTML templates correct

**⏱️ Estimated effort**: 24h (AI 14h + integration 10h)

---

## MILESTONE 10: Consistent Brand Across UIs

**Due Date**: 2026-04-10

### 📦 Deliverables

- Unified SCSS/Tailwind theme (colors + typography)
- Component library documentation (catalog + usage examples)
- Mobile responsiveness audit (320px/768px/1024px breakpoints)

### Tasks

- **Task 10.1: SCSS/Tailwind Theme System** (14 SP)

  - `_variables.scss` file (colors, fonts)
  - Tailwind config tokens
  - Color palette (primary, secondary, accents)
  - Typography (sizes, weights)
  - Apply to MudBlazor and Angular components
  - Visual consistency audit
  - Design token documentation

- **Task 10.2: Component Library Documentation** (10 SP)

  - Storybook page or equivalent
  - Documentation for each used component
  - Code examples
  - Usage guidelines
  - Visual tests

- **Task 10.3: Mobile Responsiveness Audit** (10 SP)
  - Tests 320px, 768px, 1024px
  - List responsive issues
  - CSS corrections
  - Real device tests (BrowserStack)
  - Breakpoint documentation

### ✅ Success Criteria

- Unified theme for Blazor and Angular
- Complete documentation
- All components documented
- Clear examples
- All pages responsive
- Zero major issues

**⏱️ Estimated effort**: 18h (AI 8h + integration 10h)

---

## MILESTONE 11: Automated Content Moderation

**Due Date**: 2026-04-24

### 📦 Deliverables

- Custom AI Service (Python FastAPI + scikit-learn/PyTorch + Redis queue + Hugging Face fallback)
- AI Model Monitoring (confidence tracking + retraining triggers + OTEL metrics)
- AI Moderation UI (confidence % display + appeal workflow)

### Tasks

- **Task 11.1: AI Service Python FastAPI** (30 SP)

  - Python FastAPI project
  - `/analyze` endpoint (text → score)
  - scikit-learn model (TF-IDF + Logistic Regression)
  - PyTorch model (BERT fine-tuned)
  - Redis job queue (Celery or RQ)
  - Hugging Face fallback (external API)
  - Model tests (accuracy, recall)
  - Python Dockerfile
  - API documentation

- **Task 11.2: AI Model Monitoring** (12 SP)

  - `AIAnalysis` table (post, score, confidence)
  - OTEL metrics (latency, average confidence)
  - False positive flagging (user feedback)
  - Re-training trigger (accuracy < 80%)
  - Grafana AI metrics dashboard
  - Monitoring tests

- **Task 11.3: AI Moderation UI** (15 SP)
  - `AIScoreBadge.razor` component
  - Confidence % display per post
  - "Appeal decision" form for moderators
  - Integration with moderation queue
  - bUnit tests

### ✅ Success Criteria

- Text analysis functional
- Accuracy > 85%
- Fallback operational
- Metrics collected
- Alerts configured
- AI score visible
- Appeal workflow functional

**⏱️ Estimated effort**: 32h (AI 20h + integration 12h)

---

## MILESTONE 12: Karma & Progression Visible

**Due Date**: 2026-05-08

### 📦 Deliverables

- Gamification Karma System (CQRS + event-driven + decay rules)
- Badges & Levels UI (MudBadge + progression bars)
- User Profile UI (karma/level/badges + activity breakdown)
- **Contextual reputation: per-community, constructive, no global vanity metrics**

### Tasks

- **Task 12.1: Gamification Karma System** (18 SP)

  - `Karma` entity (User, Points, LastUpdated)
  - `KarmaChangedCommand`
  - Event-driven: PostCreated (+5), VoteCast (+1/-1), CommentAdded (+2)
  - Decay rules (karma decreases if inactive)
  - PostgreSQL aggregation
  - Karma rule tests
  - Formula documentation

- **Task 12.2: Badges & Levels** (16 SP)

  - `Badge` and `Level` entities
  - Level calculation logic
  - Badge assignment rules
  - `BadgesAndLevels.razor` component
  - MudBadge for level/badge display
  - Animated progress bars for XP
  - Tests

- **Task 12.3: User Profile UI** (14 SP)
  - `UserProfile.razor` component
  - Karma, badges, post/vote stats
  - Responsive layout, mobile-first
  - Activity breakdown
  - Tests

### ✅ Success Criteria

- Karma calculated automatically
- Decay functional
- Tests validate formulas
- Badges assigned correctly
- Levels calculated properly
- UI displays progression
- Profile shows complete stats

**⏱️ Estimated effort**: 26h (AI 14h + integration 12h)

---

## MILESTONE 13: User Alerts System

**Due Date**: 2026-05-22

### 📦 Deliverables

- **Smart Disconnect Mode: suggest breaks for digital well-being based on cognitive fatigue detection**
- Implement cognitive fatigue detection and break suggestion logic
- Notifications Service (CQRS polling + EF Core event store)
- Notifications UI (MudSnackbar + notification center + preferences)
- Discord Webhooks (post events → community channels + opt-in)

### Tasks

- **Task 13.1: Notifications Service** (16 SP)

  - `Notification` entity
  - `PollStatusQuery` with EF Core store
  - Aggregate unread notifications
  - Notification preferences
  - Tests

- **Task 13.2: Notifications UI** (14 SP)

  - `Notifications.razor` component
  - MudSnackbar for live alerts
  - Notification center modal
  - User preferences
  - Tests

- **Task 13.3: Discord Webhooks** (10 SP)
  - Discord webhook integration
  - Community event → Discord message
  - Opt-in per community
  - Tests

### ✅ Success Criteria

- Notifications stored and retrieved
- Unread count accurate
- Live alerts functional
- Notification center working
- Discord messages sent
- Opt-in respected

**⏱️ Estimated effort**: 24h (AI 12h + integration 12h)

---

## MILESTONE 14: Premium Features Ready

**Due Date**: 2026-06-05

### 📦 Deliverables

- **Community governance: collective rule/algorithm decisions, elected moderators**
- Implement voting system for community rules and moderator elections
- Neo4j Recommendations (user→user/post→post graph + recommendation queries)
- Keycloak Premium Claims (feature gates for advanced search/no ads)
- k6 Scale Tests (100 concurrent users)
- Performance Optimization (query indexes + n+1 elimination + caching review)

### Tasks

- **Task 14.1: Neo4j Graph Module** (18 SP)

  - Neo4j integration
  - User→Post, Post→Post relationships
  - Recommendation queries by topic similarity
  - Graph traversal optimization
  - Tests

- **Task 14.2: Keycloak Premium Claims** (10 SP)

  - VIP/Premium flag propagation
  - Feature gates for premium-only features
  - Claims validation middleware
  - Tests

- **Task 14.3: Performance Tests** (12 SP)
  - k6 tests for 100 concurrent users
  - Identify caching & query bottlenecks
  - Performance optimization
  - Load test documentation

### ✅ Success Criteria

- Related posts shown
- Premium users unlock perks
- System handles 100 concurrent users
- k6 report shows acceptable latencies
- Bottlenecks documented

**⏱️ Estimated effort**: 28h (AI 14h + integration 14h)

---

## MILESTONE V1.0: Full System Visibility

**Due Date**: 2026-06-19

### 📦 Deliverables

- Prometheus + Grafana Stack (OTEL metrics: queue depth/latency/errors/cache hits)
- Alert Rules & Runbooks (SLA violations: response time >500ms/error rate >1%)
- Logging Aggregation (ELK: search + retention policies)

### Tasks

- **Task V1.0.1: Prometheus + Grafana Setup** (16 SP)

  - Prometheus configuration
  - Grafana dashboards for backend + AI worker
  - OTEL metrics collection (queue depth, latency)
  - Dashboard creation
  - Tests

- **Task V1.0.2: Alert Rules & Runbooks** (12 SP)

  - Alert rules for latency/error thresholds
  - Runbooks for on-call actions
  - Alert testing
  - Documentation

- **Task V1.0.3: Centralized Logging (ELK)** (14 SP)
  - Log aggregation setup
  - Search functionality
  - Retention policies
  - Tests

### ✅ Success Criteria

- Grafana dashboard live
- Logs searchable
- Alerts firing on thresholds
- Full observability achieved

**⏱️ Estimated effort**: 26h (AI 12h + integration 14h)

**🚀 Launch**: V1.0

---

## MILESTONE 15: Production-Ready Deployment

**Due Date**: 2026-07-03

### 📦 Deliverables

- Kubernetes Cluster Setup (Helm charts for all services: .NET/Python/PostgreSQL/Redis/RabbitMQ + local KinD testing)
- Hangfire Background Jobs (cleanup tasks + notification batches + cache invalidation)
- Auto-Scaling Policies (HPA rules + CPU/memory limits + load testing)
- Database Sharding Plan (horizontal scaling design doc)

### Tasks

- **Task 15.1: Kubernetes Cluster Setup** (16 SP)

  - Helm charts for all services
  - Local KinD/k3s cluster testing
  - Service configuration
  - Tests

- **Task 15.2: Hangfire Background Jobs** (12 SP)

  - Hangfire configuration
  - Cleanup tasks
  - Notification batches
  - Cache invalidation
  - Tests

- **Task 15.3: Auto-Scaling Policies** (10 SP)

  - HPA rules configuration
  - CPU/memory limits
  - Load testing
  - Documentation

- **Task 15.4: Database Sharding Plan** (6 SP)
  - Horizontal scaling design document
  - Sharding strategy
  - Migration plan
  - Documentation

### ✅ Success Criteria

- `helm deploy prod` successfully deploys all services
- Auto-scaling validated under load
- Background jobs functional
- Sharding plan documented

**⏱️ Estimated effort**: 28h (AI 14h + integration 14h)

---

## MILESTONE 16: Data Privacy Complete (GDPR)

**Due Date**: 2026-07-17

### 📦 Deliverables

- GDPR Export Module (QuestPDF: JSON + PDF for posts/karma/settings)
- Export UI (MudBlazor: download management + email delivery)
- GDPR Full Deletion (anonymization + soft-delete cascade validation)
- Privacy Policy & Consent (cookie banner + opt-in tracking preferences)

### Tasks

- **Task 16.1: GDPR Export Module** (16 SP)

  - Export endpoints
  - QuestPDF integration
  - JSON + PDF generation
  - Data accuracy validation
  - Tests

- **Task 16.2: Export UI** (12 SP)

  - MudBlazor export dialog
  - Download link management
  - Email delivery
  - Tests

- **Task 16.3: GDPR Full Deletion** (14 SP)

  - Anonymization logic
  - Soft-delete cascade validation
  - Data integrity verification
  - Tests

- **Task 16.4: Privacy Policy & Consent** (10 SP)
  - Cookie banner
  - Opt-in tracking preferences
  - Privacy policy page
  - Tests

### ✅ Success Criteria

- Users can export their data
- Users can delete accounts
- All data anonymized on deletion
- GDPR compliance achieved

**⏱️ Estimated effort**: 28h (AI 12h + integration 16h)

---

## MILESTONE 17: Monetization Live

**Due Date**: 2026-07-31

### 📦 Deliverables

- **Contribution-based monetization: reward quality and time spent, not views or attention capture**
- Implement monetization logic based on user contribution and engagement quality
- Stripe Integration (webhook handler + subscription management + billing portal)
- Google OAuth (multi-provider auth: Discord + Google)
- Premium Feature UI (upgrade prompts + pricing table + subscription status)
- Premium Claims Sync (Stripe → Keycloak role + feature gate validation)

### Tasks

- **Task 17.1: Stripe Integration** (20 SP)

  - Stripe client setup
  - Webhook handler
  - Subscription management
  - Billing portal
  - Tests

- **Task 17.2: Google OAuth** (12 SP)

  - Google OAuth provider
  - Multi-provider auth flow
  - Integration with existing auth
  - Tests

- **Task 17.3: Premium Feature UI** (16 SP)

  - Upgrade prompts
  - Pricing table
  - Subscription status
  - Tests

- **Task 17.4: Premium Claims Sync** (10 SP)
  - Stripe → Keycloak sync
  - Feature gate validation
  - Tests

### ✅ Success Criteria

- Users can sign up
- Users can upgrade to premium
- Features enabled correctly
- Stripe charges successfully

**⏱️ Estimated effort**: 30h (AI 14h + integration 16h)

---

## MILESTONE 18: Security Audit Complete

**Due Date**: 2026-08-14

### 📦 Deliverables

- Security Testing (OWASP Top 10: SQLi/XSS/CSRF test cases)
- Secrets Management (env vars + rotation policy + audit logging)
- Rate Limit Tuning (DDoS protection + endpoint-specific limits + bypass tokens)

### Tasks

- **Task 18.1: Security Testing** (12 SP)

  - OWASP Top 10 test cases
  - SQL injection tests
  - XSS tests
  - CSRF tests
  - Documentation

- **Task 18.2: Secrets Management** (10 SP)

  - Environment variables setup
  - Rotation policy
  - Audit logging
  - Tests

- **Task 18.3: Rate Limit Tuning** (8 SP)
  - DDoS protection
  - Endpoint-specific limits
  - Bypass tokens
  - Tests

### ✅ Success Criteria

- Security audit report signed off
- App meets OWASP criteria
- Secrets properly managed
- Rate limits tuned

**⏱️ Estimated effort**: 24h (AI 8h + integration 16h)

---

## MILESTONE PRODUCTION: Ready for Production

**Due Date**: 2026-08-28

### 📦 Deliverables

- OpenAPI/Swagger Documentation (full API docs + endpoint examples + auth flow diagrams + error codes)
- Deployment Guides (local docker-compose + staging KinD + production Kubernetes step-by-step)
- Architecture & ADR Finalization (all decisions + trade-offs from 52 weeks documented)
- Launch Checklist & Go-Live (staging validation + production smoke tests + monitoring setup + incident response readiness)

### Tasks

- **Task PRODUCTION.1: OpenAPI/Swagger Documentation** (10 SP)

  - Full API documentation
  - Endpoint examples
  - Auth flow diagrams
  - Error codes documentation
  - Tests

- **Task PRODUCTION.2: Deployment Guides** (12 SP)

  - Local docker-compose guide
  - Staging KinD guide
  - Production Kubernetes guide
  - Step-by-step instructions
  - Tests

- **Task PRODUCTION.3: Architecture & ADR Finalization** (8 SP)

  - All decisions documented
  - Trade-offs from 38 weeks
  - Final review
  - Documentation

- **Task PRODUCTION.4: Launch Checklist & Go-Live** (12 SP)
  - Staging validation
  - Production smoke tests
  - Monitoring setup
  - Incident response readiness
  - Go-live execution

### ✅ Success Criteria

- System live at production URL
- Monitoring active
- Team trained
- Runbooks ready
- Full documentation complete

**⏱️ Estimated effort**: 20h (AI 6h + integration 14h)

**🚀 Launch**: PRODUCTION

---

## Summary

**Total Duration**: 38 weeks (November 7, 2025 → September 11, 2026)  
**Total Effort**: ~500 hours (AI: ~300h + Integration: ~200h)  
**Major Launches**: ALPHA (Jan 16), BETA (Mar 27), V1.0 (Jul 3), PRODUCTION (Sep 11)

### Key Metrics

- **Latency**: <150ms P95
- **Test Coverage**: >80%
- **AI Accuracy**: >90% clean posts
- **User Engagement**: >20 karma/week/user

### Technology Stack

- **Backend**: .NET 10, FastEndpoints, EF Core, MassTransit, RabbitMQ
- **Frontend**: Blazor WASM + MudBlazor, Angular + Material
- **Infrastructure**: PostgreSQL 18, Redis 8, Keycloak, Aspire, Docker
- **AI**: Python FastAPI, scikit-learn, PyTorch, Hugging Face
- **Observability**: OpenTelemetry, Prometheus, Grafana

This roadmap provides a comprehensive 38-week development plan with clear milestones, deliverables, and success criteria for building a modern, scalable forum platform.
