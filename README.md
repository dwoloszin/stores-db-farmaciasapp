# export one store
python -m db.db_manager export drogasil
python -m db.db_manager export drogaleste --dir exports/2026-05-19

# export every store at once
python -m db.db_manager export-all
python -m db.db_manager export-all --dir exports/2026-05-19

# push still works exactly as before
python -m db.db_manager push drogasil drogasil_20260519.csv

# All together
python -m db.db_manager export-all-together
python -m db.db_manager export-all-together --dir exports/2026-05




# First run — enriches all ~26,000 products (~22 min at 12 workers)
python -m markets.drogasil.enrich_ean_drogasil

# Faster
python -m markets.drogasil.enrich_ean_drogasil --workers 20

# Test on 100 products first
python -m markets.drogasil.enrich_ean_drogasil --limit 100

# After each new scrape, only new products get fetched (already-enriched rows are skipped)

# Full parallel run (all 3 stores)
python -m main

# Test with 100 products per store
python -m main --limit 100

# Specific stores only
python -m main --stores drogasil drogariasaopaulo

# With EAN enrichment for Drogasil
python -m main --enrich-ean --workers 20





# main

python -m markets.drogasil.scraper_drogasil --limit 50 --enrich-ean


python -m markets.drogasil.scraper_drogasil --enrich-ean
python -m markets.drogariasaopaulo.scraper_drogariasaopaulo


python -m markets.ultrafarma.scraper_ultrafarma --limit 100   # test
python -m markets.ultrafarma.scraper_ultrafarma --enrich-ean  # full run + EAN
python -m main --stores ultrafarma                             # via parallel runner



python -m markets.paguemenos.scraper_paguemenos --limit 100   # test
python -m markets.paguemenos.scraper_paguemenos               # full run
python -m main --stores paguemenos                            # via parallel runner


python -m markets.farmaciasapp.scraper_farmaciasapp --limit 100   # test
python -m markets.farmaciasapp.scraper_farmaciasapp               # full run
python -m main --stores farmaciasapp                              # via main




# main function
python -m main --limit 100

# enrich
python -m markets.ultrafarma.scraper_ultrafarma --enrich-ean
python -m markets.panvel.scraper_ultrafarma --enrich-ean

# 1 storee
python -m main --stores ultrafarma



python -m main --stores panvel --enrich-ean          # scrape + fill EAN
python -m markets.panvel.enrich_ean_panvel           # enrich only (if scrape already ran)


# =====================================================================
# =====================================================================
# =====================================================================

# MAIN
python -m main --limit 100

# after for enrrich
python -m main --enrich-only

# =====================================================================
# =====================================================================
# =====================================================================


# Neon Account woloszin


# run main and anrrich
python -m main --enrich-ean




# Deploy To update code in all repos after changes — run python deploy/deploy.py --force from the master repo.
python deploy/deploy.py --force





