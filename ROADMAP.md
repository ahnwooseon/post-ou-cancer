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

**Success**: xUnit tests >80% coverage with Shouldly/NSubstitute/Bogus

**Dependencies**: Prerequisites

**Risks**: Mitigations included

---

## PI-1: Foundation & Trust Architecture 🔒

**Duration**: 8 Weeks (Nov 15, 2025 – Jan 9, 2026)
**Goal**: Establish solid technical foundation with authentication and **privacy-first core**.

### Sprint 1.1 (Nov 15 - Nov 28, 2025)

- Milestone 0: Architecture Blueprint
- Milestone 1: Local Dev Environment Ready

### Sprint 1.2 (Nov 29 - Dec 12, 2025)

- Milestone 2: Automated Testing Pipeline

### Sprint 1.3 (Dec 13 - Dec 26, 2025)

- Milestone 3: Auth System Functional

### Sprint 1.4 (Dec 27, 2025 - Jan 9, 2026)

- Milestone 4: Login Flow Complete

---

## PI-2: Core Social Features & Governance 🏛️

**Duration**: 10 Weeks (Jan 10, 2026 – Mar 20, 2026)
**Goal**: Build core functionality (posts, communities, voting, discussions, moderation) **and modular governance tools**.

### Sprint 2.1 (Jan 10 - Jan 23, 2026)

- Milestone ALPHA: Posts MVP + Performance Baseline 🚀

### Sprint 2.2 (Jan 24 - Feb 6, 2026)

- Milestone 5: Multi-Community Support

### Sprint 2.3 (Feb 7 - Feb 20, 2026)

- Milestone 6: Voting + Score Aggregation

### Sprint 2.4 (Feb 21 - Mar 6, 2026)

- Milestone 7: Nested Discussion Trees

### Sprint 2.5 (Mar 7 - Mar 20, 2026)

- Milestone 8: Report System + Audit Trail

---

## PI-3: Intelligence & Modularity 🧠

**Duration**: 10 Weeks (Mar 21, 2026 – May 29, 2026)
**Goal**: Add AI moderation, **contextual reputation**, notifications, and premium features

### Sprint 3.1 (Mar 21 - Apr 3, 2026)

- Milestone BETA: Full-Text Search Live 🚀

### Sprint 3.2 (Apr 4 - Apr 17, 2026)

- Milestone 9: Moderation Tools

### Sprint 3.3 (Apr 18 - May 1, 2026)

- Milestone 10: Consistent Brand Across UIs

### Sprint 3.4 (May 2 - May 15, 2026)

- Milestone 11: Automated Content Moderation

### Sprint 3.5 (May 16 - May 29, 2026)

- Milestone 12: Karma & Progression Visible

---

## PI-4: Production & Compliance

**Duration**: 16 Weeks (May 30, 2026 – Sep 18, 2026)
**Goal**: Scale to production, ensure compliance, and launch monetization

### Sprint 4.1 (May 30 - Jun 12, 2026)

- Milestone 13: User Alerts System

### Sprint 4.2 (Jun 13 - Jun 26, 2026)

- Milestone 14: Premium Features Ready

### Sprint 4.3 (Jun 27 - Jul 10, 2026)

- Milestone V1.0: Full System Visibility 🚀

### Sprint 4.4 (Jul 11 - Jul 24, 2026)

- Milestone 15: Production-Ready Deployment

### Sprint 4.5 (Jul 25 - Aug 7, 2026)

- Milestone 16: Data Privacy Complete (GDPR)

### Sprint 4.6 (Aug 8 - Aug 21, 2026)

- Milestone 17: Monetization Live

### Sprint 4.7 (Aug 22 - Sep 4, 2026)

- Milestone 18: Security Audit Complete

### Sprint 4.8 (Sep 5 - Sep 18, 2026)

- Milestone PRODUCTION: Ready for Production 🚀

---

