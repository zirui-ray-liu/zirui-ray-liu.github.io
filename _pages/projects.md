---
layout: page
permalink: /projects/
title: Projects

description: Chef's Selections
nav: true
nav_order: 2
---

<div class="tc-thread">

  <section class="tc-section">

    <article class="tc-paper">
      <a class="tc-paper-link" href="{{ '/projects/mlsys-numerics/kivi/' | relative_url }}">
        <div class="tc-paper-grid">
          <div class="tc-paper-fig">
            {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/kivi_teaser.png" class="tc-paper-img" alt="Llama-2-13B Layer 31 Head 0 key and value cache magnitude surfaces" %}
          </div>
          <div class="tc-paper-body">
            <p class="tc-paper-meta">ICML 2024 · February 2024</p>
            <p class="tc-paper-highlight">Adopted in HuggingFace Transformers (QuantizedCache)</p>
            <h3 class="tc-paper-title">KIVI: A Tuning-Free Asymmetric 2-bit Quantization for KV Cache</h3>
            <p class="tc-paper-authors">Zirui Liu*, Jiayi Yuan*, Hongye Jin, Shaochen Zhong, Zhaozhuo Xu, Vladimir Braverman, Beidi Chen, Xia Hu</p>
            <p class="tc-paper-abstract">
              The KV cache has an asymmetric structure: keys have a few fixed outlier channels, values don't. We quantize keys per-channel and values per-token at 2 bits, no tuning required. Result: 2.6× peak memory reduction and 2.35–3.47× end-to-end throughput with essentially no quality loss.
            </p>
          </div>
        </div>
      </a>
      <p class="tc-paper-links">
        <a href="{{ '/projects/mlsys-numerics/kivi/' | relative_url }}">project page</a>
        <span class="tc-dot">·</span>
        <a href="https://arxiv.org/abs/2402.02750" target="_blank" rel="noopener">Paper</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/jy-yuan/KIVI" target="_blank" rel="noopener">Code</a>
      </p>
    </article>

    <article class="tc-paper">
      <a class="tc-paper-link" href="https://github.com/datamllab/LongLM" target="_blank" rel="noopener">
        <div class="tc-paper-grid">
          <div class="tc-paper-fig">
            {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/self_extend_teaser.png" class="tc-paper-img" alt="Self-Extend grouped position encoding for long context" %}
          </div>
          <div class="tc-paper-body">
            <p class="tc-paper-meta">ICML 2024 (Spotlight) · January 2024</p>
            <p class="tc-paper-highlight">Integrated into Llama.cpp · Featured at Google I/O</p>
            <h3 class="tc-paper-title">LLM Maybe LongLM: Self-Extend LLM Context Window Without Tuning</h3>
            <p class="tc-paper-authors">Hongye Jin*, Xiaotian Han*, Jingfeng Yang, Zhimeng Jiang, Zirui Liu, Chia-Yuan Chang, Huiyuan Chen, Xia Hu</p>
            <p class="tc-paper-abstract">
              LLMs break on long contexts because they see position IDs they were never trained on. Self-Extend fixes it at inference time: keep fine-grained positions for nearby tokens, and floor-divide positions of distant tokens so they fall back into the training range. No fine-tuning required, and Llama / Mistral extend their effective context window by 4–8× with minimal quality loss.
            </p>
          </div>
        </div>
      </a>
      <p class="tc-paper-links">
        <a href="https://arxiv.org/abs/2401.01325" target="_blank" rel="noopener">Paper</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/datamllab/LongLM" target="_blank" rel="noopener">Code</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/ggerganov/llama.cpp/pull/4815" target="_blank" rel="noopener">Llama.cpp PR</a>
        <span class="tc-dot">·</span>
        <a href="https://www.youtube.com/watch?v=TV7qCk1dBWA&t=2025s" target="_blank" rel="noopener">Google I/O</a>
      </p>
    </article>

    <article class="tc-paper">
      <a class="tc-paper-link" href="{{ '/projects/mlsys-numerics/numerical-nondeterminism/' | relative_url }}">
        <div class="tc-paper-grid">
          <div class="tc-paper-fig">
            {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/numerics_teaser.png" class="tc-paper-img" alt="accuracy variation across GPU count and batch size" %}
          </div>
          <div class="tc-paper-body">
            <p class="tc-paper-meta">NeurIPS 2025 (Oral) · June 2025</p>
            <p class="tc-paper-highlight">NeurIPS 2025 Oral · Acknowledged in Thinking Machines Lab's blog</p>
            <h3 class="tc-paper-title">Understanding and Mitigating Numerical Sources of Nondeterminism in LLM Inference</h3>
            <p class="tc-paper-authors">Jiayi Yuan*, Hao Li*, Xinheng Ding, Wenya Xie, Yu-Jhe Li, Wentian Zhao, Kun Wan, Jing Shi, Xia Hu, Zirui Liu</p>
            <p class="tc-paper-abstract">
              The same prompt under BF16 greedy decoding gives different answers on different GPUs — up to 9% accuracy and 9,000 tokens of length for reasoning models. The cause is non-associative floating-point arithmetic: reduction orders shift with batch size, GPU count, and GPU type. Rounding differences cascade through long chains of thought.
            </p>
          </div>
        </div>
      </a>
      <p class="tc-paper-links">
        <a href="{{ '/projects/mlsys-numerics/numerical-nondeterminism/' | relative_url }}">project page</a>
        <span class="tc-dot">·</span>
        <a href="https://arxiv.org/abs/2506.09501" target="_blank" rel="noopener">Paper</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/nanomaoli/llm_reproducibility" target="_blank" rel="noopener">Code</a>
        <span class="tc-dot">·</span>
        <a href="https://www.youtube.com/watch?v=xtzACc7qbyI" target="_blank" rel="noopener">Talk</a>
        <span class="tc-dot">·</span>
        <a href="https://thinkingmachines.ai/blog/defeating-nondeterminism-in-llm-inference/" target="_blank" rel="noopener">Thinking Machines Lab blog</a>
      </p>
    </article>

    <article class="tc-paper">
      <a class="tc-paper-link" href="https://festive-clam-15f.notion.site/Enabling-Large-Scale-True-on-Policy-RL-by-Bringing-Tensor-Parallelism-to-Order-2b039f5cabfa807b9770fcbe339f0f9b" target="_blank" rel="noopener">
        <div class="tc-paper-grid">
          <div class="tc-paper-fig">
            {% include figure.liquid loading="eager" path="assets/img/projects/mlsys-numerics/tpdet_teaser.png" class="tc-paper-img" alt="RL rollout/training mismatch across TP sizes" %}
          </div>
          <div class="tc-paper-body">
            <p class="tc-paper-meta">Preprint · November 2025</p>
            <p class="tc-paper-highlight">Upstreaming to SGLang (PR under review, to be merged)</p>
            <h3 class="tc-paper-title">Deterministic Inference across Tensor Parallel Sizes That Eliminates Training-Inference Mismatch</h3>
            <p class="tc-paper-authors">Ziyang Zhang*, Xinheng Ding*, Jiayi Yuan, Rixin Liu, Huizi Mao, Jiarong Xing, Zirui Liu</p>
            <p class="tc-paper-abstract">
              In RL post-training, the rollout engine (vLLM, TP=8) and the trainer (FSDP, TP=1) disagree on per-token probabilities because their reduction trees differ. <em>Tree-Based Invariant Kernels</em> align intra- and inter-GPU reductions into a single tree, giving bit-wise identity across TP sizes and eliminating the silent off-policy drift.
            </p>
          </div>
        </div>
      </a>
      <p class="tc-paper-links">
        <a href="https://festive-clam-15f.notion.site/Enabling-Large-Scale-True-on-Policy-RL-by-Bringing-Tensor-Parallelism-to-Order-2b039f5cabfa807b9770fcbe339f0f9b" target="_blank" rel="noopener">Blog</a>
        <span class="tc-dot">·</span>
        <a href="https://arxiv.org/abs/2511.17826" target="_blank" rel="noopener">Paper</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/nanomaoli/llm_reproducibility" target="_blank" rel="noopener">Code</a>
        <span class="tc-dot">·</span>
        <a href="https://github.com/sgl-project/sglang/pull/15041" target="_blank" rel="noopener">SGLang PR</a>
      </p>
    </article>

  </section>

</div>

<style>
/* transformer-circuits-inspired thread page */
.tc-thread { max-width: 760px; margin: 2rem auto 4rem; }
.tc-section { margin: 2.5rem 0; }
.tc-section > p { line-height: 1.75; font-size: 1rem; }

.tc-paper { margin: 0 0 2.5rem; padding-bottom: 2rem; border-bottom: 1px solid var(--global-divider-color); }
.tc-paper:last-child { border-bottom: none; }
.tc-paper-link { text-decoration: none; color: inherit; display: block; }
.tc-paper-link:hover .tc-paper-title { color: var(--global-theme-color); }
.tc-paper-grid { display: grid; grid-template-columns: 380px 1fr; gap: 1.5rem; align-items: start; }
@media (max-width: 640px) { .tc-paper-grid { grid-template-columns: 1fr; } }
.tc-paper-fig figure { margin: 0; }
.tc-paper-img { width: 100%; height: auto; border: 1px solid var(--global-divider-color); border-radius: 4px; background: #fff; }
.tc-paper-meta { font-size: .8rem; text-transform: uppercase; letter-spacing: .08em; color: var(--global-text-color-light); margin: 0 0 .5rem; }
.tc-paper-highlight { font-size: .88rem; font-weight: 600; color: #c0392b; margin: 0 0 .5rem; }
.tc-paper-title { font-size: 1.2rem; font-weight: 600; line-height: 1.35; margin: 0 0 .5rem; }
.tc-paper-authors { font-size: .9rem; color: var(--global-text-color-light); margin: 0 0 .75rem; line-height: 1.5; }
.tc-paper-abstract { font-size: .95rem; line-height: 1.65; margin: 0; }
.tc-paper-links { font-size: .9rem; margin: 1rem 0 0; padding-left: calc(380px + 1.5rem); }
@media (max-width: 640px) { .tc-paper-links { padding-left: 0; } }
.tc-paper-links a { text-decoration: none; color: var(--global-theme-color); }
.tc-paper-links a:hover { text-decoration: underline; }
.tc-dot { margin: 0 .4rem; color: var(--global-text-color-light); }
</style>
