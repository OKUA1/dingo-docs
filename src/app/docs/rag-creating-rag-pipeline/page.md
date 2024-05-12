---
title: Creating a RAG pipeline
nextjs:
  metadata:
    title: Creating a RAG pipeline
    description: Learn how to build a RAG pipeline.
---

A basic RAG pipeline includes two main steps: the indexing step, which takes place during the application build phase and involves creating a vector store of embedded document chunks, and the retrieval step, which happens when the application is running. `RAGPromptModifier` is responsible for integrating all RAG components into a unified pipeline. It embeds user's query during retrieval, extracts the most relevant document chunks from the vector store, and appends them to a dedicated location in a prompt.

```python
from agent_dingo.llm.openai import OpenAI
from agent_dingo.rag.prompt_modifiers import RAGPromptModifier
from agent_dingo.core.message import UserMessage, SystemMessage
from agent_dingo.core.state import ChatPrompt

###### STEP 1: INDEXING ######

# Read and chunk the documents
chunks = ...

# Initialize an embedding model and embed the chunks
embedder = ...
embedder.embed_chunks(chunks)

# Initialize a vector store and populate it with embedded chunks
vector_store = ...
vector_store.upsert_chunks(chunks)

###### STEP 2: RETRIEVAL ######

# Initialize an LLM
llm = OpenAI(model="gpt-4-turbo")

# Create a RAG pipeline
rag = RAGPromptModifier(embedder, vector_store)
pipeline = rag>>llm

# A sample user's query
messages = [
    SystemMessage("You are a helpful assistant."),
    UserMessage("How many parameters does Phi-3-mini model from Microsoft have?")
]
# Run the pipeline
print(pipeline.run(ChatPrompt(messages)))
# ('retrieved chunk ...', {'prompt_tokens': 800, 'completion_tokens': 16, 'total_tokens': 816})
```

Parameters supported by `agent_dingo.rag.prompt_modifiers.RAGPromptModifier`:

- `embedder`: the embedder to use, a subclass of the `agent_dingo.rag.base.BaseEmbedder` class.
- `vector_store`: the vector store to use, a subclass of the `agent_dingo.rag.base.BaseVectorStore` class.
- `n_chunks_to_retrieve`: the number of the most similar to the user's query chunks to retrieve, by default is 5.
- `retrieved_data_location`: the place where to append the retrieved data to, can be `user` or `system` (message), by default `system`.
- `rag_template`: a custom string template with keys `original_message` and `documents`, by default None.
