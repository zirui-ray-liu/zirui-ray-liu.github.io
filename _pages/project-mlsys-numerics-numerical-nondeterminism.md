---
layout: page
permalink: /projects/mlsys-numerics/numerical-nondeterminism/
title: Numerical Sources of Nondeterminism in LLM Inference
description: Why the same prompt gives different answers on different GPUs, and what to do about it.
nav: false
---

<div class="nerf-page">

  <p class="nerf-breadcrumb">
    <a href="{{ '/projects/' | relative_url }}">&larr; Projects</a>
  </p>

  <header class="nerf-header">
    <h1 class="nerf-title">Numerical Nondeterminism in LLM Inference</h1>
    <p class="nerf-subtitle">Understanding and Mitigating Numerical Sources of Nondeterminism in LLM Inference</p>
    <p class="nerf-authors">
      <a href="https://jy-yuan.github.io/">Jiayi Yuan</a><sup>1*</sup>,
      <a href="https://nanomaoli.github.io/">Hao Li</a><sup>2*</sup>,
      <a href="https://asherding.com/">Xinheng Ding</a><sup>2</sup>,
      <a href="https://wenyaxie023.github.io/">Wenya Xie</a><sup>2</sup>,
      Yu-Jhe Li<sup>3</sup>, Wentian Zhao<sup>3</sup>, Kun Wan<sup>3</sup>, Jing Shi<sup>3</sup>,
      Xia Hu<sup>1</sup>,
      <a href="/">Zirui Liu</a><sup>2</sup>
    </p>
    <p class="nerf-affils">
      <sup>1</sup>Rice University &nbsp;
      <sup>2</sup>University of Minnesota &nbsp;
      <sup>3</sup>Adobe
    </p>
    <p class="nerf-venue">NeurIPS 2025 (Oral) &nbsp;·&nbsp; <sup>*</sup>equal contribution</p>

    <div class="nerf-buttons">
      <a class="nerf-btn" href="https://arxiv.org/abs/2506.09501" target="_blank" rel="noopener">
        <i class="fa-solid fa-file-lines"></i> Paper
      </a>
      <a class="nerf-btn" href="https://github.com/nanomaoli/llm_reproducibility" target="_blank" rel="noopener">
        <i class="fa-brands fa-github"></i> Code
      </a>
      <a class="nerf-btn" href="https://www.youtube.com/watch?v=xtzACc7qbyI" target="_blank" rel="noopener">
        <i class="fa-brands fa-youtube"></i> Talk
      </a>
      <a class="nerf-btn" href="https://asap-seminar.github.io/assets/slides/Numerical_Precision.pdf" target="_blank" rel="noopener">
        <i class="fa-solid fa-chalkboard"></i> Slides
      </a>
    </div>
  </header>

  <figure class="nerf-hero">
    {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/numerics_teaser.png" class="nerf-hero-img" alt="Accuracy and output variation from numerical nondeterminism" %}
    <figcaption>
      <strong>Figure 1.</strong> Under BF16 greedy decoding, the same reasoning model produces materially different answers depending on GPU count, evaluation batch size, and GPU type. Small rounding differences in early tokens cascade into different chains of thought.
    </figcaption>
  </figure>

  <section class="nerf-section">
    <h2>TL;DR</h2>
    <p>
      Running exactly the same prompt on the same model with greedy decoding can give you different answers on different GPUs, different batch sizes, or different GPU counts — on the order of <strong>9% accuracy</strong> and <strong>9,000-token output-length swings</strong> for DeepSeek-R1-Distill-Qwen-7B under BF16. The culprit is the non-associativity of floating-point addition combined with reduction orders that change when the serving configuration changes. We give the first systematic study of when this matters and introduce <strong>LayerCast</strong>, a drop-in inference pipeline that stores weights in 16 bits but runs each op in FP32, closing the reproducibility gap at a fraction of the cost of full-FP32 serving.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Abstract</h2>
    <p>
      Benchmark scores are treated as if they are properties of a model. We show they are also properties of the box the model runs on. Changing evaluation batch size, GPU count, or GPU version can introduce significant differences in generated responses — and the effect is sharpest in reasoning models, where a minor rounding difference at an early token can cascade into a completely different chain of thought. On DeepSeek-R1-Distill-Qwen-7B under BF16 greedy decoding we observed up to 9% accuracy variation and 9,000-token response-length variation from system configuration alone. We trace this back to the non-associative nature of floating-point arithmetic at limited precision and present the first systematic study of how and when it affects LLM evaluation. We then develop LayerCast, a lightweight pipeline that keeps weights in 16 bits but performs all computation in FP32, balancing memory efficiency with numerical stability.
    </p>
  </section>

  <section class="nerf-section">
    <h2>Why "greedy" is not deterministic</h2>
    <p>
      Greedy decoding picks the argmax token at each step, so people reasonably expect it to be a deterministic function of the prompt and the weights. It is not — once you cross a GPU.
    </p>
    <p>
      The reason is that floating-point addition is non-associative: <em>(a + b) + c</em> can differ from <em>a + (b + c)</em> in the last bit. A transformer forward pass is a chain of reductions — matmuls, layer norms, attention — each of which is a sum. The kernel implementing that sum reorders the additions depending on the problem shape, the tile size, and the number of SMs it can spread the work across. Change the batch size, and the matmul picks a different tile schedule. Change the number of GPUs, and the all-reduce picks a different reduction tree. The <em>arithmetic answer</em> is the same; the <em>bit pattern</em> is not.
    </p>
    <p>
      At 16-bit precision the last-bit gap is large. It almost always gets absorbed when the next step is "take the argmax over 32k logits," but when two candidate tokens have nearly identical logits, one last-bit nudge can flip the choice. In a 10,000-step reasoning chain there is plenty of opportunity for that to happen — and after it does, the two branches diverge completely.
    </p>
  </section>

  <section class="nerf-section">
    <h2>What we measured</h2>
    <p>
      We held the model, the prompt, and the sampler fixed, and varied only the serving configuration — GPU type (A100 / H100 / L40S), GPU count (1 / 2 / 4 / 8), and evaluation batch size. On reasoning benchmarks like AIME and GPQA, BF16 DeepSeek-R1-Distill-Qwen-7B swings up to 9 accuracy points between configurations that a practitioner would treat as equivalent. On non-reasoning tasks the drift is smaller but still enough to change published leaderboard orders.
    </p>
    <p>
      The story is not "bf16 is bad." FP16 and FP32 drift too; FP32 just drifts orders of magnitude less because the last-bit gap is smaller. And the drift is not monotonic — running on <em>more</em> GPUs is not more accurate, it is simply different.
    </p>
  </section>

  <section class="nerf-section">
    <h2>LayerCast: FP32 compute, 16-bit storage</h2>
    <p>
      The obvious fix is to serve in FP32, but that doubles weight memory and roughly halves throughput on modern GPUs. LayerCast keeps the weights stored in 16 bits — so the memory cost and the loading bandwidth stay close to BF16 serving — and casts up to FP32 inside each operator. The critical ops (matmul, attention, layernorm, softmax) run in FP32 and accumulate into FP32 accumulators before being cast back. Because the expensive part of an LLM forward pass is the matmul, not the weight-to-activation cast, the throughput cost is modest; because the reductions now happen in FP32, the last-bit gap shrinks by orders of magnitude and the output stops drifting across configurations.
    </p>
    <p>
      In our experiments LayerCast effectively eliminates the accuracy variation we observed in BF16 and leaves fewer than a handful of bits different across runs, while keeping memory close to 16-bit and throughput well above full FP32.
    </p>
  </section>

  <section class="nerf-section">
    <h2>What to take away</h2>
    <ul>
      <li><strong>Reproducibility numbers should name the GPU.</strong> "DeepSeek-R1-Distill on AIME at BF16" is under-specified — add GPU type, GPU count, and batch size.</li>
      <li><strong>Greedy decoding is not a determinism guarantee.</strong> It is a deterministic function of the logits; the logits are not deterministic across boxes.</li>
      <li><strong>Reasoning models amplify the effect.</strong> Long chains of thought magnify last-bit disagreements into different answers.</li>
      <li><strong>If you can afford FP32-compute with 16-bit storage, take it.</strong> LayerCast buys most of the reproducibility of FP32 at a small fraction of the cost.</li>
    </ul>
  </section>

  <section class="nerf-section">
    <h2>Citation</h2>
{% raw %}<pre class="nerf-bibtex">@inproceedings{yuan2025numerical,
  title     = {Understanding and Mitigating Numerical Sources of Nondeterminism in {LLM} Inference},
  author    = {Yuan, Jiayi and Li, Hao and Ding, Xinheng and Xie, Wenya and Li, Yu-Jhe
               and Zhao, Wentian and Wan, Kun and Shi, Jing and Hu, Xia and Liu, Zirui},
  booktitle = {Advances in Neural Information Processing Systems (NeurIPS)},
  year      = {2025},
  note      = {Oral}
}</pre>{% endraw %}
  </section>

</div>

{% include nerf_style.liquid %}
