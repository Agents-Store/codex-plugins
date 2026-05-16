# teleshop

> Teleshop store management plugin. Manage products, orders, categories, attributes, customers, webhooks, and addons for your Telegram store via 50 MCP tools.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/teleshop

## MCP servers

Add to `~/.codex/config.toml`:

```toml
[mcp_servers.teleshop]
url = "https://mcp.teleshop.pro/"
type = "http"

```

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **addon-management** — Addon and workflow management — listing, toggling, executing, scheduling, and configuring variables. Use when managing store addons, running automation workflows, or configuring addon schedules.
- **attribute-management** — Attribute CRUD, adding attribute values, and variant configuration. Use when creating product attributes like color, size, or material, or managing attribute values.
- **catalog-import** — Full catalog import with categories and products from JSON. Use when importing a complete catalog, migrating from another platform, or bulk-loading products.
- **category-management** — Category CRUD, batch operations, and hierarchy management. Use when creating, updating, deleting, or listing categories in a Teleshop store.
- **customer-management** — Customer listing and details with order history. Use when viewing customer information, searching customers, or checking a customer's order history.
- **examples** — MCP tool call patterns, end-to-end workflow examples, code templates, and scenario references. Use when you need reference implementations for Teleshop operations.
- **order-management** — Order listing, filtering, status updates, payment management, and tracking. Use when viewing orders, changing order status, updating payment, or adding tracking numbers.
- **product-management** — Product CRUD, batch operations, image and attribute management, variants, filtering and sorting. Use when creating, updating, deleting, or listing products in a Teleshop store.
- **webhook-management** — Webhook CRUD, event types, testing, delivery logs, statistics, and toggle. Use when setting up webhooks for order/payment notifications or debugging webhook delivery.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **teleshop-assistant** — Interactive Teleshop store management assistant. Helps merchants manage products, orders, categories, attributes, customers, webhooks, and addons for their Telegram store.
- **teleshop-catalog-manager** — Specialized catalog management agent for Teleshop. Focused on products, categories, attributes, catalog import, and customer data.

## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `create-category`

Create a new product category

Arguments: `<title> [--parent <id>] [--orderBy cheap|expensive|novelty|popular]`

<details><summary>Prompt template</summary>

# Create Category

Create a new product category in the Teleshop store.

## Arguments
Format: `<title> [--parent <id>] [--orderBy cheap|expensive|novelty|popular]`
- title: Category name (required)
- --parent: Parent category ID for subcategory
- --orderBy: Default product sorting (cheap, expensive, novelty, popular)

Parse from "$ARGUMENTS".

## Process

1. **If --parent not provided, list existing categories for context:**
   ```
   list_categories()
   ```

2. **Create the category:**
   ```
   create_category({
     title: <title>,
     parentId: <parent if provided>,
     orderBy: <orderBy if provided>
   })
   ```

3. **Display created category:**
   Show ID, title, parent, ordering.

## Example Usage
```
/create-category "Electronics"
/create-category "Smartphones" --parent 5
/create-category "Sale" --orderBy cheap
```

</details>

### `create-product`

Create a new product in the store

Arguments: `<title> <sku> <price> [--quantity <n>] [--category <id>] [--description <text>]`

<details><summary>Prompt template</summary>

# Create Product

Create a new product in the Teleshop store.

## Arguments
Format: `<title> <sku> <price> [--quantity <n>] [--category <id>] [--description <text>]`
- title: Product title (required)
- sku: Unique product SKU (required)
- price: Product price (required)
- --quantity: Available quantity (default: 0)
- --category: Category ID to assign
- --description: Product description

Parse from "$ARGUMENTS".

## Process

1. **If no category provided, list available categories:**
   ```
   list_categories()
   ```
   Show categories so user can choose.

2. **Create the product:**
   ```
   create_product({
     title: <title>,
     sku: <sku>,
     price: <price>,
     quantity: <quantity or 0>,
     categoryId: <category if provided>,
     description: <description or title>,
     sellStatus: "available",
     isActive: true,
     stockControl: true
   })
   ```

3. **Display created product details:**
   Show ID, SKU, title, price, quantity, category.

## Example Usage
```
/create-product "iPhone 15 Pro" "IPHONE-15-PRO" 999.99
/create-product "Red T-Shirt" "TSHIRT-RED" 29.99 --quantity 100 --category 5
/create-product "Wireless Mouse" "MOUSE-001" 49.99 --quantity 50 --description "Ergonomic wireless mouse"
```

</details>

### `create-webhook`

Create a new webhook for event notifications

Arguments: `<url> [<event>]`

<details><summary>Prompt template</summary>

# Create Webhook

Create a new webhook endpoint for receiving store event notifications.

## Arguments
Format: `<url> [<event>]`
- url: Webhook endpoint URL (required)
- event: Event type to subscribe to (optional — if not provided, shows available events)

Parse from "$ARGUMENTS".

## Process

