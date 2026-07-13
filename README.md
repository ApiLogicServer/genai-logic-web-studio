<!--
title: Welcome - see end for instructions to hide this
Description: Instant mcp-enabled microservices, standard projects, declarative business logic
Source: docs/Manager-readme
version info: 17.02.06 (07/09/2026)
do_process_code_block_titles: True
Used: Manager Readme (via copy_md())
demo_customs: Customs-readme
demo_customs_surtax: Customs-readme-surtax
demo_kafka: Sample-Integration
demo_allo: Sample_Allo_Dept_GL_readme
demo_ai_rules: Sample-ai-rules
demo_mcp_send: Sample-Basic-Demo-MCP-Send-Email
demo_emp_types: Sample-Types
demo_eai: Sample-Basic-EAI
demo_vibe: Sample-Basic-Demo-Vibe
demo_copilot_mcp_discovery: Sample-ai-mcp
basic_demo: Sample-Basic-Demo
codespaces_patch: |
  create_codespaces_mgr.py injects a Codespaces-only browser note immediately after
  the "## 🚀 First Time Here?" heading (sentinel: do not rename that heading without
  updating the matching logic in create_codespaces_mgr.py). The note warns Safari users to
  switch to Chrome/Edge. This avoids forking the README for Codespaces.
-->

# Welcome to GenAI-Logic

### Governed Executable Requirements

AI builds **enterprise-class systems** — from one prompt or your existing database, *5 rules instead of ~200 lines of code* — then hands off cleanly to **Dev/DevOps for extension and deployment**.

&nbsp;

## 🤖 AI Assistance

<details markdown>
<summary>First select your AI assistant, then paste the prompt below</summary>

&nbsp;

We get consistently good results with **Claude Sonnet 5.0/4.6** (GitHub Copilot or Claude Code extension). "Ask" mode will not work — use **Agent mode**.

To select Sonnet 5.0 in the Copilot chat panel: click **Agent** → the **gear icon** → choose **Claude Sonnet 5.0**.

