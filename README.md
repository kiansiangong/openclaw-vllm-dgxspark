# Self-Hosted OpenClaw + vLLM on DGX-Spark

Guide to running a self-hosted vLLM engine for OpenClaw on GB10-based hardware (single node).

**Tested on:** Nvidia DGX-Spark, MSI EdgeXpert, Dell GB10 ProMax

## Why vLLM over Ollama?

vLLM gives significantly higher throughput and lower latency. Ollama is easier to set up but doesn't perform as well under load. The GB10 architecture also requires a different setup environment — we use the community-built [spark-vllm-docker](https://github.com/eugr/spark-vllm-docker) container for this.

## Quick Start

### 1. Build the container

```bash
git clone https://github.com/eugr/spark-vllm-docker.git
cd spark-vllm-docker
./build-and-copy.sh
```

### 2. Add the model recipe

Copy the recipe into the build directory:

```bash
cp /path/to/qwen3.5-122b-int4-autoround.yaml recipes/openclaw-qwen3.5-122b-a10b-int4.yaml
```

### 3. Edit the recipe (if needed)

```bash
vi recipes/openclaw-qwen3.5-122b-a10b-int4.yaml
```

### 4. Launch the server

```bash
./run-recipe.sh openclaw-qwen3.5-122b-a10b-int4 --solo --setup
```

## OpenClaw Configuration

Once the server is running (default endpoint: `http://0.0.0.0:8000/v1`), update your OpenClaw config:

```bash
vi ~/.openclaw/openclaw.json
```

Merge the relevant blocks from [`openclaw-model-config.json`](openclaw-model-config.json) into your existing config. Don't replace the whole file — only add the `models.providers.vllm` and `agents.defaults` sections.

## Dashboard Settings

For full visual walkthrough, see the [AMD Developer Cloud Guide](https://www.amd.com/en/developer/resources/technical-articles/2026/openclaw-with-vllm-running-for-free-on-amd-developer-cloud-.html).

## Credits

- [spark-vllm-docker](https://github.com/eugr/spark-vllm-docker) — Docker setup for running vLLM on GB10 devices
- [Qwen3.5-122B-A10B](https://huggingface.co/Qwen) — Qwen team for the model, quantized via [Intel AutoRound](https://huggingface.co/Intel/Qwen3.5-122B-A10B-int4-AutoRound)
- [OpenClaw](https://github.com/openclaw)