# multi-bank

> Multi-Bank Account Manager with broadcast architecture pattern. Aggregates financial data from Monobank and PrivatBank via MCP tools, broadcasts balance updates and budget alerts to subscribed components, categorizes transactions, and exports financial reports in CSV/PDF.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/multi-bank

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.monobank]
url = "${MONOBANK_MCP_URL}"
type = "http"

[mcp_servers.privatbank]
url = "${PRIVATBANK_MCP_URL}"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **bank-api-integration** — This skill should be used when connecting to bank MCP servers, fetching account balances or transaction statements, handling MCP connection errors, or understanding bank API data formats. Relevant for queries like "show my balance", "connect my bank", "fetch last month transactions", or "why is my bank connection failing". Use this skill whenever the user mentions bank accounts, balances, transactions, MCP connections, or any bank data operation, even if they don't explicitly mention "API" or "MCP".
- **bank-balances** — This skill should be used when showing account balances across all connected banks as a unified table. It covers the BROADCAST pattern for fetching balances from every bank simultaneously and merging them into one view. Use this skill whenever the user asks about their balance, account totals, how much money they have, or wants to see all accounts — even if they only mention one bank, because the plugin always queries all connected banks.
- **bank-reports** — This skill should be used when generating analytical financial reports — spending by category, income vs expenses, period comparisons, or spending trends across all connected banks. Use this skill whenever the user asks for a financial summary, analytics, spending breakdown, income report, period comparison, or any aggregated view of their financial data, even if they just say "where does my money go" or "скільки я витратив цього місяця".
- **bank-statements** — This skill should be used when creating a bank statement (виписка) for a specific account and period — a formal document listing all transactions with opening and closing balances. Use this skill whenever the user asks for a statement, виписка, account extract, or needs a formal record of transactions for a specific account, even if they just say "зроби виписку" or "I need a statement for my account".
- **bank-transactions** — This skill should be used when listing transaction history across all connected banks for a given period as a single chronological list. It covers fetching transactions via BROADCAST, merging results, date range handling, and pagination across different bank APIs. Use this skill whenever the user asks about transactions, recent purchases, spending history, what they spent money on, or wants to see operations across banks — even for a single bank, because the plugin always queries all.
- **broadcast-pattern** — This skill should be used when setting up real-time financial event notifications, configuring event subscribers, understanding event payload formats, or implementing the broadcast pub/sub system. Relevant for queries like "notify me when balance changes", "set up transaction alerts", "how does the event system work", or "configure WebSocket updates". Use this skill whenever the user asks about real-time updates, event notifications, webhooks, or any form of live data streaming from bank accounts.
- **budget-alerts** — This skill should be used when setting spending budgets, checking budget utilization, configuring alert thresholds at 50/75/90/100%, or troubleshooting budget alerts. Relevant for queries like "set a monthly dining budget", "how much budget do I have left", "why did I not get a budget alert", or "show budget status for all categories". Use this skill whenever the user mentions spending limits, budgets, overspending, or wants to track expenses against targets, even for simple budget questions.
- **currency-rates** — This skill should be used when checking currency exchange rates, viewing USD or EUR rate history, comparing rates across banks, or monitoring rate fluctuations. Relevant for queries like "what is the dollar rate today", "show EUR exchange rate history", "compare bank rates for USD", or "how has the rate changed this week". Use this skill whenever the user asks about dollars, euros, exchange rates, currency conversion, or any mention of USD, EUR, or курс.
- **e-documents** — This skill should be used when listing electronic documents (EDO) from bank inbox or outbox, viewing document signatures, downloading documents, or managing document exchange. Relevant for queries like "show my document inbox", "list outgoing documents for March", "check signature on document", or "download document from bank". Use this skill whenever the user mentions documents from bank, EDO, inbox/outbox, acts, invoices, or any bank document exchange.
- **encrypted-storage** — This skill should be used when encrypting or decrypting financial data files, rotating the storage passphrase, understanding where data is stored, or troubleshooting encryption errors. Relevant for queries like "encrypt my transaction data", "change my storage passphrase", "where are my bank files stored", or "decrypt accounts file". Use this skill whenever the user asks about data security, encryption, storing financial data, passphrase management, or where their data is saved.
- **examples** — This skill should be used when looking for complete workflow examples, step-by-step setup guides, architecture diagrams, or command references. Relevant for queries like "how do I set up multi-bank", "show me an example of the budget alert flow", "walk me through generating a report", or "show first-time setup guide". Use this skill whenever the user asks "how do I...", wants a walkthrough, needs a step-by-step guide, or is setting up the plugin for the first time.
- **payments** — This skill should be used when preparing or sending payments, checking payment status, validating IBAN numbers, making tax or budget payments, or tracking payments by reference. Relevant for queries like "send a payment to this IBAN", "check payment status", "make a tax payment", "is my payment processed yet", or "prepare a payment of 10000 UAH". Use this skill whenever the user mentions sending money, paying someone, IBAN, payment status, or any transfer operation.
- **report-export** — This skill should be used when exporting transaction data to CSV or PDF, generating financial reports for a date range, or configuring report layout and format options. Relevant for queries like "export my transactions to CSV", "generate a PDF report for last quarter", "export 90 days of data", or "create a monthly financial report". Use this skill whenever the user wants to export, download, save, or generate any file from their financial data — CSV, PDF, report, or statement.
- **salary-management** — This skill should be used when managing employee salary contacts, creating salary payment registries, uploading or distributing payslips, generating payslip PDFs, or working with Maspay salary batches. Relevant for queries like "add an employee to salary project", "create a salary registry for March", "send payslips to employees", or "list salary contacts". Use this skill whenever the user mentions salary, payroll, employees, payslips, or any HR-related banking operation.
- **transaction-categorization** — This skill should be used when categorizing bank transactions, mapping MCC codes to spending categories, matching Ukrainian merchant names, or analyzing spending by category. Relevant for queries like "what category is this transaction", "show spending by category", "add a custom category rule", or "where does my money go". Use this skill whenever the user asks about spending categories, where their money goes, merchant classification, or wants to analyze transactions by type.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **bank-account-manager** — Multi-bank financial manager. Connects to Monobank and PrivatBank via MCP, aggregates balances and transactions, sets budget alerts, categorizes spending, and exports financial reports using the broadcast architecture pattern.

