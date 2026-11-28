# BYOE Help — Bring Your Own ETF

*For BYOE version 0.2.3 (the latest release as of November 27, 2026). If you are on an older version, update first: the screens and steps below match v0.2.3.*

*Need help? Email [contact@uptier.com](mailto:contact@uptier.com).*

## Contents

- [What's new in v0.2.3](#whats-new-in-v023)
- [What BYOE is — and is not](#what-byoe-is--and-is-not)
- [Download and install](#download-and-install)
- [First run](#first-run)
- [Create your first recipe](#create-your-first-recipe)
- [Sleeves and strategies](#sleeves-and-strategies)
- [Connect a broker](#connect-a-broker)
- [The dependency check and installing uv](#the-dependency-check-and-installing-uv)
- [Rebalance a strategy](#rebalance-a-strategy)
- [Limit orders and the Orders tab](#limit-orders-and-the-orders-tab)
- [Reconcile outside trades](#reconcile-outside-trades)
- [ETF Mirror and Backtest](#etf-mirror-and-backtest)
- [Back up and restore your data](#back-up-and-restore-your-data)
- [Where your data lives and how it is protected](#where-your-data-lives-and-how-it-is-protected)
- [Wash-sale guard](#wash-sale-guard)
- [Copy ETFs](#copy-etfs)
- [Troubleshooting](#troubleshooting)

## What's new in v0.2.3  

v0.2.3 adds drift monitoring, real historical backtesting, Copy ETFs, a round of ease-of-use improvements, and security fixes. Details:  **Drift monitoring**: the Rebalance tab flags a sleeve when a holding or the sleeve drifts more than the threshold (5 percentage points by default; you can set a different threshold per sleeve). See Drift monitoring under Rebalance a strategy; **Real backtesting**: Backtest now uses real historical prices (Yahoo Finance first, Stooq as fallback) and compares your recipe with SPY and QQQ over a date range you choose with the slider. See ETF Mirror and Backtest; **Copy ETFs**: build a recipe from an ETF's holdings. See Copy ETFs; **Easier to use**: confirmation dialogs before destructive actions, inline validation, slider plus numeric inputs, a light/dark theme toggle, ticker search, recipe presets, true allocation bars, keyboard and focus improvements, saved Settings, autosave with undo, file uploads, clearer tables and formatting, per-action loading, labels and a glossary, a responsive layout, empty states, Toast notifications, MCP form presets, broker assignment shown on sleeves, Reviewed vs Done, and a dry-run banner; **Security fixes**: every order is capped at a $250,000 notional value, the uv installer is pinned to a specific version and SHA-256-verified before it runs, and every release ships SHA-256 checksums you can verify (see Download and install). Installers are not code-signed, and the public-release sync still uses a classic personal access token; see SECURITY.md in the source repository for the full picture.

## What BYOE is — and is not 
BYOE (Bring Your Own ETF) is a local-first desktop app for retail investors. You build custom stock baskets — called "Personal ETFs" or recipes — and rebalance them across your brokerage accounts.

- **Local-first.** Your recipes, positions, orders, and broker credentials stay on your own computer. There is no cloud storage, no remote server, and no SaaS fee for BYOE itself.
- **Your brokers, your accounts.** BYOE talks to the brokers you already use through their MCP (Model Context Protocol) servers. Your money stays at your broker; BYOE never holds your money.
- **You stay in control.** BYOE never trades on its own. Every real order needs you to confirm it, and dry-run mode is on by default (see [Rebalance a strategy](#rebalance-a-strategy)).

BYOE is **not**:

- A broker, an exchange, or a bank.
- Investment advice. BYOE is a tool for carrying out *your* decisions. It does not recommend stocks, ETFs, or allocations.
- A cloud service. There is no BYOE account to create and nothing to sync. If you move to a new computer, move your backup file with you (see [Back up and restore your data](#back-up-and-restore-your-data)).

## Download and install

Download the installer for your computer from the Releases page of the public download repository:

**https://github.com/up-tier/byoe-release/releases**

Pick the **latest** release. For v0.2.3 the files are:

| Your computer | File | Size |
| --- | --- | --- |
| Windows | `Uptier.BYOE.Setup.0.2.3.exe` | 133 MB |
| macOS (Intel) | `Uptier.BYOE-0.2.3.dmg` | 142 MB |
| macOS (Apple Silicon) | `Uptier.BYOE-0.2.3-arm64.dmg` | 138 MB |
| Linux | `Uptier.BYOE-0.2.3.AppImage` | 143 MB |
| Linux (Debian/Ubuntu) | `uptier-byoe_0.2.3_amd64.deb` | 112 MB |

To install:

1. Download the file for your computer from the Releases page above.
2. Open the downloaded file and follow the installer prompts (Windows), open the disk image and install the app (macOS), or install the `.deb` package / run the `.AppImage` file (Linux).
3. Launch BYOE from your applications menu. The header shows the product name and the app version, so you can check you are on v0.2.3.

Every release also ships SHA-256 checksum files (`SHA256SUMS-windows.txt`, `SHA256SUMS-macos.txt`, and `SHA256SUMS-linux.txt`). After downloading, check the installer against its checksum file: on Windows, run `(Get-FileHash <installer-file> -Algorithm SHA256).Hash` in PowerShell; on macOS or Linux, run `shasum -a 256 <installer-file>` or `sha256sum <installer-file>`. The hash must match the entry for your file — if it does not match, do not install it; download it again. The Releases page also lists "Source code (zip)" and "Source code (tar.gz)" archives. Those are auto-generated by GitHub from the public download repository, which contains only the README and this help file — they do not contain the BYOE app. Download one of the installer files above, not the source archives.

## First run

There is no BYOE account to create and nothing to sign up for. When you first open BYOE you see:

- The **Dashboard**, with an overview of your strategies and any drift alerts.
- A sidebar with the app's sections: **Dashboard**, **Recipe Builder**, **ETF Mirror**, **Broker**, **Rebalance**, **Orders**, **Reconcile**, **Backtest**, and **Settings**.
- Two broker connections already listed on the **Broker** tab — **Robinhood** and **Alpaca** — ready for you to connect (see [Connect a broker](#connect-a-broker)).

A good first session is: create a recipe, connect a broker in dry-run mode, and run a practice rebalance. Dry-run mode is on by default, so a practice run places no real orders.

## Create your first recipe

A recipe is your Personal ETF: a list of stocks and the percentage of the basket each one should be.

1. Open the **Recipe Builder** tab.
2. Choose **Create New Recipe** (or open an existing recipe to edit it).
3. Give the recipe a name and a short description.
4. Under **Choose stocks**, add the stocks you want:
   - Type a stock symbol (for example `AAPL`) and a weight in percent.
   - Add as many stocks as you like.
   - Your weights must add up to exactly **100.00%**. BYOE shows the running total and the total must be 100.00% before you can save.
   - **Auto-Normalize to 100%** scales your weights proportionally so they add up to 100%.
   - **Equal-Weight** gives every stock in the recipe the same weight (100% divided by the number of stocks).
5. Set a **drift threshold** (for example ±2.0%). BYOE flags the recipe as drifted when a holding moves outside its target weight by more than this amount. You can also set a **cash buffer** (for example 1.0%) to keep a small amount of cash uninvested.
6. If you hold shares you never want BYOE to sell — for example employer RSUs — add them as **excluded / "Do Not Touch"** holdings. BYOE leaves those shares alone.
7. Save the recipe.

You can come back and edit a recipe at any time. Editing a recipe changes the target for future rebalances; it does not place any orders by itself.

## Sleeves and strategies

A **sleeve** (also called a strategy) is one recipe run inside one brokerage account. One brokerage account can hold several sleeves, each with its own recipe and its own share of the account. For example, one account could hold a "Growth" sleeve and a "Dividend" sleeve, each following a different recipe.

- Each sleeve follows exactly one recipe.
- Each sleeve is assigned to exactly one broker, chosen in the **Rebalance** tab. A sleeve always trades through the broker you assigned to it — it never switches brokers on its own.
- You can add, edit, and remove sleeves from the **Recipe Builder** / **Rebalance** tabs.

## Connect a broker

BYOE connects to brokers through MCP servers, which you manage on the **Broker** tab. Two connections are already set up for you; you only need to connect them.

### Robinhood

Robinhood uses streamable HTTP, so it needs no extra software on your computer.

1. Open the **Broker** tab.
2. Find the **Robinhood** connection (MCP server URL: `https://agent.robinhood.com/mcp/trading`).
3. Click **Connect** / **Test** and follow the sign-in prompts from Robinhood. Sign-in happens with Robinhood; BYOE stores the resulting credentials on your computer only.
4. The connection shows its health: the last successful sync time, the last error (if any), and where the account snapshot came from.

### Alpaca (paper trading first)

Alpaca uses a local (stdio) MCP server, so your computer needs `uv`/`uvx` and Python 3.10 or newer first. See [The dependency check and installing uv](#the-dependency-check-and-installing-uv).

1. Open the **Broker** tab.
2. Find the **Alpaca** connection (MCP command: `uvx alpaca-mcp-server`).
3. Enter your **Alpaca paper API key and secret**. Alpaca is paper-first in BYOE: start with paper-trading keys. Paper keys are separate from live keys — get them from your Alpaca dashboard and keep live keys out of BYOE until you deliberately want live trading.
4. Click **Connect** / **Test**. BYOE starts the Alpaca MCP server on your computer and checks the connection.

### After connecting

- The **Broker** tab shows each connection's health. If a connection fails, it shows the last error so you know what went wrong.
- Click **Edit** on a connection to change its settings. (In versions before v0.2.1, Edit did nothing; this is fixed in v0.2.1.)
- **Dry-run is ON by default** for every broker. While dry-run is on, order batches return simulated results and no real order is placed. Turn dry-run off only when you are ready to place real orders — and see the first-real-order confirmation below.
- The first time you connect a broker, BYOE automatically reconciles the broker's holdings against your recipes so your starting positions are correct (see [Reconcile outside trades](#reconcile-outside-trades)).

### Brokers without an MCP server (read-only)

For brokers that have no MCP server — for example Fidelity, Vanguard, or Schwab — BYOE can still work from a CSV export of your account. Import the CSV on the **Reconcile** tab (see below). These accounts are read-only in BYOE: BYOE can show your positions and generate a step-by-step trade checklist (exported as CSV or formatted text you can copy), but it cannot place orders there for you. BYOE also detects money-market sweep funds in these imports (such as SPAXX, FDRXX, SWVXX, VMFXX, and FCASH) so sweep cash is not mistaken for a stock position.

## The dependency check and installing uv

`uv` (which includes `uvx`) is a Python package runner. BYOE needs it only for the Alpaca (stdio) connection, to start `uvx alpaca-mcp-server` on your computer. Robinhood (streamable HTTP) needs none of this.

When BYOE starts, it checks, in order:

1. **uv / uvx** — required to start the Alpaca MCP server. Without it, the Alpaca connection cannot start at all.
2. **Python 3.10 or newer** — required by `alpaca-mcp-server`. `uv` can download a suitable Python itself, so a missing Python is reported as a warning, not a blocker. A Python older than 3.10 that `uvx` would actually use is a blocker.
3. **Network access to PyPI** — `uvx` downloads `alpaca-mcp-server` from PyPI the first time it runs (unless it is already cached).

If something is missing, BYOE tells you what is missing and what to do, instead of failing silently. If the `uvx` start fails, the error message says what is missing and how to fix it.

### Install uv from inside BYOE (recommended)

1. When the check reports `uv`/`uvx` as missing, BYOE offers to install it for you.
2. Click **Install**. BYOE downloads the pinned uv release for your computer, verifies its SHA-256 checksum, and installs it. If the checksum does not match, the install is refused. BYOE then verifies the install with `uv --version`.
3. The installer puts `uv`/`uvx` in `~/.local/bin` and needs no administrator rights. BYOE adds that folder to the PATH it uses to start the Alpaca server, so the fresh install is found without restarting your computer.
4. **Restart BYOE after installing**, so the new PATH is picked up everywhere. If the install fails, the Install button stays available so you can try again.

### Install uv yourself (manual fallback)

If the in-app install does not work for you, install `uv` yourself, then restart BYOE:

- **Windows:** `winget install Astral.uv` — or download the installer from `https://astral.sh/uv`.
- **macOS:** `brew install uv`
- **Linux:** use the official installer from `https://astral.sh/uv`.
- Full instructions: `https://docs.astral.sh/uv/getting-started/installation/`

After installing, check it worked by opening a terminal and running `uv --version`. Then restart BYOE and reconnect Alpaca.

## Rebalance a strategy

Rebalancing brings a sleeve's actual holdings back in line with its recipe.

1. Open the **Rebalance** tab and pick a sleeve (strategy). The sleeve's assigned broker is shown — each sleeve trades on exactly one broker.
2. Choose the rebalance mode:
   - **Tax-Smart Mode (Cash-Inflow Only)** — deploys available cash and incoming dividends into underweight stocks. It places zero SELL orders.
   - **Full Rebalance Mode** — buys and sells to reach the recipe's target dollar values. Sells are ordered largest overweight first, so cash is freed before buys are placed.
3. Review the plan. BYOE shows, for each stock, the target weight, the current weight, and the buy or sell needed. It uses **effective holdings** — current holdings, plus pending buys, minus pending sells — so orders that are already open are not double-counted.
4. Check the **midpoint check** on each order. Every order is a limit order priced at the last price when the plan was made. Before an order goes out, BYOE checks its limit against the live bid/ask midpoint:
   - If the limit crosses the spread, or is more than **1%** from the midpoint, BYOE warns you.
   - If the limit is more than **5%** from the midpoint, BYOE asks you to explicitly confirm sending that order unchanged. It never changes your limit price silently.
   - If no quote is available, the check says so and does not block the order.
5. **Dry-run first.** Dry-run mode is ON by default, and dry-run orders are simulated — nothing is sent to your broker. Review the simulated result, then turn dry-run off only when you are ready.
6. **First real order.** The first time you place a real (non-dry-run) order, BYOE asks you to confirm it in the Smart Rebalance view. Nothing is sent until you confirm.
7. Confirm the rebalance. BYOE sends the orders to the assigned broker.

BYOE never trades automatically. There is a drift scheduler (Daily, Weekly, or Hourly, on market days, Monday–Friday 9:30 AM–4:00 PM ET) that can show you a tray notification when a strategy drifts past its threshold — but it only notifies you. Placing the orders is always your explicit action.

You can also export the rebalance plan as a **trade checklist** (CSV, or formatted text you can copy) if you prefer to place the trades yourself at your broker.

### Drift monitoring

The Rebalance tab has a Drift monitoring section at the top. It shows, for each sleeve and for the whole portfolio, how far each holding has drifted from its target weight — in percentage points (pp) and in %.

- **Drift in percentage points.** A holding targeted at 20% that now sits at 27% has drifted +7 pp. The % next to it is the relative change (+35% in that example); the threshold is always in pp.
- **Default threshold: 5 pp.** BYOE only suggests a rebalance when a holding or sleeve drifts strictly over the threshold. Below it, the tab says “Within threshold — no rebalance needed” and no trades are proposed. You can still look at the plan with “Review plan anyway” — nothing is traded unless you dispatch it yourself.
- **Change the threshold.** Edit the global threshold in the Drift monitoring section. To set a different threshold for one sleeve, use that sleeve's “Sleeve threshold (pp)” field; clear it (or “Use global”) to go back to the global default.
- **No price, no guess.** Prices come from your latest snapshot only. If a holding has shares but no price, it is listed under “Holdings without a price” and its drift is shown as unknown — BYOE never invents a price for it, and it never triggers a rebalance on its own.
- **When it checks.** Drift is checked when the Rebalance tab opens and whenever a new snapshot arrives (for example after a fresh import). The last check's date and the prices it used are shown under the threshold.


## Limit orders and the Orders tab

Every order BYOE places is a **limit order** — an order to buy or sell only at your limit price or better. A limit order is not a market order, and **placed does not mean filled**.

An order moves through these states:

- **Planned** — in the rebalance plan, not yet sent.
- **Placed** — sent to your broker.
- **Pending** — open at your broker, waiting to fill.
- **Filled** — fully bought or sold.
- **Partially filled** — part of the order filled; the rest is still open.
- **Cancelled** — cancelled before it filled.
- **Expired** — the order lapsed before it filled (for example at the end of the trading day).

What this means in practice:

- A pending order may fill later, fill partly, or never fill if the market price moves away from your limit. That is normal for limit orders.
- BYOE counts open orders in its effective holdings (current + pending buys − pending sells), so a pending buy is not bought twice.
- While BYOE is open, it checks the status of open orders in the background (about every 60 seconds, and whenever you connect or refresh).
- Cancelling an order never happens automatically. If you cancel an order at your broker, BYOE picks up the cancelled status on its next check.

The **Orders** tab is where you see all of this: the full trade and order log, order and fill history, and the option to export orders and fills as CSV (dates, prices, quantities, and cost basis).

## Reconcile outside trades

If you buy or sell outside BYOE — in your broker's own app, for example — BYOE's view of your holdings can fall behind. The **Reconcile** tab fixes that.

1. Open the **Reconcile** tab.
2. Import a CSV or snapshot of your external activity. A transaction CSV has one row per trade, with columns for date, symbol, side (buy/sell), quantity, price, and optional fees and an id.
3. BYOE compares the imported activity with your current plan and shows a report: which trades match trades BYOE already knows about, which match only partly, and which are new or unmatched.
4. **Nothing is applied when you import.** Review the report first.
5. Click **Apply** to apply the corrections you approve. Only then are your holdings updated. Applied corrections are stored locally as reconciliation history, so you can see what was reconciled and when.

The first time you connect a broker, BYOE also runs this reconciliation automatically, so your starting positions match the broker.

## ETF Mirror and Backtest

- **ETF Mirror** builds a recipe from one or more real ETFs. BYOE reads the ETF's published holdings (from the issuer's daily holdings CSV, or from SEC N-PORT filings for ETFs such as QQQ, SCHD, SPY, and VTI), blends them if you pick more than one (for example 60% QQQ + 40% SCHD), and lets you take the top holdings (Top 10, 20, 30, or all) and exclude shares you do not want to hold. The result is an ordinary recipe you can edit and rebalance like any other.
- **Backtest** runs a recipe over a date range you choose and compares it with SPY and QQQ, rebalancing monthly at month-end close, so you can see how the basket would have behaved. It uses real historical prices: Yahoo Finance first (adjusted closes), and Stooq as a fallback (split-adjusted closes only). Prices are cached locally for 24 hours. If no prices can be fetched, Backtest falls back to a small sample data set and clearly says so on the screen, so you always know whether you are looking at real prices or sample data. Use the slider to set the start and end dates; the default range is the last five years, within a 10-year window. Backtest only runs when you press **Run**.

## Back up and restore your data

Because there is no cloud copy of your data, **your backup file is your safety net**. Make one regularly and keep it somewhere safe.

### Back up

1. Open the **Orders** tab.
2. Use the backup option to save a JSON backup file to a location you choose.
3. The backup contains your recipes, positions, orders, and settings. It **never** contains broker credentials or OAuth tokens — those stay in your operating system's keychain (see below).

### Restore

1. Open the **Orders** tab and use the restore option.
2. Pick the backup file.
3. BYOE checks the file first, then asks you to confirm before replacing all current data. Confirm only if you are sure — restoring replaces everything in the app with the contents of the backup.
4. After restoring, reconnect your brokers. Your broker credentials were not in the backup, so you will need to sign in / re-enter keys for each broker.

### Moving to a new computer

Install BYOE on the new computer, restore from your backup file, then reconnect your brokers. Do not copy the database file itself to the new computer — it is encrypted with a key from the old computer and will not open there (see below).

## Where your data lives and how it is protected

- Your data is stored in a local SQLite database file, `uptier-byoe.sqlite`, in the app's user-data folder on your computer.
- The database is encrypted with **AES-256-GCM**. Its 32-byte encryption key is stored in your operating system's keychain (the file `uptier-byoe.key` is the keychain-backed reference to it): Windows Credential Manager (DPAPI), macOS Apple Keychain, or the Linux Secret Service (GNOME Keyring / KWallet).
- Because the key lives in your computer's keychain, a copy of the database file alone is unreadable on another machine. That is by design. Restore from a backup file instead of copying the database.
- If the keychain is unavailable on a machine, the database stays unencrypted and the **Orders** tab says so. Your data still works locally, but it is not encrypted at rest on that machine.
- Uninstalling BYOE deletes the app. A **clean uninstall wipes the user-data folder**, including the database — so make a backup before uninstalling if you want to keep your data.
- No data is sent to Uptier or to any cloud service. The only network traffic BYOE makes is to your brokers (through their MCP servers), to PyPI the first time `uvx` downloads the Alpaca MCP server, and to Astral if you use the in-app `uv` installer.

## Wash-sale guard

If you sell a stock at a loss and buy the same stock back within 30 days before or after the sale, the tax rules on wash sales may apply. BYOE's wash-sale guard watches for this: a sale at a loss that would be repurchased within the 30-day window is blocked, or flagged, depending on the guard's mode. If a rebalance is blocked or flagged for this reason, review the affected stock before you proceed — BYOE will not silently work around it.

## Copy ETFs

> **Shipped in v0.2.2.** Copy ETFs is in the version you can download today.

**Copy ETFs** is a way to create a recipe:

1. When creating a recipe, choose **Copy ETFs** instead of **Choose stocks**.
2. Enter ETF tickers separated by commas (for example `VOO, QQQ`).
3. BYOE looks up each ETF's holdings and builds the recipe from them. Review the resulting holdings before you save — the recipe is only saved when you confirm it.
4. Holdings that appear in more than one ETF are summed; the whole recipe is normalized to 100%.
5. Very small holdings (under 0.10%) and holdings beyond the Top 50 are set aside, with a reason shown for each, and you can restore them.

The holdings come from the ETF issuer's daily holdings CSV, fetched over HTTPS — no API key is needed. iShares holdings are seeded in the app, so **IVV** works offline. For other ETFs, paste or upload a holdings CSV — BYOE validates the tickers and weights locally. If an ETF is not supported, or a lookup fails, BYOE says so in the app. It never invents holdings.

## Troubleshooting

| Problem | What to do |
| --- | --- |
| "`uvx` not found" or the Alpaca connection fails to start | Run the dependency check in BYOE, then use the in-app **Install** for `uv`. If that does not work, install `uv` yourself — Windows: `winget install Astral.uv`; macOS: `brew install uv`; Linux: the installer at `https://astral.sh/uv`. Then restart BYOE and reconnect Alpaca. Check that your computer can reach PyPI (`https://pypi.org/simple/`). |
| **Edit** on a broker connection did nothing | That was a bug in versions before v0.2.1. Update to v0.2.1 — Edit now opens the edit form. |
| Broker text is hard to read (dark text on a dark background) | That was a display bug fixed in v0.2.1. Update to v0.2.1. |
| A broker connection shows an error | Open the **Broker** tab and read the last error shown for that connection. For Alpaca, check your paper API key and secret, and that `uv`/`uvx` and Python 3.10+ are installed. For Robinhood, try reconnecting and signing in again. |
| An order stays pending for a long time | Your limit price may be away from the current market price, so the order has not filled. Check the **Orders** tab and your broker. A pending order can fill later, fill partly, or expire. |
| Rebalance numbers look wrong after trading outside BYOE | Your holdings are out of date. Use the **Reconcile** tab to import a CSV or snapshot of the outside trades, review the report, and click **Apply**. |
| You moved to a new computer and your data is missing or will not open | The database is encrypted with a key from your old computer's keychain and cannot be opened on a new machine. Restore from a JSON backup file instead (see [Back up and restore your data](#back-up-and-restore-your-data)). If you have no backup, your recipes and history on the old machine are the only copy. |
| The Orders tab says the database is unencrypted | The operating-system keychain was unavailable on this machine, so the database is stored unencrypted. Your data still works, but it is not encrypted at rest here. |
| BYOE says a rebalance is blocked or flagged for a wash sale | A sale at a loss would be repurchased within the 30-day window. Review the affected stock and decide whether to proceed. BYOE will not work around the guard silently. |
| Something else is wrong | Email [contact@uptier.com](mailto:contact@uptier.com). Say which version of BYOE you are on (shown in the app header), which tab you were on, and what you saw — including the exact error message if there is one. |

## A final note

BYOE is a tool for carrying out your own investment decisions. It does not give investment advice, and it does not promise any return. Check your orders before you confirm them, keep a backup of your data, and if anything in this guide does not match what you see in the app, email [contact@uptier.com](mailto:contact@uptier.com) and tell us which version you are on.
