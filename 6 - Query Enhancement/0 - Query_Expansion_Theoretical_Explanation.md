# Understanding Query Expansion

Query Expansion is a technique used to enhance or reformulate a user's initial query to improve the quality of document retrieval. The primary goal is to bridge the "vocabulary gap" between the simple terms a user might use and the specific, detailed language present in the knowledge base.

---

## The Problem: The User's Query is Often Not Enough

In a RAG system, the quality of the retrieved documents is entirely dependent on the quality of the query. However, users often provide queries that are:

* **Short and Ambiguous:** e.g., "LangChain memory"
* **Lacking Technical Terms:** e.g., "AI agents talk to each other"
* **Using Different Synonyms:** e.g., "How to make a car go faster" when the documents talk about "improving vehicle performance."

A standard retriever, even a semantic one, might struggle with these queries and fail to retrieve the most relevant documents, leading to a poor or incomplete final answer.

---

## The Solution: Reformulating the Query with an LLM

Query Expansion addresses this by using a Large Language Model (LLM) as an intelligent "pre-processor" for the user's query.

**How It Works:**
1.  The user's original query is taken as input.
2.  It's passed to an LLM with a specific prompt that instructs it to **reformulate, expand, or add context** to the query. The prompt explicitly tells the LLM *not* to answer the question, but to make it a better search query.
3.  The LLM generates one or more enhanced queries.
4.  These new, richer queries are then sent to the retriever.

**Analogy: The Expert Librarian 🧑‍🏫**

Imagine you walk into a library and ask the person at the front desk, "Where are the books on 'space'?"

* A **standard retriever** is like a new librarian who just points you to the "Astronomy" section. You'll find some good books, but you might miss what you're really looking for.
* **Query Expansion** is like an expert librarian. They hear "space" and immediately know what you *might* be interested in. They'll mentally expand your query to:
    > "Are you looking for books on astrophysics, the history of the space race, spacecraft engineering, rocket propulsion, or maybe even interior design and negative space?"

This expanded set of concepts is then used to search the library catalog, guaranteeing a much more comprehensive and relevant set of results.

---

### What Can Query Expansion Do?

An LLM-based query expansion can perform several enhancements:

* **Add Synonyms and Related Terms:**
    * *Original:* "LangChain memory"
    * *Expanded:* "LangChain memory types, state management in LangChain, ConversationBufferMemory, context retention in LLM chains"
* **Add Technical Specificity:**
    * *Original:* "CrewAI agents"
    * *Expanded:* "CrewAI collaborative autonomous agents, multi-agent systems framework, task delegation and execution in CrewAI"
* **Rephrase as a Clearer Question:**
    * *Original:* "RAG accuracy"
    * *Expanded:* "How to improve the accuracy and relevance of Retrieval-Augmented Generation systems?"

### Benefits and Trade-offs

* **Benefit - Improved Recall:** The primary benefit is a massive improvement in **recall**—the ability to find *all* relevant documents, not just the obvious ones.
* **Benefit - Handles Ambiguity:** It helps clarify what a user might mean by a short or poorly phrased query.
* **Trade-off - Latency and Cost:** It adds an extra LLM call to your RAG pipeline, which increases both the time and the cost to get a final answer.
* **Risk - Topic Drift:** A poorly prompted or overly creative LLM might expand the query in an incorrect or irrelevant direction, retrieving a "hallucinated context."

Query Expansion is a powerful tool to use when you find that your RAG system is failing to retrieve relevant documents for simple user queries.