<example>
user: "Show my Monobank balance"
</example>
<example>
user: "Show balances across all my accounts"
</example>
<example>
user: "What did I spend on groceries last month?"
</example>
<example>
user: "Show my PrivatBank transactions"
</example>
<example>
user: "Set a budget for dining and track across both banks"
</example>
<example>
user: "Export a financial report for Q1"
</example>
<example>
user: "Prepare a payment to this IBAN"
</example>
<example>
user: "Create a salary registry for March"
</example>
<example>
user: "Upload payslips and send to employees"
</example>
<example>
user: "Show me the electronic documents inbox"
</example>
<example>
user: "What's the current USD/UAH exchange rate?"
</example>
<example>
user: "List all corporate cards"
</example>

- **broadcast-architect** — Broadcast architecture specialist. Helps design and implement the publisher-subscriber pattern for financial event broadcasting, WebSocket server setup, event-driven UI updates, and subscription management.

<example>
user: "Help me set up event broadcasting for balance updates"
</example>
<example>
user: "Design a subscriber for budget alerts"
</example>
<example>
user: "Debug why my balance_updated events aren't reaching the dashboard"
</example>
<example>
user: "Show me the broadcast system architecture"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `balances`

Show current balances across all connected bank accounts

Arguments: `bank-name`

<details><summary>Prompt template</summary>

# Balances

Display current balances for all connected bank accounts, grouped by bank.

## Arguments
Format: `[bank-name]`
- bank-name: Optional filter — "monobank", "privatbank", or omit for all

