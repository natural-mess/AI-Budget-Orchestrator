# AI Budget Orchestrator
## A Human-in-the-Loop Financial Planner powered by Google ADK

### Problem Statement
Managing personal finances is inherently complex, deeply personal, and constantly evolving. Traditional budgeting tools and spreadsheet templates are static—they don't adapt to changing economic environments or complex, multi-variable financial goals (like saving for a house while paying off student loans). Users need more than just a calculator; they need an intelligent, dynamic financial planner that can deeply analyze their unique situation, generate highly specific strategies, and actively converse with them to refine their goals. Providing financial advice also requires a high degree of trust and verification to avoid hallucinations or reckless strategies.

### Why agents?
Financial planning is not a simple "prompt-and-response" text generation task. It requires deterministic logic, structured orchestration, memory, and oversight. Agents are the perfect solution because:
1. **Tool Use:** Agents can utilize deterministic tools (e.g., `draft_budget`, `redact_pii`) to run rigid mathematical logic rather than guessing numbers.
2. **Statefulness:** Agents can persist context across long conversations, allowing users to tweak and adjust their budgets iteratively without re-explaining their life story.
3. **Human-in-the-Loop (HITL):** Using agentic workflows, the AI can pause its execution to request explicit human approval on a drafted budget before finalizing the strategy—a critical safety mechanism for financial applications.

### What you created
I built a full-stack, stateful multi-agent web application. 
**The Architecture:**
*   **Backend Intelligence:** Powered by Google's **Agent Development Kit (ADK)** and the Gemini model ecosystem (`gemini-flash-lite-latest`). It utilizes ADK's `Runner` and `InMemorySessionService` to track isolated user sessions.
*   **API Layer:** A highly concurrent **FastAPI** server that executes the agent's logic asynchronously and streams the AI's thoughts and tool-calls back to the client via Server-Sent Events (SSE).
*   **Frontend UI:** A stunning, modern **Glassmorphism UI** built in vanilla HTML/CSS/JS. It includes a custom-built SSE buffer parser to handle fragmented data packets, and a secure Settings Modal that allows users to dynamically inject their own Gemini API keys and seamlessly switch between different Gemini models at runtime.

### Demo
You can view the full source code and run the project locally from the GitHub repository:
[https://github.com/natural-mess/AI-Budget-Planner-Expense-Analyst](https://github.com/natural-mess/AI-Budget-Planner-Expense-Analyst)

**To run it locally:**
1. Clone the repo and install dependencies with `uv sync`.
2. Start the server: `uv run uvicorn app.fast_api_app:app --host 0.0.0.0 --port 8000`
3. Navigate to `http://localhost:8000`. Click the gear icon to input your Gemini API Key and select your preferred model, then ask the orchestrator to build you a budget!

### The Build
This project was built entirely from the ground up using modern, highly modular technologies:
*   **Google Agent Development Kit (ADK):** The backbone of the project, providing the `Agent`, `LongRunningFunctionTool`, and Session management structures.
*   **Google GenAI SDK:** For direct, high-performance interactions with the Gemini models.
*   **FastAPI & Uvicorn:** Chosen for their native asynchronous support, allowing for highly efficient, non-blocking SSE streaming.
*   **Vanilla JS/CSS:** I opted out of heavy frontend frameworks like React to keep the project incredibly fast and lightweight. The UI relies on native CSS variables, flexbox, and complex JavaScript asynchronous generators to parse the SSE stream dynamically.
*   **Security Tools:** `python-dotenv` for local secret management, combined with dynamic `os.environ` injection based on secure `localStorage` client payloads.

### If I had more time, this is what I'd do
If I had more time, I would expand the single "Orchestrator" agent into a true **Multi-Agent Swarm**:
1.  **Specialized Sub-Agents:** I would create a dedicated `Tax Analyst Agent` and an `Investment Strategist Agent`. The main Orchestrator would delegate specific sub-tasks to these agents before compiling the final budget.
2.  **API Integrations:** I would integrate real-world banking APIs (like Plaid) via standard ADK Tools so the agents could pull live transaction data instead of relying on manual user input.
3.  **Cloud Deployment:** I would use the **Google Cloud Run MCP server** to automate the containerization and public deployment of this application so anyone could access it without running it locally.
4.  **Persistent Storage:** Swap out the `InMemorySessionService` for a persistent SQL or NoSQL database to ensure chat histories survive server restarts.