## MILESTONE 0: Architecture Blueprint

**Due Date**: 2025-11-21

### 📦 Deliverables

- ADR documentation (8 ADRs minimum)
- OWASP/GDPR baseline security guidelines
- Docker Compose + Aspire setup
- Environment configuration documentation

### Tasks

- ✅ **Task 0.1: Architecture Decision Records** (8 SP)

  - Create `/docs/adr/` folder structure
  - ADR-001: .NET 10 choice and justification
  - ADR-002: Modular monolith vs microservices architecture
  - ADR-003: Aspire for orchestration and observability
  - ADR-004: PostgreSQL as primary database
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

**Due Date**: 2025-11-28

### 📦 Deliverables

- Monorepo + Aspire orchestration
- PostgreSQL/Redis/RabbitMQ health checks
- EF Core migrations with auto-migration
- OpenTelemetry logging and tracing
- Docker Compose environment

### Tasks

- **Task 1.1: Monorepo + Aspire AppHost** (10 SP)

  - Create solution structure: `src/`, `tests/`, `infrastructure/`
  - `PostOuCancer.AppHost` project for orchestration
  - `PostOuCancer.ServiceDefaults` project
  - `PostOuCancer.ApiService` project (API)
  - `PostOuCancer.Web` project (Blazor Server)
  - `PostOuCancer.Web.Client` project (Blazor WASM)
  - `PostOuCancer.Domain` project (entities, value objects, repository interfaces)
  - `PostOuCancer.Application` project (use cases, DTOs, service interfaces, CQRS handlers)
  - `PostOuCancer.Infrastructure` project (EF Core, repository implementations, external service implementations)
  - `PostOuCancer.Admin` project (Angular)
  - Configure AppHost with PostgreSQL, Redis, RabbitMQ services
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

- **Task 1.3: Data Access Framework** (10 SP)

  - Install EF Core + Npgsql packages (for writes)
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

- **Task 1.5: Containerization & Docker Compose Environment** (8 SP)
  - Create Dockerfiles for each service:
    - `PostOuCancer.ApiService/Dockerfile` (.NET 10)
    - `PostOuCancer.Web/Dockerfile` (Blazor Server)
    - `PostOuCancer.Admin/Dockerfile` (Angular)
    - `workers/python-ai-worker/Dockerfile` (Python FastAPI)
  - Create `infrastructure/docker-compose.yml` with all services:
    - PostgreSQL with persistent volumes
    - Redis 8 for caching and rate-limiting
    - RabbitMQ 3 for messaging
    - Python FastAPI worker for AI
    - Keycloak for authentication
  - Configure `appsettings.Development.json` with Docker service connection strings:
    - PostgreSQL connection string
    - Redis connection string
    - RabbitMQ connection string
    - Keycloak endpoints
  - Network configuration between containers
  - Startup and shutdown scripts (`infrastructure/start.sh`, `infrastructure/stop.sh`)
  - `.dockerignore` files for optimization
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

**Due Date**: 2025-12-12

### 📦 Deliverables

- GitHub Actions CI/CD pipeline
- xUnit + Shouldly + NSubstitute + Bogus framework
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
  - Test examples with containers
  - Parallel execution configuration
  - Test pattern documentation

- **Task 2.3: xUnit + Shouldly + NSubstitute + Bogus** (8 SP)

  - `PostOuCancer.Tests.Shared` project
  - Test entity builders (Builder pattern)
  - Custom Shouldly extensions
  - Reusable NSubstitute mocks (repositories, services)
  - Bogus for test data generation
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

**Due Date**: 2025-12-26

### 📦 Deliverables

- Keycloak + Discord OAuth integration
- JWT validation middleware
- Redis rate-limiting
- YARP API Gateway
- **Opt-in identity verification with a "certified" badge.**
- **Selective end-to-end encryption for DMs, private circles, and drafts (Zero Knowledge Graph)**

### Tasks

