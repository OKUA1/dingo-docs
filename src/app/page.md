---
title: What is Agent Dingo?
---

<!-- Dingo is a lightweight microframework designed for streamlining the development of LLM pipelines and autonomous agents. {% .lead %} -->
Dingo is a lightweight microframework designed for streamlining the development of LLM pipelines and autonomous agents. It allows developers to start small and easily add more complex functionalities as needed. 

{% quick-links %}

{% quick-link title="Core" icon="installation" href="/docs/core-overview" description="Get to know the main concepts of the library." /%}

{% quick-link title="LLMs" icon="presets" href="/docs/llms-overview" description="Familarize yourself with the supported large language model providers." /%}

{% quick-link title="Agents" icon="plugins" href="/docs/agents-overview" description="Get an overview of how the agents can be created and utilized." /%}

{% quick-link title="Serving" icon="theming" href="/docs/serving-overview" description="Learn how to serve any pipeline as an OpenAI-like API." /%}

{% /quick-links %}

## Motivation

Unlike "batteries included" libraries like LangChain and LlamaIndex, which provide a vast collection of pre-built components, Dingo focuses on a small core set of building blocks. This design choice was based on several considerations:

- **Simplified Maintenance:** With a smaller codebase, maintaining Dingo becomes a more manageable task. This ensures that the framework remains up-to-date despite frequent and often backward-incompatible changes introduced by LLM providers and third-party libraries.
- **Rapid Prototyping:** Dingo's core building blocks are well suitable for fast initial experimentation. This allows developers to quickly establish a proof of concept without being overwhelmed by an extensive quantity of options and configurations.
- **Complex Tasks Require Customization:** When developing complex LLM applications, developers often need to customize the behavior of individual components, even when using the most feature-complete frameworks. By providing a minimalistic and easily extendable core, Dingo encourages developers to extend and modify the framework to meet their specific requirements rather than offering a one-size-fits-all solution.
- **Optimized Code Size:** Dingo strives to achieve an optimal balance between functionality and number of lines of code.

| Library                      | Total Lines of Code |
| ---------------------------- | ------------------- |
| Dingo                        | ~1 800              |
| LangChain / LangChain Core   | ~271 000 / ~37 000  |
| LlamaIndex / LlamaIndex Core | ~232 000 / ~52 000  |

Dingo consists of five main modules, each responsible for a specific aspect of LLM pipeline development:

- **Core:** provides the fundamental building blocks for constructing LLM pipelines.
- **LLMs:** acts as a wrapper, offering a unified interface for interacting with various LLM providers. This module simplifies the process of switching between different LLMs by abstracting away provider-specific APIs.
- **Agents:** allows developers to extend the capabilities of LLMs by incorporating custom functions. This module enables the creation of autonomous agents capable of performing complex tasks beyond the scope of traditional LLMs.
- **RAG:** allows to retrieve relevant information from external sources and seamlessly integrate it into prompts, providing LLMs with a richer knowledge base to draw from.
- **Serving:** facilitates the deployment of Dingo pipelines as web services. This enables developers to expose the apps they built as OpenAI compatible REST APIs.
