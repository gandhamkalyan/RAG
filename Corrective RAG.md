> Normal RAG trusts what it retrieves. Corrective RAG checks the retrieved information and tries to fix retrieval when it isn't good enough.

> Think of it as RAG + Quality Checker + Correction mechanism.

> CRAG becomes valuable when you have a large or noisy knowledge base.



**1. Problem with normal RAG**



Suppose your company has 100 HR documents.



User asks: "How many maternity leave days are employees eligible for?"



Normal RAG: User Question > Vector Search > Top 3 chunks > LLM > Answer



But imagine the retrieved chunks are:



Chunk 1 → Sick leave = 10 days

Chunk 2 → Casual leave = 12 days

Chunk 3 → Old maternity policy from 2022



The retriever technically found similar content, but the context is not good enough. Normal RAG may still pass these chunks to the LLM. The LLM might then produce an incorrect answer.

\---------------------------------------------------------

2\. **What Corrective RAG does**



CRAG adds a retrieval evaluator.



User Question > Retriever > Retrieved Documents > Retrieval Evaluator

&#x20;     ↓

"Are these documents actually relevant?" Then the system decides what to do.



Conceptually:



&#x20;                   Retrieved Documents

&#x20;                           ↓

&#x20;                   Relevance Evaluator

&#x20;                           ↓

&#x20;              ┌────────────┼────────────┐

&#x20;              ↓            ↓            ↓

&#x20;           Correct      Ambiguous     Incorrect

&#x20;              ↓            ↓            ↓

&#x20;         Use docs      Improve /      Search for

&#x20;                       supplement      better info

&#x20;              └────────────┼────────────┘

&#x20;                           ↓

&#x20;                          LLM

&#x20;                           ↓

&#x20;                         Answer



3\. Simple real-world analogy



Imagine you're preparing for an interview and ask your friend: "What are the limitations of Copilot Studio?"

Your friend gives you some notes. Before memorizing them, you ask yourself: "Are these notes actually related to Copilot Studio limitations?"



**> Case 1 — Correct**

Notes contain: Copilot Studio has limitations around complex orchestration, customization, model control, etc.

You say: Good. I'll use these.



**> Case 2 — Incorrect**

Notes are about: GitHub Copilot.

You say: Wrong information. I need another source.



**> Case 3 — Ambiguous**

Some information is relevant but incomplete.

You say: I'll keep the useful information and search for additional information. That's essentially what Corrective RAG is doing automatically.

\---------------------------------------------------

**4. CRAG step-by-step**



Let's take a procurement example. User asks: "Why did procurement spend increase this month?"



**Step 1 — Retrieve**



Vector DB retrieves:

Document 1:Supplier ABC increased material price by 12%.

Document 2:Procurement spend decreased in 2024.

Document 3:Supplier XYZ contract renewal process.

\-------------------------

**Step 2 — Evaluate retrieval**



CRAG evaluates each document against the question.



Question: Why did procurement spend increase this month?

Document 1:Supplier ABC increased price by 12%.

Relevant? → YES



Document 2:Procurement spend decreased in 2024.

Relevant? → NO



Document 3:Supplier XYZ contract renewal.

Relevant? → MAYBE

\-----------------------

**Step 3 — Remove bad information**



CRAG can discard irrelevant information: Document 2 ❌

and retain useful evidence: Document 1 ✅

\----------------------

**Step 4 — Correct retrieval if necessary**



Suppose the remaining information isn't sufficient. The system can perform another retrieval/search with an improved query, depending on the CRAG implementation.



Original: Why did procurement spend increase this month?

Improved retrieval query:supplier price changes procurement spend current month



It might retrieve: Supplier ABC price +12%,Purchase quantity +18%,Transportation cost +5%

\---------------------

**Step 5 — Generate answer**



Now the LLM receives higher-quality context: Supplier ABC price increased 12%.,Purchase quantity increased 18%.,Transportation costs increased 5%.



and generates: Procurement spend increased primarily because purchase volume rose by 18%, while Supplier ABC's prices increased by 12%. Transportation costs also increased by 5%. Much safer than generating from the original noisy retrieval.

\------------------------------------------------------



> Corrective RAG, or CRAG, is an advanced RAG approach that evaluates the relevance or quality of retrieved documents before giving them to the LLM. If the retrieved context is relevant, it is used for generation. If it is irrelevant or insufficient, the system performs corrective actions such as filtering bad documents, rewriting the query, retrieving again, or using another source. This reduces hallucinations caused by poor retrieval.



> For example, if a user asks about maternity leave but the vector search retrieves sick-leave documents, CRAG detects that the retrieved context is irrelevant and performs another retrieval instead of blindly sending it to the LLM.