Parse from "$ARGUMENTS".

## Process

1. **Fetch balances via MCP:**
   - List available MCP tools to discover monobank/privatbank tool names
   - Call balance/accounts tools for each connected bank
   - If a bank's MCP server is not responding, report the error and continue with others

2. **Display balances:**
   - Group by bank (Monobank, PrivatBank)
   - Show account name with masked number (****1234)
   - Show current balance in UAH
   - Calculate totals across all banks

3. **Format:**
   ```
   Account Summary
   ──────────────────────────────────────
   Monobank
     Чорна картка ****1234     ₴15 432,10
     Біла картка ****5678       ₴8 200,00

   PrivatBank
     Картка ****3456            ₴2 150,30
     Депозит ****7890          ₴50 000,00
   ──────────────────────────────────────
   Total:                     ₴75 782,40
   ```

4. **If bank-name filter provided:**
   Show only that bank's accounts.

## Example Usage
```
/balances
/balances monobank
/balances privatbank
```

</details>

### `broadcast-status`

Show broadcast system status and active subscribers

<details><summary>Prompt template</summary>

# Broadcast Status

Display the current state of the broadcast event system — active subscribers, recent events, and delivery statistics.

## Process

1. **Read event log:**
   ```bash
   tail -50 ~/.multi-bank/events.jsonl
   ```
   Parse recent events for statistics.

2. **Display status:**
   ```
   Broadcast System Status
   ──────────────────────────────────────
   Event Log: ~/.multi-bank/events.jsonl
   Total Events: 1,247

   Recent Events (last 24h):
     balance_updated    ×12
     transaction_added  ×45
     budget_alert       ×3
     sync_complete      ×2

   Last Sync: 2026-03-23T14:30:00Z
   Last Event: 2026-03-23T14:30:05Z
   ──────────────────────────────────────

   Delivery Mechanisms:
     File-based (events.jsonl): Active
     WebSocket (ws://localhost:8765): Not running
     HTTP Polling: Not running
   ```

3. **Show last 5 events:**
   Display the 5 most recent events from the log with type, timestamp, and key payload fields.

4. **Suggest actions:**
   - Start WebSocket server for real-time subscribers
   - Review broadcast-pattern skill for setup guide
   - Check specific event type: `grep "budget_alert" events.jsonl`

## Example Usage
```
/broadcast-status
```

</details>

### `budget-status`

Show budget utilization and active alerts

Arguments: `category`

<details><summary>Prompt template</summary>

# Budget Status

Display spending progress against all budgets, or a specific category.

## Arguments
Format: `[category]`
- category: Optional — show only this category's budget

Parse from "$ARGUMENTS".

## Process

1. **Load data:**
   - Decrypt budgets.enc for budget definitions
   - Decrypt transactions.enc for spending data

2. **Calculate spending per budget:**
   For each active budget:
   - Determine current period date range
   - Sum transactions matching category and period
   - Calculate percentage used
   - Determine remaining budget

3. **Display status table:**
   ```
   Budget Status — March 2026
   ─────────────────────────────────────────────────
   Category     Budget    Spent     Left      Status
   ─────────────────────────────────────────────────
   Dining       ₴5 000    ₴4 757    ₴243      95% !!
   Groceries    ₴8 000    ₴4 231    ₴3 769    53% !
   Transport    ₴2 000    ₴895      ₴1 105    45%
   Shopping     ₴3 000    ₴3 124    -₴124     104% !!!
   ─────────────────────────────────────────────────

   Legend: (none)=<50%  !=50-74%  !!=75-99%  !!!=100%+
   ```

4. **If specific category:**
   Show detailed view with:
   - Daily spending trend
   - Top merchants in this category
   - Days remaining in period
   - Projected end-of-period spending

## Example Usage
```
/budget-status
/budget-status Dining
```

</details>

### `connect-bank`

Connect a bank account — verify MCP server connectivity