- **Task 3.1: Keycloak + Discord OAuth & Selective Encryption** (28 SP)
- Deploy Keycloak in Docker Compose
- **Implement selective end-to-end encryption for DMs, private circles, and drafts**
- Ensure public content remains readable and transparent
- Create realm `postoucancer`
- Configure OAuth client (confidential)
- Add Discord as Identity Provider
- Map Discord claims → Keycloak
- Configure JWT (RS256, 15 min duration)
- Refresh tokens (7 days duration)
- JWT validation middleware in API
- End-to-end OAuth flow tests
  - **Task 3.1.1: Client-Side Key Management & Recovery** (8 SP)
    - Implement a secure flow for generating and storing the user's private key for the Zero Knowledge Graph.
    - Create a UI flow that strongly encourages the user to back up a recovery phrase (e.g., a 24-word mnemonic).
    - Display clear, unmissable warnings explaining that losing this phrase means permanent loss of access to encrypted data.
    - Implement a key recovery mechanism using the backed-up phrase.
    - Write end-to-end tests for the key backup and recovery process.

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

- **Task 3.4: Real Name Identity Verification (Yoti)** (15 SP)
  - Research and select an identity verification provider (e.g., Yoti, Veriff).
  - Implement backend API to handle verification webhooks from the provider.
  - Add `IsVerified` boolean flag to the `User` entity in the Identity module.
  - Create a `CertifiedBadge.razor` component to display on user profiles if `IsVerified` is true.
  - Develop a simple UI flow for users to initiate the verification process.
  - End-to-end tests for the verification flow.

### ✅ Success Criteria

- **Task 3.5: Two-Factor Authentication (2FA)** (13 SP)
  - Configure Keycloak's OTP policy.
  - Implement a Blazor UI for users to enable/disable 2FA using an authenticator app (QR code).
  - Generate and display one-time recovery codes upon 2FA activation.
  - Add a step in the login flow to prompt for the 2FA code if it's enabled.
  - Write end-to-end tests for the 2FA login flow and recovery process.

### ✅ Success Criteria

- Discord login functional
- Valid JWT issued by Keycloak
- **Zero Knowledge encryption operational for private content**
- **Users can complete the identity verification flow and receive a "certified" badge.**
- Refresh token flow operational
- Rate-limiting active on all endpoints
- Users can enable 2FA with an authenticator app and use it to log in.
- Configurable limits per environment
- Informative headers in responses
- Requests correctly routed
- Health checks functional
- Structured request logs

**⏱️ Estimated effort**: 30h (AI 19h + integration 11h)
**⏱️ Estimated effort**: 35h (AI 22h + integration 13h)

---

## MILESTONE 4: Login Flow Complete

**Due Date**: 2026-01-09

### 📦 Deliverables

- Blazor WASM Discord login UI (MudBlazor)
- Session management (JWT refresh + auto-renewal)
- Global exception handler
- **Functional persistence layer for local-first storage.**

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
  - **Persistence layer for local-first storage logic (offline-first, explicit cloud sync)**

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

**Due Date**: 2026-01-23

### 📦 Deliverables

- A functional Posts MVP built with a CQRS architecture.
- A denormalized read model for posts, populated by asynchronous events.
- A responsive Blazor UI for creating and viewing posts.
- Baseline performance metrics established with k6 load tests.

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
  - **Dapper for optimized reads**, GZIP storage in PostgreSQL `bytea`
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

**Due Date**: 2026-02-06

### 📦 Deliverables

- A community management module with a CQRS-based API.
- A Blazor UI for community administration, including Universe type selection.
- Role-based access control (Admin, Mod, User) integrated with Keycloak.
- CreateCommunity API (CQRS + R2 storage)

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

## MILESTONE 6: Reactions & Voting System

**Due Date**: 2026-02-20

### 📦 Deliverables

