# sendpulse

> Sendpulse multi-channel marketing plugin. Manage chatbots (Telegram, WhatsApp, Instagram, Messenger, Viber), CRM (contacts, deals, pipelines, boards, tasks), email campaigns, templates, addressbooks, and SMTP transactional email via 133+ MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/sendpulse

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.sendpulse]
url = "https://mcp.sendpulse.com/mcp"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **chatbot-contacts-messaging** — Chatbot contact management, direct messaging across channels, contact variables, tags, and notes. Use when sending messages to contacts, managing contact data, or looking up subscribers.
- **chatbot-management** — Chatbot bots, statistics, tags, campaigns, flows, and dialogs. Use when managing bots, sending chatbot campaigns, running automation flows, or viewing bot statistics.
- **crm-boards-tasks** — CRM Kanban boards and task management — create boards, manage columns, create and track tasks. Use when organizing work, managing projects, or tracking task completion.
- **crm-contacts** — CRM contact management — create, update, search, list deals for contacts, and add comments. Use when working with CRM contacts, customer records, or contact-deal relationships.
- **crm-deals-pipelines** — CRM deals and sales pipelines — create and manage deals, configure pipeline stages, move deals between pipelines. Use when working with sales processes, deal tracking, or pipeline configuration.
- **crm-products** — CRM product catalog and deal-product associations. Use when managing product listings, adding products to deals, or viewing product-deal relationships.
- **email-addressbooks** — Email addressbook and subscriber management — create addressbooks, add/remove subscribers, manage variables, check sending costs. Use when managing mailing lists, subscriber data, or email list segmentation.
- **email-campaigns-templates** — Email campaign creation, management, statistics, and template management. Use when creating email campaigns, designing templates, or analyzing campaign performance.
- **email-senders-config** — Email sender management, email tags, blacklist management, and account balance. Use when configuring senders, managing email tags, handling blacklisted addresses, or checking account balance.
- **examples** — MCP tool call patterns, multi-channel marketing workflow examples, and scenario references. Use when you need reference implementations for Sendpulse operations.
- **smtp-transactional** — SMTP transactional email sending, bounce management, unsubscribe handling, IP and sender domain management. Use when sending transactional emails, managing bounces, or configuring SMTP infrastructure.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **email-marketing-assistant** — Specialized email marketing assistant. Expert in email campaigns, templates, addressbooks, subscriber management, SMTP transactional emails, and deliverability.
- **sendpulse-assistant** — Interactive Sendpulse assistant. Helps with chatbot management, CRM operations, email marketing, SMTP transactional emails, and multi-channel campaign orchestration.

## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `add-subscriber`

Add a subscriber to an email addressbook

Arguments: `<email> [--addressbook <id>] [--name <name>]`

<details><summary>Prompt template</summary>

# Add Subscriber

Add a subscriber to an email addressbook (mailing list).

## Arguments
Format: `<email> [--addressbook <id>] [--name <name>]`
- email: Subscriber email address
- --addressbook: Target addressbook ID (optional — lists if omitted)
- --name: Subscriber name (optional)

Parse from "$ARGUMENTS".

## Process

1. **List addressbooks if not specified:**
   ```
   email_addressbooks_list()
   ```
   Ask user to select target list.

2. **Add subscriber:**
   ```
   email_addressbooks_emails_create(
     id=<addressbook-id>,
     emails=[{
       "email": "<email>",
       "variables": {"name": "<name>"}
     }]
   )
   ```

3. **Report result:**
   - Confirmation of addition
   - Addressbook name
   - Subscriber email

## Example Usage
```
/add-subscriber user@example.com --addressbook list_001 --name "John Doe"
/add-subscriber lead@company.com
```

## Notes
- If addressbook not specified, lists available addressbooks
- Name is optional but recommended for personalization

</details>

### `check-balance`

Check Sendpulse account balance and email credits

Arguments: `--detailed`

<details><summary>Prompt template</summary>

# Check Balance

Check Sendpulse account balance and available email credits.

