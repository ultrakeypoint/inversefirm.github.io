---
sidebar_position: 5
---

# Parallelism

LLMBoost supports multiple dimensions of parallelism, within a single node the two a user should consider are Tensor Parallelism(tp) and Data Parallelism(dp).

To achieve the best performance for your model you can experiment with the different level of parallelism, the general rules are:

1. `tp * dp = #number of GPUs in the system`
2. `dp = max(ceil( Total GPU Memory Size / (Model Size * 2) / ), #number of GPUs in the system)`

For example, to tune Llama3.1-8B-Instruct on the g6e.48large instance:

```
Total GPU Memory = 48G * 8 = 384
Model Size = 8 * 2 = 16
dp = max((16 * 2) / 384, 8) = max(12, 8) = 8
tp = 1
```

The values for `tp` and `dp` can be changed via the runtime instantiation `LLMBoost(..., tp=1, dp=1,...)`