Arguments: `<bank-name>`

<details><summary>Prompt template</summary>

# Connect Bank

Verify connectivity to a bank's MCP server and list available accounts.

## Arguments
Format: `<bank-name>`
- bank-name: "monobank" or "privatbank"

Parse from "$ARGUMENTS".

## Process

1. **Verify MCP server:**
   - List available MCP tools to check if the bank's server is connected
   - If no tools found for the bank, inform user to configure .mcp.json with the correct URL

2. **Fetch accounts:**
   - Call the bank's account listing MCP tool
   - Display available accounts with masked numbers

3. **Confirm connection:**
   ```
   Monobank connected!
   Accounts found:
     • Чорна картка ****1234  Balance: ₴15 432,10
     • Біла картка ****5678   Balance: ₴8 200,00
   ```

4. **If MCP server not configured:**
   ```
   Monobank MCP server not found.
   Configure .mcp.json in the plugin directory:
   {
     "mcpServers": {
       "monobank": {
         "type": "http",
         "url": "https://your-mcp-server.example.com/mcp/monobank"
       }
     }
   }
   ```

## Example Usage
```
/connect-bank monobank
/connect-bank privatbank
```

</details>

### `corporate-cards`

List corporate cards with balances and status

Arguments: `open|closed|all`

<details><summary>Prompt template</summary>

# Corporate Cards

List corporate cards from connected bank MCP servers.

## Arguments
Format: `[filter]`
- `open` — show only active cards (default)
- `closed` — show only closed cards
- `all` — show all cards

Parse from "$ARGUMENTS". If empty, default to "open".

## Process

1. **Discover corporate card tools:**
   - Search for tools with keywords: `corporate`, `card`, `картк`
   - If not found, inform user that their MCP server doesn't support corporate card listing

2. **Fetch cards:**
   - Pass filter: `o` for open, `c` for closed, empty for all
   - Parse response — may include multiple corporations with their cards

3. **Display results:**
   ```
   Корпоративні картки:

   | # | Картка      | Статус  | Баланс     | Валюта |
   |---|------------|---------|------------|--------|
   | 1 | ****1234   | Активна | ₴25 000,00 | UAH    |
   | 2 | ****5678   | Активна | $1 200,00  | USD    |
   | 3 | ****9012   | Закрита | ₴0,00      | UAH    |
   ```

4. **Offer related actions:**
   - View transactions for a specific card
   - Check balance history

## Safety

- **Always mask card numbers** — show only last 4 digits
- Never display full card numbers in output

</details>

### `currency-rates`

Show current or historical currency exchange rates

Arguments: `history <start-date> <end-date>`

<details><summary>Prompt template</summary>

# Currency Rates

Show currency exchange rates from connected bank MCP servers.

## Arguments
Format: no arguments (current rates) or `history <start-date> <end-date>`

Parse from "$ARGUMENTS". If empty, show current rates.

## Process

### Current Rates (no arguments)

1. **Discover rate tools:**
   - Search for tools with keywords: `currency`, `rate`, `курс`
   - If not found, inform user that their MCP server doesn't support currency rates

2. **Fetch and display:**
   ```
   Курси валют (станом на 23.03.2026):

   | Валюта | Купівля  | Продаж   |
   |--------|----------|----------|
   | USD/UAH | 41.20   | 41.80    |
   | EUR/UAH | 44.50   | 45.30    |
   ```

3. If multiple banks connected, show comparison:
   ```
   | Валюта  | Monobank     | PrivatBank   |
   |---------|-------------|--------------|
   | USD buy  | 41.20      | 41.15        |
   | USD sell | 41.80      | 41.85        |
   ```

### History (with dates)

1. Parse start/end dates from arguments
2. Note: max 15 days per request — split if needed
3. Convert dates to format expected by tool (check schema)
4. Display as table or trend summary

## Notes

- Not all MCP servers provide currency rate tools
- History is limited to 15 days per request

</details>

