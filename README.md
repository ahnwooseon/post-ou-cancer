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

## ✨ A New Philosophy for Social Media

We offer solutions to the systemic flaws plaguing current social platforms, focusing on user control and content quality.

* 🔒 **Client-Side Encrypted Graph**: All private interactions (DMs, private groups) are encrypted **on your device** before being sent. The server only stores opaque blobs of data. **Even admins cannot read your private content.**
* ✅ **Opt-In Identity Verification**: Users can choose to verify their identity via a trusted third-party service to earn a "certified" badge. This provides a powerful, optional layer of trust without compromising the platform's core principles of pseudonymity and privacy.
* 🧭 **Transparent Social Feed (The Anti-Algorithm)**: No hidden algorithms or black boxes. The feed is fully explainable and customizable. Users can adjust their own ranking logic via simple sliders or advanced configurations (using a simple format like **TOML**). **This completely removes the "algorithmic circus" and political bias currently plaguing micro-blogging platforms like X.**
* **Hybrid AI (Privacy-First)**: AI features run in two modes: **Local AI** (in-browser) for privacy-sensitive tasks like summarization, ensuring no personal data leaves your device. For platform-wide safety, a **Server-Side AI** handles content moderation proactively.
* �🧘 **Digital Wellbeing by Design**: The platform includes features like "Attention Limiting" (a cap on daily reactions) and proactive suggestions for breaks to encourage mindful usage, not infinite scrolling.
* **Offline-First (PWA)**: The application is designed as a Progressive Web App with local-first storage. Key data is stored on your device, allowing for offline access and explicit, encrypted synchronization with the cloud.
* 🪞 **Profiles Without Global Vanity Metrics**: No universal follower counts or global scoreboards. The platform prevents "performance anxiety" and encourages focus on content quality, not popular appeal.
* 💬 **Contextual Reputation System (The Useful Metric)**: Reputation is earned **per-community and per-topic**, reflecting genuine expertise, not mere popularity. This system provides a **useful, localized signal** to identify top contributors within a niche.
* 🌐 **Long-Term Vision (Post-V1)**: Our ambition includes **"Living Communities"** (adaptive threads that resurface relevant content) and a native **ActivityPub Gateway** for full interoperability with the Fediverse (Mastodon, Lemmy).
* 🤝 **"True Mode" by Default (Configurable Governance)**: Core communities adopt **"True Mode"** (no badges, points, or forced reactions). **However, Community Admins have the explicit power to enable gamification, likes, follower counts, and other vanity metrics** for specific sub-worlds.

---

## 🛠️ Core Technology & Features

See our [Architecture Decision Records (ADR)](./docs/adr/) for detailed technical choices.

### 💻 Key Features

* **Content & Interaction**: Nested threading, rich message reactions, full Unicode/emoji support, internationalization (i18n), GIF integration (via Giphy/Tenor with privacy notice), native OS sharing (copy link, social apps), verified external links, and efficient asset storage via Cloudflare R2.
* **AI Moderation**: Dedicated Python/FastAPI worker, Redis queue, and Hugging Face fallback for **proactive content analysis and toxicity scoring**.
* **Performance**: Engineered for a sub-150ms (P95) latency target, with Redis caching for drafts and Hangfire for reliable background job processing.
* **Security**: Robust authentication via Keycloak (Discord/Google OAuth), optional identity verification, rate-limiting, and hCaptcha protection.
* **Search**: A powerful hybrid search strategy using **Elasticsearch** as the primary engine for speed and relevance, with a **PostgreSQL Full-Text Search fallback** to ensure high availability.
* **Gamification (Community-Level Opt-In)**: Karma system, badges, and levels, with graph-based analytics via **Neo4j**. **This system is configurable by Community Admins** to be enabled, disabled, or customized according to the universe's purpose.
* **UI/UX**: Mobile-first PWA design. Blazor WASM + MudBlazor (public front-end) and Angular (admin interface), styled with SCSS/Tailwind CSS.

### ⚙️ Architecture & Stack

| Component         | Technology / Framework   | Role                                                                  |
| :---------------- | :----------------------- | :-------------------------------------------------------------------- |
| **Backend**       | .NET (Modular Monolith)  | Core application logic using FastEndpoints, CQRS, and EF Core.        |
| **Messaging**     | RabbitMQ + MassTransit   | Event-driven communication between modules and services.              |
| **Frontend**      | Blazor WASM & Angular    | Public-facing PWA and a separate, data-intensive Admin dashboard.     |
| **Persistence**   | PostgreSQL (EF Core)     | Primary relational data store.                                        |
| **Caching**       | Redis                    | High-speed caching for drafts, API responses, and rate-limiting.      |
| **Observability** | OpenTelemetry & Aspire   | Unified, end-to-end distributed tracing, logging, and metrics.        |
| **Auth**          | Keycloak, Discord OAuth  | Centralized identity and access management (JWT).                     |
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
* Docker and Docker Compose
* An IDE like Visual Studio, Rider, or VS Code
* Python 3.12 (For optional AI moderation worker)

### Local Development Setup

To run the full stack locally (including all services orchestrated by Aspire):

```bash
git clone https://github.com/ahnwooseon/post-ou-cancer.git
cd post-ou-cancer
dotnet restore
dotnet run --project src/Aspire.AppHost # This will start all services.
```

### Production Deployment

For production-ready deployment, refer to the deployment strategy ([ADR-008](./docs/adr/ADR-008.md)). The general steps are:

```bash
# 1. Generate deployment manifests from Aspire
dotnet publish ./src/Aspire.AppHost -o ./aspire-manifest

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
