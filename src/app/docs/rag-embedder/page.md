---
title: Embedder
nextjs:
  metadata:
    title: Embedder
    description: Definition of a embedder.
---

The embedder is responsible for converting the chunks into vectors. Every embedder in Dingo must be a subclass of the `agent_dingo.rag.base.BaseEmbedder` class. The embedder should implement two methods:

- `embed`: which takes a list of strings and returns a list of embeddings.
- `embed_chunks`: which takes a list of chunks and modifies the embedding field of each chunk.

---

At the moment, Dingo provides open source `SentenceTransformer` embedder that uses the [Sentence Transformers](https://sbert.net/) library, as well as an `OpenAIEmbedder` that supports any provider that is compatible with a standard OpenAI REST API.

```python
from agent_dingo.rag.embedders.sentence_transformer import SentenceTransformer

embedder = SentenceTransformer(model_name="paraphrase-MiniLM-L6-v2")
embeddings = embedder.embed(["text 1"])
print(embeddings)
# [[...]]
```
Supported parameters:

- `agent_dingo.rag.embedders.sentence_transformer.SentenceTransformer`:
  - `model_name`: the model to use, by default is `paraphrase-MiniLM-L6-v2`.
  - `batch_size`: the number of samples passed in one request, by default is 128.

- `agent_dingo.rag.embedders.openai.OpenAIEmbedder`:
  - `model`: the model to use, by default is `text-embedding-3-small`.
  - `base_url`: the base URL of the provider. By default None, which means it will use the OpenAI API.
  - `dimensions`: the number of dimensions the resulting output embeddings should have, by default is None.