### `edoc-journal`

Browse electronic documents (EDO) — inbox, outbox, all documents

Arguments: `<inbox|outbox|all> [date-range]`

<details><summary>Prompt template</summary>

# Electronic Document Journal

Browse electronic documents exchanged with bank counterparties via MCP tools.

## Arguments
Format: `<view> [start-date] [end-date]`
- `inbox` — incoming documents
- `outbox` — outgoing documents
- `all` — all documents

Optional date range: `2026-03-01 2026-03-31` (defaults to last 30 days)

Parse from "$ARGUMENTS". If empty, default to "all" for last 30 days.

## Process

1. **Discover EDO tools:**
   - Search for tools with keywords: `edoc`, `document`, `journal`, `inbox`, `outbox`
   - If not found, inform user that their MCP server doesn't support EDO

2. **Fetch documents:**
   - Convert dates to the format expected by the tool (check schema — typically YYYY-MM-DD)
   - Call the appropriate journal endpoint (inbox/outbox/all)
   - Handle pagination if results exceed page limit

3. **Display results:**
   Present as a table:
   ```
   | # | Дата       | Тип    | Статус   | Від/Кому (ОКПО) | Сума       |
   |---|-----------|--------|----------|------------------|------------|
   | 1 | 2026-03-20 | Акт    | Підписано | ****5678        | ₴15 000,00 |
   | 2 | 2026-03-19 | Рахунок | Очікує   | ****1234        | ₴8 500,00  |
   ```

4. **Offer actions:**
   - View specific document details
   - Check signature info
   - Delete document (with confirmation)

## Safety

- **Mask ОКПО/ЄДРПОУ** — show only last 4 digits unless user requests full code
- Confirm before deleting documents

</details>

### `export-report`

Export financial report as CSV or PDF

Arguments: `<format> [date-range]`

<details><summary>Prompt template</summary>

# Export Report

Generate and export a financial report in CSV or PDF format.

## Arguments
Format: `<format> [date-range]`
- format: `csv` or `pdf`
- date-range: Time period — `30d`, `90d`, `ytd`, `last-month`, `2026-Q1`, `2026-03`, or explicit `YYYY-MM-DD to YYYY-MM-DD`

Parse from "$ARGUMENTS". Default date-range: `90d`.

## Process

1. **Resolve plugin directory:**
   ```
   Glob: multi-bank/scripts/export_csv.js → parent dir
   ```

2. **Check dependencies:**
   For PDF: verify pdfkit is installed (`node -e "require('pdfkit')"`)
   If missing, ask permission to `npm install`.

3. **Ask output location:**
   "Where should I save the file?" Default: current working directory.

4. **Prepare data:**
   - Decrypt transactions.enc and accounts.enc
   - Parse date range using shortcuts from report-export skill
   - Filter and aggregate transactions

5. **Build input JSON:**
   Write to temp file following the format in report-export skill.

6. **Run export script:**
   ```bash
   node <plugin_dir>/scripts/export_csv.js /tmp/export_input.json
   # or
   node <plugin_dir>/scripts/export_pdf.js /tmp/report_input.json
   ```

7. **Verify and report:**
   - Check script output JSON for success
   - Report file path and size
   - Clean up temp input file

## Example Usage
```
/export-report csv 90d
/export-report pdf 2026-Q1
/export-report csv last-month
/export-report pdf ytd
```

</details>

### `payslips`

Manage payslips — upload, send to employees, check status, generate PDF

Arguments: `<upload|send|status|pdf|delete> <period> [identification]`

<details><summary>Prompt template</summary>

# Payslips (Розрахункові листки)

Manage employee payslips through bank MCP tools.

## Arguments
Format: `<action> <period> [identification]`
- `upload <YYYY-MM>` — upload payslips for a period
- `send <YYYY-MM>` — send payslips to employees' mobile apps
- `status <YYYY-MM>` — check import status
- `pdf <YYYY-MM> <identification>` — generate PDF for specific employee
- `delete <YYYY-MM>` — delete import for a period

