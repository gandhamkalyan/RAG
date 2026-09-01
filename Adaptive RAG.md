Adaptive RAG first understands the question, then decides which retrieval strategy is needed.Adaptive RAG chooses the most appropriate retrieval method of each query,balancing speed,and accuracy.

Not every question needs the same RAG pipeline.
Think: Adaptive RAG = "Choose the right path based on the question."

**1. Why do we need Adaptive RAG?**

Imagine you're building a company AI assistant.

> Users ask three questions: Question 1: "What is 2 + 2?"
Do we need Vector DB? ❌ No. The LLM can answer directly.

> Question 2: "What is our company's maternity leave policy?"
Need company documents? ✅Search the Vector DB.


> Question 3:"What is our leave policy, and how does it compare with the latest government regulation?"
Now we may need: Company Vector DB,Web/latest source,Multiple retrieval steps

So why send every question through exactly the same pipeline? Adaptive RAG solves this.

------------------------------------------

Normal RAG: User Question > Vector Search > Top 3 chunks > LLM > Answer

Adaptive RAG : 

First classify/route the question:

                  User Question

                        ↓

                  Query Analyzer

                         ↓

            "What type of question?"

                         ↓

           ┌────────────┼────────────┐

            ↓            ↓            ↓

          Simple      Knowledge      Complex

            ↓            ↓            ↓
        LLM Direct    Vector DB    Multi-step

                                     Retrieval

            ↓            ↓            ↓

            └────────────┼────────────┘

                        ↓

                       Answer
--------------------------------------------------------
**What can Adaptive RAG adapt?**

It's not limited to deciding RAG vs no RAG. It can adapt several things.
For example: User Question > Query Analyzer / Router > Decides:

1 Do I need retrieval?
2.Which data source?
3.Which retrieval technique?(Semantic Search,Key Word Search, Hybrid Search)
4.How many documents?
5.Do I need multiple retrieval steps?
---------------------------------------------------

> Once the query analysis redirected to retriever(db) we are grading the information that is coming from the retriever whether it is related to particular query. If it is reated then go ahead and generate the content .we can also check the hallucination.If it is hallucination then we ask to regenrate .if it is no then will go ahead and get the answer .we can ask or tell the LLm to compare the answer wether it is accurate or not if it is not then it will entirely redirect to retriever 





