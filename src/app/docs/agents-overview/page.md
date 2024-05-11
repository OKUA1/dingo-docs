---
title: Agents overview
nextjs:
  metadata:
    title: Agents overview
    description: Definition of agents.
---

`Agent` is a block which wraps an `LLM` and allows to register external functions to be invoked autonomously.

---

## Function Registration

The function can be registered with a single line of code using the `agent.function` decorator, which generates the function descriptor from the docstring.

```python
from agent_dingo.agent import Agent
from agent_dingo.llm.openai import OpenAI
import requests

llm = OpenAI(model="gpt-3.5-turbo")
agent = Agent(llm, max_function_calls=3)

@agent.function # equivalent to `agent.register_function(get_temperature)`
def get_temperature(city: str) -> str:
    """Retrieves the current temperature in a city.

    Parameters
    ----------
    city : str
        The city to get the temperature for.

    Returns
    -------
    str
        String representation of the json response from the weather api.
    """
    base_url = "https://api.openweathermap.org/data/2.5/weather"
    params = {
        "q": city,
        "appid": "<openweathermap_api_key>",
        "units": "metric"
    }
    response = requests.get(base_url, params=params)
    data = response.json()
    return str(data)

pipeline = agent.as_pipeline()
...

```

{% callout title="Note" %}
Currently only the numpy style docstrings are supported.
{% /callout %}

If the function does not have a docstring, it can still be registered. In that case, the docstring will be generated automatically by the LLM.

However, there are several drawbacks of this approach:

- The source code of your function is passed to the model;
- The generated docstring might be inaccurate (especially for complex functions);
- The generated docstrings are not persistant (i.e. they are not saved to disk) and will have to be regenerated every time the function is registered.

If you wish to disable the automatic docstring generation, you can set the `allow_codegen` parameter to `False` when instantiating the agent.

By default the `allow_codegen` parameter is set to `"env"` which means that the value is read from the `DINGO_ALLOW_CODEGEN` environment variable. If the variable is not set, it is assumed to be `True`.

Overall, it is recommended to provide the docstrings manually to ensure the best performance.

---

## Multiple Agents / Sub-agents

Each agent can be used as a function descriptor, which allows to create a hierarchy of agents. This can be useful when you want to create several specialized agents that can be used from a manager agent when appropriate.

```python
from agent_dingo.core.message import UserMessage, SystemMessage, AssistantMessage
from agent_dingo.core.state import State, ChatPrompt, KVData, Context, Store
from agent_dingo.core.blocks import (
    PromptBuilder,
)
from agent_dingo.agent import Agent
from agent_dingo.llm.openai import OpenAI


llm = OpenAI(model="gpt-3.5-turbo")


messages = [
    SystemMessage("You are a helpful assistant. Provide concise anwers."),
    UserMessage("What is the weather in {city}?"),
]

qa_prompt = PromptBuilder(messages)

agent = Agent(
    llm,
    max_function_calls=2,
    name="manager",
    description="An agent with access to assisting agents.",
)

another_agent = Agent(
    llm,
    max_function_calls=2,
    name="another_agent",
    description="An agent with access to weather forecast data.",
)


@another_agent.function
def get_temperature(city: str) -> str:
    """Retrieves the current temperature in a city.

    Parameters
    ----------
    city : str
        The city to get the temperature for.

    Returns
    -------
    str
        str representation of the json response from the weather api.
    """
    return '{{"temperature"}}:"20"'  # fake response


agent.register_descriptor(another_agent.as_function_descriptor())


pipeline = qa_prompt >> agent

print(pipeline.get_required_context_keys())
print(pipeline.run(city="Berlin"))

```

## Compatibility

Currently, `LLM` in Dingo do not have a standardized interface for function calling. Hence, Dingo relies on lower-level API to interact with the model and is not compatible with all LLMs. This is a known limitation and will be addressed in the future. Currently, Dingo is compatible with the following LLMs:

- OpenAI
- Gemini (VertexAI)