## Arguments
Format: `[--detailed]`
- --detailed: Show detailed balance breakdown (optional)

Parse from "$ARGUMENTS".

## Process

1. **Get balance:**
   ```
   email_balance_show()
   ```
   Show current credit balance.

2. **If --detailed:**
   ```
   email_balance_show_detail()
   ```
   Show breakdown by plan type, bonus credits, and expiration dates.

3. **Report result:**
   - Total available credits
   - Plan details (if detailed)

## Example Usage
```
/check-balance
/check-balance --detailed
```

## Notes
- Basic view shows just the current balance
- Detailed view includes plan breakdown and expiration

</details>

### `create-contact`

Create a new CRM contact

Arguments: `<email> [--name <name>] [--phone <phone>]`

<details><summary>Prompt template</summary>

# Create Contact

Create a new CRM contact with email, name, and phone.

## Arguments
Format: `<email> [--name <name>] [--phone <phone>]`
- email: Contact email address (required)
- --name: Contact full name (optional)
- --phone: Contact phone number (optional)

Parse from "$ARGUMENTS".

## Process

1. **Check for duplicates:**
   ```
   crm_contacts_list_by_email(email=<email>)
   ```
   If contact exists, show it and ask if user wants to update instead.

2. **Create contact:**
   ```
   crm_contacts_create(
     emails=[{"value": "<email>"}],
     name="<name>",
     phones=[{"value": "<phone>"}]
   )
   ```

3. **Report result:**
   - Contact ID
   - Name, email, phone

## Example Usage
```
/create-contact user@example.com --name "John Doe" --phone "+380501234567"
/create-contact lead@company.com --name "Jane Smith"
/create-contact prospect@corp.com
```

## Notes
- Checks for existing contact with same email before creating
- Phone should include country code
- Name and phone are optional

</details>

### `create-deal`

Create a new CRM deal in a pipeline

Arguments: `<deal-name> [--pipeline <id>] [--amount <amount>]`

<details><summary>Prompt template</summary>

# Create Deal

Create a new CRM deal in a sales pipeline.

## Arguments
Format: `<deal-name> [--pipeline <id>] [--amount <amount>]`
- deal-name: Name for the deal
- --pipeline: Pipeline ID (optional — lists pipelines if omitted)
- --amount: Deal amount (optional)

Parse from "$ARGUMENTS".

## Process

1. **List pipelines if not specified:**
   ```
   crm_pipelines_list()
   ```
   Show available pipelines. Ask user to select.

2. **Get pipeline steps:**
   ```
   crm_pipelines_steps_list(pipeline_id=<pipeline-id>)
   ```
   Use the first step as default.

3. **Create deal:**
   ```
   crm_deals_create(
     name=<deal-name>,
     pipeline_id=<pipeline-id>,
     step_id=<first-step-id>,
     amount=<amount>
   )
   ```

4. **Report result:**
   - Deal ID
   - Pipeline name and stage
   - Amount

## Example Usage
```
/create-deal "Enterprise License" --pipeline pipe_001 --amount 50000
/create-deal "Website Redesign"
/create-deal "Q2 Campaign" --amount 10000
```

## Notes
- Deal is created at the first pipeline step by default
- If no pipeline specified, shows all available pipelines
- Amount is optional and can be added later

</details>

### `create-email-campaign`

Create and send an email campaign

Arguments: `<subject> [--addressbook <id>] [--sender <email>] [--template <id>]`

<details><summary>Prompt template</summary>

# Create Email Campaign

Create and send an email campaign with pre-flight checks.

## Arguments
Format: `<subject> [--addressbook <id>] [--sender <email>] [--template <id>]`
- subject: Email subject line
- --addressbook: Target addressbook ID (optional — lists if omitted)
- --sender: Sender email address (optional — lists if omitted)
- --template: Template ID (optional — lists if omitted)

Parse from "$ARGUMENTS".

## Process

1. **Check balance:**
   ```
   email_balance_show()
   ```
   Verify sufficient credits.

2. **List addressbooks if not specified:**
   ```
   email_addressbooks_list()
   ```
   Ask user to select target list.

3. **Check sending cost:**
   ```
   email_addressbooks_cost(id=<addressbook-id>)
   ```
   Confirm cost fits within balance.

4. **List senders if not specified:**
   ```
   email_senders_list()
   ```
   Ask user to select verified sender.

5. **List templates if not specified:**
   ```
   email_templates_list()
   ```
   Ask user to select template.

6. **Create campaign:**
   ```
   email_campaigns_create(
     name=<subject>,
     subject=<subject>,
     sender_email=<sender>,
     addressbook_id=<addressbook-id>,
     template_id=<template-id>
   )
   ```

7. **Report result:**
   - Campaign ID
   - Target audience size
   - Estimated cost
   - Sender and template used

## Example Usage
```
/create-email-campaign "March Newsletter" --addressbook list_001 --sender news@acme.com --template tmpl_monthly
/create-email-campaign "Product Launch Announcement"
```

## Notes
- Always checks balance before creating
- Shows cost estimate before sending
- If sender is not verified, warns the user
- Campaign is sent immediately unless send_date is specified

</details>

### `find-contact`

Find a CRM contact by email address

Arguments: `<email>`

<details><summary>Prompt template</summary>

# Find Contact

Find a CRM contact by email and show their details and deals.

## Arguments
Format: `<email>`
- email: The email address to search for

Parse from "$ARGUMENTS".

## Process

1. **Search by email:**
   ```
   crm_contacts_list_by_email(email=<email>)
   ```

2. **If found, get full details:**
   ```
   crm_contacts_show(id=<contact-id>)
   ```

3. **Show associated deals:**
   ```
   crm_contacts_deals_list(id=<contact-id>)
   ```

4. **Report result:**
   - Contact name, email, phone
   - Custom fields
   - Associated deals with amounts and stages

## Example Usage
```
/find-contact user@example.com
/find-contact cto@bigcorp.com
```

## Notes
- Shows full contact details including custom fields
- Lists all associated deals
- Returns "not found" if no contact matches the email

</details>

### `list-addressbooks`

List email addressbooks with subscriber counts

Arguments: `--limit <number>`

<details><summary>Prompt template</summary>

# List Addressbooks

List email addressbooks (mailing lists) with subscriber counts.

## Arguments
Format: `[--limit <number>]`
- --limit: Number of addressbooks to return (default: 50)

Parse from "$ARGUMENTS".

## Process

1. **List addressbooks:**
   ```
   email_addressbooks_list(limit=<limit>, offset=0)
   ```

2. **Display as table:**
   - ID
   - Name
   - Subscriber count
   - Status

## Example Usage
```
/list-addressbooks
/list-addressbooks --limit 10
```

## Notes
- Shows all mailing lists with subscriber counts
- Default limit is 50

</details>

### `list-bots`

List all connected chatbots with their channels and statistics

Arguments: `--stats`

<details><summary>Prompt template</summary>

# List Bots

List all connected chatbots with their channels, and optionally show statistics.

## Arguments
Format: `[--stats]`
- --stats: Include subscriber count and engagement stats for each bot (optional)

Parse from "$ARGUMENTS".

## Process

1. **List all bots:**
   ```
   chatbots_bots_list()
   ```

2. **Display as table:**
   Show bot ID, name, channel type.

3. **If --stats flag provided, get statistics for each bot:**
   ```
   chatbots_bots_statistics_show(bot_id=<each-bot-id>)
   ```
   Add subscriber count and message stats to the table.

4. **Optionally show account info:**
   ```
   chatbots_account_show()
   ```
   Show plan limits and message usage.

## Example Usage
```
/list-bots
/list-bots --stats
```

## Notes
- Without --stats, only basic bot info is shown (faster)
- With --stats, makes one API call per bot — may be slower for many bots

</details>

### `list-contacts`

List CRM contacts

Arguments: `--limit <number>`

<details><summary>Prompt template</summary>

# List Contacts

List CRM contacts with optional limit.

## Arguments
Format: `[--limit <number>]`
- --limit: Number of contacts to return (default: 50)

Parse from "$ARGUMENTS".

## Process

1. **List contacts:**
   ```
   crm_contacts_list(limit=<limit>, offset=0)
   ```

2. **Display as table:**
   - Name
   - Email
   - Phone
   - ID

## Example Usage
```
/list-contacts
/list-contacts --limit 20
```

## Notes
- Default limit is 50
- Results are paginated — use offset for more

</details>

### `list-deals`

List CRM deals with optional pipeline filter

Arguments: `[--pipeline <id>] [--limit <number>]`

<details><summary>Prompt template</summary>

# List Deals

List CRM deals, optionally filtered by pipeline.

## Arguments
Format: `[--pipeline <id>] [--limit <number>]`
- --pipeline: Filter by pipeline ID (optional)
- --limit: Number of deals to return (default: 50)

Parse from "$ARGUMENTS".

## Process

1. **If pipeline filter requested but no ID, list pipelines:**
   ```
   crm_pipelines_list()
   ```
   Ask user to select.

2. **List deals:**
   ```
   crm_deals_list(limit=<limit>, offset=0)
   ```

3. **Display as table:**
   - Deal name
   - Amount
   - Pipeline stage
   - Contact
   - ID

## Example Usage
```
/list-deals
/list-deals --pipeline pipe_001
/list-deals --limit 10
```

## Notes
- Default limit is 50
- Shows deal name, amount, and current pipeline stage

</details>

### `list-tasks`

List CRM tasks

Arguments: `--limit <number>`

<details><summary>Prompt template</summary>

# List Tasks

List CRM tasks from all boards.

## Arguments
Format: `[--limit <number>]`
- --limit: Number of tasks to return (default: 50)

Parse from "$ARGUMENTS".

## Process

1. **List tasks:**
   ```
   crm_tasks_list(limit=<limit>, offset=0)
   ```

2. **Display as table:**
   - Task name
   - Status/step
   - Due date
   - ID

## Example Usage
```
/list-tasks
/list-tasks --limit 20
```

## Notes
- Shows tasks from all boards
- Default limit is 50

</details>

### `run-flow`

Run a chatbot automation flow for a contact

Arguments: `<flow-id> <contact-id>`

<details><summary>Prompt template</summary>

# Run Flow

Run a chatbot automation flow for a specific contact.

## Arguments
Format: `<flow-id> <contact-id>`
- flow-id: The automation flow ID (optional — lists flows if omitted)
- contact-id: The chatbot contact ID

Parse from "$ARGUMENTS".

## Process

1. **If no flow-id, list available bots and flows:**
   ```
   chatbots_bots_list()
   ```
   Ask user to select a bot.

   ```
   chatbots_flows_list(bot_id=<bot-id>)
   ```
   Show available flows. Ask user to select.

2. **Run the flow:**
   ```
   chatbots_flows_run(
     flow_id=<flow-id>,
     contact_id=<contact-id>
   )
   ```

3. **Report result:**
   - Flow name
   - Contact ID
   - Execution status

## Example Usage
```
/run-flow flow_welcome contact_abc123
/run-flow
```

## Notes
- If flow-id not provided, lists all bots and their flows
- The flow runs asynchronously — results depend on flow configuration

</details>

### `send-campaign`

Send a chatbot campaign on a specific channel

Arguments: `<channel> <bot-id> [message]`

<details><summary>Prompt template</summary>

# Send Campaign

Send a chatbot campaign to all subscribers on a specific channel.

## Arguments
Format: `<channel> <bot-id> [message]`
- channel: telegram, whatsapp, instagram, messenger, or viber
- bot-id: The bot ID (optional — will list bots if omitted)
- message: The message text to send (optional — will prompt if omitted)

Parse from "$ARGUMENTS".

## Process

1. **List bots if needed:**
   ```
   chatbots_bots_list()
   ```
   Show available bots with their channels. Ask user to select if bot-id not provided.

