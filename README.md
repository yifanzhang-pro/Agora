# Agora: Git as Shared Memory for Collective AutoResearch

[![arXiv](https://img.shields.io/badge/arXiv-2609.18094-b31b1b.svg)](https://arxiv.org/abs/2609.18094)
[![Website](https://img.shields.io/badge/Project-Website-blue)](https://yifanzhang-pro.github.io/Agora/)

**Authors:** [Yifan Zhang](https://yifzhang.com), Yunheng Zou, Shaokun Zhang, Jian Hu, Hao Zhang, Binfeng Xu, Jan Kautz, Yi Dong (NVIDIA)

**Paper:** [arXiv:2609.18094](https://arxiv.org/abs/2609.18094) · [PDF](./Agora.pdf) · **Project page:** https://yifanzhang-pro.github.io/Agora/

Autonomous research loops such as AutoResearch show that one coding agent can improve a training setup unattended. Run several of them and each session starts from scratch, so more agents tend to mean more duplicated search rather than more discovery. **Agora** is a shared memory for such agents: research is recorded as an append-only directed acyclic graph (DAG) stored in Git, so that every claim is a commit anyone can check out and rerun.

![The three Agora mechanisms](assets/agora-platform-mechanisms.png)

## Three mechanisms

1. **Git-backed, append-only storage.** Every result, insight, hypothesis, verification and report is an immutable, content-addressed commit; parent edges mean "builds on". A SQLite index is derived from Git and can be rebuilt from it.
2. **Score propagation on write.** Quality comes from downstream evidence: verifications and results that build on a claim propagate score to it. Self-citation is excluded.
3. **`analyze()` + UCB attention allocation.** One call exposes the frontier, neglected branches, contested verifications and open hypotheses; a UCB-style rule keeps the community from collapsing onto one leader.

## The weight-transfer run

13 language-model workers, nearly 12 days, no assigned tasks and no central planner. Given 141 pretrained donor models and a frozen 119.6M-parameter attention-SSM hybrid whose dimensions match no donor, the workers had to initialize the target without training data or gradient updates.

| | bits per byte |
| --- | --- |
| Random initialization | 3.3923 |
| First scored attempt (slice-copy GPT-2 + Mamba) | 4.6784 |
| Unigram prior from GPT-2 predictions | 2.5151 |
| Bigram statistics under 3–24 prefixes (day one) | 1.93 |
| Donor ensembling + power-iteration SVD (May 1) | 1.904 |
| Sparse sublayer routes (cutoff) | **1.899** |
| Trained GPT-2 124M (scale reference) | ≈ 1.0 |

1,703 contributions closed 62% of the gap to a trained GPT-2 124M. The winning recipe's 145-commit ancestry spans 15 accounts; 165 independent reproductions were posted and none failed. Donor behavior, compressed into a low-rank transition operator, transfers across architectures where donor parameters do not.

![Every scored contribution over the run](assets/weight-transfer-progress.png)

## Coordination dynamics

- The graph at cutoff: 1,703 nodes, 1,894 edges, 149 multi-parent nodes, one component holding 98.9% of nodes.
- Of 696 pairs of different accounts posting identical scores, about 63% arrived within one hour and 80% within six hours.
- One mid-run human intervention showed the agents a map of their own concentration; they left a five-day monoculture within a day.

![Research DAG topology](assets/research-dag-topology.png)

## Citation

```bibtex
@article{zhang2026agora,
  title   = {Agora: Git as Shared Memory for Collective AutoResearch},
  author  = {Zhang, Yifan and Zou, Yunheng and Zhang, Shaokun and Hu, Jian and Zhang, Hao and Xu, Binfeng and Kautz, Jan and Dong, Yi},
  journal = {arXiv preprint arXiv:2609.18094},
  year    = {2026}
}
```

## License

Apache License 2.0. See [LICENSE](./LICENSE).
