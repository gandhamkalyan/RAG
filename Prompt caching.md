Prompt caching is an optimization technique used in LLM applications to avoid repeatedly processing or sending the same prompt/context when multiple requests reuse the same information.



1\. What is Prompt Caching?



Suppose your application sends this to an LLM:



System Prompt: You are an SAP Data Engineering assistant.



Knowledge:

SAP ECC is an ERP system...

Azure Data Factory is...

Microsoft Fabric is...



User: What is SAP ECC?



The next user asks:



User: How does SAP ECC connect to Azure?



A large part of the prompt may be identical: System Prompt + Instructions + Large knowledge/context

Instead of repeatedly processing the same information, we can cache the reusable portion.



Conceptually:



&#x20;               First Request

&#x20;                    ↓

&#x20;             Large Prompt

&#x20;                    ↓

&#x20;                   LLM

&#x20;                    ↓

&#x20;             Store reusable

&#x20;                context

&#x20;                    ↓

&#x20;                 CACHE



&#x20;               Second Request

&#x20;                    ↓

&#x20;             Same context

&#x20;                    ↓

&#x20;                  CACHE

&#x20;                    ↓

&#x20;           Only new information

&#x20;                    ↓

&#x20;                   LLM



This can reduce latency and cost, depending on the LLM/provider and how its caching mechanism works.

\------------------------------------------

**What exactly can be cached?**



Depending on the architecture/provider, you might cache things such as:



**> Static system instructions** :You are an SAP Data Engineer assistant...

**> Large static context :** Company policies,Product documentation,SAP documentation

**> Repeated prompt prefixes** : System prompt + Instructions + Few-shot examples

> **Previously computed results**

Sometimes applications also cache: Query → LLM response or: Query → Retrieved documents



These are related optimization techniques, but technically they aren't all the same thing as provider-level prompt caching.

\--------------------------------------

**Two approaches you mentioned :**  In-memory caching ,Database caching



**In-Memory Prompt Cache** : Store cached prompt/context in the application's memory/RAM.Now the application doesn't need to reconstruct/load the same prompt repeatedly.



Problem with In-Memory Cache



**The biggest limitation is:** Memory disappears when the application restarts.

\-----------------------

**Database Prompt Cache :** Instead of storing cached information only in RAM, you can store it in a persistent database.

\--------------------------

Important: **Prompt Cache vs Response Cache**



These are often confused.



**Prompt Cache** : Caches reusable input/context.



Prompt > Cache reusable portion > LLM



**Response Cache** : Caches the final answer.



Example:



User asks: "What is SAP ECC?"



First request:



Question

&#x20;↓

LLM

&#x20;↓

" SAP ECC is an ERP system..."

&#x20;↓

Cache answer



Second request: Same question >  Cache > Return answer

No LLM call may be necessary.



**Response caching can be dangerous**



Imagine:



User A : "What is my account balance?"



If you cache only: "What is my account balance?" and return the cached answer to User B, you could expose User A's data.



Therefore, cache keys need to account for things like: user\_id,permissions,tenant,query,version,context



For example: cache\_key =tenant\_id + user\_id +query + model\_version

This is especially important in enterprise applications.

