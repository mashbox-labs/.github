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
| **[Hy-Nomas](https://github.com/mashbox-labs/mashbox/tree/v0.1.6/src/mashbox/models/hy_nomas)** by Tencent Hunyuan | text and movement keys in, video out | Compute per second of video, quality preset, Tencent decoder, 1.00 = real time: **1.045 s** (TPU v6e-4) | TPU v6e slice (v6e-4) |
| **[Qwen3.8-27B](https://github.com/mashbox-labs/mashbox/tree/v0.1.6/src/mashbox/models/qwen3_8_27b)** by Qwen | text in, text out | **38.1 vs 31.4 tok/s** vs vLLM on the same chip, 1 request at a time; **74.84 tok/s** with speculative decoding (--mtp 2), 1 request at a time, greedy; 37.77 tok/s with it off; **326.6 vs 1,606.7 s** first start to ready, weights on disk, empty compile caches, vs vLLM on the same chip type (TPU v6e-1) | TPU v5e, TPU v6e, TPU v6e slice (v6e-4) |
| **[Track2Rig](https://github.com/mashbox-labs/mashbox/tree/v0.1.6/src/mashbox/models/track2rig)** by MASHBOX Labs<br>Experimental | 3D point tracks in, rig (JSON and glTF) out | **0.784% vs 0.915%** held-out error vs SSDR at the same bone count, synthetic test set with noisy, partly hidden tracks, % of body size, lower is better; **6.24% vs 7.73%** joint error vs SSDR, % of body size, lower is better (CPU, AMD EPYC 7502P) | CPU, TPU v6e |

**GGUF files that run in our engine**

GGUF files from other publishers, run as they are by mashbox. Level and Runs on say how far each file has been checked; a speed shows only where a chip run measured it on our kernels.

Tested on TPU (8 of 14 files with a passing chip test):

| Model | GGUF file | Architecture | Quant | Level | Runs on |
|---|---|---|---|---|---|
| **Llama-3.2-1B-Instruct**<br>by Meta | [Llama-3.2-1B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Llama-3.2-1B-Instruct-GGUF/blob/067b946cf014b7c697f3654f621d577a3e3afd1c/Llama-3.2-1B-Instruct-Q4_K_M.gguf)<br>from bartowski, 0.81 GB | `llama` | Q4_K_M | Tested<br>Runs, checked against llama.cpp on CPU: same weights NMSE 4.2e-12, top-1 133/133; llama.cpp's own path top-1 130/133; greedy 4 x 16 tokens: 3 identical, 1 near tie | TPU v6e-1 |
| **Qwen2.5-0.5B-Instruct**<br>by Qwen | [Qwen2.5-0.5B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Qwen2.5-0.5B-Instruct-GGUF/blob/41ba88dbac95fed2528c92514c131d73eb5a174b/Qwen2.5-0.5B-Instruct-Q4_K_M.gguf)<br>from bartowski, 0.4 GB | `qwen2` | Q4_K_M | Tested | TPU v6e-1 |
| **Qwen2.5-0.5B-Instruct**<br>by Qwen | [qwen2.5-0.5b-instruct-q4_k_m.gguf](https://huggingface.co/Qwen/Qwen2.5-0.5B-Instruct-GGUF/blob/9217f5db79a29953eb74d5343926648285ec7e67/qwen2.5-0.5b-instruct-q4_k_m.gguf)<br>from Qwen, 0.49 GB | `qwen2` | q4_k_m | Tested<br>Runs, checked against llama.cpp on CPU: same weights NMSE 2.6e-11, top-1 131/131; llama.cpp's own path top-1 126/131; greedy 4 x 16 tokens: 3 identical, 1 near tie | TPU v6e-1 |
| **Qwen2.5-1.5B-Instruct**<br>by Qwen | [Qwen2.5-1.5B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Qwen2.5-1.5B-Instruct-GGUF/blob/9eadc66189c7641e1ddd226b8267a9119b2ce2d4/Qwen2.5-1.5B-Instruct-Q4_K_M.gguf)<br>from bartowski, 0.99 GB | `qwen2` | Q4_K_M | Tested | TPU v6e-1 |
| **Qwen2.5-1.5B-Instruct**<br>by Qwen | [qwen2.5-1.5b-instruct-q4_k_m.gguf](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct-GGUF/blob/91cad51170dc346986eccefdc2dd33a9da36ead9/qwen2.5-1.5b-instruct-q4_k_m.gguf)<br>from Qwen, 1.12 GB | `qwen2` | q4_k_m | Tested<br>Runs, checked against llama.cpp on CPU: same weights NMSE 1.2e-11, top-1 131/131; llama.cpp's own path top-1 125/131; greedy 4 x 16 tokens: 3 identical, 1 near tie | TPU v6e-1 |
| **Qwen2.5-7B-Instruct**<br>by Qwen | [Qwen2.5-7B-Instruct-Q4_K_M.gguf](https://huggingface.co/bartowski/Qwen2.5-7B-Instruct-GGUF/blob/8911e8a47f92bac19d6f5c64a2e2095bd2f7d031/Qwen2.5-7B-Instruct-Q4_K_M.gguf)<br>from bartowski, 4.68 GB | `qwen2` | Q4_K_M | Tested<br>Runs, checked against llama.cpp on CPU: same weights NMSE 1.1e-11, top-1 131/131; llama.cpp's own path top-1 124/131; greedy 4 x 16 tokens: 3 identical, 1 near tie | TPU v6e-1 |
| **Qwen2.5-Coder-1.5B-Instruct**<br>by Qwen | [qwen2.5-coder-1.5b-instruct-q4_k_m.gguf](https://huggingface.co/Qwen/Qwen2.5-Coder-1.5B-Instruct-GGUF/blob/f86cb2c1fa58255f8052cc32aeede1b7482d4361/qwen2.5-coder-1.5b-instruct-q4_k_m.gguf)<br>from Qwen, 1.12 GB | `qwen2` | q4_k_m | Tested<br>Runs, checked against llama.cpp on CPU: same weights NMSE 2.1e-12, top-1 131/131; llama.cpp's own path top-1 124/131; greedy 4 x 16 tokens: 3 identical, 1 near tie | TPU v6e-1 |
| **Qwen3-0.6B**<br>by Qwen | [Qwen3-0.6B-Q8_0.gguf](https://huggingface.co/unsloth/Qwen3-0.6B-GGUF/blob/50968a4468ef4233ed78cd7c3de230dd1d61a56b/Qwen3-0.6B-Q8_0.gguf)<br>from unsloth, 0.64 GB | `qwen3` | Q8_0 | Tested | TPU v6e-1 |

[Full catalog](https://mashbox.gg/docs/supported-models): 26 entries: 2 MASHBOX ports, 1 MASHBOX model and 23 GGUF files of 18 models.
<!-- models:end -->

Weights are on [Hugging Face](https://huggingface.co/mashbox-labs), named by type with size, bits per weight and measured quality on every card. Claude Code, Codex and Pi can use the LLMs as their model: [Use with coding agents](https://github.com/mashbox-labs/mashbox/blob/main/docs/agents-on-tpu.md). [AGENTS.md](https://github.com/mashbox-labs/mashbox/blob/main/AGENTS.md) gives a coding agent the steps to set up a chip itself.

### Contributing

Bug reports, fixes and model requests are welcome: [CONTRIBUTING.md](https://github.com/mashbox-labs/.github/blob/main/CONTRIBUTING.md). Security reports go through [SECURITY.md](https://github.com/mashbox-labs/.github/blob/main/SECURITY.md).

[mashbox.gg](https://mashbox.gg)&nbsp;&nbsp;&nbsp;[x.com/mashboxgg](https://x.com/mashboxgg)&nbsp;&nbsp;&nbsp;hello@mashbox.gg
