# Few-Shot Meta-Baseline

This repository contains the code for Better Generalized Few-Shot Learning \\ Despite No Base Data

<img src="https://user-images.githubusercontent.com/10364424/76388735-bfb02580-63a4-11ea-8540-4021961a4fbe.png" width="600">



## Main Results

*The models on *miniImageNet* use ResNet-12 as backbone,  and *tieredImageNet* use ResNet-18.


####


## Running the code

### Preliminaries

**Environment**
- Python 3.7.4
- Pytorch 1.9.0

**Datasets**
- [miniImageNet](https://drive.google.com/file/d/1fJAK5WZTjerW7EWHHQAR9pRJVNg1T1Y7/view?usp=sharing) (courtesy of [Spyros Gidaris](https://github.com/gidariss/FewShotWithoutForgetting))
- [tieredImageNet](https://drive.google.com/open?id=1nVGCTd9ttULRXFezh4xILQ9lUkg0WZCG) (courtesy of [Kwonjoon Lee](https://github.com/kjunelee/MetaOptNet))
- [ImageNet-800](http://image-net.org/challenges/LSVRC/2012/)

Download the datasets and link the folders into `materials/` with names `mini-imagenet`, `tiered-imagenet` and `imagenet`.
Note `imagenet` refers to ILSVRC-2012 1K dataset with two directories `train` and `val` with class folders.

When running python programs, use `--gpu` to specify the GPUs for running the code (e.g. `--gpu 0,1`).
For Classifier-Baseline, we train with 4 GPUs on miniImageNet and tieredImageNet and with 8 GPUs on ImageNet-800. Meta-Baseline uses half of the GPUs correspondingly.

In following we take miniImageNet as an example. For other datasets, replace `mini` with `tiered` or `im800`.
By default it is 1-shot, modify `shot` in config file for other shots. Models are saved in `save/`.

### 1. Pre-training
```
python train_classifier.py --config configs/train_classifier_mini.yaml
```

(The pretrained model can be downloaded [here](https://www.dropbox.com/scl/fo/nw3syorhlme1tsnvq1rsd/h?dl=0&rlkey=f4sw6pop547i2ws1mr4dxncw9))


### 2. Fine-tuning

To test the performance, modify `configs/test_few_shot.yaml` by setting `load_encoder` to the saving file of Pre-training.

E.g., `load: ./save/mini-iamgenet/max-va.pth`

Then run
```
python test_few_shot.py --shot 1
```

## Advanced instructions

### Configs

A dataset/model is constructed by its name and args in a config file.

For a dataset, if `root_path` is not specified, it is `materials/{DATASET_NAME}` by default.

For a model, to load it from a specific saving file, change `load_encoder` or `load` to the corresponding path.
`load_encoder` refers to only loading its `.encoder` part.

In configs for `train_classifier.py`, `fs_dataset` refers to the dataset for evaluating few-shot performance.

In configs for `train_meta.py`, both `tval_dataset` and `val_dataset` are validation datasets, while `max-va.pth` refers to the one with best performance in `val_dataset`.


### Citation
```
@inproceedings{chen2021meta,
  title={Meta-Baseline: Exploring Simple Meta-Learning for Few-Shot Learning},
  author={Chen, Yinbo and Liu, Zhuang and Xu, Huijuan and Darrell, Trevor and Wang, Xiaolong},
  booktitle={Proceedings of the IEEE/CVF International Conference on Computer Vision},
  pages={9062--9071},
  year={2021}
}
```
