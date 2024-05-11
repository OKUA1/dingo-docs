---
title: LLMs overview
nextjs:
  metadata:
    title: LLMs overview
    description: Overview of LLM blocks.
---

LLMs are standardized blocks to interact with various external providers. They are designed to simplify the process of switching between different models by abstracting away provider-specific APIs.

In line with the core principles of Dingo, the number of integrations is kept to a minimum to ensure the framework remains lightweight and easy to maintain.

When adding a new integration, we mainly consider the following factors:

- Popularity: the model/provider should be widely used and recognized in the community.
- Chat mode support: the model should be able to generate text in a conversational manner since those models cover a wider range of use cases. Supporting only one type of models simplifies the burden of choosing the right one.
- Native function calling support: the model should be able to perform OpenAI-style function calls, so it can be used with [Dingo Agents](/docs/agents-overview).

At the moment, we have a first-class support for OpenAI GPT and Google Gemini as they are the most performant models meeting all of the criteria above. In addition to that, to allow for maximum flexebility we integrated Llama.cpp which can run most of the open-source models locally (both on CPU and GPU), and LiteLLM which provides an OpenAI-like interface to many online providers.

---

## Usage

Every LLM is a [`Reasoner`](/docs/core-blocks) block that expects a `ChatPrompt` input and returns a `KVData` state. The `ChatPrompt` can either be built during the pipeline run using `PromptBuilder` or passed as an initial state to the pipeline.

```python
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage, AssistantMessage, SystemMessage
from agent_dingo.llm.openai import OpenAI

llm = OpenAI("gpt-3.5-turbo")

messages = [
    SystemMessage("You are a helpful assustant. Answer the user's questions as clearly as possible."),
    UserMessage("What is the capital of France?"),
    AssistantMessage("The capital of France is Paris."),
    UserMessage("How large is it?"),
]

pipeline = llm.as_pipeline()

pipeline.run(ChatPrompt(messages))
```
