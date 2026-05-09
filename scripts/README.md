# Scripts

Engångsskript och verktyg som inte hör hemma i din huvudkod-mapp. Exempel:

- `scripts/seed_database.py` — fyller en lokal databas med test-data
- `scripts/migrate_old_data.py` — engångsmigrering från tidigare format
- `scripts/verify_deploy.py` — smoke-test mot deployed app
- `scripts/generate_invoice.py` — månadsvis fakturering

Använd `scripts/` för saker du kör manuellt eller via cron, inte saker som hör till huvudkoden.

Ta bort denna README och skapa egna scripts efter behov.
