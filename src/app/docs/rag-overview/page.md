---
title: RAG overview
nextjs:
  metadata:
    title: RAG overview
    description: Definition of RAG technique.
---

Retrieved Augmented Generation (RAG) technique can be used to overcome some of the limitations of LLMs such as high frequency of hallucinations or incomplete knowledge base. RAG allows to retrieve relevant information from a database and include it into the prompt. This way, the model can be guided towards generating more accurate and relevant responses.

The basic stages of RAG are ingestion (performed once or periodically), and retrieval (performed at each inference step).

The ingestion stage involves reading the documents from various sources, chunking their content into smaller pieces, vectorizing the chunks, and storing them in a vector store.

The retrieval stage involves embedding the query (which is derived from the prompt), and finding the most relevant chunks in the vector store. The retrieved chunks are then concatenated with the prompt and fed into the model.

There are multiple variations of RAG, each addressing either a specific component of RAG pipeline (e.g. improved chunking) or a pipeline as a whole (e.g. re-ranking, generator-enhanced retrieval, etc.). The list of RAG variations is continuously expanding, with new advanced techniques being introduced regularly. At the moment, there is no a universal approach, and the choice of the technique is heavily dependent on the specific use case.

Dingo provides a baseline implementation of RAG, which can be used as a starting point for further customization. In the future, we plan to extend the RAG support, but following the core principles of Dingo, the number of pre-built components will be kept to a minimum and include only widely used and well-tested techniques.
