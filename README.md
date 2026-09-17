# VendorWatch AI

**Real-Time Vendor Drift & Risk Signal Dashboard**

VendorWatch AI is a front-end mock-up of a third-party risk management (TPRM) console for
compliance officers, risk analysts, and the platform engineers who keep their data pipelines
running. It presents a single pane of glass over a portfolio of 147 vendors fed by 6 external
data integrations — Dun & Bradstreet, BitSight, SEC EDGAR, the OFAC sanctions feed, Dow Jones
Risk, and a news-NLP stream — showing where vendor risk scores are drifting, which signals fired
overnight, and whether the underlying integrations are actually healthy.

The application is a **static, single-file prototype**: all data is hard-coded sample content and
every interaction is client-side. There is no backend, no API keys, and no real vendor data. It
exists to demonstrate the interaction model and information architecture of the product.

## Features

### Dual-mode dashboard

A single dashboard serves two audiences via a mode toggle, each with its own KPI tiles and
section set. **Platform Mode** loads by default.

- **Analyst Mode** — business decisions, vendor risk, and signal review.
  KPIs: Active Vendors, High Risk Signals, Vendors in Drift, Alerts Today.
- **Platform Mode** — data pipelines, API health, and integrations.
  KPIs: Integration Health, Failed Webhooks, Avg API Latency, Events Processed Today.

### Analyst Mode

- **AI Morning Briefing** — Claude-generated overnight summary cards (critical infrastructure
  alerts, multi-factor risk convergence, portfolio risk trends), each with suggested next actions
  such as drafting a vendor inquiry email, initiating a contract review, or generating a board report.
- **Vendor Drift Monitor** — a table of flagged vendors with risk score, drift status, and last
  signal, plus a configurable drift threshold ("alert if score drops more than X% in Y hours")
  with support for custom per-vendor thresholds.
- **Risk vs. Criticality Matrix** — a quadrant scatter plot (Low Priority / Monitor / Emerging Risk /
  Critical — Act Now) positioning each vendor by risk score against business criticality, with
  up-risk movement markers.
- **Time scrub slider** — replays the matrix across 90, 60, and 30 days ago up to today to show how
  the portfolio has shifted.
- **Live Signal Feed** — severity-coded signal cards (sanctions hits, SEC filings, negative news
  clusters, beneficial ownership changes, financial health updates, expiring certifications),
  filterable by category (Financial, Geopolitical, Cyber, ESG, Regulatory), severity, and source.

### Platform Mode

- **API Telemetry & Diagnostic Console** — a four-tab console:
  - *Integration Health* — per-provider uptime, average latency, and rate-limit consumption.
  - *Raw Payload Inspector* — request/response JSON viewer for individual screening calls, with copy-to-clipboard.
  - *Log Controls* — per-integration log verbosity (Standard / Debug / Trace).
  - *Webhook Manager* — failed inbound webhooks with timestamp, source, event type, and error, plus
    single or bulk replay.
- **On-Demand Analysis & Integration Hub** — run an ad-hoc diagnostic against a vendor domain or
  name, upload a SOC-2 report, and force-sync individual data partners with live sync status.

### Reports

- **Ad-Hoc Document Analysis** — drag-and-drop upload zone for SOC-2 reports, security policies,
  and vendor contracts (PDF, DOCX, TXT up to 50MB), analyzed by Claude.
- **Recent Analyses** — history of reviewed documents with gap counts, critical-finding counts,
  and links to each gap analysis.

### Shell and navigation

- Persistent sidebar (Dashboard, Vendors, Signals, Reports, Alerts, Analytics, Settings) with
  client-side page switching and breadcrumb updates between the Dashboard and Reports pages.
- Dark, high-density analytics layout tuned for long monitoring sessions.
- Export Report and date-range controls on the dashboard header.

## Tech stack

- Single `index.html` file — no build step, no dependencies to install.
- [Tailwind CSS](https://tailwindcss.com) via CDN for styling.
- Inline vanilla JavaScript for page routing, mode switching, tab navigation, filter chips, the
  time scrubber, and the payload viewer.
- Inline SVG for all icons and the risk matrix.

## Running locally

Open `index.html` directly in a browser, or serve the directory:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

Deployed to GitHub Pages by the `.github/workflows/deploy.yml` workflow. The build job runs on
pushes to `main` and any `claude/**` branch (and on manual dispatch), uploading the repository
root as a Pages artifact with Jekyll processing bypassed. The deploy job runs only for `main`.
