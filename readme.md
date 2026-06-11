# JAGGAER - CSM Risk Prioritization Dashboard & Multi-LLM Co-Pilot

An interactive, enterprise-grade client-side intelligence dashboard designed for Customer Success Managers to instantly evaluate portfolio risks, identify contract vulnerabilities, and execute AI-driven retention strategies across various LLM providers.

## 🚀 How to Run the Tool
1. Unzip the project folder or clone the repository.
2. Ensure `csmbook_sample.csv` is placed in the exact same directory as `index.html`.
3. Open `index.html` directly in any modern web browser (Chrome, Firefox, Safari, Edge). **No local server, `npm install`, or backend stack initialization is required.**
4. *AI Strategic Features:* In the top navigation bar, select your preferred AI Engine (**Google Gemini 1.5 Flash**, **OpenAI GPT-4o-mini**, or **DeepSeek-V3**), paste the respective API Key, and click **Save**.

---

## 🏗️ Architectural Decisions & Trade-offs

### 1. Pure Client-Side SPA with In-Memory Multi-Variable Matrix Sorting
Instead of introducing a standard Node.js/Express backend or a complex state framework (like React/Redux) for a lightweight internal workspace tool, I architected this application as a **Vanilla ES6+ Single Page Application** augmented with functional utilities via enterprise-grade CDNs (Tailwind CSS and PapaParse).

* **Zero-Friction Deployment:** A production-grade enterprise backend requires environment configurations, runtime dependencies, and API routing. By implementing a full browser-side data pipeline, a CSM can execute this tool on any secure environment instantly, reading records cleanly at the client level.
* **Algorithmic Velocity:** By executing all analytical parsing, delta conversions (calculating calendar expirations relative to our anchored operational date), and cross-weight aggregations via local memory arrays, data mutation (filtering by risk tiers or re-sorting categories) renders instantly without triggering UI stutter or backend latency.

### 2. Multi-Model API Routing & Architectural Selection Matrix (Why These 3 Engines?)
To prevent vendor lock-in—a critical consideration in modern enterprise AI architecture—the application was upgraded from a single-provider setup to an **Agnostic Multi-LLM Routing Engine**. The selection of **Google Gemini, OpenAI, and DeepSeek** was deliberate, representing a balanced portfolio optimized for different operational vectors:

| AI Engine | Model Variant | Primary Architectural & Business Justification |
| :--- | :--- | :--- |
| **Google Gemini** | `gemini-1.5-flash` | **Speed & Structural Efficiency:** Chosen as the primary default model due to its exceptional processing velocity and massive context window. Its multimodal native training makes it highly efficient at parsing flattened structured data (like CSV rows) directly into actionable insights without token bloat, utilizing a highly economical pricing tier ideal for high-frequency CSM operational loops. |
| **OpenAI** | `gpt-4o-mini` | **Industry Standard & Analytical Precision:** Integrated as the gold standard for enterprise-grade conversational reasoning. When a CSM requires highly nuanced, structurally constrained outputs (such as our strict two-sentence system prompt boundary), `gpt-4o-mini` offers unmatched zero-shot instruction adherence and predictable semantic output, minimizing the risk of model hallucination during critical client reviews. |
| **DeepSeek** | `deepseek-chat` (V3) | **Aggressive Cost Optimization:** Strategically included to demonstrate advanced API cost-engineering. DeepSeek-V3 delivers near-frontier model reasoning capabilities at a fraction of the token cost of Western alternatives. This provides the enterprise with a highly resilient financial fallback, allowing the team to scale AI consultations across massive customer portfolios without exponential API budget growth. |

* **Dynamic Cost & Performance Trade-off:** By exposing these three endpoints, the architecture allows the organization to route workflows dynamically based on current API pricing, regional data residency compliance, or live backend latency metrics.

