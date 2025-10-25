# Post Ou Cancer

## 🚀 Project Overview

**Post Ou Cancer** is a **meta-social platform** designed to reinvent online communities by moving beyond outdated, single-format social models. Our core philosophy prioritizes **trust, privacy, and authentic, high-quality exchanges.**

Built as a modular monolith using **Clean Architecture** and orchestrated by **.NET Aspire**, the platform is engineered for sub-150ms latency and robust scaling. It features a unique **Multi-Universe Architecture**, allowing every community to configure its own governance, features, and social metrics.

---

## 🏛️ The Multi-Universe Architecture: A Unified, Configurable Platform

**Post Ou Cancer** eliminates the need to choose between the long-form depth of forums and the instant communication of micro-blogging. Our architecture allows communities to configure their core functionality to match their purpose:

| Universe Type                  | Primary Goal                                                    | Key Settings Enabled                                                                                                     | Analogy                                   |
| :----------------------------- | :-------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------- | :---------------------------------------- |
| **The Forum (Depth)**          | In-depth discussion, knowledge sharing, lasting archives.       | Nested threads, long-form posts, **"True Mode"** (no vanity metrics), strong Contextual Reputation.                      | **Reddit / Niche Forum**                  |
| **The Stream (Instantaneity)** | Real-time news, rapid conversation, content amplification.      | Short-form posts, rapid feed updates, **Community-Level Likes/Followers enabled**, focus on the Transparent Social Feed. | **X (Twitter) / Bluesky**                 |
| **The Micro-World (Private)**  | Secure spaces for trusted groups, teams, or private DMs.        | **End-to-End Encryption** by default, ephemeral messages, and seamless transition from public threads.                 | **Private Discord / Signal Group**        |

*This flexible design means users do not migrate to a single format, but to a platform where they can **create the exact social experience they need**, all within a single, high-performance, privacy-focused environment.*

---

## ✨ Core Principles & Features

This project is built on a set of core principles designed to address the systemic flaws of current social media platforms. Each principle is backed by concrete features and modern technology.

*   🔒 **Client-Side Encrypted Graph (Zero-Knowledge)**: All private interactions (DMs, private groups) are end-to-end encrypted **on your device** using a zero-knowledge graph model. The server only stores opaque blobs of data, meaning **even admins cannot read your private content.**

*   ✅ **Opt-In Identity Verification**: To foster trust without compromising privacy, users can optionally verify their identity via a trusted third-party service to earn a "certified" badge. This provides a powerful layer of trust while respecting pseudonymity.

*   🧭 **Transparent Social Feed (The Anti-Algorithm)**: We reject hidden, manipulative algorithms. The feed is fully explainable and customizable via simple UI controls, removing the "algorithmic circus" and political bias common on other platforms.

*   🤖 **Hybrid & Proactive AI Moderation**: AI runs in two modes for a balance of privacy and safety. A **Local AI** runs in-browser for privacy-sensitive tasks (like summarization), ensuring no personal data leaves your device. For platform-wide safety, a **Server-Side AI worker** (Python/FastAPI, with a Hugging Face fallback) proactively handles content analysis and toxicity scoring via a Redis queue.

*   🔗 **Immutable Audit Trail (Event Sourcing on Blockchain)**: To provide the highest level of trust and transparency, events from our **event sourcing architecture** are anchored to the **Stratis/Cirrus blockchain** (a .NET-native platform). This creates a permanent, verifiable "proof-of-existence" for critical actions, as detailed in [ADR-009](./docs/adr/ADR-009.md).

*   🧘 **Digital Wellbeing by Design**: The platform encourages mindful usage, not infinite scrolling. Features include "Attention Limiting" (a cap on daily reactions) and proactive suggestions for breaks.

*   🌐 **Offline-First (PWA)**: Designed as a Progressive Web App, the application uses local-first storage. Key data is stored on your device for offline access, with explicit, encrypted synchronization to the cloud.

