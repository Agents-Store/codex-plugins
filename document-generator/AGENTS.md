# document-generator

> Professional document generator. Creates proposals, invoices, estimates/quotations, reports, presentations, contracts, NDAs, and certificates of completion in PDF, DOCX, and PPTX formats. Supports multi-language documents with embedded fonts (Cyrillic, Latin). First-use onboarding for style preferences, company profiles, and logo management. Converts between MD, DOCX, PDF, HTML, and PPTX.

Canonical source: https://github.com/agents-store/claude-public-plugins/tree/main/plugins/document-generator

## Skills

This plugin ships the following skills under `skills/`. Codex loads them contextually:

- **design-best-practices** — Comprehensive library of professional document design best practices sourced from top consulting firms (McKinsey, Deloitte, BCG), tech leaders (Stripe, Apple, Google), and typography/layout research. Use this skill when making design decisions, choosing fonts, colors, layouts, or when the user asks for a "professional" or "corporate" look.
- **document-generator** — Document generation process -- format selection, data collection, script invocation, and delivery. This skill should be used when generating any document (proposal, invoice, estimate, report, presentation, contract, NDA, certificate of completion), deciding which format or engine to use, or running generation scripts.
- **document-templates** — Document template structures and data collection checklists for each document type (proposal, invoice, estimate, report, presentation, contract, NDA, certificate of completion). This skill should be used when determining what data to collect from the user, what fields are required or optional, or how to structure a specific document type.
- **examples** — End-to-end document generation examples, workflow walkthroughs, and complete JSON input samples for all document types (proposal, invoice, estimate, report, presentation, contract, NDA, certificate of completion). This skill should be used when the user asks for a worked example, wants to see a sample document, needs a template pattern to follow, or asks how to create a specific document step by step.
- **formatting-standards** — Typography, font, margin, color, and layout standards for professional business documents. Use this skill when making any formatting decisions — choosing fonts, colors, margins, spacing, or layout patterns for any document type.
- **user-preferences** — First-use onboarding, user style preferences, company profiles, and logo management for document generation. This skill should be used on the first document generation request to collect user preferences, when the user wants to change their style, or when managing company logos and branding profiles.

## Subagents

Defined under `.codex/agents/` as TOML files:

- **document-specialist** — Professional document generator. Creates business proposals, invoices, estimates/quotations, reports, presentations, contracts, NDAs, and certificates of completion in PDF, DOCX, and PPTX formats. Supports multi-language documents. Also converts between document formats using pandoc.

<example>
user: "Create a business proposal for our consulting services"
</example>
<example>
user: "Generate an invoice for client XYZ"
</example>
<example>
user: "Make a presentation about Q1 results"
</example>
<example>
user: "Create a service agreement contract"
</example>
<example>
user: "Generate a certificate of completion"
</example>
<example>
user: "Create an NDA for our partnership"
</example>
<example>
user: "Create a cost estimate for the website project"
</example>
<example>
user: "Convert this markdown file to PDF"
</example>


## Workflows (canonical slash commands)

Codex CLI doesn't support custom slash commands — invoke these workflows via natural language. Each entry below is a prompt template you can adapt:

### `convert-document`

Convert between document formats (MD/DOCX/PDF/HTML/PPTX)

Arguments: `<input-file> --to <format>`

<details><summary>Prompt template</summary>

# Convert Document

Convert a document between formats using pandoc.

## Arguments

Format: `<input-file> --to <format>`
- input-file: Path to the source file (required)
- --to: Target format -- pdf, docx, html, md, pptx (required)

Parse from "$ARGUMENTS".

## Supported Conversions

| From | To |
|------|----|
| Markdown (.md) | PDF, DOCX, HTML, PPTX |
| DOCX (.docx) | PDF, Markdown, HTML |
| HTML (.html) | PDF, DOCX, Markdown |

## Process

1. **Validate input file exists:**
   Use Glob or Read to confirm the file is present.

2. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/convert.sh`.

3. **Check pandoc is installed:**
   ```bash
   which pandoc
   ```
   If missing, tell user: `brew install pandoc` (macOS) or `apt install pandoc` (Linux).

4. **Determine output filename:**
   Same name as input, with new extension. Example: `report.md` -> `report.pdf`.

5. **Run conversion:**
   ```bash
   <plugin_dir>/scripts/convert.sh <input-file> <output-file>
   ```

6. **Parse output:**
   Script returns JSON: `{ "success": true, "outputPath": "...", "size": N }`.
   Show the output file path and size.

## Example Usage
```
/convert-document report.md --to pdf
/convert-document proposal.docx --to pdf
/convert-document notes.md --to docx
```

</details>

### `generate-act`

Generate an Act of Completed Works (PDF)

Arguments: `<title> [--format <pdf|docx>]`

<details><summary>Prompt template</summary>

# Generate Act of Completed Works

Generate a professional Act of Completed Works with a services table, totals, and two-party signature blocks. Supports any language for all data values (company names, service descriptions, labels). The document language is determined by the `language` field in the input data.

## Arguments

Format: `<title> [--format <pdf|docx>]`
- title: Document title or act number (required, e.g., "ACT-001")
- --format: Output format — pdf (default) or docx

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_docx.js`.

2. **Check dependencies.**

3. **Gather required data from user:**
   - Act number (e.g., ACT-001/2026)
   - Date and city
   - Reference to contract (optional, e.g., "Contract No. 001 dated 01.01.2026")
   - Contractor (service provider): name, representative, title, registration number, address
   - Customer (client): name, representative, title, registration number, address
   - Services table: each row needs description, unit (hrs/pcs/service), quantity, unit price, total
   - VAT rate (0 if not VAT payer, e.g., 20 for standard VAT)
   - Currency symbol ($ default)
   - Notes (optional)

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/act_template.json
   ```

5. **Build JSON input:**
   Use type "act" with playwright engine. Services is an array of rows. Write to `.doc_input.json`.

6. **Generate document:**
   For PDF (default):
   ```bash
   cd <plugin_dir> && node scripts/generate_pdf.js /absolute/path/.doc_input.json
   ```
   For DOCX:
   ```bash
   cd <plugin_dir> && node scripts/generate_docx.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Show file path and size. Remind user the act must be signed by both parties.

## Example Usage
```
/generate-act "ACT-001"
/generate-act "ACT-002/2026" --format docx
```

## Output filename
Pattern: `act_{number}_{date}.pdf` (e.g., `act_akt-001_2026-03-19.pdf`)

</details>

### `generate-contract`

Generate a contract or agreement (DOCX/PDF)

Arguments: `<title> [--format <docx|pdf>] [--type <service|nda|employment>]`

<details><summary>Prompt template</summary>

# Generate Contract

Generate a professional contract with numbered clauses, party details, and signature blocks.

## Arguments