Parse from "$ARGUMENTS". If empty, show available actions and ask for period.

## Process

### Action: upload

1. **Discover payslip tools** (keywords: `payslip`, `upload`, `import`, `batch`)
2. **Collect payslip data:**
   Ask user for source — JSON file path, CSV file path, or manual input.

   Each employee entry needs:
   - `identification` — ІПН (tax ID)
   - `attributes` — array of salary components:
     ```
     { attributeName: "Базова зарплата", value: "50000", attributeSuffix: "грн", sortOrder: 1 }
     { attributeName: "Премія", value: "5000", attributeSuffix: "грн", sortOrder: 2 }
     { attributeName: "ПДФО (18%)", value: "-9900", attributeSuffix: "грн", sortOrder: 3 }
     { attributeName: "До виплати", value: "45100", attributeSuffix: "грн", sortOrder: 4 }
     ```

3. **Validate:**
   - Period format: YYYY-MM
   - Max 1000 employees per batch
   - Each employee must have identification + at least 1 attribute

4. **Confirm:**
   ```
   Період:       2026-03
   Працівників:  45
   ```
   Ask: "Завантажити розрахункові листки?"

5. **Upload and report** — call batch upload MCP tool

### Action: send

1. **Confirm period** with user
2. **Warn:** "Розрахункові листки будуть надіслані всім працівникам у мобільний додаток"
3. Call send-to-mobile MCP tool with period
4. Report result

### Action: status

1. Call import status MCP tool with period
2. Display: upload state, employee count, errors if any

### Action: pdf

1. Call PDF generation MCP tool with identification and period
2. Save or display the generated PDF
3. Note: identification = ІПН of the employee

### Action: delete

1. **Confirm:** "Видалити всі розрахункові листки за {period}? Цю дію не можна скасувати."
2. Call delete import MCP tool with period
3. Report result

## PrivatBank Paysheets

PrivatBank uses a different paysheet model (journal-based via SFTP, not batch upload).
Discover paysheet tools with keywords: `paysheet`, `journal`, `savePaysheetFiles`.

If PrivatBank paysheet tools are detected:
- `getPaysheetJournal` — list payslip projects
- `savePaysheetFiles` — save uploaded files to SFTP

The workflow differs from Monobank — check available tools and adapt accordingly.

## Safety

- **Mask ІПН** in output — show only last 4 digits
- **Confirm before send** — payslips go to all employees at once
- **Confirm before delete** — irreversible
- **Validate period format** — must be YYYY-MM

</details>

### `prepare-payment`

Prepare a payment to a recipient via bank MCP tools

Arguments: `<recipient-iban> <amount-uah> <destination>`

<details><summary>Prompt template</summary>

# Prepare Payment

Prepare a payment through the bank's MCP server. The payment will be sent to the user's mobile app for signing.

## Arguments
Format: `<recipient-iban> <amount-uah> <destination>` or interactive (no arguments).

Parse from "$ARGUMENTS". If empty or incomplete, ask user for each field.

## Process

1. **Discover payment tools:**
   - Search available MCP tools for keywords: `payment`, `prepare`, `pay`
   - If no payment tools found, inform user that their MCP server doesn't support payments

2. **Collect payment details:**
   - Sender IBAN — ask user which account to send from (list accounts first)
   - Recipient IBAN — validate UA + 27 digits format
   - Recipient ЄДРПОУ — 8-digit company code
   - Recipient name — legal entity name
   - Amount — in UAH (convert to kopiykas: × 100)
   - Destination — payment purpose (6–420 characters)
   - External reference (optional) — for tracking

3. **Confirm with user:**
   Display a summary table:
   ```
   Від:          ****0001 (Monobank)
   Кому:         ****5678 (Назва отримувача)
   ЄДРПОУ:       12345678
   Сума:         ₴10 000,00
   Призначення:  Оплата за послуги за березень 2026
   ```
   Ask: "Підтвердити платіж? Після підготовки його потрібно підписати у мобільному додатку."

