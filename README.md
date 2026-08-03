<img width="1406" height="728" alt="image" src="https://github.com/user-attachments/assets/19ce3be2-f33c-4538-b99b-83676af61da8" />
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
| Infrastructure | Microsoft Azure Virtual Machine |
| CI/CD | GitHub Actions → Docker Hub → deployed via `docker compose up` on the Azure VM |
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
The service follows a layered **Controller → Service → DAO → Repository** architecture with DTO/mapper boundaries (via MapStruct) so API contracts stay decoupled from JPA entities, and a Spring Security + JWT filter chain protecting authenticated and role-restricted routes.

## ☁️ Deployment
The application is deployed on a **Microsoft Azure Virtual Machine** using **Docker Compose**, running a 3-container stack:

```
Azure Virtual Machine
  └── docker compose up
        ├── spring_backend   → Spring Boot API (port 8080), built from the repo Dockerfile
        │                       and published to Docker Hub via CI
        ├── mysql_db         → MySQL 8 database (port 3307 → 3306), with a persistent volume
        │                       for data durability across restarts
        └── pma (phpMyAdmin) → web-based DB admin UI (port 9000), for inspecting/managing
                                 the MySQL database directly
  All three containers share a dedicated Docker bridge network (app-network),
  so the backend reaches MySQL by its service name (db) rather than a public address.
```
- The Spring Boot image is built from a multi-stage-friendly `Dockerfile` (Java 21 base, exploded JAR layout for smaller layers) and run with the `prod` Spring profile.
- Database connection, credentials, and Hibernate DDL mode are injected via environment variables in `docker-compose.yml`, keeping the same image portable across environments.
- `phpMyAdmin` is included in the same compose stack purely as an operational/debugging tool for the team, not part of the application's runtime path.

## 🔗 Related Repository
- Frontend: `SocialAntiFakeNews-Frontend` (Vue 3 + TypeScript)

## 📸 Screenshots
<img width="1406" height="728" alt="image" src="https://github.com/user-attachments/assets/cd8ba4b7-cb96-4f71-bd73-a6123be92c7e" />
<img width="471" height="738" alt="image" src="https://github.com/user-attachments/assets/c3fa8b19-b65f-4ff2-b826-aa576b986bf1" />
<img width="461" height="471" alt="image" src="https://github.com/user-attachments/assets/da90a390-d59f-47bd-a678-dfdbcba6e899" />
<img width="702" height="492" alt="image" src="https://github.com/user-attachments/assets/7b2ed0ac-a8ea-428b-8b67-fadb8dc86080" />
<img width="1400" height="728" alt="image" src="https://github.com/user-attachments/assets/ce44d0a4-f51a-41f2-bf32-339740d1bbf3" />
<img width="1410" height="736" alt="image" src="https://github.com/user-attachments/assets/d0c36251-b439-4089-b9e5-11352f2da17d" />






