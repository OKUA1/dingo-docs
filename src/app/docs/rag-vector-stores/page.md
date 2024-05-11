---
title: Vector stores
nextjs:
  metadata:
    title: Vector stores
    description: Definition of RAG vector stores.
---

The vector store is responsible for storing the embeddings of the chunks and retrieving the most relevant chunks for a given query. Every vector store in Dingo must be a subclass of the `agent_dingo.rag.base.BaseVectorStore` class. The vector store should implement two methods:

- `upsert_chunks`: which takes a list of (embedded) chunks and stores them in the database.
- `retrieve`: which takes an embedded query and returns a list of the most relevant chunks.

---

Currently, Dingo supports two major open-sorce vector databases: [Qdrant](https://qdrant.tech/) and [Chroma DB](https://www.trychroma.com/).

```python
from agent_dingo.rag.vector_stores.qdrant import Qdrant

chunks = [...] # list of embedded chunks

query = "query"
query_embedding = embedder.embed([query])[0]

vector_store = Qdrant(collection_name="collection_name", embedding_size=384)
vector_store.upsert_chunks(chunks)
retrieved_chunks = vector_store.retrieve(k = 1, query = query_embedding)
print(retrieved_chunks)
#RetrievedChunk(content='...', document_metadata={'source': 'file.docx', 'paragraph': 7}, score=0.913)
```

Supported parameters:

- `agent_dingo.rag.vector_stores.qdrant.Qdrant`:
  - `collection_name`: the name of the collection in Qdrant.
  - `embedding_size`: the size of the embeddings.
  - `host`: the host of the Qdrant server, by default is None.
  - `port`: the port of the Qdrant server, by default is None.
  - `path`: the path of the Qdrant database (in case of local instance), by default is None, in which case the database is stored in memory.
  - `url`: the full url of the Qdrant hosted qdrant service, by default None.
  - `api_key`: the api key for the Qdrant service, by default None.
  - `recreate_collection`: a boolean flag that determines whether to recreate the collection if it already exists, by default is False.
  - `upsert_batch_size`: the number of chunks to upsert in a single batch, by default is 32.
- `agent_dingo.rag.vector_stores.chromadb.ChromaDB`:
  - `collection_name`: the name of the collection in ChromaDB.
  - `path`: the path of the ChromaDB database, by default is None.
  - `host`: the host of the ChromaDB server, by default is None.
  - `port`: the port of the ChromaDB server, by default is None.
  - `recreate_collection`: a boolean flag that determines whether to recreate the collection if it already exists, by default is False.
  - `upsert_batch_size`: the number of chunks to upsert in a single batch, by default is 32.