1. **If no event specified, list available events:**
   ```
   get_webhook_events()
   ```
   Show all available event types for user to choose.

2. **Create the webhook:**
   ```
   create_webhook({
     url: <url>,
     events: [<event>]
   })
   ```

3. **Get signing secret:**
   ```
   get_webhook_secret(id=<webhook-id>)
   ```
   Show secret for payload verification.

4. **Test the webhook:**
   ```
   test_webhook(id=<webhook-id>)
   ```
   Verify delivery is working.

5. **Display results:**
   Show webhook ID, URL, events, secret, and test result.

## Example Usage
```
/create-webhook https://my-server.com/webhooks
/create-webhook https://my-server.com/webhooks order.created
```

## Notes
- Always save the signing secret for payload verification
- Test webhook immediately after creation to verify your endpoint is reachable

</details>

### `import-catalog`

Import a full catalog of categories and products from JSON data

Arguments: `<json-data-or-description> [--mode merge|replace]`

<details><summary>Prompt template</summary>

# Import Catalog

Import categories and products into the store from structured JSON data.

## Arguments
Format: `<json-data-or-description> [--mode merge|replace]`
- json-data: JSON catalog data or a description of what to import
- --mode: Import mode — "merge" (default, safe) or "replace" (full reset)

Parse from "$ARGUMENTS".

## Process

1. **Parse the input data:**
   If user provides raw JSON, use it directly.
   If user describes what to import, construct the JSON.

2. **Import the catalog:**
   ```
   import_catalog({
     options: { mode: <mode or "merge">, stockControl: true },
     categories: [...],
     products: [...]
   })
   ```

3. **Verify the import:**
   ```
   list_categories() -> Show imported categories
   list_products(limit=10) -> Show sample of imported products
   ```

4. **Report results:**
   Show count of categories and products imported.

## Example Usage
```
/import-catalog {"categories": [{"externalId": "cat1", "title": "Electronics"}], "products": [{"sku": "P1", "title": "Phone", "price": 999}]}
/import-catalog --mode replace (with JSON data)
```

## Notes
- Default mode is "merge" (adds new items, updates existing by SKU)
- "replace" mode deletes ALL existing data first — use with caution
- Max request size: 10 MB
- Image URLs are downloaded automatically by the platform

</details>

### `list-addons`

List all available store addons and their status

<details><summary>Prompt template</summary>

# List Addons

List all available addons for the Teleshop store with their current status.

## Process

1. **List all addons:**
   ```
   list_workflows()
   ```

2. **Display as table:**
   Show ID, name, description, enabled/disabled status, last execution time.

## Example Usage
```
/list-addons
```

</details>

### `list-attributes`

List all product attributes with their values

Arguments: `--search <query>`

<details><summary>Prompt template</summary>

# List Attributes

List all product attributes in the Teleshop store.

## Arguments
Format: `[--search <query>]`
- --search: Search attributes by name (optional)

Parse from "$ARGUMENTS".

## Process

1. **List all attributes:**
   ```
   list_attributes({
     search: <search if provided>,
     sortBy: "name",
     sortOrder: "ASC"
   })
   ```

2. **For each attribute, get details with values:**
   ```
   get_attribute(id=<each-attribute-id>)
   ```

3. **Display as table:**
   Show ID, name, type, isVariant flag, and values list.

## Example Usage
```
/list-attributes
/list-attributes --search Color
```

## Notes
- Types: select (dropdown), color (color picker), text (free text)
- Attributes with isVariant=true can create product variants

</details>

### `list-categories`

List all product categories with optional search

Arguments: `--search <query>`

<details><summary>Prompt template</summary>

# List Categories

List all product categories in the Teleshop store.

## Arguments
Format: `[--search <query>]`
- --search: Search categories by title (optional)

Parse from "$ARGUMENTS".

## Process

1. **List categories:**
   ```
   list_categories({
     search: <search if provided>,
     sortBy: "position",
     sortOrder: "ASC"
   })
   ```

2. **Display as hierarchy:**
   Show categories with indentation for subcategories.
   Include ID, title, product ordering, parent.

## Example Usage
```
/list-categories
/list-categories --search Electronics
```

</details>

### `list-customers`

List store customers with optional search

Arguments: `--search <query>`

<details><summary>Prompt template</summary>

# List Customers

List customers of the Teleshop store.

## Arguments
Format: `[--search <query>]`
- --search: Search by name, phone, email, or Telegram username (optional)

Parse from "$ARGUMENTS".

## Process

1. **List customers:**
   ```
   list_customers({
     search: <search if provided>,
     sortBy: "createdAt",
     sortOrder: "DESC"
   })
   ```

2. **Display as table:**
   Show ID, name, phone, email, Telegram username, registration date.

## Example Usage
```
/list-customers
/list-customers --search John
/list-customers --search "+380991234567"
```

</details>

### `list-orders`

List orders with optional status, payment type, and search filters

Arguments: `[--status <status>] [--payment <type>] [--search <query>]`

