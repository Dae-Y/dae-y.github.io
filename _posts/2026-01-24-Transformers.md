---
layout: single
title: "Note on Attention, Transformers, and the New Linear Models"
category: technology
tag: ai
author_profile: false
sidebar:
    nav: "counts"
---

A Quick Note on Attention, Transformers, and the New Linear Models.

## Overview

I came across a YouTube talk on **Kimi Linear** and found the ideas compelling enough to jot down a quick, structured note. With GPU demand surging and memory getting pricier, architectures that **reduce KV‑cache and speed up decoding** without hurting quality are increasingly important.


## Self‑Attention & the Transformer (Quick Primer)

**From sequential RNNs to attention.**  
Before 2017, NLP models relied on RNNs/LSTMs that processed text one token at a time and struggled with **long‑range dependencies**.

**The attention mechanism.**  
Instead of strict left→right processing, attention lets each token “look at” all others:

- **Query, Key, Value (Q/K/V):**
  - **Query** — what a token is looking for
  - **Key** — what a token represents
  - **Value** — the information carried by the token
- **Weighted relationships.** The model scores Q·Kᵀ to decide which tokens to focus on, then aggregates the corresponding **Values**.

**Transformers (2017, “Attention Is All You Need”).**
- **Parallel processing** of entire sequences (faster training than RNNs).
- **Multi‑Head Attention** learns multiple relationship patterns (syntax, semantics, long‑range links).
- Foundation of modern LLMs (e.g., GPT, BERT, etc.).


## What Kimi Linear Proposes (Doc dated **2025‑11‑01**)

Kimi Linear introduces a **hybrid architecture** that interleaves *linear attention* with *occasional full attention*, aiming to **match or beat full attention under fair training conditions** while being **cheaper at long context lengths**.

### Key Ideas

- **Kimi Delta Attention (KDA).**  
  A linear‑attention module extending *Gated DeltaNet*. It moves from **head‑wise** forgetting to **channel‑wise gating**, so each feature dimension can “forget” at its own rate—making better use of the limited (finite‑state) recurrent memory common to linear attention.

- **Hardware‑efficient chunkwise algorithm.**  
  Based on a specialized **Diagonal‑Plus‑Low‑Rank (DPLR)** transition structure, tailored to reduce compute vs. more general DPLR variants while staying consistent with the delta‑rule view.

- **Hybrid stacking (3:1).**  
  The recipe emphasized is **3 KDA layers : 1 full‑attention layer**—retaining global information flow while keeping most layers **fast** and **KV‑cache‑light**.

- **Design choices.**  
  - **No positional embeddings (NoPE)** to better extend to very long contexts; compared against a RoPE variant.  
  - **MoE setup:** an example configuration mentions **48B total / 3B activated** (Mixture‑of‑Experts) when comparing to matched baselines.


## Claimed Empirical Results

- **Quality across regimes.**  
  Reported as the first *hybrid linear‑attention* approach to **outperform full attention** under matched training (short‑context, long‑context, and RL post‑training).

- **Long‑context leaderboards (e.g., 128k).**  
  Strong results on suites like **RULER** and **RepoQA**, with best average among compared models at very long context.

- **RL behavior (math‑focused).**  
  Under the same RL setup, training/test curves show **faster improvement and higher final scores** than a full‑attention baseline.

- **Scaling & training setup.**  
  “Fair” comparisons reportedly use **4,096** pretrain context and **1.4T tokens** (shared across models). A longer‑trained checkpoint (**5.7T tokens**) is mentioned with support up to **1M context**.

> *Note:* These are **their** reported results in the referenced document; verify against the latest numbers as the ecosystem moves quickly.



## Efficiency Wins

- **KV‑cache reduction.**  
  With most layers using linear attention, reported **up to ~75% less KV‑cache** usage in long‑sequence generation.

- **Decoding throughput at long context.**  
  At **1M tokens**, they report up to **~6.3× faster** time‑per‑output‑token (TPOT) by leveraging memory savings to run larger batches—**and** sustaining low TPOT as decode length grows.


## Practical Takeaway

If your workloads are **long‑context** and **decoding‑heavy** (agentic tool use, repo‑level code understanding, long RL trajectories), the pitch is:

- Keep **some global full attention** for true long‑range interactions.  
- Make **most layers linear + gated‑delta (KDA)** to **slash KV‑cache** and **speed up decoding**, while targeting **equal or better quality** than a full‑attention stack.


## Reference

### 📘 [Kimi Linear: An Expressive, Efficient Attention Architecture](https://arxiv.org/abs/2510.26692)

For readers who want the full technical details, the complete paper is available at the link above.