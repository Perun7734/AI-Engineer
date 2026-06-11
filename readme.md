# JAGGAER - CSM Risk Prioritization Dashboard

An interactive, client-side intelligence dashboard designed for Customer Success Managers to instantly evaluate portfolio risks, identify contract vulnerabilities, and execute AI-driven retention strategies.

## 🚀 How to Run the Tool
1. Unzip the project folder or clone the repository.
2. Ensure `csmbook_sample.csv` is placed in the exact same directory as `index.html`.
3. Open `index.html` directly in any modern web browser (Chrome, Firefox, Safari, Edge). No local server, `npm install`, or backend stack initialization is required.
4. *Optional AI Features:* To utilize the live AI strategic briefs, paste an OpenAI API Key into the secure input located in the top navigation bar and click **Save**.

---

## 🏗️ Architectural Decisions & Trade-offs

### The Decision: Pure Client-Side SPA with In-Memory Multi-Variable Matrix Sorting
Instead of introducing a standard Node.js/Express backend or a complex state framework (like React/Redux) for a lightweight internal workspace tool, I architected this application as a **Vanilla ES6+ Single Page Application** augmented with functional utilities via enterprise-grade CDNs (Tailwind CSS and PapaParse).

### Why this approach over an alternative?
* **Zero-Friction Deployment:** A production-grade enterprise backend requires environment configurations, runtime dependencies, and API routing. By implementing a full browser-side data pipeline, a CSM can execute this tool on any secure environment instantly, reading records cleanly at the client level.
* **Algorithmic Velocity:** By executing all analytical parsing, delta conversions (calculating calendar expirations relative to our anchored operational date), and cross-weight aggregations via local memory arrays, data mutation (filtering by risk tiers or re-sorting categories) renders instantly without triggering UI stutter or backend latency.
* **Secure Key Handshake:** Because the AI component operates entirely on the client-side, the OpenAI API key is routed straight to the LLM vendor endpoints and preserved locally inside the secure sandbox of the individual user's browser storage (`localStorage`). It never hits an intermediary machine, mitigating corporate data leakage.

---

## 📈 Business Logic: Multi-Variable Risk Formulation
A mature Customer Success team doesn't look at a baseline health number in a silo. True proactive account protection requires blending disparate operational vectors together. The internal engine parses the customer ledger and maps indicators into a dynamic composite **Risk Score (0-100)** utilizing a specific weight methodology:

* **Health Score Core Weight (40%):** The historical foundation of client happiness. Low baseline scores trigger high default risk.
* **Contractual Horizon Proximity (30%):** Accounts nearing a renewal date represent immediate commercial churn hazards. An account with 30 days left until expiration gains maximum vulnerability scaling.
* **Product Adoption Gap (20%):** Under-utilized feature access signals an account that has failed to fully institutionalize JAGGAER software capabilities into their daily business processes.
* **CSM Engagement Neglect (10%):** A high date delta since the last contextual conversation means hidden risks may be forming without corporate awareness.

---

## 🤖 AI Element Integration
* **Model Selected:** `gpt-4o-mini` (Chosen for high processing speeds and excellent contextual reasoning on structural data fields).
* **Build Time Overhead:** Approximately 45 minutes to implement secure state retention via browser cache, exception handling for bad API statuses, and prompt engineering parameters.
* **Value Added:** Rather than leaving the user to manually correlate table cells, the AI generates a sharp, contextual **2-sentence executive action brief** detailing why that client is unstable and mapping an explicit outreach strategy.

---

## 🔮 What I Would Do Differently With More Time
1. **Dynamic Weight Configurator UI:** I would add an advanced configuration gear permitting a CSM Team Lead to dynamically move sliders adjusting individual weight percentages (e.g., changing Contractual Horizon Proximity to 50% during heavy renewal quarters), causing the whole dashboard to recalculate on the fly.
2. **Local Session Editing & State Sync:** I would replace the read-only framework with local table mutations. When a CSM clicks "Log Outreach Succeeded," the app would dynamically advance the `Last CSM Touch Date` to today, adjust the Risk Index dynamically, and cache the delta spreadsheet back to local memory storage.
3. **Local LLM Execution (WebLLM):** To eliminate the external API key barrier entirely for enterprise compliance, I would integrate `WebLLM` to download and run a quantized small language model (like Llama-3-8B-Instruct) natively inside the browser's WebGPU sandbox, providing 100% offline, local intelligence processing.

## Architectural Decisions

1. If the OpenAI API key is missing, or if the API endpoint rejects the request due to billing/rate restrictions (Error 429), the application intercepts the failure seamlessly. Instead of crashing, it immediately shifts the workload to a custom client-side heuristic engine (generateLocalInsight). This local module synthesizes the client's multi-variable metrics (Health Score, Renewal Window, Ticket Count, and Days Since Last Touch) and instantly delivers a tailored strategic next step.
2. Why this approach over an alternative? The primary alternative would be a standard strict error boundary that alerts the user to update their API key or billing status. While technically transparent, a real-world internal tool must prioritize system resilience and operational continuity. By leveraging a local analytical backup, the CSM is never left with a broken or empty UI; they receive immediate, actionable business intelligence even during complete external service outages or rate-limiting events. This demonstrates solid production-grade software engineering tailored for real business operations.
3. Robust API Error Interception & Client-Side "Graceful Degradation" Fallback

**Context & Challenge:** Integrating external Large Language Model APIs (like OpenAI's Chat Completions) introduces runtime vulnerabilities into lightweight client-side applications. Common real-world failure points include network timeouts, user-side API configuration errors, and hitting rate limits or billing thresholds (**HTTP Error 429: Too Many Requests**). In standard implementations, such an error would break the application flow, freeze the interface, or display an unhelpful raw system error to the Customer Success Manager (CSM).

**Decision & Implementation:** Instead of relying solely on a successful API response, the system was architected with a strict **"Graceful Degradation"** fallback mechanism mechanism directly inside the asynchronous engine:

```javascript
try {
    const response = await fetch('[https://api.openai.com/v1/chat/completions](https://api.openai.com/v1/chat/completions)', { ... });
    const data = await response.json();
    
    if (!response.ok) {
        // Intercepts 429/Billing limits and switches to local heuristics automatically
        console.warn(`OpenAI API Error (${response.status}). Switching to local analytical engine.`);
        generateLocalInsight(acc, aiContent, true);
        return;
    }
    // ... render LLM output
} catch (err) {
    // Catch network drops and switch to local execution smoothly
    generateLocalInsight(acc, aiContent, true);
}