- **A flexible reaction system (likes, laughs, etc.) that respects Universe-level metric settings.**
- An "Attention Limiting" feature capping the total number of reactions a user can give per day.
- A responsive UI component that displays the total reaction count and shows a detailed breakdown on hover.

### Tasks

- **Task 6.1: Votes Module CQRS** (15 SP)

  - `Vote` entity (Domain)
  - `CastVoteCommand` (Up/Down/Remove)
  - Validation: 1 reaction/user/post
  - Permission verification (banned users)
  - `VoteCast` event to RabbitMQ
  - `/api/votes` endpoint
  - Idempotence tests
  - Documentation

- **Task 6.2: Reaction UI Component** (16 SP)

  - `Reactions.razor` component.
  - Optimistic update (immediate UI)
  - Displays total reaction count.
  - On hover, a `MudMenu` or `MudTooltip` shows the breakdown of each reaction type and its count (e.g., "😂: 12, 👍: 5").
  - Redis caching for reaction counts to reduce database load.
  - bUnit tests for UI logic and display.

- **Task 6.3: Reaction Aggregation Worker** (13 SP)
  - MassTransit consumer for `ReactionCast` event.
  - Recalculates post scores based on the configured value of each reaction type.
  - Invalidates the relevant Redis cache keys.
  - Handles concurrency to prevent race conditions.
  - Consumer integration tests.
  - OpenTelemetry monitoring for queue depth and processing time.

### ✅ Success Criteria

- Unique reaction per user/post is enforced.
- Event published correctly
- Tests > 80%
- UI updates optimistically and displays the total count.
- Hovering over the count displays a correct breakdown of reaction types.
- Fallback if error
- Efficient caching
- Reaction scores are recalculated correctly by the background worker.
- Cache invalidated
- Tests validate aggregation

**⏱️ Estimated effort**: 22h (AI 12h + integration 10h)

---

## MILESTONE 7: Nested Discussion Trees

**Due Date**: 2026-03-06

### 📦 Deliverables

- A complete system for creating and viewing nested discussion threads.
- A recursive comment creation API with depth validation.
- A responsive Blazor UI for displaying threaded conversations with draft auto-saving.

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

- **Task 7.5: GIF Integration (Giphy)** (12 SP)
  - Integrate the Giphy SDK/API into the Blazor comment/post editor.
  - Create a `GifPicker.razor` component that opens in a MudDialog.
  - Implement a one-time privacy consent dialog before the first use, explaining that search data is sent to Giphy.
  - Add a button to the editor toolbar to open the GIF picker.
  - Write bUnit tests for the picker component and the privacy consent flow.
  - Document the API key configuration in the secrets management strategy.

- **Task 7.6: Native Sharing Integration** (8 SP)
  - Create a `ShareService` in Blazor to abstract the Web Share API.
  - Implement a fallback to "copy to clipboard" for browsers that do not support the Web Share API.
  - Add a "Share" button with an icon to the `PostCard.razor` and `Comment.razor` components.
  - The button should call the `ShareService` with the post/comment's canonical URL.
  - Write bUnit tests to verify the correct API is called or the fallback is triggered.
  - Document the feature and its browser compatibility.

### ✅ Success Criteria

- Thread created with validation
- Optimal performance
- Drafts auto-saved
- PWA installable
- Nesting up to 5 levels
- No circularity
- Tree displayed correctly
- Users can search for and insert GIFs from Giphy after accepting a privacy notice.
- Users can share content using their device's native share sheet.
- Acceptable performance

**⏱️ Estimated effort**: 34h (AI 18h + integration 16h)

---

## MILESTONE 8: Report System + Audit Trail

**Due Date**: 2026-03-20

### 📦 Deliverables

- A user content reporting system with hCaptcha and rate-limiting.
- An asynchronous moderation queue that aggregates reports.
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

**Due Date**: 2026-04-03

### 📦 Deliverables

