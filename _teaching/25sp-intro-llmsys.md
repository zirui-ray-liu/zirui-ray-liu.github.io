---
layout: page
title: Spring 25 CSCI 8980 Introduction to LLM System
description: 
img:
importance: 1
category: teaching
pretty_table: true
related_publications: 
---

**Instructor**: Zirui "Ray" Liu

**Time**: Spring 2025, M/W 4:00 PM - 05:15 PM

#### **Course Description**

Recent progress of Artificial Intelligence has been largely driven by advances in large language models (LLMs) and other generative methods. The success of LLMs is driven by three key factors: the scale of data, the size of models, and the computational resources available. For example, Llama 3, a cutting- edge LLM with 405 billion parameters, was trained over 15 trillion tokens using 16,000 H100 GPUs. Thus, training, serving, fine-tuning, and evaluating such models demand sophisticated engineering practices that leverage modern hardware and software infrastructures. Building scalable systems for LLMs is essential for further advancing AI capabilities.
In this course, students will learn the design principle of a good Machine Learning System for LLMs. Specifically, this course mainly covers three topics (1) the components of modern transformer-based language models; (2) the knowledge about GPU programming; (3) how to train and deploy LLMs in a scalable way.

#### **Grading Policy**

*The grading policy is subject to minor change.*

The course will include **five short quizzes** and **a final project**. The final project can be completed individually or collaboratively in groups of up to three students. Presentations for the final project will occur during the last three weeks of the semester and will include a Q&A session.
For the final project, students must select a research paper focused on MLSys. They will critically analyze the paper to identify its limitations or areas that could be improved. Based on this analysis, students are expected to propose and develop a solution to address the identified issues or introduce an innovative idea to enhance the research.


####  **Course Schedule (tentative)**

*The course schedule may be changed.*

| Week   | Topic                                               | Papers & Materials                                                                                       |
|--------|-----------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Week 1 | Course overview                                     | -                                                                                                       |
| Week 1 | Automatic Differentiation — Reverse Mode Automatic Differentiation           | [Wikipedia of Automatic Differentiation](https://en.wikipedia.org/wiki/Automatic_differentiation)      |
| Week 2 | Automatic Differentiation — Computation Graph      | [PyTorch's Blog on Computation Graph](https://pytorch.org/blog/computational-graphs-constructed-in-pytorch/) |
| Week 2       | Transformers -- Multi-Head Attention      | [Sasha Rush's notebook: The Annoted Transformers](https://nlp.seas.harvard.edu/annotated-transformer/)   |
| Week 3       | Transformers -- Layer Normalization and Feed Forward Network | [Sasha Rush's notebook: The Annoted Transformers](https://nlp.seas.harvard.edu/annotated-transformer/)   |
| Week 3       | Transformers  -- Positional Embedding   | [Sasha Rush's notebook: The Annoted Transformers](https://nlp.seas.harvard.edu/annotated-transformer/) |   
| Week 4       | Llama's change over original Transformer  -- RMSNorm & SwiGLU activation & Gated MLP   | [ The official Llama 1 report](https://arxiv.org/pdf/2302.13971) <br> [Pytorch'doc on RMSNorm](https://pytorch.org/docs/stable/generated/torch.nn.modules.normalization.RMSNorm.html) <br> [Noam Shazeer's report: GLU Variants Improve Transformer](https://arxiv.org/pdf/2002.05202)| 
| Week 4       | Llama’s change over original Transformer  -- Rotary Positional Embedding (ROPE)   | [ EleutherAI's blog on Rotary Positional Embeddings](https://blog.eleuther.ai/rotary-embeddings/) <br> [The original ROPE paper](https://arxiv.org/abs/2104.09864)| 
| Week 5       | Adam Optimizer | [The original Adam Optimizer paper](https://arxiv.org/abs/1412.6980) <br> [Pytorch's Adam Optimizer doc](https://pytorch.org/docs/stable/generated/torch.optim.Adam.html) | 
| Week 5       | Mixed Precision Training | [The original mixed precision training paper](https://arxiv.org/abs/1710.03740) <br> [Pytorch's blog on mixed precision training](https://pytorch.org/blog/what-every-user-should-know-about-mixed-precision-training-in-pytorch/) | 
| Week 6       | Distributed Large Model Training -- Data Parallelism, Model Parallelism, Pipeline Parallelism | [HuggingFace's blog on multi-GPU training](https://huggingface.co/docs/transformers/main/en/perf_train_gpu_many) <br> [GPipe](https://arxiv.org/abs/1811.06965) | 
| Week 6       | Distributed Large Model Training -- Model & Optimizer State Sharding   | [ZeRO Optimizer](https://arxiv.org/abs/1910.02054) <br> [Model FLOPs Utilization of PaLM](https://arxiv.org/pdf/2204.02311) | 
| Week 7- End       | TBD | -|

<!-- | Week X       | GPU Programming Basic -- Roofline Analysis | [NERSC Tutorials on Roofline Analysis](https://docs.nersc.gov/tools/performance/roofline/) |
| Week X       | GPU Programming Basic -- Matrix Multiplication with Triton | [Triton's blog on Matrix Multiplication](https://triton-lang.org/main/getting-started/tutorials/03-matrix-multiplication.html) | -->