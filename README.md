# ⚡ Cloudflare AI Firewall Rule Copilot

AI-powered developer tool that converts natural language into Cloudflare WAF firewall expressions and JSON rules, with safety analysis, ambiguity handling, and persistent rule history using Durable Objects.

------------------------------------------------------------------------

# 🚀 Features

### AI Firewall Rule Generation

-   Converts natural language into valid Cloudflare Firewall
    Expressions
-   Generates JSON WAF rule objects
-   Performs ambiguity detection
-   Performs rule safety analysis
-   Provides short technical explanations

### Safety Engine

Detects dangerous patterns such as: - Blocking all traffic
- Blocking Cloudflare IP ranges
- Blocking admin paths
- Country-wide blocks without restrictions
- Unrestricted actions
- Invalid expressions

### Durable Object Memory

-   Stores full rule history
-   Persistent memory
-   Clear audit trail
-   Worker → DO coordination

### Cloudflare-Style Dashboard UI

-   Input panel
-   Expression viewer
-   JSON rule viewer
-   Warning panel
-   Rule history sidebar

### Technologies Used

-   Cloudflare Workers
-   Durable Objects
-   Workers AI (Llama 3.3)
-   HTML + CSS + JavaScript

------------------------------------------------------------------------

# 🧩 Architecture

``` text
+---------------------+       +---------------------------+
|     Frontend UI     | <---> |   Worker API (index.js)   |
+---------------------+       +-------------+-------------+
                                            |
                                            v
                                +---------------------------+
                                |     Workers AI Model      |
                                |  (Llama 3.3 Instruct)     |
                                +---------------------------+
                                            |
                                            v
                                +---------------------------+
                                |     Durable Object        |
                                |      RuleHistoryDO        |
                                +---------------------------+
```

------------------------------------------------------------------------

# 📁 Project Structure

``` text
cf_ai_waf_firewall_copilot/
│
├── public/
│   ├── index.html
│   ├── style.css
│   └── app.js
│
├── src/
│   ├── index.js
│   └── rule-history.js
│
├── PROMPTS.md
├── wrangler.jsonc
└── README.md
```

------------------------------------------------------------------------

# 🧠 How the AI Works

The Worker constructs a structured system prompt enforcing:

-   Valid JSON output
-   Strict Cloudflare WAF expression syntax
-   Safety warnings
-   Clarification detection
-   Short explanations

AI output is parsed, validated, and stored in a Durable Object as
persistent rule history.

**Model used:**\
`@cf/meta/llama-3.3-8b-instruct`

------------------------------------------------------------------------

# 📝 Example Inputs

Try:

-   "block POST requests to /login from India"\
-   "allow only IP 123.45.67.89"
-   "challenge non-US traffic"
-   "block python-requests bots"
-   "block everything" → triggers warnings & clarification

------------------------------------------------------------------------

# 🗂 Rule History API

  Endpoint                    Description
  --------------------------- ----------------------------------
  `GET /api/history`          Returns full stored rule history
  `POST /api/history/clear`   Clears history

Durable Object provides the required memory/state.

------------------------------------------------------------------------

# ⚙️ Setup & Deployment

### 1. Install Wrangler

``` bash
npm install -g wrangler
```

### 2. Initialize Project

``` bash
wrangler init
```

### 3. Create KV Namespace

``` bash
wrangler kv namespace create rule_cache_namespace
```

Paste the generated ID into `wrangler.jsonc`.

### 4. Run Locally

``` bash
wrangler dev
```

### 5. Deploy

``` bash
wrangler deploy
```

------------------------------------------------------------------------

# 📄 PROMPTS.md

All prompt engineering used for the AI model is stored in `PROMPTS.md`
as required by the assignment.

------------------------------------------------------------------------

# 📚 License

MIT License