- Full-text search functionality powered by Elasticsearch.
- A search API with advanced filtering and fuzzy matching capabilities.
- A responsive Blazor UI with debounced, real-time search suggestions.
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

**Due Date**: 2026-04-17

### 📦 Deliverables

- A dedicated Angular application for administrators.
- A moderation dashboard for reviewing and acting on reported content.
- An integrated email notification system for moderation actions.
- **Internationalization (i18n) support for both Blazor and Angular UIs.**
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

- **Task 9.5: Internationalization (i18n) Framework** (15 SP)
  - Set up `.resx` resource files for the Blazor WASM application.
  - Implement a language selector component (`LanguageSelector.razor`).
  - Configure the Angular i18n framework (`ng-extract-i18n`).
  - Provide initial translation files (e.g., `messages.fr.xlf`) for both apps.
  - Create unit tests to verify that language switching works correctly.
  - Document the process for adding new languages.

### ✅ Success Criteria

- Reports list functional
- Bulk actions working
- Admin search functional
- CSV export working
- Pending posts visible only to author and mods
- Emails sent on decisions
- HTML templates correct
- UI can be switched between at least two languages (e.g., English and French).
- Adding a new language is a documented and straightforward process.

**⏱️ Estimated effort**: 30h (AI 18h + integration 12h)

---

## MILESTONE 10: Consistent Brand Across UIs

**Due Date**: 2026-05-01

### 📦 Deliverables

- A unified design system and theme applied across the Blazor and Angular UIs.
- A documented component library and a successful mobile responsiveness audit.
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
- Accessibility audit (axe-core) passes with no critical errors.
- Keyboard navigation is fully functional across all interactive elements.

**⏱️ Estimated effort**: 18h (AI 8h + integration 10h)

---

## MILESTONE 11: Automated Content Moderation

**Due Date**: 2026-05-15

### 📦 Deliverables

- An AI-powered content moderation service using a Python/FastAPI worker.
- A monitoring dashboard for AI model performance and confidence scores.
- A browser-based "Local AI" worker for privacy-preserving tasks like summarization.
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

**Due Date**: 2026-05-29

### 📦 Deliverables

- A contextual reputation system where karma and progression are scoped per-community.
- An event-driven system for calculating karma based on user actions.
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

**Due Date**: 2026-06-12

### 📦 Deliverables

- A complete user notification system with real-time alerts and preferences.
- A "Digital Wellbeing" feature that suggests breaks based on activity.
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

**Due Date**: 2026-06-26

### 📦 Deliverables

- A user-configurable "Transparent Feed" with UI controls for ranking logic.
- A graph-based recommendation engine using Neo4j.
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

**Due Date**: 2026-07-10

### 📦 Deliverables

- A production-grade observability stack (Prometheus, Grafana, ELK).
- Pre-configured alert rules and runbooks for critical SLA violations.
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

**Due Date**: 2026-07-24

### 📦 Deliverables

- A complete set of Helm charts for deploying all services to Kubernetes.
- A background job processing system using Hangfire for cleanup and batch tasks.
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

**Due Date**: 2026-08-07

### 📦 Deliverables

- A GDPR-compliant module for users to export all their data in JSON and PDF formats.
- A fully tested process for complete user data deletion and anonymization.
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

**Due Date**: 2026-08-21

### 📦 Deliverables

- A complete monetization system using Stripe for premium subscriptions.
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

**Due Date**: 2026-09-04

### 📦 Deliverables

- A comprehensive security audit report confirming mitigation of OWASP Top 10 vulnerabilities.
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

**Due Date**: 2026-09-18

### 📦 Deliverables

- Complete and finalized API documentation, deployment guides, and architectural records.
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
  - Trade-offs from 44 weeks
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

## PI-5: Blockchain Integration & Tokenization 🔗

**Duration**: 12 Weeks (Sep 19, 2026 – Dec 12, 2026)
**Goal**: Integrate a blockchain to create an immutable audit trail for events and lay the groundwork for a native token economy.

