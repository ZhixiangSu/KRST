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
you can reproduce the confidence results mentioned in the paper.

Also, you can just download the model trained by our team (currently not available because of anonymous requirement) and test them using
```shell
./test.sh
```

## Step by Step Implementation

### Dataset Preparation

All datasets mentioned in paper are provided in `data\data`. They are from [BERTRL](https://github.com/zhw12/BERTRL/blob/master/README.md) and are reorganized. If you want to mannually create a new dataset, please provide the following files:
* `train$suffix$.txt`: file for training. Each line is a triplet (h,r,t) seperated by '\t'.
* `valid.txt` and `test.txt`: files for validation and testing. 
* `relation2text.txt` and `entity2text.txt`: description file. Each line contains a relation (or entity) and corresponding descriptions, seperated by '\t'.
* `ranking_head.txt` and `ranking_tail.txt` (optional): files of positive and negative testing triplets. If not provided, use the following command to generate:
```shell
python neg_sampling.py --dataset $dataset --suffix $suffix --finding_mode $finding_mode --training_mode test --neg_num 50 --seed $seed
```

### Path Finding and Path Filtering

After preparing the dataset, you can do path finding and path filtering using the shell file 'generate.sh'. The parameters can be fine-tuned include:
* `npaths_ranking`: number of paths generated for each triplet.
* `support_type`: choice of path filtering function. 0: none, 1: relation path coverage 2: relation path confidence.
* `support_threshold`: threshold of path filtering function.
* `search_depth`: breadth-first search depth.

Then you can see the generated files in `data\relation_prediction_path_data\$dataset$\ranking_$finding_mode$$suffix$'.
If you want to skip this step, please download the generated files here (currently not available because of anonymous requirement).

### Sentence Formation and KRST training

Using file 'train.sh', you can format sentence, load pre-trained sentence transformer and train the model by yourself. The parameters can be fine-tuned include:
* `epochs`: upper epoch limit.
* `batch_size`: batch size.
* `learning_rate`: learning rate.
*  `neg_sample_num_train`, `neg_sample_num_valid` and `neg_sample_num_test`: number of negative samples (should be same as generated by negative_sampling.py)
*  `max_path_num`: number of paths loaded for each triplet.

After training, the best model is saved in `save\$dataset$$suffix$\relation_prediction_$finding_mode$\best_val.pth`.

### KRST testing

By default, KRST load the saved best validation model for testing. Run 'test.sh' to get the test results.
You can just download the model trained by our team (currently not available because of anonymous requirement) and test them.