For more information, see [AI-Enabled Projects](https://apilogicserver.github.io/Docs/Project-AI-Enabled/) or [click here](https://apilogicserver.github.io/Docs/Manager-readme/).

</details>

&nbsp;

```
Please load `.github/.copilot-instructions.md`.
```

&nbsp;

## 🚀 First Time Here?
<!-- CODESPACES-INSERT-POINT: create_codespaces_mgr.py injects browser note here — do not rename this heading -->
&nbsp;

You're already running in GitHub Codespaces — a cloud VS Code environment in your browser. Nothing to install. (Use Chrome or Edge — Safari has known compatibility issues with VS Code in the browser.)


<details markdown>
<summary>The Ideal — executable business prompts, held to an enterprise standard</summary>

<br>The goal here isn't a demo — it's an **enterprise-class** system you can trust and maintain:

1. **Dev Friendly** — standard language, standard tooling; devs can extend this project in the IDE they already use.
2. **DevOps Friendly** — containers; deploy to cloud or on-prem.
3. **Enterprise Friendly** — pluggable security (SQL or Keycloak), full REST API, event/messaging integration (Kafka, webhooks) — built in, not bolted on.

See it for yourself in the two examples below.

</details>

&nbsp;

<details markdown>
<summary>Example — Existing Database, including logic you can read, trust, and maintain</summary>

<br>Five steps, expand each in turn (allow several minutes for the first):

&nbsp;

<details markdown>
<summary>&emsp;&emsp;1. Declare it — governing logic, not documentation</summary>

<br>Say this to your AI assistant:

```
Create basic_demo from samples/dbs/basic_demo.sqlite (customers, orders, products).

Include a notes field for orders.

On Placing Orders, Check Credit    
    1. The Customer's balance is less than the credit limit
    2. The Customer's balance is the sum of the Order amount_total where date_shipped is null
    3. The Order's amount_total is the sum of the Item amount
    4. The Item amount is the quantity * unit_price
    5. The Item unit_price is copied from the Product unit_price
```

> **Running in Codespaces?** During project creation, a browser tab may auto-open (or offer to) showing it running — safe to decline or dismiss.

The prompt above didn't just describe a database — the "Check Credit" block is **governing logic**, not documentation. Left unguided, an AI assistant would default to procedural code for rules like these — readable at 5 rules, but every future change means re-checking every code path by hand, and that stops scaling long before a real system's requirement count does. Declarative rules avoid that: they're specifications the engine enforces automatically, not procedure you maintain.

That's what you just declared. Next: run it, and see it enforced.

</details>

&nbsp;

<details markdown>
<summary>&emsp;&emsp;2. Run it — see the API and logic operate</summary>

<br>You've probably used AI to generate code before — so what's different here?

**Difference 1: it produces models, not code.** Run the basic_demo prompt above, and instead of a pile of procedural code, you get artifacts that declare structure or policy rather than procedure — same 5 requirements, same AI:

1. **Data model** — `database/models.py`
2. **Full JSON:API** — Swagger, pagination, optimistic locking (`api/expose_api_models.py` — 52 lines, zero per-table code)
3. **Admin App** — multi-table, with navigations and lookups (`ui/admin/admin.yaml` — simple YAML, not HTML/JS)
4. **Business logic** — [logic_discovery/place_order/check_credit.py](samples/basic_demo_sample/logic/logic_discovery/place_order/check_credit.py) — 5 rules (~40X less), same requirements, same AI

**Difference 2: the logic itself is declarative** — more on why that matters below.

Each small, readable, yours. Plain Python — standard tooling applies. Security is opt-in, not default — bootstrap RBAC anytime with `genai-logic add-auth`.

**See it running:** Press F5 using "API Logic Server Run (run project from manager)", and open the Admin App. Explore the API via Swagger, browse the data, and follow the relationships — all auto-generated from the data model.

Now trigger it: open an **unshipped** Order for Alice, edit the Widget item:

```
Change the quantity to a very large number. Save.
```

<details markdown>
<summary>&emsp;&emsp;&emsp;&emsp;Detail Instructions -- Screen Shots</summary>

<br>Alter the quantity for an *unshipped* item:

1. Show the Customer List
2. Show the first Customer
3. Show first Order
4. Edit the Item
5. Set the quantity

![credit-check](https://github.com/ApiLogicServer/Docs/blob/main/docs/images/basic_demo/credit-check.png?raw=true?raw=true)

</details>

<br>

The save fails — note the dialog. Why? Let's look.

</details>

&nbsp;

<details markdown>
<summary>&emsp;&emsp;3. Debug it — standard logging, standard debugger</summary>

<br>No new tools required. The rule chain that just fired is written to the standard log — plain text, readable in your terminal or editor: `logs/als.log`.

Every rule is a plain Python function or lambda. Set a breakpoint on any `calling=` function or `as_condition=` lambda in your IDE, exactly like you would anywhere else in the codebase — no proprietary debugger, no special UI.

![logic-debug](https://github.com/ApiLogicServer/Docs/blob/main/docs/images/logic/logic-debug.png?raw=true)

</details>

&nbsp;

<details markdown>
<summary>&emsp;&emsp;4. Iterate — 1 AI prompt adds table, relationship, 2 rules</summary>

<br>Ask your AI assistant for a new rule, in plain English:

```
Customers should not be able to create new orders if they have unresolved past due letters.
```

There was no `Letter` table in the model — the AI adds it, relates it to `Customer`, and declares a `count` + a `constraint`. One sentence creates a schema change and two new rules — automatically integrated with the 5 already there. No need to open `check_credit.py` to find where this belongs, or trace the other rules to check for conflicts: **maintainable**, demonstrated, not asserted.

Without the engine, an AI rewriting procedural code from scratch would have to re-read and re-touch every existing rule to check for dependencies — more surface area for a missed path, more tokens spent doing it. At 5 rules that's a nuisance; **at 500, the re-read is the bottleneck and the missed-path risk compounds.** Here, the engine resolves dependencies at load time, so adding this rule doesn't touch the rest — no regeneration, no regression risk, at any scale.

</details>

&nbsp;

<details markdown>
<summary>&emsp;&emsp;5. Why rules are easy to Read, Trust, and Maintain</summary>

<br>Iteration above worked the way it did because rules are **declarative**, not procedural. A quick debrief on what you just saw, then the full picture of why that matters at any scale.

**What you just saw:**

- **No need to call the new logic.** Rules are invoked automatically - regardless of the originating path.  You can **trust** that they'll always run.
- **Order doesn't matter.** Open `check_credit.py` and shuffle the five rules into any order you like. Rerun — still correct. Try that with 200 lines of procedural code.  You can **trust** that they'll run in the right order.
- **You got more than you asked for.** The original requirement said *"On Placing Orders, Check Credit"* — insert time. But the save that failed was an *edit* to an existing order. Nobody wrote an update-time check.

Functions don't behave like that. So why is that?

> **Traditional logic is procedural** — you own *how*: when it's called, and in what order. **Declarative logic — rules** — is about *what*, not how: you state the fact, and the system takes responsibility for invocation and ordering. That's why the new rule didn't need to be called, and why order didn't matter.

**What a rule actually is:** **Rules** enforce business policy — multi-table derivations, constraints, and actions like messaging. **LogicBank**, the rule engine, hooks SQLAlchemy's commit event to run them on every transaction — authored as plain Python functions in `logic/logic_discovery/`, readable, version-controlled, owned like any other source file.

**How it works:**
1. **At startup** — rules load, and the engine computes their dependency graph once.
2. **At commit** — for each transaction, the engine finds the rules relevant to what changed, and fires them in the right order.

Unlike procedural code, they're **declarative** — solving exactly the three problems raised above (AI great, but hard to Read, Trust, and Maintain):

| Property | What it means | Why it matters |
|---|---|---|
| **Readable** | 5 lines, one per requirement — declared once, e.g. `Customer.balance = sum of unpaid orders` | No archaeology needed to see what it does |
| **Trustworthy** | Rules fire at every commit, from every caller — you never call them | Can't be forgotten, can't be bypassed |
| **Maintainable** | Dependency order is computed once, automatically | Add a rule anywhere, it finds its place |

> Think of a **spreadsheet:** `B10 = SUM(B1:B9)` isn't called, it *reacts* — change any input cell, it recalculates. Rules react the same way to changes in what they depend on.

That's why order didn't matter — the engine computes it, not your source file — and why the edit was caught: it fires at every commit, from every caller, not just the one you wrote it for.

> With procedural code, even if you find the right passage — how do you know it's called for *every* transaction source? API, MCP, agent, Kafka, a future caller you haven't written yet? With thousands of code paths, you can't know. That's not a testing gap; it's a representation problem. Rules solve it structurally — declared once, fired at one commit point, from every caller, with no bypass possible. The 40x reduction in code isn't the point. The verifiable coverage is: ***you can read the rules, and trust they are being enforced. Always.***

<details markdown>
<summary>&emsp;&emsp;&emsp;&emsp;How this works: Context Engineering (CE) + a commit-time rules engine</summary>

<br>Two things have to be true for this to work:

**Step 1 — Context Engineering trains the AI to write rules, not code.** That same AI, left unguided, would have produced the ~200 buggy lines from earlier. Writing rules instead wasn't its own idea — it was told to, in detail, by **Context Engineering** — the same files driving this conversation right now:

- **Directs rules, not code.** When you ask for business logic, CE steers the AI toward the *right* rule type (sum vs. count vs. Allocate vs. Request Pattern) for what you actually asked for, instead of letting it default to the procedural code it's seen a million times in training.

- **Trains the AI to automate everything above, and to help you when it breaks.** EAI's 2-message Kafka pattern, the AI/Request Pattern wiring, Executable Requirements' pre-coding schema assessment — all of it is documented training material (`docs/training/*`) the AI reads *before* writing your code, not generic knowledge it's guessing from. Ask "what are rules?" or "how do rules work?" — or, without an AI handy, just read [samples/basic_demo_logic_gov/logic/readme_logic.md](samples/basic_demo_logic_gov/logic/readme_logic.md) — same material.

- **Answers your own questions, too.** Same materials, same AI — ask it directly:
    - Is this really infrastructure, like a database?
    - Is this a black box? How do I debug a rule chain?
    - What does it take to migrate off this if we ever wanted to?
    - How does this perform at scale?
    - What does this integrate with — APIs, workflows, agents, MCP?
    - Does this work with my existing database?

  More background: [Eval Guide](https://apilogicserver.github.io/Docs/Eval/).

  <details markdown>
  <summary>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;The AI was trained on this material — can you trust its answers?</summary>

  <br>Don't take them on faith. Ask the same question a different way, or ask something not covered here — like where this architecture breaks down. If it just recites the same lines back, you've caught it. If it reasons, that's the test passing.

  </details>

Put together: once the AI knows how the system works, it doesn't just generate rules instead of code — it helps you debug them, and helps you understand them. A design assistant, not just a coding assistant.

**Step 2 — the rules engine runs the rules.** Rules aren't called from your code — they're wired into a single SQLAlchemy `before_flush` listener, loaded once at server start as described above. Every write, from any path — API, custom endpoint, Kafka consumer, agent — passes through that one listener before it commits. No bypass — there's no second door.

</details>

<br>

Full writeup: [declarative/procedural comparison](samples/basic_demo_logic_gov/logic/procedural/declarative-vs-procedural-comparison.md).

</details>

</details>

&nbsp;

<details markdown>
<summary>Example — New Database</summary>

<br>No existing database? Same idea, same governing logic — just describe the tables instead of pointing at a `.sqlite` file (allow 8-10 mins):

```
Create basic_demo_new (no existing database) with:
    Customer: name, balance, credit_limit
    Order: customer, date_shipped, notes
    Item: order, product, quantity, unit_price, amount
    Product: name, unit_price

On Placing Orders, Check Credit
    1. The Customer's balance is less than the credit limit
    2. The Customer's balance is the sum of the Order amount_total where date_shipped is null
    3. The Order's amount_total is the sum of the Item amount
    4. The Item amount is the quantity * unit_price
    5. The Item unit_price is copied from the Product unit_price
```

Either path gets you the same thing: a working API and Admin App **plus the governing logic above** — not just a static schema. Same "Run it / Debug it / Iterate" walkthrough from the Existing Database example above applies here too, once it's running.

</details>

&nbsp;

<details markdown>
<summary>Scaling to the Enterprise — here's how</summary>

<br>With logic off its plate, AI can create remarkable results — solid enterprise systems, from requirements, not just demos that become tech debt.

**We add key *enterprise architecture* integration:**

- **Enterprise Integration (EAI)** — the demo above showed ***Publish** the Order to Kafka topic*. For the **subscribe** side, see [samples/basic_demo_eai/readme.md](samples/basic_demo_eai/readme.md): B2B orders from partner systems, via a Custom API or Kafka subscriber, including *lookups* so partners send `"Account": "Alice"` (not internal IDs).

- **MCP** — your API is **MCP-discoverable** out of the box (`/.well-known/mcp.json`).
Copilot, Claude, or ChatGPT can find the schema and answer natural-language queries against it.  There's no discovery layer for you to write — see [samples/basic_demo_ai_rules-supplier/readme_ai_mcp.md](samples/basic_demo_ai_rules-supplier/readme_ai_mcp.md)

- **AI Rules** — rules that call AI for genuinely judgment-call decisions (e.g. picking a supplier under disrupted shipping lanes).  Such AI "proposals" are governed by the deterministic rules to ensure results conform to business policy — see [samples/basic_demo_ai_rules-supplier/readme.md](samples/basic_demo_ai_rules-supplier/readme.md)

- **Custom UIs, safely** — Vibe tools (Cursor, v0, etc.) generate the UI; it's built against the same governed API, so the logic runs the same regardless of what's calling it — `genai-logic genai-add-app --vibe`.

- **Governance you can prove, not just assert** — three reports, generated from the running system, not hand-written:
    - **[Logic flow diagram](samples/basic_demo_logic_gov/docs/requirements/logic_flow_basic_demo_logic_gov.md)** — NL requirement, dependency diagram, and rule summary, for every rule chain
    - **[Ad-libs report](samples/basic_demo_logic_gov/docs/requirements/ad-libs.md)** — every assumption the AI made beyond the spec, so you know exactly what to verify
    - **[Health check](samples/basic_demo_logic_gov/docs/requirements/health_check.md)** — rule adoption, dependency-tracking integrity, missing docstrings, across the whole project

    A compliance reviewer can check the implementation in minutes, not by reading code.

    <img src="samples/basic_demo_logic_gov/docs/requirements/logic_diagrams/logic_diagram.svg" alt="Logic diagram: Item/Order/Customer rule chain, generated from the running rules" width="480">


That combination — AI, logic automation, and that enterprise architecture — is what enables ***Executable Requirements***: AI building real enterprise-class systems, from formats you already are familiar with, not a new syntax to learn:

- **Gherkin-style scenarios** — [business description](samples/demo_customs_clvs/readme.md), and the [actual requirements](samples/demo_customs_clvs/docs/requirements/customs_demo/requirements.md) used by AI to create the system.

- The **short prompt that built a system straight from an actual government tariff regulation** (Canada, CBSA) — [the prompt](samples/demo_customs_surtax/readme.md), and [the rules it produced](samples/demo_customs_surtax/logic/logic_discovery/cbsa_steel_surtax.py)

    > So, simply by referencing the regs, you get a complete enterprise system — including governed logic you can audit, trust, and maintain. AI implements the spec end-to-end and reports an **ad-libs list** — every decision it made beyond what the spec said — so you know exactly where it guessed.

&nbsp;

<img src="https://github.com/ApiLogicServer/Docs/blob/main/docs/images/architecture/logic-architecture-exec.png?raw=true" alt="Design and Runtime funnels into one governed Rules Engine" height="380" width="380" align="right">

The architecture that makes this work: two funnels, converging on one engine. All requirement formats, and all transaction sources, passing through **the same commit point. No bypass.**

**What AI delivers, once logic is off its plate: entire, *governed* systems from requirements** — not just code that becomes instant tech debt. It is this approach that **caught an 8-figure compliance exposure** a major logistics company's hand-coded system missed for months. [Full writeup →](https://apilogicserver.github.io/Docs/Tech-Ent-AI)

</details>

&nbsp;

<details markdown>
<summary>Go Deeper — guided tour, APIs, messaging</summary>

&nbsp;

You don't need any of this on day one — the admin app and the rules you just saw are enough to get going. Once the basics feel natural, here's more: a hands-on tour, and proof the same governed logic runs identically no matter how a transaction arrives — Admin App, API, or message queue. No separate logic to maintain per channel, and no gap for compliance to worry about.

&nbsp;

<details markdown>
<summary>Guided tour — 30-45 min, hands-on</summary>

&nbsp;

**Create basic_demo** (auto-opens with guided tour option):
```bash
genai-logic create --project_name=basic_demo --db_url=sqlite:///samples/dbs/basic_demo.sqlite
```

**Inside the project:** Say to your AI assistant: *"Guide me through basic_demo"* (30-45 min hands-on tour).

> Teaches API creation, declarative rules, security, and Python customization. Fail-safe — scripts ensure no coding errors.

</details>

&nbsp;

<details markdown>
<summary>APIs — partner-specific formats, same governed logic</summary>

&nbsp;

Every project already has a full REST API (JSON:API — filtering, sorting, pagination, related-data access), generated automatically — that's what you saw in Swagger. Beyond that, describe a **custom API** for a specific partner's format in plain English, and your AI assistant builds it:

```
Accept orders from partner Acme in this format: {"Account": "...", "Items": [{"Name": "...", "QuantityOrdered": ...}]}.
Map Account to Customer by name, Items.Name to Product by name.
Enforce the same Check Credit rules as regular orders.
```

A partner-format order and an Admin-App order run through the identical commit point — same rules, same guarantees, zero duplicated logic.

</details>

&nbsp;

<details markdown>
<summary>Messages — async delivery via Kafka, for when the receiving system might be down</summary>

&nbsp;

APIs assume the other system is up right now. Message brokers like Kafka don't: you **publish** a message to a **topic**, the broker guarantees delivery even if the receiver is offline, and other systems **subscribe** to that topic — usually called "pub/sub." Describe it the same way you'd describe anything else:

```
When an Order's date_shipped is set, publish it to Kafka topic 'order_shipping'.
```

You can try this without standing up Kafka at all — a debug endpoint sends a test message straight to the logic, so you can confirm the behavior before wiring up real infrastructure.

</details>

&nbsp;

</details>

&nbsp;