*   💬 **Contextual Reputation & Opt-In Gamification**: To provide useful signals of expertise, reputation is earned **per-community**, not as a global vanity metric. This system—which includes karma, badges, and levels—is powered by a **Neo4j graph database** and is fully configurable by community admins, who can enable or disable it to fit their community's purpose.

*   🚀 **High-Performance & High-Availability**: The entire system is engineered for a **sub-150ms (P95) latency target**.
    *   **Search**: A powerful hybrid strategy uses **Elasticsearch** for speed and relevance, with a **PostgreSQL Full-Text Search fallback** to ensure high availability.
    *   **Database**: Read/write separation is achieved with **EF Core for writes** and **Dapper for optimized read queries**.
    *   **Caching**: **Redis** is used extensively for caching drafts, API responses, and rate-limiting.
    *   **Background Jobs**: **Hangfire** processes reliable background jobs like sending notifications or cleaning up data.

*   🛡️ **Robust Security**:
    *   **Authentication**: Centralized identity and access management via **Keycloak**, supporting Discord & Google OAuth providers.
    *   **Protection**: The system is shielded with endpoint-specific **rate-limiting** and **hCaptcha protection** against bots.

*   ✍️ **Rich Content & Interaction**: The platform supports nested threading, rich message reactions, GIF integration (Giphy/Tenor with a privacy notice), native OS sharing, verified external links, and efficient asset storage via Cloudflare R2.

### ⚙️ Architecture & Stack

| Component         | Technology / Framework   | Role                                                                  |
| :---------------- | :----------------------- | :-------------------------------------------------------------------- |
| **Backend**       | .NET 10 (Modular Monolith)  | Core application logic using FastEndpoints, CQRS, Mediator, FluentValidation. |
| **Data Access**   | EF Core (Write) + Dapper (Read) | Command/Query separation: EF Core for writes, Dapper for optimized reads. |
| **Messaging**     | RabbitMQ + MassTransit   | Event-driven communication between modules and services. |
| **API Gateway**   | YARP (Yet Another Reverse Proxy) | Reverse proxy for routing, load balancing, and API aggregation. |
| **Real-time**     | SignalR                    | Real-time bidirectional communication for live updates. |
| **Frontend**      | Blazor WASM & Angular    | Public-facing PWA and a separate, data-intensive Admin dashboard.     |
| **Persistence**   | PostgreSQL                 | Primary relational data store.                                        |
| **Caching**       | Redis                    | High-speed caching for drafts, API responses, and rate-limiting.      |
| **Background Jobs** | Hangfire                 | Reliable background job processing and scheduled tasks. |
| **Resilience**    | Polly                     | Transient fault handling and resilience patterns for HTTP calls. |
| **HTTP Clients**  | Refit                     | Type-safe HTTP client library for external API integration. |
| **Observability** | OpenTelemetry & Aspire   | Unified, end-to-end distributed tracing, logging, and metrics.        |
| **Auth**          | Keycloak + JWT Bearer    | Centralized identity and access management. |
| **DI Container**  | Scrutor                  | Advanced dependency injection with service decoration. |
| **Testing**       | xUnit + Shouldly + NSubstitute + Bogus | Unit and integration testing with mocking and test data generation. |
| **Excel**         | ClosedXML                 | Excel file generation and manipulation (MIT license). |
| **AI/ML**         | Python FastAPI worker    | Content classification (scikit-learn/PyTorch), Hugging Face fallback. |
| **Deployment**    | Docker, k3s, Helm        | Containerized services for consistent local dev and production envs.  |

### 📊 Key Performance Indicators (KPIs)

We measure success by performance and community health:

* **Latency**: $<150\text{ms (P95)}$
* **Safety**: $>90\% \text{ clean posts (AI-verified)}$
* **Engagement**: $\text{Targeted engagement levels}$ (> 20 karma/week/user) to ensure active contribution, primarily measured within communities where the karma system is enabled.

---

## 💖 Support the Project

