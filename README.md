# Multi-Aspect Explainable Inductive Relation Prediction by Sentence Transformer

This repository contains code and data for AAAI submission **Multi-Aspect Explainable Inductive Relation Prediction by Sentence Transformer**.
In the paper, we propose a novel approach for inductive relation prediction namely KRST. KRST adopts sentence transformer for path-based prediction and could provide a comprehensive explanation.


## Requirements

- colorama 0.4.4
- matplotlib 3.3.4
- networkx 2.5.1
- numpy 1.18.5
- scikit_learn 1.1.2
- torch 1.9.1
- tqdm 4.64.0
- transformers 4.18.0

## Quick Start

We provide an all-in-one file `generate_train_and_test.sh` to automatic generate paths, train model, and test model.  To specify datasets and few-shot settings, change variables in `generate_train_and_test.sh` as follows:
* `dataset`: Available datasets include `FB15k-237-subset`, `NELL-995-subset`, `WN18RR-subset`, `FB15k-237-subset-inductive`, `NELL-995-subset-inductive`, and `WN18RR-subset-inductive`.
* `suffix`: This is for few-shot settings, including `_full`, `_2000` and `_1000`.
* `finding_mode`: `head` or `tail`.
