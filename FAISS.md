FAISS



> FAISS (Facebook AI Similarity Search) is a library for searching large collections of dense vectors/embeddings efficiently.



> The important thing is that FAISS doesn't have just one search technique. It provides different index types, and each index uses a different strategy to find nearest vectors



**1. Brute-Force Search — IndexFlatL2**



\- This is the simplest approach.

\- Suppose you have: 1 million vectors and the user gives: Query Vector

\- **FAISS compares the query against every vector.It uses L2/Euclidean distance.**



**Advantage :** Very accurate because it checks everything.

**Disadvantage :** Expensive when you have millions/billions of vectors.

**-----------------------------------**

**2.IVF — Inverted File Index**



* This is one of the most important FAISS techniques.
* IVF = Inverted File Index
* Instead of comparing the query against every vector, FAISS first divides vectors into clusters.



1,000,000 vectors



&#x20;       ↓



&#x20;     Clustering



&#x20;       ↓



┌──────┬──────┬──────┬──────┐

│ C1   │ C2   │ C3   │ C4   │

│250K  │200K  │300K  │250K  │

└──────┴──────┴──────┴──────┘



When a query arrives:



Query

&#x20; ↓

Find nearest clusters

&#x20; ↓

Search only those clusters

&#x20; ↓

Top K results



Instead of searching: 1,000,000 vectors you might search: 10,000–50,000 vectors depending on configuration.

\------------------------------------

**3.HNSW**



* Another very important technique is **HNSW — Hierarchical Navigable Small World.**



Instead of clustering vectors, HNSW creates a graph.



&#x20;            A

&#x20;          /   \\

&#x20;         B     C

&#x20;        / \\     \\

&#x20;       D   E     F

&#x20;            \\   /

&#x20;              G

Each vector is a node. Similar vectors are connected.



When searching:

