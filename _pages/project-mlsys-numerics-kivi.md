---
layout: page
permalink: /projects/mlsys-numerics/kivi/
title: KIVI
description: A Tuning-Free Asymmetric 2-bit Quantization for KV Cache
nav: false
---

<div class="nerf-page">

  <p class="nerf-breadcrumb">
    <a href="{{ '/projects/' | relative_url }}">&larr; Projects</a>
  </p>

  <header class="nerf-header">
    <h1 class="nerf-title">KIVI</h1>
    <p class="nerf-subtitle">A Tuning-Free Asymmetric 2-bit Quantization for KV Cache</p>
    <p class="nerf-authors">
      <a href="/">Zirui Liu</a><sup>1*</sup>,
      <a href="https://jy-yuan.github.io/">Jiayi Yuan</a><sup>2*</sup>,
      Hongye Jin<sup>3</sup>, Shaochen Zhong<sup>2</sup>, Zhaozhuo Xu<sup>4</sup>,
      Vladimir Braverman<sup>5</sup>, Beidi Chen<sup>6</sup>, Xia Hu<sup>2</sup>
    </p>
    <p class="nerf-affils">
      <sup>1</sup>University of Minnesota &nbsp;
      <sup>2</sup>Rice University &nbsp;
      <sup>3</sup>Texas A&amp;M &nbsp;
      <sup>4</sup>Stevens &nbsp;
      <sup>5</sup>Johns Hopkins &nbsp;
      <sup>6</sup>CMU
    </p>
    <p class="nerf-venue">ICML 2024 &nbsp;·&nbsp; <sup>*</sup>equal contribution</p>

    <div class="nerf-buttons">
      <a class="nerf-btn" href="https://arxiv.org/abs/2402.02750" target="_blank" rel="noopener">
        <i class="fa-solid fa-file-lines"></i> Paper
      </a>
      <a class="nerf-btn" href="https://github.com/jy-yuan/KIVI" target="_blank" rel="noopener">
        <i class="fa-brands fa-github"></i> Code
      </a>
      <a class="nerf-btn" href="https://huggingface.co/docs/transformers/main/en/internal/generation_utils#transformers.QuantizedCache" target="_blank" rel="noopener">
        <i class="fa-solid fa-cube"></i> In HF Transformers
      </a>
    </div>
  </header>

  <figure class="nerf-hero">
    {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/kivi_teaser.png" class="nerf-hero-img" alt="Llama-2-13B Layer 31 Head 0 key and value cache magnitude surfaces" %}
    <figcaption>
      <strong>Figure 1.</strong> Elementwise magnitude of the key and value caches in Llama-2-13B (Layer 31, Head 0) as a function of token and channel. The key cache has a small set of channels with persistently large magnitudes — the outlier channels — while the value cache shows no such structure. This motivates per-channel quantization for keys and per-token quantization for values.
    </figcaption>
  </figure>

  <section class="nerf-section">
    <h2>TL;DR</h2>
    <p>
      The KV cache is the memory bottleneck of long-context LLM serving, and the obvious fix — quantize it — is harder than it looks at 2 bits. We study the element distribution of KV tensors in Llama, Falcon, and Mistral and find a clean, model-independent asymmetry: <strong>keys have a few fixed outlier channels</strong> and should be quantized along the channel axis, while <strong>values show no such structure</strong> and should be quantized per token. KIVI applies this rule with a hardware-friendly kernel and needs no calibration or fine-tuning. The result is up to <strong>2.6× peak memory reduction</strong>, up to <strong>4× larger batch size</strong>, and <strong>2.35–3.47× end-to-end throughput</strong> at essentially the same quality.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Abstract</h2>
    <p>
      Efficiently serving large language models requires batching many requests to reduce per-request cost. But as batch size and context length grow, the key-value (KV) cache — which stores attention keys and values to avoid recomputation — dominates memory and speed. Quantization is the natural response, yet there has been little study of the element distribution of the KV cache itself. We conduct a systematic study and find that the key cache should be quantized per-channel (group along the channel dimension) while the value cache should be quantized per-token. From this we design KIVI, a tuning-free 2-bit KV cache quantization algorithm with a hardware-friendly implementation. KIVI lets Llama, Falcon, and Mistral keep essentially the same quality while using 2.6× less peak memory (weights included), enabling up to 4× larger batch sizes and 2.35–3.47× higher throughput on real inference workloads.
    </p>
  </section>

  <section class="nerf-section">
    <h2>The observation: keys and values are not the same</h2>
    <p>
      Quantization has two knobs that matter: the bit-width and the <em>group</em> over which the scale and zero-point are shared. The folklore prescription has been to quantize everything per-token — i.e., share the scale across the hidden dimension, token by token. That works fine at 4 bits but collapses at 2 bits.
    </p>
    <p>
      When we plotted the magnitude distribution of the KV tensors across many layers and many models, the reason jumped out. The <strong>key</strong> cache has a handful of channels with dramatically larger magnitudes than the rest — the classic activation-outlier pattern — and those outlier channels are the <em>same</em> ones at every token. A per-token scale has to stretch to fit the outliers, wasting all of its dynamic range on the small-magnitude channels. A per-<em>channel</em> scale gives each outlier channel its own budget.
    </p>
    <p>
      The <strong>value</strong> cache looks completely different. There is no persistent outlier structure along the channel axis, but there <em>is</em> meaningful variation between tokens. So values should be quantized the ordinary way: per token.
    </p>
  </section>

  <section class="nerf-section">
    <h2>The algorithm</h2>
    <p>
      KIVI applies each rule where it helps. For the key cache we group along the channel dimension and quantize to 2 bits with a shared scale and zero-point per group; for the value cache we quantize per token, also at 2 bits. Everything is asymmetric (<em>scale</em> + <em>zero-point</em>) so that nonnegative activations don't waste a sign bit. There is no calibration set, no fine-tuning, and no knob to tune per model — hence "tuning-free."
    </p>
    <p>
      There is one system wrinkle. Per-channel quantization of keys conflicts with streaming decode: each new token writes a new row, but the quantization group spans all existing rows. We handle this by keeping a small floating-point residual buffer of the most recent tokens and quantizing in chunks, so the amortized memory still approaches the 2-bit target while the newest tokens stay in full precision until the group is full.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Results</h2>
    <p>
      Quality: on Llama-2, Falcon, and Mistral across MMLU-style and long-context benchmarks, KIVI at 2 bits is within noise of full-precision KV. On generation tasks the accuracy deltas we saw were typically under one point.
    </p>
    <p>
      System: shrinking the cache by 8× in bits (16 bits → 2 bits) means the serving-time budget is freed for more requests. In practice we see up to 2.6× lower peak memory (including model weights), 4× larger batch size at the same context length, and 2.35–3.47× end-to-end throughput on long-context workloads.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Impact</h2>
    <p>
      The per-channel-keys / per-token-values split turned out to be a durable observation — KIVI’s finding inspired the <a href="https://huggingface.co/docs/transformers/main/en/internal/generation_utils#transformers.QuantizedCache" target="_blank" rel="noopener">QuantizedCache</a> in Hugging Face Transformers and has been replicated in a range of follow-up KV compression work. In a later <a href="https://arxiv.org/abs/2502.01563" target="_blank" rel="noopener">ICML 2025 paper</a> we traced the asymmetry itself to RoPE: rotary position encoding interacts with the key projection in a way that concentrates magnitude into a small, fixed set of channels, explaining why the keys — and only the keys — have persistent outliers.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Citation</h2>
{% raw %}<pre class="nerf-bibtex">@inproceedings{liu2024kivi,
  title     = {{KIVI}: A Tuning-Free Asymmetric 2-bit Quantization for {KV} Cache},
  author    = {Liu, Zirui and Yuan, Jiayi and Jin, Hongye and Zhong, Shaochen
               and Xu, Zhaozhuo and Braverman, Vladimir and Chen, Beidi and Hu, Xia},
  booktitle = {International Conference on Machine Learning (ICML)},
  year      = {2024}
}</pre>{% endraw %}
  </section>

</div>

{% include nerf_style.liquid %}