Format: `<title> [--format <docx|pdf>] [--type <service|nda|employment>]`
- title: Contract title (required)
- --format: Output format -- docx (default, editable) or pdf (final version)
- --type: Contract type for suggested clause structure (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_docx.js`.

2. **Check dependencies.**

3. **Gather required data from user:**
   - Contract number (optional)
   - Date
   - Party 1: name, address, registration number
   - Party 2: name, address, registration number
   - Clause content:
     - Definitions (key terms)
     - Scope of services
     - Payment terms (amount, schedule, method)
     - Duration and termination conditions
     - Confidentiality terms
     - Governing law (jurisdiction)
   - Optional: IP rights, liability limits, force majeure, dispute resolution

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/contract_template.json
   ```

5. **Build JSON input:**
   Structure clauses with numbered format. Each clause can have multiple paragraphs (sub-clauses). Write to `.doc_input.json`.

6. **Generate document:**
   For DOCX (default):
   ```bash
   cd <plugin_dir> && node scripts/generate_docx.js /absolute/path/.doc_input.json
   ```
   For PDF (final version):
   ```bash
   cd <plugin_dir> && node scripts/generate_pdf.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Show file path and size. Remind user this is a template -- recommend legal review before signing.

## Example Usage
```
/generate-contract "Service Agreement" --type service
/generate-contract "Non-Disclosure Agreement" --type nda
/generate-contract "Employment Contract"
```

</details>

### `generate-estimate`

Generate a professional cost estimate / quotation (PDF)

Arguments: `<estimate-number> [--company <name>] [--client <name>]`

<details><summary>Prompt template</summary>

# Generate Estimate

Generate a detailed, professional cost estimate with phase-based pricing, team composition, timeline, scope, and payment schedule.

## Arguments

Format: `<estimate-number> [--company <name>] [--client <name>]`
- estimate-number: Estimate reference number (required)
- --company: Your company name (optional, loaded from preferences)
- --client: Client name (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_pdf.js`.

2. **Check dependencies.**

3. **Gather data from user (structured for detailed estimate):**

   **Required:**
   - Company info (from preferences or ask): name, address, email, phone
   - Client info: company name, contact person, address, email
   - Estimate number and date
   - Project executive summary (2-4 sentences)
   - Scope of work:
     - Description (what the project is)
     - Inclusions list (what IS included)
     - Exclusions list (what is NOT included)
     - Assumptions list (conditions/prerequisites)
   - Team composition: role, hourly rate, allocation % for each member
   - Phases with tasks: for each phase:
     - Phase name (e.g., "Phase 1: Discovery & Planning")
     - Phase color (optional, defaults provided)
     - Tasks: description, role, hours, rate for each task
   - Timeline: total duration + phases with start/end weeks and milestones
   - Payment schedule: milestones with percentage splits

   **Optional:**
   - Optional items / add-ons (shown separately from total)
   - Contingency percentage and label (default: 10%)
   - Tax amount
   - Discount amount
   - Validity period (default: 30 days)
   - Terms and conditions (array of {title, text})
   - Acceptance/signature block (default: enabled)
   - Notes

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/estimate_template.json
   ```

5. **Build JSON input:**
   ALWAYS use `"phases"` format (not flat `"items"`). Structure:
   ```json
   {
     "type": "estimate",
     "outputPath": "...",
     "data": {
       "estimateNumber": "EST-2026-001",
       "date": "...",
       "validDays": 30,
       "companyInfo": { "name": "", "address": "", "email": "", "phone": "" },
       "recipient": { "name": "", "contactPerson": "", "address": "", "email": "" },
       "executiveSummary": "...",
       "scope": {
         "description": "...",
         "inclusions": ["..."],
         "exclusions": ["..."],
         "assumptions": ["..."]
       },
       "team": [
         { "role": "Project Manager", "rate": 120, "rateUnit": "hour", "allocation": "25%" }
       ],
       "phases": [
         {
           "name": "Phase 1: Discovery",
           "color": "#6366F1",
           "tasks": [
             { "description": "Requirements gathering", "role": "Project Manager", "hours": 16, "rate": 120 }
           ]
         }
       ],
       "optionalItems": [
         { "description": "...", "hours": 40, "rate": 175, "total": 7000 }
       ],
       "contingency": { "percent": 10, "label": "Risk contingency" },
       "timeline": {
         "totalDuration": "12 weeks",
         "phases": [
           { "name": "Discovery", "startWeek": 1, "endWeek": 3, "milestones": ["..."], "color": "#6366F1" }
         ]
       },
       "paymentSchedule": [
         { "milestone": "Project kickoff", "percent": 30, "dueDate": "Upon signing" }
       ],
       "terms": [
         { "title": "Payment Terms", "text": "..." }
       ],
       "acceptance": { "enabled": true }
     }
   }
   ```
   Write to `.doc_input.json`.

6. **Generate PDF:**
   ```bash
   cd <plugin_dir> && node scripts/generate_pdf.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Show file path, size, and summary (total, phases count, timeline, team size).

## Phase Color Palette (suggested defaults)
- Phase 1: `#6366F1` (indigo)
- Phase 2: `#8B5CF6` (violet)
- Phase 3: `#A78BFA` (purple)
- Phase 4: `#C4B5FD` (lavender)
- Phase 5: `#818CF8` (blue-indigo)

## Example Usage
```
/generate-estimate "EST-2026-001" --company "TechCo" --client "Acme Corp"
/generate-estimate "Q-042"
```

</details>

### `generate-invoice`

Generate a professional invoice (PDF)

Arguments: `<invoice-number> [--company <name>] [--client <name>]`

<details><summary>Prompt template</summary>

# Generate Invoice

Generate a professional PDF invoice with company header, itemized table, tax calculation, and payment details.

## Arguments

Format: `<invoice-number> [--company <name>] [--client <name>]`
- invoice-number: Invoice reference number (required)
- --company: Your company name (optional)
- --client: Client name (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_pdf.js`.

2. **Check dependencies:**
   ```bash
   cd <plugin_dir> && node -e "require('playwright')" 2>&1
   ```

3. **Gather required data from user:**
   - Company info: name, address, email, phone
   - Client info: name, address, email
   - Date and due date
   - Line items: description, quantity, unit price for each
   - Tax rate or amount (if applicable)
   - Discount (if applicable)
   - Payment details: bank, IBAN, SWIFT (optional)
   - Notes (optional)

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/invoice_template.json
   ```

5. **Build JSON input:**
   Calculate totals: `item.total = qty * unitPrice`, `subtotal = sum(totals)`, `total = subtotal + tax - discount`.
   Write to `.doc_input.json`.

6. **Generate PDF:**
   ```bash
   cd <plugin_dir> && node scripts/generate_pdf.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Parse output, show file path, size, and invoice summary table.

## Example Usage
```
/generate-invoice "INV-2026-001" --company "TechCo" --client "Acme Corp"
/generate-invoice "INV-042"
```

</details>

### `generate-nda`

Generate a Non-Disclosure Agreement (NDA) — PDF

Arguments: `[--type <mutual|unilateral>] [--party1 <name>] [--party2 <name>]`

<details><summary>Prompt template</summary>

# Generate NDA

Generate a professional Non-Disclosure Agreement with standard legal clauses and signature blocks.

## Arguments

Format: `[--type <mutual|unilateral>] [--party1 <name>] [--party2 <name>]`
- --type: NDA type -- mutual (default, both parties bound) or unilateral (one-way)
- --party1: Disclosing party name (optional)
- --party2: Receiving party name (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_pdf.js`.

2. **Check dependencies.**

3. **Gather required data from user:**
   - NDA type: mutual (default) or unilateral
   - Date and jurisdiction (governing law)
   - Party 1 (Disclosing Party): name, address, representative, title
   - Party 2 (Receiving Party): name, address, representative, title
   - Optional customization:
     - Definition of confidential information (default provided)
     - Term duration (default: 2 years)
     - Survival period (default: 3 years)
     - Additional clauses
   - Most clauses have professional defaults -- only ask for party info and customizations

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/nda_template.json
   ```

5. **Build JSON input:**
   Set `"type": "nda"` and `"ndaType": "mutual"` or `"unilateral"`.
   Default clauses are built-in -- only override if user requests specific wording.
   Write to `.doc_input.json`.

6. **Generate PDF:**
   ```bash
   cd <plugin_dir> && node scripts/generate_pdf.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Show file path and size. Remind user this is a template -- recommend legal review before signing.

## Example Usage
```
/generate-nda --type mutual --party1 "Acme Corp" --party2 "TechStart Inc"
/generate-nda --type unilateral
/generate-nda
```

</details>

### `generate-presentation`

Generate a professional presentation (PPTX)

Arguments: `<title> [--slides <number>] [--theme <corporate|minimal|bold>]`

<details><summary>Prompt template</summary>

# Generate Presentation

Generate a professional PowerPoint presentation with title slide, agenda, content slides, and summary.

## Arguments

Format: `<title> [--slides <number>] [--theme <corporate|minimal|bold>]`
- title: Presentation title (required)
- --slides: Approximate number of content slides (optional, default: auto)
- --theme: Color theme (optional, default: corporate)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_pptx.js`.

2. **Check dependencies:**
   ```bash
   cd <plugin_dir> && node -e "require('pptxgenjs')" 2>&1
   ```

3. **Gather required data from user:**
   - Subtitle (optional)
   - Author/presenter name
   - Audience context (who is this for?)
   - Key topics/agenda items
   - Content for each slide (bullets, key points)
   - Optional: images, charts, branding colors

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/presentation_template.json
   ```

5. **Build slide array:**
   Construct slides based on user content:
   - First slide: type "title"
   - Second slide: type "agenda" (from topics list)
   - Content slides: type "content" or "two-column" per topic
   - Optional: type "chart" for data visualization
   - Second-to-last: type "summary" with key takeaways
   - Last: type "contact" (optional)

6. **Generate PPTX:**
   ```bash
   cd <plugin_dir> && node scripts/generate_pptx.js /absolute/path/.doc_input.json
   ```

7. **Deliver result:**
   Show file path, size, and number of slides generated.

## Example Usage
```
/generate-presentation "Product Launch Strategy" --slides 10 --theme corporate
/generate-presentation "Q1 Review" --theme minimal
```

</details>

### `generate-proposal`

Generate a professional business proposal (DOCX/PDF)

Arguments: `<title> [--format <docx|pdf>] [--company <name>]`

<details><summary>Prompt template</summary>

# Generate Proposal

Generate a professional business proposal with cover page, table of contents, structured sections, and branded formatting.

## Arguments

Format: `<title> [--format <docx|pdf>] [--company <name>]`
- title: Proposal title (required)
- --format: Output format -- docx (default, editable) or pdf (final version)
- --company: Your company name for branding (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_docx.js`.

2. **Check dependencies:**
   ```bash
   cd <plugin_dir> && node -e "require('docx')" 2>&1
   ```
   If missing, tell user to run `npm install` in plugin dir.

3. **Gather required data from user:**
   - Recipient (client name/company)
   - Executive summary (what is this proposal about?)
   - Problem statement (what problem does it solve?)
   - Proposed solution (what do you propose?)
   - Timeline (phases, milestones, duration)
   - Pricing (breakdown of costs)
   - Optional: about us, terms, branding colors

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/proposal_template.json
   ```

5. **Build JSON input:**
   Merge user data into the template structure. Write to `.doc_input.json` in current directory.

6. **Generate document:**
   ```bash
   cd <plugin_dir> && node scripts/generate_docx.js /absolute/path/.doc_input.json
   ```
   For PDF format, use `scripts/generate_pdf.js` instead.

7. **Deliver result:**
   Parse JSON output, show file path and size. Clean up temp input file. Offer to convert to another format.

## Example Usage
```
/generate-proposal "Cloud Migration Strategy" --company "TechCo Solutions"
/generate-proposal "Q2 Marketing Campaign" --format pdf
```

</details>

### `generate-report`

Generate a professional report (DOCX/PDF)

Arguments: `<title> [--format <docx|pdf>] [--type <annual|quarterly|project>]`

<details><summary>Prompt template</summary>

# Generate Report

Generate a professional report with cover page, table of contents, structured sections, page numbers, and headers/footers.

## Arguments

Format: `<title> [--format <docx|pdf>] [--type <annual|quarterly|project>]`
- title: Report title (required)
- --format: Output format -- docx (default, editable) or pdf (final)
- --type: Report type hint for structure suggestions (optional)

Parse from "$ARGUMENTS".

## Process

1. **Resolve plugin directory:**
   Find the plugin dir by globbing for `**/document-generator/scripts/generate_docx.js`.

2. **Check dependencies.**

3. **Gather required data from user:**
   - Author name
   - Date
   - Introduction (context and purpose)
   - Findings (key data and observations)
   - Conclusions (what the data tells us)
   - Optional: methodology, analysis, recommendations, appendices

4. **Read template:**
   ```bash
   cat <plugin_dir>/templates/report_template.json
   ```

5. **Build JSON input:**
   Merge user data into template. Write to `.doc_input.json`.

6. **Generate document:**
   ```bash
   cd <plugin_dir> && node scripts/generate_docx.js /absolute/path/.doc_input.json
   ```

7. **Deliver result.**

## Example Usage
```
/generate-report "Q1 2026 Performance Analysis" --format pdf
/generate-report "Security Audit Findings" --type project
```

</details>

### `setup`

Set up document generator preferences — style, language, company profile, and logo.

<details><summary>Prompt template</summary>

# Document Generator Setup

Run the onboarding interview to configure document generation preferences.

## Steps

1. **Check existing preferences:**
   ```bash
   cat ~/.document-generator/preferences.json 2>/dev/null
   ```
   If preferences already exist, ask the user if they want to update them or start fresh.

2. **Check dependencies:**
   ```bash
   cd <plugin_dir> && node scripts/check_deps.js
   ```
   If any dependencies are missing, show the user what needs to be installed and ask for permission.

3. **Run the onboarding interview** from the **user-preferences** skill:
   - Ask about preferred document style (show 5+ presets with descriptions)
   - Ask about default language (en, uk, de, fr, es)
   - Optionally collect company information (name, address, phone, email, website, registration/tax ID)
   - Optionally collect company logo (file path → validate → convert to base64 → store)
   - Optionally set formatting preferences (paper size A4/Letter, currency symbol, date format)

4. **Save preferences** to `~/.document-generator/preferences.json`

5. **Confirm setup is complete** and show a summary of what was configured.

## Notes

- This command can be run at any time to update preferences
- Use the **user-preferences** skill for the full onboarding flow details
- Preferences are stored at `~/.document-generator/preferences.json`
- Logos are stored at `~/.document-generator/logos/`

</details>
