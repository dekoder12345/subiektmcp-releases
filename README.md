# SubiektMCP — MCP Server for Subiekt GT (Polish ERP)

> **Connect Claude Desktop, ChatGPT, Cursor, Windsurf, n8n and any MCP-compatible AI agent to your Subiekt GT ERP database.**

[![License](https://img.shields.io/badge/license-Commercial-blue)](https://subiektgt.chat/regulamin)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2B-lightgrey)]()
[![MCP](https://img.shields.io/badge/Model%20Context%20Protocol-2025--11--25-green)](https://modelcontextprotocol.io)
[![Latest Release](https://img.shields.io/github/v/release/dekoder12345/subiektmcp-releases)](https://github.com/dekoder12345/subiektmcp-releases/releases/latest)

SubiektMCP is a production-ready **Model Context Protocol (MCP) server** for **Subiekt GT** — the most popular ERP system in Poland, made by **InsERT**. Install it on your Windows machine and instantly give any AI agent direct, read-write access to your invoicing, inventory, customers and sales data.

---

## 👉 Get your license key: **[subiektgt.chat](https://subiektgt.chat?utm_source=github_releases&utm_medium=readme&utm_campaign=discover)**

> SubiektMCP is commercial software. **You need a valid license key to run the server.**
> Download the installer here, then buy a license at [subiektgt.chat](https://subiektgt.chat?utm_source=github_releases&utm_medium=readme_cta).
> Pricing starts at **99 PLN/month** with a 14-day free trial.

---

## ✨ Features

### 🔍 Read tools (Starter plan — 99 PLN/month)
- `get_product_stock` — stock levels and prices by product symbol
- `get_product_stock_multi_warehouse` — stock across all warehouses
- `get_low_stock_products` — items below minimum threshold
- `search_products` — fuzzy search by name or symbol
- `get_price_list` — full pricing catalog
- `search_customers` — find clients by name, NIP, or phone
- `get_customer_balance` — receivables and payables
- `get_customer_order_history` — purchase history per customer
- `get_unpaid_invoices` — aged receivables by customer
- `get_sales_report` — period-based sales analytics
- `get_top_selling_products` — bestsellers by revenue or quantity
- `get_document_details` — full invoice/order breakdown by number
- `search_documents` — find documents by date range, customer, type

### ✍️ Write tools (Pro plan — 199 PLN/month, requires Sfera GT license)
- `create_sales_order` — create Zamówienie od Klienta directly from AI
- `create_invoice` — issue Faktura Sprzedaży
- `create_receipt` — issue Paragon
- `update_customer` — modify customer records

### 🔒 Privacy by design
- **Your ERP data never leaves your computer.** The MCP server runs locally as a Windows service.
- AI agents (Claude, ChatGPT) communicate directly with your local server over MCP — nothing is routed through our infrastructure.
- License verification is the only outbound call (to Keygen.sh, once per hour with local cache).

### 🛠️ Technical requirements
- Windows 10/11 (64-bit)
- Subiekt GT 1.50+ (any version)
- MS SQL Server 2012+ (where your Subiekt database lives)
- Sfera GT license (for Pro plan write tools only)
- Node.js 18+ (for `mcp-remote` bridge to Claude Desktop)

---

## 🤖 Supported AI agents

| AI agent | Status | How to connect |
|---|---|---|
| **Claude Desktop** | ✅ Native | Auto-configured by installer |
| **Claude.ai** (web) | ⚠️ | Requires tunneling (ngrok, Cloudflare Tunnel) |
| **ChatGPT Desktop** | ✅ | Via Custom GPT with MCP connector |
| **Cursor** | ✅ | `~/.cursor/mcp.json` with `mcp-remote` bridge |
| **Windsurf** | ✅ | Config → Add MCP Server |
| **n8n** | ✅ | MCP node → point to `http://localhost:8000/mcp` |
| **Anthropic Agent SDK** | ✅ | `from claude_agent_sdk import MCPClient` |
| **Any MCP-compatible client** | ✅ | HTTP Streamable transport on `:8000/mcp` |

---

## 🚀 Quick Start (5 minutes)

### 1. Buy a license
Go to **[subiektgt.chat](https://subiektgt.chat?utm_source=github_releases&utm_medium=readme_quickstart)** and purchase Starter or Pro plan. **14-day free trial — no card required upfront if you use a promo code.**

### 2. Download the installer
Get the latest `.exe` from [Releases](https://github.com/dekoder12345/subiektmcp-releases/releases/latest).

```
SubiektMCP_Setup_v1.0.1.exe   (68.7 MB, SHA256 verified)
```

### 3. Run installer
- Launch `SubiektMCP_Setup_v1.0.1.exe`
- Enter your SQL Server connection (auto-detected from Subiekt GT registry in most cases)
- Paste your license key from the email you got after purchase
- Done — the MCP server starts automatically

### 4. Ask Claude
Open Claude Desktop and ask:
- _"Sprawdź stan magazynowy towaru ABC-001"_
- _"Pokaż mi 10 klientów z największymi nierozliczonymi fakturami"_
- _"Raport sprzedaży za ostatni miesiąc w układzie per klient"_
- _"Utwórz fakturę dla kontrahenta NIP 1234567890 na 5 szt. towaru SYMBOL_X"_ (Pro plan)

---

## 🏗️ Architecture

```
┌─────────────────────────────────────┐
│  AI Agent (Claude / ChatGPT / ...)  │
└─────────────────┬───────────────────┘
                  │  MCP Protocol (HTTP Streamable / STDIO)
                  ▼
┌─────────────────────────────────────┐
│  SubiektMCP Server                  │
│  (Windows Service, localhost:8000)  │
│  - FastMCP framework                │
│  - License verification (Keygen.sh) │
└───┬────────────────────────┬────────┘
    │ pyodbc                 │ pywin32 COM
    ▼                        ▼
┌─────────────┐       ┌──────────────┐
│ MS SQL      │       │ Sfera GT     │
│ Server      │       │ (InsERT.GT)  │
│ (Subiekt    │       │ COM API      │
│  database)  │       │              │
└─────────────┘       └──────────────┘
```

---

## 💰 Pricing

| Plan | Monthly | Yearly (~20% off) | What's included |
|---|---|---|---|
| **Starter** | 99 PLN | 950 PLN | 13 read tools, 14-day trial, 1 machine |
| **Pro** | 199 PLN | 1910 PLN | Starter + 4 write tools (Sfera GT), 3 machines floating |

**Partner program**: 15–35% recurring commission. [Apply here](https://panel.subiektgt.chat/register?utm_source=github_releases).

**Get license: [subiektgt.chat](https://subiektgt.chat?utm_source=github_releases&utm_medium=readme_pricing)**

---

## ❓ FAQ

### Can I use this with Subiekt Nexo?
No — SubiektMCP is specifically for **Subiekt GT** (the desktop ERP). Support for Subiekt Nexo is on the roadmap.

### Does it work offline?
Yes, up to 7 days. License is verified online every hour; if offline, the local cache keeps the server running for 7 days before requiring re-verification.

### Is my data safe?
Yes. The MCP server runs entirely on your Windows machine. Your ERP data never leaves your computer — AI agents connect directly to your local server. Only the license key is verified with Keygen.sh.

### What about GDPR / RODO compliance?
SubiektMCP is fully RODO-compliant. See our [Privacy Policy](https://subiektgt.chat/polityka-prywatnosci) and [Terms](https://subiektgt.chat/regulamin) (in Polish, as the target market is Poland).

### How do I cancel?
Via [panel.subiektgt.chat/konto](https://panel.subiektgt.chat/konto) — self-service cancellation, no fees.

### Can I self-host the server for multiple users?
Not currently. Each license is tied to a single machine (Starter) or a floating pool of 3 (Pro). Contact us for enterprise licensing.

### Is the source code open?
No — SubiektMCP is commercial software. This repository only contains installer binaries and release notes. The source is in a private repository.

### Is there a test/demo version?
14-day free trial. Sign up at [subiektgt.chat](https://subiektgt.chat?utm_source=github_readme_faq).

---

## 📦 Releases

All versioned installers live in [Releases](https://github.com/dekoder12345/subiektmcp-releases/releases).

- **v1.0.1** (latest) — Robust Claude Desktop config, PowerShell-based setup, Node.js prereq check
- **v1.0.0** — Initial release

---

## 📚 Documentation & Support

- **Pricing & signup**: [subiektgt.chat](https://subiektgt.chat?utm_source=github_docs)
- **Partner panel**: [panel.subiektgt.chat](https://panel.subiektgt.chat)
- **Support email**: pomoc@subiektgt.chat
- **Business inquiries**: kontakt@subiektgt.chat
- **Bug reports**: pomoc@subiektgt.chat (please include `python scripts/diagnose.py` output — the diagnose tool ships with the installer)

---

## 🏢 About

**SubiektMCP** is developed by Nikodem Rudziński (jednoosobowa działalność gospodarcza, Poland).
- **NIP**: 6932197547
- **REGON**: 529808221
- **Address**: ul. Gustawa Morcinka 16b/5, 67-200 Głogów, Poland
- **Email**: kontakt@subiektgt.chat

---

## 📄 License

This software is commercial and requires a valid license key to run. See `LICENSE.rtf` bundled with the installer, [Terms of Service](https://subiektgt.chat/regulamin), and [Privacy Policy](https://subiektgt.chat/polityka-prywatnosci).

---

## 🔑 Keywords

`mcp` `model-context-protocol` `subiekt-gt` `subiekt` `polish-erp` `erp-integration` `insert-gt` `sfera-gt` `claude` `anthropic-claude` `claude-desktop` `chatgpt` `cursor-ide` `windsurf` `n8n` `ai-agents` `llm-tools` `windows-service` `saas` `commercial-software` `poland-it` `mssql` `pyodbc` `fastmcp` `python-mcp` `ai-erp` `invoice-api` `inventory-api` `crm-integration`

**Szukasz integracji Subiekta GT z AI? SubiektMCP to serwer MCP, który łączy Twojego Subiekta z Claude, ChatGPT i innymi agentami AI. Działa na Windows, zgodny z RODO, stworzony w Polsce. Kup licencję: [subiektgt.chat](https://subiektgt.chat?utm_source=github_readme_keywords).**
