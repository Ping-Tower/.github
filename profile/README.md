<div align="center">

<img src="../assets/logo-mark.svg" width="96" alt="PingTower">

# PingTower

### Real-time server availability monitoring

PingTower probes your servers on a schedule over HTTP/HTTPS, TCP and ICMP, keeps the latency history
and alerts you the moment a server goes down or recovers — in the browser, by email and in Telegram.

![C#](https://img.shields.io/badge/C%23_·_.NET_10-512BD4?logo=dotnet&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?logo=go&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![React](https://img.shields.io/badge/React_19-20232A?logo=react&logoColor=61DAFB)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white)
![ClickHouse](https://img.shields.io/badge/ClickHouse-FFCC01?logo=clickhouse&logoColor=black)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?logo=rabbitmq&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker_Compose-2496ED?logo=docker&logoColor=white)
![Traefik](https://img.shields.io/badge/Traefik-24A1C1?logo=traefikproxy&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana_·_Loki-F46800?logo=grafana&logoColor=white)

</div>

---

## How it works

| 📡 Probes | ⚖️ Evaluates | 💾 Stores | 🔔 Alerts |
| :---: | :---: | :---: | :---: |
| A dedicated goroutine per target checks it over HTTP/HTTPS, TCP or ICMP at the configured interval | Consecutive-failure and latency thresholds move the server to `UP` / `DOWN` | Every ping — latency, RTT, packet loss, TLS, DNS — is batched into ClickHouse | Status changes reach the browser via SignalR, email and Telegram, with a cooldown |

<div align="center">
<a href="../assets/architecture.png"><img src="../assets/architecture.png" width="680" alt="PingTower architecture"></a><br>
<sub>Independent services talk over RabbitMQ (topic exchanges and work queues)</sub>
</div>

## Repositories

<table>
<tr><th colspan="2">🖥 Product</th></tr>
<tr>
<td width="48" align="center">⚙️</td>
<td><a href="https://github.com/Ping-Tower/api"><b>api</b></a> — REST API, live status SignalR hub, authentication, notification routing<br><sub>C# · ASP.NET Core 10 · MediatR · SignalR · EF Core · PostgreSQL</sub></td>
</tr>
<tr>
<td align="center">🖥️</td>
<td><a href="https://github.com/Ping-Tower/frontend"><b>frontend</b></a> — dashboard: servers, real-time statuses, latency charts, notification settings<br><sub>React 19 · TypeScript · Vite · TanStack Query · Zustand · Radix UI · Recharts</sub></td>
</tr>
<tr><th colspan="2">📡 Monitoring</th></tr>
<tr>
<td align="center">📡</td>
<td><a href="https://github.com/Ping-Tower/ping-service"><b>ping-service</b></a> — HTTP/HTTPS, TCP and ICMP probes, one goroutine per target<br><sub>Go · pro-bing · RabbitMQ · Redis</sub></td>
</tr>
<tr>
<td align="center">⚖️</td>
<td><a href="https://github.com/Ping-Tower/state-elevator"><b>state-elevator</b></a> — status lifecycle: failure and latency thresholds → <code>UP</code> / <code>DOWN</code><br><sub>C# · .NET Worker Service · RabbitMQ · Redis</sub></td>
</tr>
<tr>
<td align="center">💾</td>
<td><a href="https://github.com/Ping-Tower/metrics-writer"><b>metrics-writer</b></a> — batched ping history writes to ClickHouse<br><sub>C# · .NET Worker Service · ClickHouse</sub></td>
</tr>
<tr><th colspan="2">🔔 Notifications</th></tr>
<tr>
<td align="center">📨</td>
<td><a href="https://github.com/Ping-Tower/email-service"><b>email-service</b></a> — SMTP email with retries and a circuit breaker<br><sub>C# · .NET Worker Service · MailKit · Polly</sub></td>
</tr>
<tr>
<td align="center">💬</td>
<td><a href="https://github.com/Ping-Tower/tg-bot"><b>tg-bot</b></a> — Telegram bot: account linking and alert delivery<br><sub>Python · FastAPI · aiogram · FastStream</sub></td>
</tr>
<tr><th colspan="2">🏗️ Platform</th></tr>
<tr>
<td align="center">🏗️</td>
<td><a href="https://github.com/Ping-Tower/infra"><b>infra</b></a> — Traefik, PostgreSQL, Redis, ClickHouse, RabbitMQ, logging; one Makefile for the whole stack<br><sub>Docker Compose · Traefik · Loki / Promtail / Grafana · GitLab CI</sub></td>
</tr>
</table>

## Getting started

| I want to… | Go to |
| --- | --- |
| **run everything locally** | [infra](https://github.com/Ping-Tower/infra): `make -C infra up` — storages, broker, migrations and all services |
| **understand the event flow** | the diagram above and the [RabbitMQ topology](https://github.com/Ping-Tower/infra/blob/HEAD/rabbitmq/config/definitions.json) |
| **integrate over REST** | [api](https://github.com/Ping-Tower/api) — endpoints, JWT auth, `/hubs/monitoring` hub |

## Data model

<div align="center">
<a href="../assets/erd.png"><img src="../assets/erd.png" width="680" alt="PingTower ERD"></a><br>
<sub><b>PostgreSQL:</b> servers, ping_settings, users, tokens, telegram_accounts, notification_settings · <b>ClickHouse:</b> server_pings</sub>
</div>
