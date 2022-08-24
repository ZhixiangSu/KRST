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

We provide an all-in-one file `generate_train_and_test.sh` to automatically extract paths, train model, and test model.  To specify datasets and few-shot settings, change variables in `generate_train_and_test.sh` (line 1-3) as follows:
* `dataset`: Available datasets include `FB15k-237-subset`, `NELL-995-subset`, `WN18RR-subset`, `FB15k-237-subset-inductive`, `NELL-995-subset-inductive`, and `WN18RR-subset-inductive`.
* `suffix`: This is for few-shot settings, including `_full`, `_2000` and `_1000`.
* `finding_mode`: whether `head` or `tail` is fixed.

Then execute the following command:
```shell
./generate_train_and_test.sh
```
you can reproduce the results mentioned in paper.

Also, you can just download the model trained by our team (currently not available because of anonymous requirement) and test them using
```shell
./test.sh
```

## Step by Step Implementation

# Dataset Preparation

All datasets mentioned in paper are provided in `data\data`. They are from [BERTRL](https://github.com/zhw12/BERTRL/blob/master/README.md) and reorganized. If you want to mannually create a new dataset, please provide the following files:
* `train$suffix$.txt`: File for training. Each line is a triplet (h,r,t) seperated by '\t'.
* `valid.txt` and `test.txt`: Files for validation and testing. 
* `relation2text.txt` and `entity2text.txt`: Description file. Each line contains a relation (or entity) and corresponding descriptions, seperated by '\t'.
* `ranking_head.txt` and `ranking_tail.txt` (optional): Files of positive and negative testing triplets. If not provided, use the following command to generate:
```shell
python neg_sampling.py --dataset $dataset --suffix $suffix --finding_mode $finding_mode --training_mode test --neg_num 50 --seed $seed
```shell
