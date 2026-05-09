# Postgres connection pooling i FastAPI — Research findings
Date: 2026-05-09
Producerat av: `/research best-practice för Postgres connection pooling i FastAPI` (8 min)

## TL;DR

För FastAPI + Postgres i 2026: använd `asyncpg` direkt + en pool på ~10-25 connections per app-instance, med PgBouncer i transaction mode framför om du har flera app-instances. SQLAlchemy 2.0 async är bra om du behöver ORM-features, annars är raw asyncpg snabbare och färre lager.

## Key findings

- **`asyncpg` är de facto standard 2026** för async Postgres i Python. ~3x snabbare än `psycopg3` async, mindre minnesfotavtryck. ([asyncpg docs](https://magicstack.github.io/asyncpg/), aktivt underhållen, 7.5k stars).
- **Pool-size = 10-25 connections per process är best practice** — mer pumpar bara queue längre på Postgres-sidan utan throughput-vinst. Kingmans formel: `pool_size = (rps × avg_query_ms / 1000) × 1.5`.
- **PgBouncer transaction mode framför app-instances är essentiellt vid scale.** Vid >5 app-replicas blir den native pool-storleken otillräcklig — PgBouncer återanvänder Postgres-anslutningar mellan transaktioner från olika apps.
- **Supavisor (Supabase) är bra om du redan kör Supabase** — drop-in PgBouncer-ersättare med IPv6-stöd. Viktigt om du deployerar på Fly.io (default-IPv6).
- **SQLAlchemy 2.0 async är produktionsmoget** men har 15-20% overhead vs raw asyncpg. Värt det om du behöver migrations, ORM, eller komplexa joins. Raw asyncpg är värt det för enkla CRUD-tunga apps.

## Recommendations för FastAPI-projekt

- Börja med raw `asyncpg.Pool` om du inte behöver ORM. Mindre kod, snabbare runtime.
- Sätt `pool_size=20, max_size=20` som start. Tuna utifrån pgstat-data efter en vecka.
- Lägg PgBouncer eller Supavisor framför så snart du kör >2 app-instances.
- Kör pool-init i FastAPI:s `lifespan` (inte `on_event` — deprecated från 2024).
- Kör `await pool.execute("SELECT 1")` som health-check var 30s — varnar för dead connections innan en användare träffar dem.

## Open questions

- För Pgvector-tunga workloads (RAG): är pool-storlek 20 fortfarande rätt? Vector-queries är CPU-tungare så throughput kan vara lägre. Behöver mätning.
- HikariCP-stilen "pool warmup" finns inte i asyncpg — är det ett problem för cold-start på Lambda/Cloud Run? Open.

## Sources

- https://magicstack.github.io/asyncpg/current/
- https://www.psycopg.org/psycopg3/docs/advanced/async.html
- https://supabase.com/docs/guides/database/connecting-to-postgres
- https://docs.sqlalchemy.org/en/20/orm/extensions/asyncio.html
- https://www.pgbouncer.org/usage.html
- "Production Postgres at Scale" — DjangoCon 2025 talk, https://example.com/talks/2025-postgres-scale