**Post Ou Cancer** is an independent project developed **full-time** by a solo developer. For this alternative to exist and remain true to its values (no ads, no data selling), it relies on a **donoware** ("pay what you want/can") model.

Your direct support allows me to cover infrastructure costs, but more importantly, to **continue working full-time on this project and make a living from it**. Every contribution, whether a one-time donation or a future Premium/VIP subscription, is an investment in a healthier, more respectful internet.

### Financial Transparency

In line with our philosophy of transparency, here is an overview of contributions and costs. *(This table will be updated periodically.)*

| Period       | Donations Received | Operational Costs | Net Result (Developer Support) |
| :----------- | :----------------- | :---------------- | :----------------------------- |
| Nov-Dec 2025 | €0                 | €0                | €0                             |
| ...          | ...                | ...               | ...                            |

*(Initial operational costs are zero, thanks to the use of free tiers and open-source software.)*

<p align="center">
  <a href="https://www.buymeacoffee.com/yourusername" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-orange.png" alt="Buy Me A Coffee" style="height: 41px !important;width: 174px !important;" ></a>
  &nbsp;
  <a href="https://paypal.me/yourusername" target="_blank">
    <img src="https://img.shields.io/badge/PayPal-Donate-blue?style=for-the-badge&logo=paypal" alt="PayPal">
  </a>
  &nbsp;
  <a href="https://tipeee.com/yourusername" target="_blank">
    <img src="https://img.shields.io/badge/Tipeee-Support-green?style=for-the-badge&logo=tipeee" alt="Tipeee">
  </a>
</p>

### Your Privacy Remains the Priority

Making a donation does not change our fundamental commitments. Financial data is managed by secure third-party platforms and is entirely decoupled from your activity on the platform. **We never link your donor identity to your user profile by default**. To recognize supporters, we offer an **opt-in system**: after a donation, you can choose to activate a special badge on your profile or have your username appear on a dedicated "Supporters" page. Your privacy and your choice to remain anonymous are, and always will be, our priority.

---

## 🚀 Installation & Setup

### Prerequisites

* .NET SDK (Latest Stable/Preview)
* Podman and Podman Compose (or Docker and Docker Compose)
* An IDE like Visual Studio, Rider, or VS Code
* Python 3.12 (For optional AI moderation worker)

### Local Development Setup

To run the full stack locally (including all services orchestrated by Aspire):

```bash
git clone https://github.com/ahnwooseon/post-ou-cancer.git
cd post-ou-cancer
dotnet restore
dotnet run --project src/PostOuCancer.AppHost # This will start all services.
```

### Production Deployment

For production-ready deployment, refer to the deployment strategy ([ADR-008](./docs/adr/ADR-008.md)). The general steps are:

```bash
# 1. Generate deployment manifests from Aspire
dotnet publish ./src/PostOuCancer.AppHost -o ./aspire-manifest

# 2. Deploy to a Kubernetes cluster using the generated Helm charts
helm upgrade --install postoucancer ./aspire-manifest/postoucancer.AppHost.helm
```

## 🗺️ Roadmap & Community

The project follows a structured development plan with four major launches planned over 44 weeks. See the [**ROADMAP.md**](./ROADMAP.md) for a detailed breakdown of all sprints and milestones.

* **ALPHA Launch** (Jan 23, 2026): Core posting functionality with CQRS and performance baseline.
* **BETA Launch** (Apr 3, 2026): Full-text search with Elasticsearch and advanced filtering.
* **V1.0 Launch** (Jul 10, 2026): Production-grade observability stack (Prometheus, Grafana).
* **PRODUCTION Launch** (Sep 18, 2026): Final deployment, security audit, and go-live.

## Get in touch

* X : [@ahnwooseon](https://twitter.com/ahnwooseon)
* Discord: [Join on Discord](https://discord.gg/nJMRCgCTqZ)
* GitHub Issues: [Report a bug](https://github.com/ahnwooseon/post-ou-cancer/issues)

## License

All Rights Reserved.
