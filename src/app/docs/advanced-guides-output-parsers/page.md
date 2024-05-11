---
title: Output parsers
nextjs:
  metadata:
    title: Output parsers
    description: Definition of output parsers.
---

As it was already covered in the previous sections, the output of each of the pipeline blocks can be either a `ChatPrompt` or a `KVData` object. At the same time, the output of the pipeline itself is a tuple containing two elements: the string response and the usage statistics. While the output statistic is collected automatically and does not require any additional configuration, the output response must be parsed into a string. This is where the output parsers come into play.

---

## How it works

The output parser needs to implement two methods: `_parse_chat` and `_parse_kvdata`, which are invoked automatically based on the type of the last block output. Both methods should return a string.

Both methods should be implemented even if only a single type of output is expected. The best practice is to unconditionally raise an exception in the method that is not expected to be called.

There are two ways to set the output parser for the specific pipeline:

1. By passing the parser instance to the `Pipeline` constructor.

```python
from agent_dingo.core.blocks import Pipeline

pipeline = Pipeline(..., output_parser=MyOutputParser())
```

2. By setting the `output_parser` attribute of the pipeline instance after its creation.

```python
block1, block2, block3 = ..., ..., ...

pipeline = block1 >> block2 >> block3

pipeline.output_parser = MyOutputParser()
```

---

## Default output parser

When the output parser is not set explicitly, the default output parser is used. The default parser expects the output to be a `KVData` object with a key `_out_0` containing the string response (which is the case for all LLMs and Agents). If the output is a `ChatPrompt` object, or the key `_out_0` is missing, the default parser raises an exception.
