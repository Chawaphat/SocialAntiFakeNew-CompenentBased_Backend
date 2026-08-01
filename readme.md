# Social Anti-Fake News System — Backend

## 📌 Overview
A full-stack platform that lets a community report, review, and vote on suspected fake news, with a moderated admin workflow for verifying and publishing reports. This repository is the **backend REST API**, built as a component-based software design project, containerized and deployed to a cloud VM.

## ✨ Key Features
- **News reporting & moderation** — CRUD for news reports, each with a status lifecycle (e.g. pending / reviewed / published) managed by admins.
- **Community voting** — users vote on whether a reported news item is fake or real; votes are aggregated to surface community consensus.
- **Comment system** — threaded comments on each news report.
- **Authentication & authorization** — JWT-based auth (access + refresh tokens) with a dedicated token store, and role-based access control (e.g. member vs. admin) enforced via Spring Security.
- **User & role management** — admin endpoints to manage user accounts and roles.
- **Media storage** — image/attachment upload integrated with Supabase Storage (S3-compatible), for photo evidence attached to news reports.
- **Seeded/mock data** — an app-startup initializer for demo/test data.

## 🛠️ Tech Stack
| Layer | Technology |
|---|---|
| Framework | Java, Spring Boot (Web, Data JPA, Security) |
| Database | MySQL |
| Auth | JWT (`jjwt`), Spring Security filter chain |
| Object mapping | MapStruct, Lombok |
| File storage | AWS S3 SDK (pointed at Supabase Storage) |
| Build & deploy | Maven, Docker, Docker Compose |
| CI/CD | GitHub Actions, deployed to an Azure cloud VM |
| Testing | Spring Boot Test |

## 🏗️ Architecture (High Level)
```
Client (Vue.js Frontend)
        │  HTTPS + JWT
        ▼
Spring Boot Application
  ├── Security layer        (JwtAuthenticationFilter, SecurityConfiguration) → validates JWT, sets auth context
  ├── Controllers            (News, Comments, User, Auth, Supabase) → REST endpoints
  ├── Services               (NewsService, CommentService, UserService, TokenService) → business logic
  ├── DAO layer               (NewsDao, CommentDao, UserDao, TokenDao) → data-access abstraction over repositories
  ├── Repositories (Spring Data JPA)  → News, Comment, User, Token
  ├── DTO / Mapper layer     (MapStruct: NewsMapper, CommentMapper, UserMapper) → entity ↔ API contract
  └── Entities                (News, Comment, Vote, User, Token, NewsStatus, VoteType)
        │
        ▼
   MySQL Database                Supabase Storage (S3) — image uploads
```
The service follows a layered **Controller → Service → DAO → Repository** architecture with DTO/mapper boundaries (via MapStruct) so API contracts stay decoupled from JPA entities, and a Spring Security + JWT filter chain protecting authenticated and role-restricted routes. The backend is containerized with Docker and shipped to a cloud VM through an automated GitHub Actions CI/CD pipeline.

## 🔗 Related Repository
- Frontend: `SocialAntiFakeNews-Frontend` (Vue 3 + TypeScript)

## 📸 Screenshots
> _Screenshots / architecture diagram to be added._
