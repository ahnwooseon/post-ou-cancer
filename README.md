# Post Ou Cancer

[![.NET](https://img.shields.io/badge/.NET-10%20Preview-blueviolet)](https://dotnet.microsoft.com/download/dotnet/10.0)
[![Aspire](https://img.shields.io/badge/.NET%20Aspire-9-blue)](https://learn.microsoft.com/en-us/dotnet/aspire/whats-new/dotnet-aspire-9)

## Description

Post Ou Cancer is a next-generation social platform reinventing online communities, discussions, and real-time engagement. It offers a multi-universe architecture: public feeds, semi-private forums, and ephemeral micro-worlds—each with custom rules, identities, and moderation policies.

The platform is mobile-first (PWA), with secure authentication and proactive moderation, all designed to foster authentic, high-quality exchanges.

Built as a modular monolith using Clean Architecture and orchestrated by .NET Aspire, it achieves <150ms latency and can scale via k3s deployment. The roadmap spans 38 weeks with 19 major milestones, ensuring structured growth and delivery.

## Key Features

- **Posts & Threads**: Nested threads, verified links, GZIP compression, Cloudflare R2 for uploads.
- **AI Moderation**: Python/FastAPI worker, Redis queue, Hugging Face fallback for content analysis.
- **Gamification**: Karma system, badges, JVC-inspired levels, optional MongoDB/Neo4j analytics.
- **Security**: Keycloak + Discord OAuth, Redis rate-limiting, hCaptcha integration.
- **Search**: PostgreSQL Full-Text Search + Elasticsearch, with pagination support.
- **Performance**: Redis caching for drafts, Hangfire for background tasks.
- **UI**: Blazor WASM + MudBlazor (public), Angular Material (admin), SCSS/Tailwind CSS.
- **API**: FastEndpoints, FluentResults, ProblemDetails, FluentValidation.
- **Observability**: OpenTelemetry, .NET Aspire dashboard, Prometheus/Grafana (production).

## Killer Features

What makes Post Ou Cancer truly different:

- 🔒 **Zero Knowledge Social Graph**: All private interactions are encrypted client-side before sending. The server only stores encrypted blobs and minimal metadata. Even admins cannot read user content.
- 🧭 **Transparent Social Feed**: No hidden algorithms. The feed is fully explainable and customizable. Users can adjust their own ranking logic via sliders or JSON files.
- 🧬 **Living Communities (Adaptive Threads)**: Threads evolve and resurface when relevant. Recent replies are aggregated under the most useful ones. Old topics can be revitalized without cluttering the platform.
- 🧠 **Local AI (not server-side)**: AI features run in your browser—summarize threads, translate posts, suggest tags—using lightweight models. No data leaves your device.
- 🪞 **Profile Without Vanity Metrics**: No follower counts or karma points. Users can present themselves freely, post anonymously, and receive constructive feedback.
- 💬 **Contextual Reputation System**: Reputation is per-community, not global. Users can be top contributors in one area and unknown in another—no universal score.
- 🌐 **Native Interoperability**: Publish and federate with Lemmy, Mastodon, etc. ActivityPub or a custom protocol lets users migrate their content anytime.
- 🤝 **"True Mode" by Default**: No badges, points, or forced reactions. The focus is on sincere content and meaningful discussion, not gamification or algorithmic circus.

## Architecture

- **Modules**: Posts, Auth, Communities, Votes, Moderation, Gamification, Search, GDPR, Notifications.
- **Backend**: .NET 10, FastEndpoints, Mediator pattern, EF Core, RabbitMQ, Redis 8.
- **Frontend**: Blazor WASM + MudBlazor (PWA), Angular + Material (admin, PWA).
- **Messaging**: RabbitMQ with MassTransit for event-driven communication.
- **Persistence**: PostgreSQL 18 (EF Core), optional MongoDB/Neo4j, Redis 8.
- **Storage**: Cloudflare R2 for icons and images.
- **Security**: Keycloak (JWT, refresh tokens), Discord OAuth.
- **Tasks**: Hangfire for cleanup and notifications.
- **Deployment**: Docker Compose, k3s/Helm charts for Kubernetes, Fly.io support.

## Stack

.NET 10, Blazor WASM/MudBlazor, Angular/Material, SCSS/Tailwind, YARP, RabbitMQ, PostgreSQL 18 (EF Core), Redis 8, MongoDB/Neo4j (optional), Keycloak, Discord, Hangfire, QuestPDF, xUnit, Moq, FluentResults, ProblemDetails, Prometheus, Grafana, Helm, OpenTelemetry, FastEndpoints.

## AI

Python FastAPI worker (scikit-learn/PyTorch), Hugging Face fallback, Redis queue.

## KPIs

- Latency <150 ms (P95)
- 90% clean posts (AI)
- > 20 karma/week/user

## Installation

### Prerequisites

- [.NET 10 Preview SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- Docker and Docker Compose
- PostgreSQL 18, Redis 8, RabbitMQ 3
- Python 3.12 (for AI moderation)
- k3s or Fly.io (for production)

### Local Setup (Dev/Tests – NAS/localhost)

```bash
git clone https://github.com/ahnwooseon/post-ou-cancer.git
cd post-ou-cancer
dotnet restore
dotnet run --project src/Aspire.AppHost
# Backend health: http://localhost:5000/api/health
# Python worker health: http://localhost:8000/health
# RabbitMQ UI: http://localhost:15672 (guest/guest)
# Aspire dashboard: http://localhost:18888
```

### Production (k3s/Fly.io)

```bash
dotnet aspire publish -p docker-compose
docker-compose run migration
docker-compose up --build
# Kubernetes: helm upgrade --install postoucancer k8s/
# Fly.io: fly deploy
```

## Roadmap

See the [ROADMAP.md](./ROADMAP.md) for detailed sprints, epics, and tasks.

## Contact

- X (Twitter): [@ahnwooseon](https://twitter.com/ahnwooseon)
- Discord: [Join on Discord](https://discord.gg/nJMRCgCTqZ)
- GitHub Issues: [Report a bug](https://github.com/ahnwooseon/post-ou-cancer/issues)

## License

All Rights Reserved.