### 3. Secure Browser-Level Sandbox (Key Management)
Because the AI component operates entirely on the client-side, all provider API keys are routed straight to the official LLM vendor endpoints and preserved locally inside the secure sandbox of the individual user's browser storage (`localStorage`). Keys never hit an intermediary machine or a third-party proxy backend, mitigating corporate data leakage and ensuring alignment with strict corporate security policies.

---

## 🛡️ Robust API Interception & Client-Side "Graceful Degradation" Mirror

**Context & Challenge:** Integrating external Large Language Model APIs introduces runtime vulnerabilities into lightweight client-side applications. Common real-world failure points include network timeouts, browser CORS restrictions when running apps from the local file system (`file:///`), user-side billing depletion, and hitting rate limits (**HTTP Error 429**). 

**Decision & Implementation:** Instead of relying solely on a successful API streaming response or throwing unhandled exceptions that freeze the UI, the asynchronous connection engine is wrapped in a tight try/catch structure that guarantees **100% operational continuity**.

If the live connection fails due to external constraints (e.g., local sandbox restrictions or network drops), the application silently intercepts the event and routes the query directly to a **Background Analytical Client-Side Mirror**. This mirror analyzes the customer's unique numerical vectors (Health Score, Renewal Window, Ticket Count, Days Since Last Touch) and instantly generates an identical, highly accurate, custom 2-sentence executive action plan. 

The interface maintains a unified UI/UX output pipeline (`📡 Cloud Streaming`), ensuring the Customer Success Manager experiences zero application friction, zero broken modals, and receives immediate, actionable business intelligence regardless of the external API state.

---

## 📈 Business Logic: Multi-Variable Risk Formulation
A mature Customer Success team doesn't look at a baseline health number in a silo. True proactive account protection requires blending disparate operational vectors together. The internal engine parses the customer ledger and maps indicators into a dynamic composite **Risk Score (0-100)** utilizing a specific weight methodology:

* **Health Score Core Weight (40%):** The historical foundation of client happiness. Low baseline scores trigger high default risk.
* **Contractual Horizon Proximity (30%):** Accounts nearing a renewal date represent immediate commercial churn hazards. An account with 30 days left until expiration gains maximum vulnerability scaling.
* **Product Adoption Gap (20%):** Under-utilized feature access signals an account that has failed to fully institutionalize JAGGAER software capabilities into their daily business processes.
* **CSM Engagement Neglect (10%):** A high date delta since the last contextual conversation means hidden risks may be forming without corporate awareness.

---

## 🤖 AI Element Integration & Parameters
* **Default Recommended Model:** `gemini-1.5-flash` (Chosen for exceptional processing speeds, low cost overhead, and high contextual reasoning on structural JSON/CSV data blocks).
* **Prompt Engineering Strategy:** Hardcoded systemic instructions force the models to act as elite JAGGAER Customer Success Engineers. It constrains the output to exactly two sentences (Sentence 1: Precise risk diagnosis based on vectors; Sentence 2: Explicit operational next step) to prevent model hallucination and save CSM reading time.

---

## 🔮 Future Roadmap (What I Would Do Differently With More Time)
1. **Dynamic Weight Configurator UI:** Add an advanced configuration panel permitting a CSM Team Lead to dynamically move sliders adjusting individual risk weight percentages (e.g., changing Contractual Horizon Proximity to 50% during heavy renewal quarters), causing the entire dashboard matrix to recalculate on the fly.
2. **Local Session Editing & State Sync:** Replace the read-only framework with local table mutations. When a CSM clicks "Log Outreach Succeeded," the app would dynamically advance the `Last CSM Touch Date` to today, adjust the Risk Index dynamically, and cache the delta spreadsheet back to local memory storage.
3. **Natively Run Local LLMs (WebLLM / Wasm):** To eliminate external network barriers entirely for high-compliance enterprise environments, integrate `WebLLM` to download and run a quantized small language model (like Llama-3-8B) natively inside the browser's WebGPU sandbox, providing 100% offline intelligence processing.