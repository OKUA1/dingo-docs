---
title: Reader
nextjs:
  metadata:
    title: Reader
    description: Definition of RAG reader.
---

The reader is responsible for preparing the documents for ingestion. Every reader in Dingo is a subclass of the `agent_dingo.rag.base.BaseReader` class. The reader should implement the `read` method, which takes a source identifier (e.g. a URL, a file path, etc.) and returns a list of documents. The document is an instance of the `agent_dingo.rag.base.Document` class, which contains the `content` as a string and the `metadata` as a (non-nested) dictionary.

---

## List reader

The `ListReader` is a simple reader that reads the documents from a list of strings. It is useful for the cases when the content is already pre-loaded into memory.

```python
from agent_dingo.rag.readers.list import ListReader

reader = ListReader()
docs = reader.read(["Document 1", "Document 2"])
print(docs)
# [Document(content='Document 1', metadata={'source': 'memory'}), Document(content='Document 2', metadata={'source': 'memory'})]
```

---

## PDF reader

The `PDFReader` reads the documents from a PDF file. It uses `PyPDF2` library to extract the text from the PDF file and returns a list of documents corresponding to the pages of the PDF.

```python
from agent_dingo.rag.readers.pdf import PDFReader

reader = PDFReader()
docs = reader.read("path/to/pdf/file.pdf")
print(docs)
# [Document(content='...' metadata={'source': 'file.pdf', 'page': 1})]
```

---

## Webpage reader

The `WebpageReader` reads the documents from a webpage. It uses the `requests` library to download the webpage and the `beautifulsoup4` library to extract the text from the HTML content.

```python
from agent_dingo.rag.readers.web import WebpageReader

reader = WebpageReader()
docs = reader.read("https://en.wikipedia.org/wiki/Python_(programming_language)")
print(docs)
# [Document(content='...' metadata={'source': 'https://en.wikipedia.org/wiki/Python_(programming_language)'})]
```

---

## Word reader

The `WordDocumentReader` reads the documents from a Word file. It uses the `python-docx` library to extract the text from the Word file and returns a list of documents corresponding to the paragraphs of the Word document.

```python
from agent_dingo.rag.readers.word import WordDocumentReader

reader = WordDocumentReader()
docs = reader.read("path/to/word/file.docx")
print(docs)
# [Document(content='...' metadata={'source': 'file.docx', 'paragraph': 1})]
```
