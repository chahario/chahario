<h1 align="center">👋 Hi, I'm Dinesh Chahar</h1>

<p align="center">
  <b>Backend Engineer</b> · Java · Spring Boot · Python<br>
  I build systems that stay correct when load, retries and partial failure all show up at once.
</p>

<p align="center">
  <a href="https://www.linkedin.com/in/chahario/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="mailto:dineshchahar4666@gmail.com"><img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"></a>
  <img src="https://img.shields.io/badge/Delhi,%20India-34A853?style=for-the-badge&logo=googlemaps&logoColor=white" alt="Delhi, India">
</p>

---

### 🧭 About

I work mostly in **Java and Spring Boot**, and I'm drawn to the parts of a system that are easy to get subtly wrong — caching, messaging, idempotency, billing, consistency under concurrency.

Most of what's below was built from first principles rather than assembled from a tutorial: a cache with its own wire protocol, a link service that has to bill every single visit, an analytics agent that refuses to answer when the data can't support it. Every README explains the reasoning and what got traded away — including the decisions that didn't pan out.

More recently I've been building **AI systems with the same discipline I'd apply to a payments service**: closed output schemas, verification in code rather than in prompts, and an eval harness that tells me when a change made things worse.

---

## 🚀 Backend systems

### ⚡ [Distributed In-Memory Cache](https://github.com/chahario/Distributed-Cache)

A sharded, in-memory key–value cache written from scratch in Java — a minimal Redis. Consistent hashing with 200 virtual nodes per physical node, 32 lock-striped partitions, pluggable LRU/LFU eviction, TTL expiry, and a 12-byte binary protocol over non-blocking NIO. A client can hit any node; keys owned elsewhere are forwarded one hop and relayed back.

📊 Benchmarked at two levels, which turned out to be the interesting part. The core data structure does **~4.9M gets/sec** in-process under JMH with 8 threads. End-to-end over TCP it plateaus near **30K ops/sec** — the bottleneck is the single NIO selector thread serialising every request onto one core, not the cache itself. The README walks through p50/p99/p99.9 at each concurrency level and why the tail climbs.

`Java` · `NIO` · `JMH` · `consistent hashing` · `lock striping`

### 🔗 [URL Shortener with Per-Visit Billing](https://github.com/chahario/Url-Shortner)

A URL shortener built around a requirement that makes it genuinely hard: **every visit has to be billable**. That forces decisions a `create → redirect` service never faces — counting visits asynchronously through Kafka instead of writing a row per click, a cache-first read path, idempotency on writes, and immutable invoices over plan tiers.

📐 There's a separate [ARCHITECTURE.md](https://github.com/chahario/Url-Shortner/blob/main/ARCHITECTURE.md) covering the why behind each component and what was given up to get it.

`Java` · `Spring Boot` · `Kafka` · `PostgreSQL` · `Redis`

### 📨 [Notification Service](https://github.com/chahario/Notification-Service)

Spring Boot service that publishes and consumes notification events over Kafka across email, SMS and push channels, using the **outbox pattern** so a delivery isn't lost when the database commit and the broker publish disagree. The README has a Decisions & Tradeoffs section rather than just setup steps.

`Java` · `Spring Boot` · `Kafka` · `outbox pattern`

---

## 🤖 AI engineering

My interest here isn't prompt-wrangling — it's making a non-deterministic component trustworthy enough to put in front of users. Both projects below enforce correctness **in code**, not in instructions to the model.

### 🧠 [NYC 311 Analytics Agent](https://github.com/chahario/Text2SQL-AI-Agent)

Ask a question in plain English, get back the statistic, the SQL that produced it, the rows it came from, and a verification pass confirming every number in the answer actually exists in those rows. When the data can't answer the question, it says so — refusing is treated as a normal, correct output.

**Six agents on LangGraph** (Router → SQL Analyst → Reviewer → Narrator → Viz) over DuckDB, behind FastAPI and a React + TypeScript frontend, Dockerised into one command.

🔒 What keeps it honest, all enforced by code:
- A **sqlglot AST guard** — single read-only `SELECT`, one table, every referenced column must exist; failures are fed back for retry
- **Number grounding** — any figure the narrator invents dies before it reaches the user
- A **hand-written semantic layer** plus coverage computed from the live database at startup, so nothing about the dataset is hard-coded
- **Prompt-injection defence** — data cells are sanitised and framed as data before entering any prompt
- **98 offline tests** and an **eval harness** with golden cases scored against truth SQL, comparing by value so any equivalent query passes, with ▲▼ deltas against the previous run

`Python` · `LangGraph` · `DuckDB` · `FastAPI` · `React` · `sqlglot` · `pytest` · `Docker`

### 🗺️ [AI Trip Planner](https://github.com/chahario/Trip-Planner)

Turns a loose idea — *"10 days in Thailand, I like beaches and food"* — into a real day-by-day itinerary: which cities and in what order, where to stay each night, named stops with commute legs and fares, day-by-day weather on real dates, and a transparent cost breakdown fetched via **tool calls rather than guessed**. It plans a single city, a country, or a whole continent.

🛡️ Every place is verified against real geo data, and a **Critic step** enforces the exact day count and flags ungrounded output before repairing it. The whole agent pipeline streams live over **Server-Sent Events** so you can watch it work. If the LLM or any upstream API is down it degrades to curated data instead of crashing.

`Python` · `FastAPI` · `SSE` · `OpenAI` · `React` · `TypeScript` · `OpenStreetMap` · `Open-Meteo`

---

## 📦 Also here

| Project | What it is |
|---|---|
| 💸 [Expense Tracker](https://github.com/chahario/expense-tracker) | Full-stack expense tracker built for correctness under retries, duplicate submissions and page refreshes. Spring Boot + React, deployed. |
| 💳 [Payflow](https://github.com/chahario/Payflow-payment-gateway) | Payment-gateway microservice — Spring Boot, Postgres, RabbitMQ, JPA, global exception handling, Docker Compose. |
| 🧩 [DSA](https://github.com/chahario/DSA) | Working through interview patterns in Java, with notes on the signal and invariant behind each one rather than just solutions. |
| 🏛️ [Design Patterns](https://github.com/chahario/Design-Patterns-Java) | Gang-of-Four patterns in Java, written out to understand them properly. |

---

## 🛠️ Stack

**Languages**

![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)

**Backend**

![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Hibernate](https://img.shields.io/badge/JPA_/_Hibernate-59666C?style=flat-square&logo=hibernate&logoColor=white)

**Data & messaging**

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Kafka](https://img.shields.io/badge/Kafka-231F20?style=flat-square&logo=apachekafka&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=flat-square&logo=rabbitmq&logoColor=white)
![DuckDB](https://img.shields.io/badge/DuckDB-FFF000?style=flat-square&logo=duckdb&logoColor=black)

**AI**

![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=flat-square&logo=langchain&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=flat-square&logo=openai&logoColor=white)

**Frontend**

![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)

**Testing & infra**

![JUnit5](https://img.shields.io/badge/JUnit_5-25A162?style=flat-square&logo=junit5&logoColor=white)
![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)

---

<p align="center">
  📫 <a href="https://www.linkedin.com/in/chahario/">LinkedIn</a> ·
  <a href="mailto:dineshchahar4666@gmail.com">dineshchahar4666@gmail.com</a> ·
  📍 Delhi, India
</p>

