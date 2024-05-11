---
title: LLMs integrations
nextjs:
  metadata:
    title: LLMs integrations
    description: Overview of the use of various large language model providers.
---

Dingo supports several large language model providers.

---
## OpenAI

OpenAI supports any LLM provider that is compatible with a standard OpenAI REST API. By default, it is configured to work with the OpenAI itself.

To use the provider, you need to set the `OPENAI_API_KEY` environment variable to your OpenAI API key.

```python
import os
from agent_dingo.llm.openai import OpenAI
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage

os.environ["OPENAI_API_KEY"] = "your_openai_api_key"
gpt = OpenAI("gpt-3.5-turbo")

pipeline = gpt.as_pipeline()
prompt = ChatPrompt([UserMessage("Hello")])
pipeline.run(prompt)
```

Supported parameters:

- `model`: the model name to use.
- `temperature`: the sampling temperature. By default, it is set to 0.7.
- `base_url`: the base URL of the provider. By default None, which means it will use the OpenAI API.

---

## Gemini (VertexAI)

Gemini is a state-of-the-art model from Google available on VertexAI platform. To use it, you first need to set up the Google Cloud SDK and authenticate it with your Google Cloud account. To do that, follow the instructions [here](https://cloud.google.com/sdk/docs/install). After the SDK is installed, you can use the model directly by providing your project ID and the location of the model.

```python
from agent_dingo.llm.gemini import Gemini
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage

project = "dingo-123456" # replace with your project ID
location = "europe-west4" # replace with your location
gemini = Gemini(model="gemini-1.0-pro", project=project, location=location)

pipeline = gemini.as_pipeline()
prompt = ChatPrompt([UserMessage("Hello")])
pipeline.run(prompt)
```

Supported parameters:

- `model`: the model name to use.
- `project`: the Google Cloud project ID.
- `location`: the location of the model.
- `temperature`: the sampling temperature. By default, it is set to 0.7.

---

## Llama.cpp

[Llama.cpp](https://github.com/ggerganov/llama.cpp) is a C++ library based on [GGML](https://ggml.ai/) that allows to run local models converted to GGUF format. The GGUF models can be obtained from the [Hugging Face Hub](https://huggingface.co/models?sort=trending&search=GGUF).

```python
from agent_dingo.llm.llama_cpp import LlamaCPP
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage

model = "/path/to/model.gguf"
llama = LlamaCPP(model=model)

pipeline = llama.as_pipeline()
prompt = ChatPrompt([UserMessage("Hello")])
pipeline.run(prompt)
```

Supported parameters:

- `model`: the path to the GGUF model.
- `temperature`: the sampling temperature. By default, it is set to 0.7.
- `verbose`: whether to print additional information during the model execution. By default, it is set to False.
- `kwargs`: additional parameters to pass to the model initialization. You can find the full list of supported parameters in the [documentation of python bindings for Llama.cpp](https://llama-cpp-python.readthedocs.io/en/latest/api-reference/#high-level-api).

---

## LiteLLM

[LiteLLM](https://github.com/BerriAI/litellm) is an open-source library that provides an OpenAI-like interface to many online providers.

```python
import os
from agent_dingo.llm.litellm import LiteLLM
from agent_dingo.core.state import ChatPrompt
from agent_dingo.core.message import UserMessage

# Using LiteLLM with default Mistral AI
os.environ['MISTRAL_API_KEY'] = "<your-mistral-api-key>"
litellm = LiteLLM("mistral/mistral-tiny")

pipeline = litellm.as_pipeline()
prompt = ChatPrompt([UserMessage("Hello")])
pipeline.run(prompt)
```

Supported parameters:

- `model`: the model to use.
- `temperature`: the sampling temperature. By default, it is set to 0.7.
- `completion_extra_kwargs`: additional parameters to pass to the LiteLLM completion function.