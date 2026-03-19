# ⚡ AI Firewall Rule Copilot for Cloudflare WAF

AI-powered developer tool that converts natural language into Cloudflare WAF firewall expressions and JSON rules, with safety analysis, ambiguity handling, and persistent rule history using Durable Objects.

------------------------------------------------------------------------
## Why This Project Matters
Modern cloud security systems require precise and safe firewall configurations, but writing WAF rules manually is error-prone and complex. This project explores how LLMs can assist developers by translating natural language into valid firewall rules while enforcing safety constraints and preventing misconfigurations.

------------------------------------------------------------------------
## Key Engineering Decisions
- Used Cloudflare Durable Objects to maintain persistent rule history and enable stateful interactions across requests
- Implemented validation and safety checks to prevent dangerous or overly broad firewall rules
- Structured prompts to enforce strict JSON output and valid WAF expression syntax
- Designed separation between frontend UI, Worker API, and state management for scalability and maintainability

-------------------------------------------------------------------------

# 🚀 Features

### AI Firewall Rule Generation

-   Converts natural language into valid Cloudflare Firewall
    Expressions
-   Generates JSON WAF rule objects
-   Performs ambiguity detection
-   Performs rule safety analysis
-   Provides short technical explanations

### Safety Engine

Detects dangerous patterns such as: 
- Blocking all traffic
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

## Tech Stack
- Cloudflare Workers (serverless backend)
- Durable Objects (stateful storage)
- Workers AI (Llama 3.3)
- JavaScript (frontend + backend)
- HTML/CSS (UI)

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
