---
title: Context
nextjs:
  metadata:
    title: Context
    description: Definition of a context.
---

Context allows to pass non-chat arguments to the pipeline. Unlike the initial state, that gets rewritten by each block in the pipeline, the context is immutable and can be accessed directly by each block.

```python
from agent_dingo.core.blocks import PromptBuilder
from agent_dingo.core.message import UserMessage
from agent_dingo.core.state import KVData, Store, Context

context = Context(country="France")
builder = PromptBuilder([UserMessage("What is the capital of {country}?")])
print(builder.forward(None, context, Store()))
```
