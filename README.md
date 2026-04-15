# Elegansky M6PM — Debt Collection Report System

## What it does
- **Function 1**: Upload QuickBooks export → generates per-agent Excel debt reports
- **Function 2**: Upload morning + evening QuickBooks exports → generates comparison reports showing who paid, who has bike in office, who has bike at police

## Setup

### 1. Clone & install
```bash
git clone git@github.com:cliffpyne/elegansky-m6pm.git
cd elegansky-m6pm
pip install -r requirements.txt
```

### 2. Add Google Service Account credentials
Place your `credentials.json` file in the root of the project.
Make sure the service account has **Viewer** access to the Google Sheet.

The Google Sheet used:
- ID: `1wrM7E9qGKcWJvN4mBwYMpkgp31jlxPGgEYCDsHn0bkc`
- Tab 1: **OFFICE** — customers with bike in office
- Tab 2: **POLICE** — customers with bike at police

### 3. Run locally
```bash
python app.py
```
Visit: http://localhost:5000

## Deploy to Render

1. Push to GitHub
2. Go to render.com → New Web Service → Connect your repo
3. Set:
   - **Build command**: `pip install -r requirements.txt`
   - **Start command**: `gunicorn app:app`
4. Add environment variable or upload `credentials.json` as a Secret File at path `/etc/secrets/credentials.json`
5. Update `CREDS_FILE` in `app.py` to `/etc/secrets/credentials.json`

## QuickBooks Export Format
The system expects the standard QuickBooks XLS export with these columns:
- **Customer**: `BRANCH:AGENT:SUBAGENT:CUSTOMER` format
- **Balance**: Invoice amount to sum per customer

## Output Excel Format

### Debt Report
| Agent | Customer Name | Total Debt | Status |
|-------|--------------|-----------|--------|
| EDITHA BODA NEW | ABDALLAH JUMA | 50,000 | |
| EDITHA BODA NEW | SOME CUSTOMER | 30,000 | Bike in Office |

### Comparison Report
| Agent | Customer Name | Morning Amount | Evening Amount | Status |
|-------|--------------|---------------|---------------|--------|
| EDITHA BODA NEW | ABDALLAH JUMA | 50,000 | 50,000 | |
| EDITHA BODA NEW | SOME CUSTOMER | 30,000 | | Paid |