2. **Map channel to tool:**
   - telegram → `chatbots_bots_campaigns_t_send`
   - messenger → `chatbots_bots_campaigns_m_send`
   - whatsapp → `chatbots_bots_campaigns_wa_send`
   - instagram → `chatbots_bots_campaigns_i_send`
   - viber → `chatbots_bots_campaigns_v_send`

3. **Send campaign:**
   ```
   chatbots_bots_campaigns_{channel}_send(
     bot_id=<bot-id>,
     messages=[{"type": "text", "text": "<message>"}]
   )
   ```

4. **Report result:**
   - Campaign status
   - Bot name and channel
   - Estimated audience size

## Example Usage
```
/send-campaign telegram bot_abc "New arrivals are here! Check our latest collection."
/send-campaign whatsapp
/send-campaign instagram bot_ig_001 "Flash sale: 30% off everything today!"
```

## Notes
- If no bot-id provided, lists all bots and asks user to choose
- If no message provided, prompts user for message content
- Channel names are case-insensitive

</details>

### `send-message`

Send a direct message to a chatbot contact on any channel

Arguments: `<contact-id> <message>`

<details><summary>Prompt template</summary>

# Send Message

Send a direct message to a chatbot contact. Auto-detects the correct channel.

## Arguments
Format: `<contact-id> <message>`
- contact-id: The chatbot contact ID
- message: The text message to send

Parse from "$ARGUMENTS".

## Process

1. **Get contact details:**
   ```
   chatbots_contacts_show(contact_id=<contact-id>)
   ```
   Determine the contact's channel type.

2. **Send message on correct channel:**
   Route to the appropriate tool based on channel:
   - Telegram → `chatbots_contacts_messages_t_send`
   - Messenger → `chatbots_contacts_messages_m_send`
   - WhatsApp → `chatbots_contacts_messages_wa_send`
   - Instagram → `chatbots_contacts_messages_i_send`
   - Viber → `chatbots_contacts_messages_v_send`
   - Live Chat → `chatbots_contacts_messages_lc_send`

   ```
   chatbots_contacts_messages_{channel}_send(
     contact_id=<contact-id>,
     messages=[{"type": "text", "text": "<message>"}]
   )
   ```

3. **Report result:**
   - Contact name and channel
   - Delivery status

## Example Usage
```
/send-message contact_abc123 "Hi! Your order has been shipped."
/send-message contact_xyz "Reminder: your appointment is tomorrow at 10am."
```

## Notes
- Channel is auto-detected from contact data
- If contact not found, an error is returned
- Message is sent as plain text

</details>

### `send-smtp-email`

Send a transactional email via SMTP

Arguments: `<to-email> <subject> <body>`

<details><summary>Prompt template</summary>

# Send SMTP Email

Send a transactional email via SMTP with delivery checks.

## Arguments
Format: `<to-email> <subject> <body>`
- to-email: Recipient email address
- subject: Email subject line
- body: Email body (HTML or plain text)

Parse from "$ARGUMENTS".

## Process

1. **Check unsubscribe status:**
   ```
   smtp_unsubscribes_is_unsubscribed(email=<to-email>)
   ```
   If unsubscribed, warn user and stop.

2. **List senders:**
   ```
   smtp_senders_list()
   ```
   Use first available sender or ask user to choose.

3. **Send email:**
   ```
   smtp_emails_send(email={
     subject: <subject>,
     from: {name: <sender-name>, email: <sender-email>},
     to: [{email: <to-email>}],
     html: <body>
   })
   ```

4. **Report result:**
   - Message ID
   - Delivery status
   - Sender used

## Example Usage
```
/send-smtp-email customer@example.com "Order Confirmed" "<h1>Thank you!</h1><p>Your order has been confirmed.</p>"
/send-smtp-email user@example.com "Password Reset" "Click here to reset your password: ..."
```

## Notes
- Checks unsubscribe status before sending
- If recipient is unsubscribed, warns and does not send
- Body can be HTML or plain text

</details>
