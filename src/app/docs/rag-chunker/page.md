---
title: Chunker
nextjs:
  metadata:
    title: Chunker
    description: Definition of a chunker.
---

The chunker is responsible for splitting the documents into smaller chunks. Every chunker in Dingo must be a subclass of the `agent_dingo.rag.base.BaseChunker` class. The chunker should implement the `chunk` method, which takes a list of documents and returns a list of chunks. The chunk is an instance of the `agent_dingo.rag.base.Chunk` class, which contains the content as a string, a reference to the parent document, and the embedding as a list of floats (initially `None`).

---

Dingo provides a simple `RecursiveChunker` that splits the document recursively until the specified chunk size is reached.

```python
from agent_dingo.rag.chunkers.recursive import RecursiveChunker

chunker = RecursiveChunker(chunk_size=512)
chunks = chunker.chunk(docs)
print(chunks)
# [Chunk(content='...', document=Document(content='...', metadata={'source': 'file.docx', 'paragraph': 1}), embedding=None), ...]
```

Supported parameters:

- `chunk_size`: the maximum number of characters in a chunk.
- `separators`: a list of strings that can be used as separators to split the document. By default is None, in which case `["\n\n", "\n", " ", ""]` are used.
- `keep_separator`: a boolean flag that determines whether to keep the separators in the chunks. By default is False.
- `merge_separator`: a string that is used to merge small chunks into larger ones. By default `" "`.