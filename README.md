<div align="center">

# LatentDE: <u>Latent</u>-based <u>D</u>irected <u>E</u>volution accelerated by Gradient Ascent for Protein Sequence Design
</div>

## Table of Contents:

- [Introduction](#introduction)
- [Structure Description](#structure-description)
- [Installation](#installation)
- [Usage](#usage)
    - [Training](#training)
    - [Inference](#inference)
- [Citation](#citation)

## Introduction
This is the official implementation of the paper *"Latent-based Directed Evolution accelerated by Gradient Ascent for Protein Sequence Design"*.

<div align="center">
    <img src="static/pipeline.png" alt="Pipeline Overview" width="800"/>
    <p><em>Figure: Overview of the LatentDE pipeline.</em></p>
</div>

Our paper just got accepted for NeurIPS's workshop!! The pre-print version is available [here](https://www.biorxiv.org/content/10.1101/2024.04.13.589381v2).

## Structure description

Our repository is structured as follows:
```shell
.
├── active_optimize.sh          # inference + active learning
├── environment.yml
├── exps                        # experiments results
├── optimize.sh                 # inference
├── preprocessed_data
├── README.md
├── scripts                     # main executable scripts
├── src
│   ├── common                  # common utilities
│   ├── dataio                  # dataloader
│   └── models
├── train.sh                    # training script
└── visualize_latent.sh         # visualize trained latent
```

## Installation

You should have Python 3.10 or higher. I highly recommend creating a virtual environment like conda. If so, run the below commands to install:

```shell
conda env create -f environment.yml
```

Download the oracle landscape models by the following commands (using script provided [here](https://github.com/HeliXonProtein/proximal-exploration)):
```shell
cd scripts
bash download_landscape.sh
```

## Usage

### Training

To train VAE model for each benchmark dataset, go to the root directory and execute the `train.sh` file. Take `avGFP` as the example, run the following command:

```shell
bash train.sh ./scripts/configs/rnn_template.py 0 template avGFP 20 256
```

Checkpoints will be saved in `exps/ckpts/` folder. Details of passed arguments can be found [here](./scripts/train_vae.py)

### Inference

To perform optimization, go to the root directory and execute the `optimize.sh` file. Take `avGFP` as the example, run the following command:

```shell
bash optimize.sh avGFP 0 template <model_ckpt_path> <oracle_ckpt_path> 1 rnn
```

Similar to perform active learning alongside with optimization, you can see details of passed argumetns in [`active_optmize.sh`](./active_optimize.sh) file.

Results will be saved in `exps/results_no_active` and `exps/results` folders.

To average results of 5 seeds, check [`calculate.py`](./scripts/calculate.py).

## Citation
If you find our work useful for your research, please cite:

```bibtex
@article {Ngo2024.04.13.589381,
	author = {Ngo, Nhat Khang and Tran, Thanh V. T. and Duy Nguyen, Viet Thanh and Hy, Truong Son},
	title = {Latent-based Directed Evolution accelerated by Gradient Ascent for Protein Sequence Design},
	elocation-id = {2024.04.13.589381},
	year = {2024},
	doi = {10.1101/2024.04.13.589381},
	publisher = {Cold Spring Harbor Laboratory},
	URL = {https://www.biorxiv.org/content/early/2024/04/18/2024.04.13.589381},
	eprint = {https://www.biorxiv.org/content/early/2024/04/18/2024.04.13.589381.full.pdf},
	journal = {bioRxiv}
}
```