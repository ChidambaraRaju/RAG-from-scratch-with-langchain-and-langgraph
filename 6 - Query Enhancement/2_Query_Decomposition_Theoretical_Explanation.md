# Understanding Query Decomposition

Query Decomposition is an advanced technique in which a complex user query, containing multiple distinct questions or concepts, is broken down into a series of simpler, standalone sub-questions. Each sub-question is then processed individually by the RAG system, and the final answers are synthesized to provide a comprehensive response to the original query.

---

## The Problem: Complex Queries Confuse Retrievers

Standard RAG systems are optimized to handle a single, focused question. When a user asks a multi-faceted question, the retriever can get confused.

**Example:**
- **Complex Query:** "How does LangChain's memory system compare to CrewAI's agent delegation model, and which is better for collaborative tasks?"

A simple vector search on this entire sentence is problematic. The retriever is forced to find a single set of documents that somehow covers "LangChain memory," "CrewAI agent delegation," and "collaborative tasks" all at once. It will likely return documents that are a mediocre match for all topics, rather than finding the best document for each individual topic. This leads to a weak context and an incomplete or inaccurate final answer.

---

## The Solution: Decompose, Retrieve, and Synthesize

Query Decomposition solves this by using an LLM as an intelligent "query planner." The process follows a multi-step workflow:

1.  **Decompose:** The original complex query is sent to an LLM with a prompt that instructs it to break the query into a list of simpler, logical sub-questions.
2.  **Retrieve & Answer (per sub-question):** Each sub-question is then run through its *own* RAG process.
    -   The sub-question is sent to the retriever to find highly focused, relevant documents.
    -   These documents are passed with the sub-question to an LLM to generate a specific answer for that part alone.
3.  **Synthesize:** The individual answers for all the sub-questions are collected and combined to form the final, comprehensive response.

**Analogy: The Project Manager and the Specialist Team 🧑‍💼**

Imagine a client gives a project manager (the decomposition chain) a complex request: *"Build our company a new website with a user login system and an integrated payment gateway."*

-   **Decomposition:** The project manager doesn't just hand this vague request to a single developer. They *decompose* it into clear, independent tasks:
    1.  "What are the requirements for the user authentication system?"
    2.  "What are the best options for a payment gateway?"
    3.  "What front-end framework should we use?"

-   **Retrieve & Answer:** Each task (sub-question) is assigned to a specialist (the sub-query RAG chain).
    -   The back-end developer researches authentication.
    -   The payments specialist investigates Stripe and PayPal.
    -   The front-end developer evaluates React and Vue.

-   **Synthesis:** The project manager takes the detailed reports from each specialist and combines them into a single, cohesive project plan (the final answer) to present to the client.

---

### Key Benefits

* **Improved Retrieval Accuracy:** Searching for focused sub-questions (e.g., "What is LangChain memory?") yields far better and more relevant documents than searching for the entire complex query at once.
* **Enables Multi-Hop Reasoning:** It allows the system to answer questions that require connecting information from different, unrelated documents in the knowledge base.
* **Reduces Hallucinations:** By providing the LLM with a smaller, highly relevant context for each sub-question, you reduce the risk of confusion and fabricated answers.

### Trade-offs

* **Increased Latency and Cost:** This is the biggest drawback. The process requires multiple LLM calls (one for decomposition, one for each sub-question, and potentially a final one for synthesis), which takes longer and costs more than a single RAG call.