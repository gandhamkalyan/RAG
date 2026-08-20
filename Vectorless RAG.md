Vectorless RAG



> RAG without relying on embeddings + a vector database as the primary way to find relevant information.

> Instead, it can retrieve information using things like keyword/BM25 search, SQL queries, metadata filters, knowledge graphs, full-text search, or LLM-based document selection.

> We will be having the summary of the page in the Nodes



> A JSON Tree Index is simply the tree created by the Tree Builder stored in JSON format.

> JSON Tree Index = an intelligent Table of Contents stored as JSON, telling the system where different information exists in the original documents.



> Whenever a user query comes , the LLM will be giving the context as entire json tree index



It usually stores topics, subtopics, summaries/descriptions, and references/locations to the original content.



Let's say we have PDF Document



**What does the LLM Tree Builder do?**



Suppose the original document contains 1,000 chunks/pages/sections. The LLM analyzes the document structure and organizes it hierarchically.



&#x20;                   Employee Handbook

&#x20;                          │

&#x20;         ┌────────────────┼─────────────────┐

&#x20;         ↓                ↓                 ↓

&#x20;     HR Policies       Benefits           Travel

&#x20;         │                │                 │

&#x20;    ┌────┼────┐       ┌───┼────┐       ┌────┴────┐

&#x20;    ↓    ↓    ↓       ↓   ↓    ↓       ↓         ↓

&#x20;  Leave WFH Attendance Health Life Retirement Domestic International

&#x20;    │

&#x20;┌───┼────────┐

&#x20;↓   ↓        ↓

Sick Casual Maternity



Each node can contain information such as: Node Name: Maternity Leave



Summary: Contains information about maternity leave eligibility, duration, salary, extension and return-to-work rules.

Children: Eligibility,Duration,Salary,Extension



> So the LLM has created a map of the knowledge base.

\-------------------------------------------

> What if my pdf/documents don't have any Table of Content,then the LLM itself reads pages, infer headings + structure what it means is basically LLM will be reading all the pages it will be dividing like sections 

> LLM summarizes each section , Generates node\_id,title,page,summary and finally it assemble the hierarchical tree(Parent>child>grand child nodes)



Retrieval Process



> First step is to send the user query



Step 1 : Read the tree index , LLM scans titles,pages,summaries in content

Step 2 : Reason and select the node Returns thinking + node\_list JSON

Step 3 : Extract section content ,Fetch raw pages for selected node\_ids

step 4 :Sufficient to answer(LLM Evaluates completeness) , if No then loops to Step 2.If YES Generate the Answer(cited by section title + page)





Traditional RAG : Chunk > Embed > Cosine Similarity > Retrieve

PageIndex RAG : Build Tree > LLM Reasons over tree > Retrieve exact sections



Problem with vector Rag is 

Simialrity not equals to Relevance