4. **Execute payment:**
   - Call the prepare-payment MCP tool with collected parameters
   - Amount must be in kopiykas (integer)
   - Currency: "980" (UAH)

5. **Report result:**
   - Show payment ID for tracking
   - Remind user to sign in mobile app
   - Offer to check payment status

## Budget Payments

If user mentions "бюджетний платіж", "податки", or "ПДВ":
- Ask for `payCode` (3-character code)
- Ask for `additionalInfo` (6–130 characters)
- Include both in the payment call

## Safety

- **Always confirm** before executing — payments are irreversible after signing
- **Mask IBANs** — show only last 4 digits
- **Display amounts in UAH**, not kopiykas
- Warn: "Після підпису платіж не можна скасувати"

</details>

### `salary-registry`

Manage salary registries — create, check status, list types

Arguments: `<create|status|types> [registry-id]`

<details><summary>Prompt template</summary>

# Salary Registry

Manage salary registries (зарплатні відомості) through bank MCP tools.

## Arguments
Format: `<action> [id]`
- `types` — list available registry types
- `create` — create a new salary registry (interactive)
- `status <registry-id>` — check registry status

Parse from "$ARGUMENTS". If empty, show available actions.

## Process

### Action: types

1. Discover salary registry type tools (keywords: `salary`, `registry`, `type`)
2. Call the types endpoint
3. Display available types in a table

### Action: create

1. **Discover salary tools:**
   - Search for tools with keywords: `salary`, `registry`, `create`
   - If not found, inform user their MCP doesn't support salary operations

2. **Collect registry details:**
   - Registry name (e.g., "Заробітна плата за березень 2026")
   - Sender IBAN — company account to send from
   - Registry type — from available types
   - Period: from date (YYYY-MM-DD) and to date (YYYY-MM-DD)

3. **Collect recipients:**
   Ask user for a list of recipients. Accept as:
   - Typed list (ПІБ, IBAN, сума)
   - JSON file path
   - CSV file path

   Each recipient needs:
   - fullName: "Прізвище Ім'я По батькові"
   - iban: recipient's IBAN
   - amount: in **kopiykas** (convert from UAH × 100)
   - inn (optional): ІПН

4. **Confirm:**
   Show summary:
   ```
   Відомість:    Заробітна плата за березень 2026
   Від рахунку:  ****0001
   Тип:          salary
   Період:       2026-03-01 — 2026-03-31
   Отримувачів:  15
   Загальна сума: ₴450 000,00
   ```
   Ask: "Підтвердити створення відомості?"

5. **Execute and report:**
   - Create registry via MCP tool
   - Return registry ID
   - Offer to check status

### Action: status

1. Call the salary registry status tool with the provided ID
2. Display current status and details
3. If still processing, offer to check again later

## Safety

- **Always confirm** before creating registries — involves real money
- **Mask IBANs and ІПН** in output
- **Show amounts in UAH**, not kopiykas
- **Verify total** — sum all recipient amounts and display before confirming

</details>

### `set-budget`

Create or update a budget threshold with alerts

Arguments: `<category> <amount> [period]`

<details><summary>Prompt template</summary>

# Set Budget

Create or update a spending budget for a category with automatic alert thresholds.

## Arguments
Format: `<category> <amount> [period]`
- category: Spending category (Dining, Groceries, Shopping, Transportation, etc.)
- amount: Budget limit in UAH
- period: Budget period — weekly, monthly (default), quarterly, yearly

Parse from "$ARGUMENTS".

## Process

1. **Validate category:**
   Check against known categories from transaction-categorization skill.
   If unknown category, warn but allow (user may have custom categories).

