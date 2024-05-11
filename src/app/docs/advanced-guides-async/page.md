---
title: Running pipelines asynchronously
nextjs:
  metadata:
    title: Running pipelines asynchronously
    description: Overview of async.
---

When serving the pipeline, it will have multiple runs in parallel. In many cases, the most efficient resource utilization is achieved by running the pipeline (and all its underlying blocks) asynchronously. To achieve this, Dingo provides an `async_run` method that runs the pipeline asynchronously and in its turn, calls the `async_forward` method of the blocks.

---

## Async

By default, every block has an `async_forward` method that would simply call the `forward` method of the block in a separate thread. However, this method is not efficient and should be overridden by a proper async implementation.

Dingo already provides an asynchronous implementation for the standard blocks. Depending on the block, the implementation does the following:

- Runs the block asynchronously if the operation is I/O bound (e.g. calling remote LLM providers);
- Runs the block synchronously in the main thread if the operation is CPU bound and implemented in pure Python. Running such an operation in a separate thread would not make sense due to the Global Interpreter Lock (GIL).
- Runs the block asynchronously in a separate (single) thread if the operation is CPU bound, but not implemented in Python and releases the GIL.

```python
from agent_dingo.llm.openai import OpenAI
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage
import asyncio

llm = OpenAI(model="gpt-3.5-turbo")
pipeline = llm.as_pipeline()
prompt = ChatPrompt([UserMessage("Hello")])
print(asyncio.run(pipeline.async_run(prompt)))
```

---

## Async Custom Blocks

When creating a custom block via subclassing, the `async_forward` method must be implemented instead (or in addition to) the `forward` method.

```python
from typing import List
from agent_dingo.core.blocks import Block
from agent_dingo.core.state import KVData
import asyncio

class AsyncBlock(Block):
    def get_required_context_keys(self) -> List[str]:
        return []

    def forward(self, state, store, context):
        # optionally implement forward for synchronous execution
        pass

    async def async_forward(self, state, store, context):
        return KVData(result="Hello, World!")

block = AsyncBlock()
print(asyncio.run(block.async_forward(None, None, None)))
```

When using the `InlineBlock`, the async block can be defined simply by decorating an async function.

```python
from agent_dingo.core.blocks import InlineBlock
from agent_dingo.core.state import KVData

import asyncio


@InlineBlock(required_context_keys=[])
async def block(state, store, context):
    return KVData(result="Hello, World!")

print(asyncio.run(block.async_forward(None, None, None)))
```
