# markets_db_farm

Price-tracking scrapers for Brazilian pharmacy e-commerce sites.
Each store is scraped daily via GitHub Actions and stored in its own
PostgreSQL database (NeonDB), with automatic price-change history.

## Stores

| Store | Platform | EAN source |
|-------|----------|-----------|
| Drogaleste | VTEX | inline |
| Drogasil | Custom API | product page (enrichment) |
| Droga Raia | Custom API | product page (enrichment) |
| Drogaria São Paulo | VTEX | inline |
| Ultrafarma | LeanCommerce | product page (enrichment) |
| Pague Menos | VTEX IO | inline |
| Farmais | VTEX | inline |
| Panvel | Angular SPA / API | product page (enrichment) |
| Farmácias App | Typesense API | inline |
| Farmaconde | — | product page (enrichment) |

## How it works

- `main.py` runs one scraper subprocess per store in parallel.
- `db/db_manager.py` handles persistence: `offers` (current state, upserted)
  and `price_history` (append-only, written by a DB trigger only when a
  price actually changes).
- Scrapers with no inline barcode have an `enrich_ean_*.py` step that
  fetches EANs from product pages after the scrape.

## Usage

```bash
pip install -r requirements.txt
cp .env.template .env        # fill in your DATABASE_URL_* values

python -m main                          # scrape all stores
python -m main --stores drogasil        # one store
python -m main --limit 100              # test run
python -m main --enrich-ean             # scrape + EAN enrichment
python -m main --enrich-only            # EAN enrichment only
python -m main --log                    # write per-store logs to logs/

# Exports
python -m db.db_manager export-all-together --dir exports   # combined CSV
python -m db.db_manager export drogasil                     # one store, full tables

# Maintenance
python -m db.db_manager prune all --days 180   # trim old price history
```

## Deployment

One public GitHub repo per store (`stores-db-<store>`), each with a daily
scheduled workflow. Managed from this master repo:

```bash
python deploy/deploy.py            # create/update all store repos + secrets
python deploy/deploy.py --force    # force-push code updates
python deploy/deploy.py --dry-run  # preview
```

Schedules are configured in `deploy/deploy.py` (`STORE_SCHEDULE`, UTC).

## Secrets

All credentials live in `.env` (never committed) and in GitHub Actions
secrets. See `.env.template` for the required variables.