<details><summary>Prompt template</summary>

# List Orders

List orders from the Teleshop store with optional filters.

## Arguments
Format: `[--status <status>] [--payment <type>] [--search <query>]`
- --status: Filter by status (created, processing, awaiting, shipped, ready, completed, canceled)
- --payment: Filter by payment type (offlineCard, offlineCash, onlinePayment, cardPrepay, cardPartialPrepay, telegramStars)
- --search: Search by customer name, phone, or email

Parse from "$ARGUMENTS".

## Process

1. **List orders with filters:**
   ```
   list_orders({
     status: <status if provided>,
     paymentType: <payment if provided>,
     search: <search if provided>,
     sortBy: "createdAt",
     sortOrder: "DESC"
   })
   ```

2. **Display as table:**
   Show order number, status, customer name, total amount, payment status (paid/unpaid), payment type, date.

3. **Show summary:**
   Total orders found, orders by status breakdown.

## Example Usage
```
/list-orders
/list-orders --status created
/list-orders --status processing --payment onlinePayment
/list-orders --search "John"
```

</details>

### `list-products`

List products with optional search, category, status, and limit filters

Arguments: `[--search <query>] [--category <name>] [--status available|unavailable|preorder] [--limit <n>]`

<details><summary>Prompt template</summary>

# List Products

List products from the Teleshop store with optional filters.

## Arguments
Format: `[--search <query>] [--category <name>] [--status available|unavailable|preorder] [--limit <n>]`
- --search: Search products by title
- --category: Filter by category name
- --status: Filter by sell status (available, unavailable, preorder)
- --limit: Number of products to show (default: 20)

Parse from "$ARGUMENTS".

## Process

1. **List products with provided filters:**
   ```
   list_products({
     search: <search if provided>,
     category: <category if provided>,
     sellStatus: <status if provided>,
     limit: <limit if provided, default 20>,
     sortBy: "createdAt",
     sortOrder: "DESC"
   })
   ```

2. **Display as table:**
   Show ID, SKU, title, price (with discount if any), quantity, status, category.

3. **Show pagination info:**
   Display current page, total items, total pages.

## Example Usage
```
/list-products
/list-products --search iPhone
/list-products --category Electronics --status available
/list-products --limit 50
```

</details>

### `list-webhooks`

List all configured webhooks with their status

<details><summary>Prompt template</summary>

# List Webhooks

List all configured webhooks in the Teleshop store.

## Process

1. **List all webhooks:**
   ```
   list_webhooks()
   ```

2. **Optionally get stats for each:**
   ```
   get_webhook_stats(id=<each-webhook-id>)
   ```

3. **Display as table:**
   Show ID, URL, events, enabled/disabled, delivery success rate.

## Example Usage
```
/list-webhooks
```

</details>

### `run-addon`

Manually execute a store addon workflow

Arguments: `<addon-id> [--variables <json>]`

<details><summary>Prompt template</summary>

# Run Addon

Manually trigger execution of a store addon.

## Arguments
Format: `<addon-id> [--variables <json>]`
- addon-id: The addon/workflow ID (required)
- --variables: JSON object with variables to set before execution (optional)

Parse from "$ARGUMENTS".

## Process

1. **If no addon-id provided, list available addons:**
   ```
   list_workflows()
   ```
   Show addons for user to choose.

2. **If --variables provided, check current variables:**
   ```
   get_workflow_variables(id=<addon-id>)
   ```
   Show current configuration.

3. **Execute the addon:**
   ```
   execute_workflow(id=<addon-id>)
   ```

4. **Report execution:**
   Confirm the addon was triggered and show result.

## Example Usage
```
/run-addon addon-123
/run-addon addon-456 --variables {"sync_url": "https://example.com/feed.xml"}
```

## Notes
- Addon must be enabled (toggled on) to execute
- Use /list-addons to see available addon IDs

</details>

### `update-order-status`

Update the status of an order (created, processing, shipped, completed, etc.)

Arguments: `<order-id> <status>`

<details><summary>Prompt template</summary>

# Update Order Status

Change the status of an order in the Teleshop store.

## Arguments
Format: `<order-id> <status>`
- order-id: The order ID (required)
- status: New status — created, processing, awaiting, shipped, ready, completed, canceled (required)

Parse from "$ARGUMENTS".

## Process

1. **Get current order details:**
   ```
   get_order(id=<order-id>)
   ```
   Show current status, customer name, total amount.

2. **Update the status:**
   ```
   update_order_status(id=<order-id>, status=<status>)
   ```

3. **Confirm the change:**
   Show order number with old status → new status.

## Example Usage
```
/update-order-status 456 processing
/update-order-status 789 shipped
/update-order-status 123 completed
```

## Notes
- Valid statuses: created, processing, awaiting, shipped, ready, completed, canceled
- Status flow: created → processing → awaiting → shipped → ready → completed
- Orders can be canceled from most statuses

</details>