2. **Create budget definition:**
   ```json
   {
     "id": "budget_<category>_<period>",
     "category": "<category>",
     "amount": <amount>,
     "currency": "UAH",
     "period": "<period>",
     "thresholds": [50, 75, 90, 100],
     "accountFilter": [],
     "enabled": true
   }
   ```

3. **Check for existing budget:**
   - Decrypt budgets.enc
   - If budget for same category+period exists, update it
   - If new, add to the array

4. **Store encrypted:**
   Re-encrypt and write budgets.enc

5. **Confirm:**
   ```
   Budget created!
     Category: Dining
     Limit: ₴5 000,00/month
     Alerts at: 50%, 75%, 90%, 100%
     Tracking: All accounts
     Period: Mar 1 - Mar 31, 2026
   ```

6. **Show current spending if available:**
   If transactions exist for this category in the current period, show progress.

## Example Usage
```
/set-budget Dining 5000
/set-budget Groceries 8000 monthly
/set-budget Entertainment 1500 weekly
/set-budget Travel 20000 quarterly
```

</details>

### `sync-accounts`

Force-sync all connected bank accounts via MCP

<details><summary>Prompt template</summary>

# Sync Accounts

Trigger a full sync of all connected bank accounts — fetch latest balances and transactions from Monobank and PrivatBank via MCP, emit broadcast events.

## Process

1. **Discover available banks:**
   - List available MCP tools
   - Identify which banks have active MCP connections (monobank, privatbank)

2. **Sync each bank:**
   For each connected bank:
   a. Call balance/accounts MCP tool → update balances
   b. Call transactions/statements MCP tool (since last sync) → get new transactions
   c. Auto-categorize new transactions using merchant patterns
   d. Emit `balance_updated` events for each account
   e. Emit `transaction_added` events for each new transaction

3. **Check budgets:**
   For each new transaction, run budget alert checks.
   Emit `budget_alert` events for any threshold crossings.

4. **Emit sync_complete:**
   After all banks processed.

5. **Cache results:**
   Write synced data to encrypted cache for offline access.

6. **Report results:**
   ```
   Sync complete!
   • 2 banks synced (Monobank, PrivatBank)
   • 4 accounts updated
   • 28 new transactions
   • 1 budget alert (Groceries at 85%)

   Events emitted:
     balance_updated    ×4
     transaction_added  ×28
     budget_alert       ×1
     sync_complete      ×1
   ```

## Example Usage
```
/sync-accounts
```

</details>

### `transactions`

List and search transaction history across all accounts

Arguments: `<days> [category] [bank]`

<details><summary>Prompt template</summary>

# Transactions

List recent transactions with optional filtering by time range, category, and bank.

## Arguments
Format: `<days> [category] [bank]`
- days: Number of days to look back (default: 90)
- category: Optional category filter (e.g., "Dining", "Groceries")
- bank: Optional bank filter — "monobank" or "privatbank"

Parse from "$ARGUMENTS".

## Process

1. **Fetch transactions via MCP:**
   - Call transaction/statement MCP tools for each bank
   - Apply date range filter (last N days)
   - If a bank filter is specified, query only that bank

2. **Auto-categorize:**
   - Use merchant name patterns from transaction-categorization skill
   - Apply category filter if specified

3. **Display results:**
   ```
   Transactions — Last 30 days
   ──────────────────────────────────────────────────────────────
   Date        Bank         Merchant          Category      Amount
   ──────────────────────────────────────────────────────────────
   2026-03-22  Monobank     Сільпо            Groceries    -₴345,67
   2026-03-22  Monobank     Bolt              Transport    -₴89,00
   2026-03-21  PrivatBank   Rozetka           Shopping     -₴1 299,00
   ──────────────────────────────────────────────────────────────
   Showing 156 transactions | Total: -₴12 523,45
   ```

4. **Offer follow-up actions:**
   - Export: `/export-report csv 30d`
   - Set budget: `/set-budget <category> <amount>`

## Example Usage
```
/transactions 30
/transactions 90 Groceries
/transactions 7 Dining monobank
```

</details>
