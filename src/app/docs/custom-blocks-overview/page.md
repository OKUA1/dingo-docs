---
title: Building custom blocks
nextjs:
  metadata:
    title: Building custom blocks
    description: Definition of different types of custom blocks.
---

It is intended that developers heavily rely on creating custom blocks tailored to their specific use cases. To facilitate this process, Dingo blocks are designed to be lightweight and with minial abstractions over the underlying logic, which allows for maximum flexibility and control over the pipeline.

Depending on the complexity of the task, a custom block can be as simple as a single function or as complex as a multi-step pipeline. The only requirement is that the block should be able to accept `State`, `Context` and `Store` objects as input and return a `State` object as output.

---

## Inline blocks

The simplest way to create a custom block is by defining a function with the required signature and wrapping it with the `@InlineBlock` decorator.

```python
from agent_dingo.core.blocks import InlineBlock
from agent_dingo.core.state import State, Context, Store, KVData

@InlineBlock(required_context_keys=["key1"])
def my_custom_block(state: State, context: Context, store: Store) -> State:
    return KVData(_out_0=context["key1"])

pipeline = my_custom_block.as_pipeline()
print(pipeline.run(key1="some_value"))
```

In this example, the `my_custom_block` function extracts the value of `key1` from the `Context` and returns it inside a `State` (`KVData`) object. The `@InlineBlock` decorator takes the function and transforms it into an instance of a `Block`, which can be used in a pipeline like any other block.

Notice that the `@InlineBlock` decorator requires the `required_context_keys` argument, which specifies the keys that the block expects to find in the `Context`. This information gets passed to the pipeline, such that it can validate the input before running.

---

## Class-based blocks

This `@InlineBlocks` approach demonstrated in the previous section is mostly suitable for small, self-contained blocks that do not require any external dependencies. For more complex scenarios, it might be beneficial to define a block as a class. This allows for more flexibility in terms of initialization, configuration, and reuse.

Even when using the class-based approach, the block interface remains simple: the class should implement the `forward` method, which accepts `State`, `Context`, and `Store` objects as input and returns a `State` object as output. In addition, a `get_required_context_keys` method should be implemented to specify the required context keys.

```python
from agent_dingo.core.blocks import Block
from agent_dingo.core.state import State, Context, Store, KVData

class MyCustomBlock(Block):

    def get_required_context_keys(self):
        return ["key1"]

    def forward(self, state: State, context: Context, store: Store) -> State:
        return KVData(_out_0=context["key1"])

my_custom_block = MyCustomBlock()

pipeline = my_custom_block.as_pipeline()
pipeline.run(key1="some_value")
```
