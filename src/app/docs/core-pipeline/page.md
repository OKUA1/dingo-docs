---
title: Pipeline
nextjs:
  metadata:
    title: Pipeline
    description: Definition of a pipeline.
---

A pipeline is a sequence of building blocks that process data in a pre-defined order. 

---
## Defining a pipeline

There are two ways to create a pipeline in Dingo:

1. Using the instance of the `Pipeline` class and `add_block()` method.

```python
from agent_dingo.core.blocks import PromptBuilder, Pipeline
from agent_dingo.llm.openai import OpenAI
from agent_dingo.core.message import UserMessage

builder = PromptBuilder([UserMessage("What is the capital of {country}.")])
llm = OpenAI("gpt-3.5-turbo")

pipeline = Pipeline()
pipeline.add_block(builder)
pipeline.add_block(llm)
```

2. Using the `>>` as a shorthand.

```python
builder = PromptBuilder(...)
llm = OpenAI(...)
pipeline = builder >> llm
```

---

## Running a pipeline and parsing the output

To run a pipeline, call the `.run()` method with the input data. For this pipeline we do not pass the chat history, so the initial state is `None`:

```python
pipeline.run(None, country="France")
```

which is equivalent to:

```python
pipeline.run(country="France")
```

The output of the pipeline is a tuple containing the string response and the usage statistics.

```bash
('The capital of France is Paris.',
 {'prompt_tokens': 14, 'completion_tokens': 7, 'total_tokens': 21})
```

Each time run is called, the pipeline will instantiate a fresh context (from kwargs) and store (empty) objects. The pipeline will then call the `.forward()` method of each block in the pipeline in the order they were added. The output of each block is passed as input to the next block. The final output of the pipeline is the output of the last block processed by the [output parser](/docs/advanced-guides-output-parsers).

---

## Conversational pipelines

Earlier we mentioned that the initial state of the pipelines can contain the chat history (in a form of a `ChatPrompt` object). This is useful for conversational pipelines, which are used for multi-turn conversations (e.g. chatbots). For example, let's create one of the simplest pipelines that consists of a single LLM:

```python
from agent_dingo.llm.openai import OpenAI
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage, AssistantMessage

messages = [
    UserMessage("What is the capital of France?"),
    AssistantMessage("The capital of France is Paris."),
    UserMessage("How large is it?"),
]


llm = OpenAI("gpt-3.5-turbo")
pipeline = llm.as_pipeline()
pipeline.run(ChatPrompt(messages))
```

An important observation is that Dingo does not have a persistent state. This means that the initial state of the pipeline is defined solely by the input data passed to the `.run()` method and is reset during each run. This is different from some other frameworks where the state can be stored between the calls. After reading the section about the [custom blocks](/docs/custom-blocks-overview), you will see that Dingo is flexible enough to implement such behavior if needed, however, we strongly encourage you to avoid it. We believe that the stateless design is much easier to reason about and debug, whereas the state management is out of the scope of the framework and should be handled externally whenever possible.
