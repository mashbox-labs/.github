<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="banner-dark.png">
    <img alt="MASHBOX Labs" src="banner-light.png" width="100%">
  </picture>
</p>

<h3 align="center">MASHBOX Labs makes open models run faster and cheaper on TPUs and accelerated hardware, as well as exploring the future of the intersection of AI and gaming.</h3>

<p align="center">
  <a href="https://github.com/mashbox-labs/mashbox"><b>mashbox</b></a> •
  <a href="https://mashbox.gg/docs"><b>Docs</b></a> •
  <a href="https://huggingface.co/mashbox-labs"><b>Hugging Face</b></a> •
  <a href="https://github.com/mashbox-labs/mashbox/blob/main/AGENTS.md"><b>For coding agents</b></a> •
  <a href="https://x.com/mashboxgg"><b>X</b></a>
</p>

<p align="center">
  <a href="https://github.com/mashbox-labs/mashbox/blob/main/LICENSE"><img alt="Licence: Apache-2.0" src="https://img.shields.io/badge/licence-Apache--2.0-blue"></a>
  <a href="https://github.com/mashbox-labs/mashbox"><img alt="TPU v6e" src="https://img.shields.io/badge/TPU-v6e-ff5a1f"></a>
  <a href="https://huggingface.co/mashbox-labs"><img alt="Hugging Face" src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-mashbox--labs-ffcc4d"></a>
</p>

We port open models to Google Cloud TPU, and anyone can run them. The table below lists every model in the current mashbox release and the chips it runs on. Start with **[mashbox](https://github.com/mashbox-labs/mashbox)**.

On a Google Cloud TPU VM (the [setup guide](https://mashbox.gg/docs/google-cloud) gets you there from a new Google account):

```bash
python3 -m pip install uv && python3 -m uv tool install --python 3.12 "mashbox[tpu]"
~/.local/bin/mashbox run qwen3-8-27b --prompt "Write a haiku about the sea."
```

### Models

<!-- models:start -->
**MASHBOX builds**

Models we port or make ourselves. A BSHJ build is our weights file for the model, run on our TPU kernels. Measured is each one's number against its baseline, on the chip named.

| Model | What it does | Measured | Chips |
|---|---|---|---|
| **[Hy-Nomas](https://github.com/mashbox-labs/mashbox/tree/v0.1.5/src/mashbox/models/hy_nomas)** by Tencent Hunyuan | text and movement keys in, video out | Compute per second of video, quality preset, Tencent decoder, 1.00 = real time: **1.045 s** (TPU v6e-4) | TPU v6e slice (v6e-4) |
| **[Qwen3.8-27B](https://github.com/mashbox-labs/mashbox/tree/v0.1.5/src/mashbox/models/qwen3_8_27b)** by Qwen | text in, text out | **38.1 vs 31.4 tok/s** vs vLLM on the same chip, 1 request at a time; **74.84 tok/s** with speculative decoding (--mtp 2), 1 request at a time, greedy; 37.77 tok/s with it off; **326.6 vs 1,606.7 s** first start to ready, weights on disk, empty compile caches, vs vLLM on the same chip type (TPU v6e-1) | TPU v5e, TPU v6e, TPU v6e slice (v6e-4) |
| **[Track2Rig](https://github.com/mashbox-labs/mashbox/tree/v0.1.5/src/mashbox/models/track2rig)** by MASHBOX Labs<br>Experimental | 3D point tracks in, rig (JSON and glTF) out | **0.784% vs 0.915%** held-out error vs SSDR at the same bone count, synthetic test set with noisy, partly hidden tracks, % of body size, lower is better; **6.24% vs 7.73%** joint error vs SSDR, % of body size, lower is better (CPU, AMD EPYC 7502P) | CPU, TPU v6e |

**GGUF files that run in our engine**

GGUF files from other publishers, run as they are by mashbox. Level and Runs on say how far each file has been checked; a speed shows only where a chip run measured it on our kernels.

Tested on TPU:

| Model | GGUF file | Architecture | Quant | Level | Runs on |
|---|---|---|---|---|---|
| **Qwen2.5-0.5B-Instruct**<br>by Qwen | [Qwen2.5-0.5B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Qwen2.5-0.5B-Instruct-GGUF/blob/41ba88dbac95fed2528c92514c131d73eb5a174b/Qwen2.5-0.5B-Instruct-Q4_K_M.gguf)<br>from bartowski, 0.4 GB | `qwen2` | Q4_K_M | Tested | TPU v6e-1 |
| **Qwen2.5-1.5B-Instruct**<br>by Qwen | [Qwen2.5-1.5B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Qwen2.5-1.5B-Instruct-GGUF/blob/9eadc66189c7641e1ddd226b8267a9119b2ce2d4/Qwen2.5-1.5B-Instruct-Q4_K_M.gguf)<br>from bartowski, 0.99 GB | `qwen2` | Q4_K_M | Tested | TPU v6e-1 |
| **Qwen3-0.6B**<br>by Qwen | [Qwen3-0.6B-Q8_0.gguf](https://huggingface.co/unsloth/Qwen3-0.6B-GGUF/blob/50968a4468ef4233ed78cd7c3de230dd1d61a56b/Qwen3-0.6B-Q8_0.gguf)<br>from unsloth, 0.64 GB | `qwen3` | Q8_0 | Tested | TPU v6e-1 |
| **Qwen3.5-0.8B**<br>by Qwen | [Qwen3.5-0.8B-Q8_0.gguf](https://huggingface.co/ggml-org/Qwen3.5-0.8B-GGUF/blob/807dfd9a6fadbf703207aac3bfb0b2b3040d077c/Qwen3.5-0.8B-Q8_0.gguf)<br>from ggml-org, 0.83 GB | `qwen35` | Q8_0 | Tested | TPU v6e-1 |
| **Qwen3.5-4B**<br>by Qwen | [Qwen3.5-4B-Q4_K_M.gguf](https://huggingface.co/unsloth/Qwen3.5-4B-GGUF/blob/e87f176479d0855a907a41277aca2f8ee7a09523/Qwen3.5-4B-Q4_K_M.gguf)<br>from unsloth, 2.74 GB | `qwen35` | Q4_K_M | Tested | TPU v6e-1 |

[Full catalog](https://mashbox.gg/docs/supported-models): 17 entries: 2 MASHBOX ports, 1 MASHBOX model and 14 GGUF files of 12 models.
<!-- models:end -->

Weights are on [Hugging Face](https://huggingface.co/mashbox-labs), named by type with size, bits per weight and measured quality on every card. Claude Code, Codex and Pi can use the LLMs as their model: [Use with coding agents](https://github.com/mashbox-labs/mashbox/blob/main/docs/agents-on-tpu.md). [AGENTS.md](https://github.com/mashbox-labs/mashbox/blob/main/AGENTS.md) gives a coding agent the steps to set up a chip itself.

### Contributing

Bug reports, fixes and model requests are welcome: [CONTRIBUTING.md](https://github.com/mashbox-labs/.github/blob/main/CONTRIBUTING.md). Security reports go through [SECURITY.md](https://github.com/mashbox-labs/.github/blob/main/SECURITY.md).

[mashbox.gg](https://mashbox.gg)&nbsp;&nbsp;&nbsp;[x.com/mashboxgg](https://x.com/mashboxgg)&nbsp;&nbsp;&nbsp;hello@mashbox.gg
