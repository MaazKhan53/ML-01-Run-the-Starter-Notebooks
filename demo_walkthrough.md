# FL-09 Demo Walkthrough & Script

*Note: This document serves as the end-to-end run walkthrough, detailing the exact execution flow, design decisions, and limitations of the Predictive Content Auditing Agent.*

### Part 1: End-to-End Run
1. **Initialization:** The agent initializes by loading the FlyRank ML Internship dataset (filtered to 16,513 active rows).
2. **Execution:** Running `capstone.ipynb` triggers the Random Forest classifier. The system processes historical search logs (Impressions, CTR, Position) to assign a decline probability score to each page.
3. **Output Generation:** The agent bypasses the raw scores and outputs a prioritized CSV playbook (`w07_action_queue.csv`), ranking pages from "Code 1: Critical Refresh" to "Code 3: Deprioritize."

### Part 2: The Design Decision
**Why Random Forest over Deep Learning?**
A critical design decision in this architecture was utilizing a Random Forest Classifier rather than a more complex neural network. In a business context, model interpretability is paramount. Random Forest allows us to extract explicit feature importance (e.g., proving that Average Position and CTR drive the prediction). This transparency is required to build trust with the SEO and marketing teams who rely on the agent's output.

### Part 3: The Limitation & Guardrail
**The "No-Go" List Guardrail**
A strict limitation of this agent is that it cannot understand overarching business strategy; it only reads mathematical metrics. Therefore, a core guardrail was hardcoded into the pipeline: the "No-Go" list. Certain protected URLs (like core product pages or legal compliance pages) are programmatically stripped from the automated action queue. This ensures that a high decline probability score never accidentally triggers the modification or deletion of an untouchable business asset, enforcing a strict human-in-the-loop requirement.
