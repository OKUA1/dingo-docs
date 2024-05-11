---
title: Store
nextjs:
  metadata:
    title: Store
    description: Definition of a store.
---

Store is a mutable object that is also directly accessible by each block in the pipeline. It allows to store KVData, ChatPrompts, or any other arbitrary data. At the begining of each run, an empty store is created which is then remains accessible for the duration of the pipeline run.

<!-- --- -->
```python
from agent_dingo.core.state import Store, ChatPrompt, KVData

prompt = ChatPrompt([])
kvdata = KVData(k="v")
num = 1234

# data is automatically dispatched based on the type
store = Store()
store.update("key", prompt)
store.update("key", kvdata)
store.update("key", num)
assert store.get_prompt("key") is prompt
assert store.get_data("key") is kvdata
assert store.get_misc("key") == num
```
