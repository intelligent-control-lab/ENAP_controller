<div align="center">

# Emergent Neural Automaton Policies: Learning Symbolic Structure from Visuomotor Trajectories

**Yiyuan Pan\***, **Xusheng Luo\***, Hanjiang Hu, Peiqi Yu, Changliu Liu

*Robotics Institute, Carnegie Mellon University*

\* *Equal contribution*

[![arXiv](https://img.shields.io/badge/arXiv-2603.25903-b31b1b.svg)](https://arxiv.org/abs/2603.25903)
[![Python 3.10](https://img.shields.io/badge/python-3.10-blue.svg)](https://www.python.org/downloads/release/python-3100/)
[![ManiSkill](https://img.shields.io/badge/simulator-ManiSkill-green.svg)](https://github.com/haosulab/ManiSkill)

</div>

## TODO

- **Reference implementation (current):** This repository is a lightweight, simplified release of ENAP. It implements the full pipeline on a **custom ManiSkill `PegInsertionSide-v1` setup** (Versus stock ManiSkill, the main change is dual-end insertion: either the orange (head) or white (tail) end can satisfy the success condition.) Additional tasks, environments, and experiment scripts aligned with the full paper will be uploaded in future updates.

## Abstract

> Scaling robot learning to long-horizon tasks remains a formidable challenge. While end-to-end policies often lack the structural priors needed for effective long-term reasoning, traditional neuro-symbolic methods rely heavily on hand-crafted symbolic priors. To address the issue, we introduce ENAP (Emergent Neural Automaton Policy), a framework that allows a bi-level neuro-symbolic policy to adaptively emerge from demonstrations. Specifically, we first employ adaptive clustering and an extension of the L* algorithm to infer a Mealy state machine from visuomotor data, which serves as an interpretable high-level planner capturing latent task modes. Then, this discrete structure guides a low-level reactive residual network to learn precise continuous control via behavior cloning. By explicitly modeling the task policy with discrete transitions and continuous residuals, ENAP achieves high sample efficiency and interpretability without requiring task-specific labels. Extensive experiments on complex manipulation and long-horizon tasks demonstrate that ENAP outperforms state-of-the-art end-to-end VLA policies by up to 27% in low-data regimes, while offering a structured representation of robotic intent.

<div align="center">

![Method overview](docs/MethodOverview.jpg)

**Figure 1.** ENAP follows a three-stage pipeline—(i) symbol abstraction, (ii) structure extraction via an extended L*, and (iii) bi-level control—to learn structured policies from demonstrations.

</div>

## Overview

This repository implements ENAP using [ManiSkill](https://github.com/haosulab/ManiSkill) for visuomotor simulation and control. The pipeline learns a discrete PMM structure from demonstrations, then trains a residual policy conditioned on that structure.

## Installation

We recommend a Conda environment with Python 3.10. From the repository root:

```bash
bash setup_enap_env.sh
conda activate enap_env
```

The script creates replaces ManiSkill’s stock `peg_insertion_side.py` with our customized task file [`peg_insertion_side_replace.py`](peg_insertion_side_replace.py) (required for experiments in this codebase).

Install any additional packages your workflow needs (e.g. `tyro`, `matplotlib`, `tqdm`) via `pip` as you run the scripts.

## Data and training

1. **Demonstrations and Clustering**  
   Place trajectory data in the format expected by the preprocessing script:

   ```bash
   python data/preprocess.py --pkl <path_to_trajectories.pkl> [--output-pkl data/episodes_with_states.pkl]
   ```

   This produces `data/episodes_with_states.pkl` with per-step cluster labels and centers.

2. **RNN pretraining and residual training**  

   ```bash
   bash scripts/train/train.sh
   ```

   This runs `scripts/train/rnn_train.py` then `scripts/train/residual_train.py` with their default CLI arguments. Checkpoints and artifacts are written under `results/` and `results/checkpoints/`.

## Evaluation

```bash
python scripts/eval/peg_insert_eval.py
```

## Citation

If you find this work useful, please cite our paper:

```bibtex
@article{pan2026enap,
  title={Emergent Neural Automaton Policies: Learning Symbolic Structure from Visuomotor Trajectories},
  author={Pan, Yiyuan and Luo, Xusheng and Hu, Hanjiang and Yu, Peiqi and Liu, Changliu},
  journal={arXiv preprint arXiv:2603.25903},
  year={2026}
}
```

**Paper:** [https://arxiv.org/abs/2603.25903](https://arxiv.org/abs/2603.25903)

## Acknowledgments

This code builds on [ManiSkill](https://github.com/haosulab/ManiSkill). We thank the ManiSkill team for the simulation stack and APIs.
