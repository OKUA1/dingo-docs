---
title:  LangChain tools overview 🦜️🔗 
nextjs:
  metadata:
    title: Agents overview
    description: Definition of agents.
---

It is possible to convert [Langchain Tools](https://python.langchain.com/docs/modules/agents/tools/) into dingo-compatible functions (referred to as function descriptors) in order to register them with Dingo. The converter can be used as follows:

1. Install langchain:

```bash
pip install agent_dingo[langchain]
```

2. Define the tool, we will use the Wikipedia tool as an example:

```python
from langchain.tools.wikipedia.tool import WikipediaQueryRun
from langchain.utilities.wikipedia import WikipediaAPIWrapper
tool = WikipediaQueryRun(api_wrapper = WikipediaAPIWrapper())
```

Please refer to the [LangChain documentation](https://python.langchain.com/docs/modules/agents/tools/) for more details on how to define the tools.

3. Convert the tool into a function descriptor and register:

```python
from agent_dingo.agent.langchain import convert_langchain_tool
descriptor = convert_langchain_tool(tool)
agent.register_descriptor(descriptor)
```