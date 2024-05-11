---
title: Serving overview
nextjs:
  metadata:
    title: Serving overview
    description: Decription of serving.
---

Dingo natively supports serving any pipeline as an OpenAI-like API. The server can be started using the `serve_pipeline` function, which takes either a single pipeline (which will be used as a default model and named "dingo") or a dictionary of pipelines (where the keys are the model names).

```python
#server.py
from agent_dingo.core.state import State, ChatPrompt, KVData
from agent_dingo.core.blocks import *
from agent_dingo.core.message import *
from agent_dingo.serve import serve_pipeline
from agent_dingo.llm.openai import OpenAI

# If you are using a Jupyter Notebook:
# !pip install nest_asyncio
# import nest_asyncio
# nest_asyncio.apply()

llm = OpenAI(model="gpt-3.5-turbo")

translation_prompt_template = "Translate to {language}: {text}"

translation_builder = PromptBuilder(
    [UserMessage(translation_prompt_template)], from_state=["text"]
)

pipeline = llm >> translation_builder >> llm

pipeline_raw = llm.as_pipeline()

serve_pipeline(
    {"gpt35-translated": pipeline, "gpt35-raw": pipeline_raw},
    host="127.0.0.1",
    port=8000,
    is_async=True,
)
```

Arguments:

- `models`: a dictionary of pipelines, where the keys are to be used as model names, or a single pipeline (which will be used as a default model and named "dingo").
- `host`: the host to bind the server to.
- `port`: the port to bind the server to.
- `is_async`: defines whether to run the server in an asynchronous mode. In asynchronous mode, the server invokes the `async_run` method of the pipeline. To learn more about asynchronous mode of Dingo please refer to the [Async](/docs/advanced-guides-async) section.

Once the server is running, the pipeline can be accessed using the `openai` python package:

```python
#client.py
import openai

client = openai.OpenAI(base_url="http://localhost:8000")

messages = [
    {"role": "user", "content": "What is the capital of France?"},
    {"role": "context_language", "content": "French"}, # context keys are passed as custom roles with the "context_" prefix
]

out = client.chat.completions.create(messages=messages, model="gpt35-translated")
print(out)

out = client.chat.completions.create(messages=messages, model="gpt35-raw") # the language context will be ignored
print(out)

models = client.models.list()
print(models.models[0])

```

{% callout title="Note" %}
Dingo pipelines often have additional context keys that are part of the input. To make this behaviour compatible with the OpenAI api, context keys are passed as custom roles with the "context\_" prefix. In the example above, the context key "language" is passed as a custom role "context_language". It is irrelevant at which position the context keys are passed in the messages list.
{% /callout %}