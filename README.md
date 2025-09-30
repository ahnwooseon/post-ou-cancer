# Post Ou Cancer

[![.NET](https://img.shields.io/badge/.NET-10%20Preview-blueviolet)](https://dotnet.microsoft.com/download/dotnet/10.0)
[![Aspire](https://img.shields.io/badge/.NET%20Aspire-10-blue)](https://learn.microsoft.com/aspnet/aspire)

## Description
Post Ou Cancer is a modern forum platform surpassing Reddit, 4chan, and jeuxvideo.com in engagement, proactive moderation, and gamification (JVC-inspired levels like "Mage Noir"). Built as a modular monolith with Clean Architecture and orchestrated by .NET Aspire on .NET 10 Preview (LTS November 2025), it’s optimized for <150 ms latency and deployment on NAS DS220+. No live chat; a Discord server handles community interaction and secure authentication via Keycloak. Mobile-first PWAs ensure a seamless mobile experience.

## Key Features
- **Posts & Threads**: Nested threads, verified links (Imgur, YouTube), GZIP, R2 uploads.
- **AI Moderation**: Custom AI (Python/FastAPI, scikit-learn/PyTorch, Redis queue), human queue, Hugging Face fallback.
- **Gamification**: Karma, badges, JVC-inspired levels (e.g., "Mage Noir"); VIP perks; MongoDB/Neo4j analytics (opt.).
- **Security**: Keycloak + Discord OAuth (>7 days, Redis rate-limits), Google OAuth (opt.), hCaptcha.
- **Communication**: Discord server, webhooks for notifications.
- **Search**: PostgreSQL FTS + Elasticsearch, pagination, Soundex (opt.).
- **GDPR**: QuestPDF PDF exports, anonymized deletions.
- **Performance**: Redis cache/drafts, Hangfire tasks.
- **API**: FastEndpoints (modular), FluentResults, ProblemDetails, FluentValidation, Scalar docs.
- **UI**: Angular Material (admin), MudBlazor (public), SCSS mobile-first, Tailwind (opt.).

## Architecture
- **Modules**: Posts, Auth, Communities, Votes, Moderation, Gamification, Search, GDPR, Notifications.
- **Layers**: Domain (entities), Infrastructure (EF Core, Redis, RabbitMQ), API (FastEndpoints).
- **Frontend**: Blazor WASM + MudBlazor + Scalar + PWA (public), Angular + Material + Scalar + PWA (admin), SCSS/Tailwind.
- **Backend**: .NET 10, FastEndpoints, Mediator, EF Core, FluentValidation, AutoMapper, MassTransit, OpenTelemetry.
- **Gateway**: YARP (routing, rate-limits).
- **Messaging**: RabbitMQ + MassTransit (events).
- **Persistence**: PostgreSQL (ES, EF Core), MongoDB/Neo4j (opt.), Redis (cache, queues).
- **AI**: Python FastAPI worker (scikit-learn/PyTorch, Redis queue), Hugging Face fallback.
- **Storage**: Cloudflare R2 (images, icons).
- **Search**: PostgreSQL FTS + Elasticsearch.
- **Security**: Keycloak (JWT, refresh tokens), Discord/Google OAuth, hCaptcha.
- **Observability**: OpenTelemetry, Aspire (local), Prometheus/Grafana (prod).
- **Tasks**: Hangfire (cleanup, notifications).
- **Tests**: xUnit, Shouldly, Moq.
- **Deployment**: Docker Compose, Helm charts for Kubernetes.
- **Docs**: GitHub Wiki, MkDocs, Scalar for API.

## Installation
### Prerequisites
- [.NET 10 Preview SDK](https://dotnet.microsoft.com/download/dotnet/10.0) (RC1, LTS Nov 2025).
- Docker, Docker Compose (prod).
- PostgreSQL 16, Redis 7, RabbitMQ 3 (local/DS220+).
- Python 3.12 (AI).

### Local Setup (DS220+)
1. Clone: `git clone https://github.com/<your-username>/PostOuCancer.git && cd PostOuCancer`
2. Restore: `dotnet restore`
3. Run: `dotnet run --project src/Aspire.AppHost` (Backend, PostgreSQL, Redis, RabbitMQ, PythonWorker; auto migrations in dev).
4. Test: `http://<ds220-ip>:5000/api/health`; verify PostgreSQL (`SELECT 1`), RabbitMQ UI (`http://<ds220-ip>:15672`), PythonWorker (`http://<ds220-ip>:8000/health`).
5. Observability: Aspire dashboard (`http://<ds220-ip>:18888`).

### Production (Docker Compose)
1. Publish: `dotnet aspire publish -p docker-compose`
2. Migrations: `docker-compose run migration`
3. Run: `docker-compose up --build`
4. Test: `http://<ds220-ip>:5000/api/health`.
5. Kubernetes: `kubectl apply -f k8s/` (Helm).
6. Monitoring: Prometheus/Grafana.

## Contact
[@ahnwooseon on X](https://x.com/ahnwooseon) | [Issues](https://github.com/ahnwooseon/post-ou-cancer/issues)