### Phase 1: Immutable Audit Trail (6 Weeks)

- **Milestone 19: Blockchain PoC Complete**
  - **Deliverables**: A working Proof-of-Concept using Stratis/Cirrus to anchor application events.
  - **Tasks**:
    - **Task 19.1: Stratis/Cirrus SDK Integration** (15 SP): Set up a test project, connect to a testnet, and demonstrate writing/reading a transaction.
    - **Task 19.2: Event Anchoring Service** (18 SP): Develop a service that listens to internal application events (e.g., `PostCreated`) and anchors their hash on the blockchain.
    - **ADR-009**: Formally document the architecture and decision.

- **Milestone 20: Production Integration**
  - **Deliverables**: The event anchoring service is integrated into the main application and deployed.
  - **Tasks**:
    - **Task 20.1: Integrate Anchoring Service** (13 SP): Wire the service into the `PostOuCancer.Infrastructure` project, triggered by MassTransit consumers.
    - **Task 20.2: Monitoring & Alerting** (10 SP): Add OpenTelemetry metrics for blockchain transactions (success, failure, cost) and alerts for failures.

### Phase 2: Native Token Foundation (6 Weeks)

- **Milestone 21: Tokenomics & Smart Contract Design**
  - **Deliverables**: A detailed document outlining the purpose, supply, and distribution of the new token (Tokenomics). A C# smart contract for the token.
  - **Tasks**:
    - **Task 21.1: Tokenomics Definition** (20 SP): Define the economic model of the token (utility, rewards, sinks).
    - **Task 21.2: Stratis C# Smart Contract** (25 SP): Develop the core ERC-20-like token smart contract in C#.

- **Milestone 22: Wallet & Transaction PoC**
  - **Deliverables**: A basic in-app wallet UI and a PoC for token transfers.
  - **Tasks**:
    - **Task 22.1: Basic Wallet UI** (18 SP): Create a Blazor UI for users to view their balance and transaction history.
    - **Task 22.2: Token Transfer API** (20 SP): Develop an API endpoint to initiate token transfers between users.

---

## Summary

**Total Duration**: 44 weeks (November 15, 2025 → September 18, 2026)
**Total Effort**: ~500 hours (AI: ~300h + Integration: ~200h)  
**Major Launches**: ALPHA (Jan 23, 2026), BETA (Apr 3, 2026), V1.0 (Jul 10, 2026), PRODUCTION (Sep 18, 2026)

### Key Metrics

- **Latency**: <150ms P95
- **Test Coverage**: >80%
- **AI Accuracy**: >90% clean posts
- **User Engagement**: >20 karma/week/user (in enabled communities)

### Technology Stack

- **Backend**: .NET 10, FastEndpoints, CQRS, Mediator, FluentValidation
- **Data Access**: EF Core (Write) + Dapper (Read), PostgreSQL 18
- **Messaging**: RabbitMQ, MassTransit
- **API Gateway**: YARP (Yet Another Reverse Proxy)
- **Real-time**: SignalR
- **Background Jobs**: Hangfire
- **Resilience**: Polly
- **HTTP Clients**: Refit
- **Logging**: OpenTelemetry (structured logging)
- **Auth**: Keycloak, JWT Bearer
- **DI Container**: Scrutor
- **Testing**: xUnit, Shouldly, NSubstitute, Bogus, Testcontainers
- **Excel**: ClosedXML
- **Frontend**: Blazor WASM + MudBlazor, Angular + Material
- **Infrastructure**: PostgreSQL 18, Redis 8, Keycloak, Aspire, Docker
- **AI**: Python FastAPI, scikit-learn, PyTorch, Hugging Face
- **Observability**: OpenTelemetry, Prometheus, Grafana

This roadmap provides a comprehensive 44-week development plan with clear milestones, deliverables, and success criteria for building a modern, scalable social platform.
