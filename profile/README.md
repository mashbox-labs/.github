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

Our first open model port runs on Google Cloud TPU, and anyone can run it: a 27B language model on a single chip. A world model you steer in the browser is coming soon. Start with **[mashbox](https://github.com/mashbox-labs/mashbox)**.

On a Google Cloud TPU VM (the [setup guide](https://mashbox.gg/docs/google-cloud) gets you there from a new Google account):

```bash
python3 -m pip install uv && python3 -m uv tool install --python 3.12 "mashbox[tpu]"
~/.local/bin/mashbox run qwen3-8-27b --prompt "Write a haiku about the sea."
```

### Models

<!-- models:start -->
| Model | What it does | Measured | Chip tests |
|---|---|---|---|
| **[Qwen3.8-27B](https://github.com/mashbox-labs/mashbox/tree/v0.1.1/src/mashbox/models/qwen3_8_27b)** by Qwen | text in, text out | **38.1 vs 31.4 tok/s** vs vLLM on the same chip, 1 request at a time (TPU v6e-1) | TPU v6e (2026-10-28) |

Coming soon: **Hy-Nomas** by Tencent Hunyuan (world model: text and movement keys in, video out).
<!-- models:end -->

Weights are on [Hugging Face](https://huggingface.co/mashbox-labs), named by type with size, bits per weight and measured quality on every card. Claude Code, Codex and Pi can use the LLMs as their model: [Use with coding agents](https://github.com/mashbox-labs/mashbox/blob/main/docs/agents-on-tpu.md). [AGENTS.md](https://github.com/mashbox-labs/mashbox/blob/main/AGENTS.md) gives a coding agent the steps to set up a chip itself.

### Contributing

Bug reports, fixes and model requests are welcome: [CONTRIBUTING.md](https://github.com/mashbox-labs/.github/blob/main/CONTRIBUTING.md). Security reports go through [SECURITY.md](https://github.com/mashbox-labs/.github/blob/main/SECURITY.md).

[mashbox.gg](https://mashbox.gg)&nbsp;&nbsp;&nbsp;[x.com/mashboxgg](https://x.com/mashboxgg)&nbsp;&nbsp;&nbsp;hello@mashbox.gg
