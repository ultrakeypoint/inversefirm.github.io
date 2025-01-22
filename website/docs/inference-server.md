---
sidebar_position: 3
---

# Inference Server

As well as the In-Process SDK, LLMBoost has an Inference Server option that can be started using the `llmboost` command line option.

## Deployment

There are multiple deployment options:

### 1. Single server (`llmboost serve`)

This will run a single application server.

```
llmboost serve --model_path /models/models/Meta-Llama-3.1-8B-Instruct-FP8-KV --port 8011 --dp 8
```

### 2. Multi model deployment (`llmboost deploy`)

LLMBoost supports the deployment of multiple models on a multi-GPU server. To use this feature, you'll need a configuration file that specifies the deployment details for each model. The configuration options are consistent with those used in LLMBoost benchmark runs. An example configuration file is shown below:

```
common:
  kv_cache_dtype: auto
  host: 127.0.0.1

models:
  - model_name: Llama-3.2-1B-Instruct
    model_path: /models/models/Meta-Llama-3.1-8B-Instruct-FP8-KV
    port: 8011
    tp: 1
    dp: 2

  - model_name: Llama-3.1-70B-Instruct
    model_path: /models/models/Llama-3.1-70B-Instruct
    port: 8012
    tp: 2
    dp: 2
```

Finally, you can initiate the deployment by running :

```
llmboost deploy --config /workspace/examples/config/sample_config.yaml
```

#### Check deployment status

You can check the LLMBoost instances status by running `llmboost status`, and will get similar output as below:

```
+------+------------------------+---------+
| Port |          Name          |  Status |
+------+------------------------+---------+
| 8011 | Llama-3.2-1B-Instruct  | running |
| 8012 | Llama-3.1-70B-Instruct | running |
+------+------------------------+---------+
```

#### Run Server-side Benchmark

After deploying the models, you can run a benchmark on each model by executing the following command:

```
llmboost benchmark --port 8011 --num_prompts 100 --input_len 128 --output_len 128 --output_file /workspace/benchmarks/mi300/bench_result.csv 
```

Explanation: Execute a benchmark on the server that is running on port `8011` by sending `100` prompts, each having an input and output length of `128` tokens, and save the results in `/workspace/benchmarks/mi300/bench_result.csv`.



#### Shutdown instance

You can run `llmboost shutdown --port XXXX` to delete a specific instance. Or, you can use `llmboost shutdown --all` to shutdown all instances in the current server.

## Interactive client

Once you have an LLMBoost instance up and running, you can use `llmboost client` to connect to it.

```
llmboost client --port 8011
```
