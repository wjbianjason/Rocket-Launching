Rocket Launching
==============

PyTorch code for "Rocket Launching: A unified and effecient framework for 
                            training well-behaved light net" <https://arxiv.org/abs/><br>

## About this code
This code is based on the [attention-transfer code](https://github.com/szagoruyko/attention-transfer), the code uses PyTorch <https://pytorch.org>.

What's in this repo so far:
 * Rocket-Interval code for CIFAR-10,CIFAR-100 experiments
 * Code for Rocket-Bottle (ResNet-16-ResNet-40)
 * gradient block
 * parameter sharing

bibtex:
```
@inproceedings{Zagoruyko2017AT,
    author = {Sergey Zagoruyko and Nikos Komodakis},
    title = {Paying More Attention to Attention: Improving the Performance of
             Convolutional Neural Networks via Attention Transfer},
    booktitle = {ICLR},
    url = {https://arxiv.org/abs/1612.03928},
    year = {2017}}
```

## Requirements

First install [PyTorch](https://pytorch.org), then install [torchnet](https://github.com/pytorch/tnt):

```
pip install git+https://github.com/pytorch/tnt.git@master
```

Then install [OpenCV](https://opencv.org) with Python bindings (e.g. `conda install -c menpo opencv3`), and other Python packages:

```
pip install -r requirements.txt
```

## Experiments

### Table 1 

This section describes how to get the results in the table 1 of the paper.

Third column, train student basic:

```
python rocket_interval.py --save logs/resnet_16_1_basic --depth 16 --width 1
python rocket_interval.py --save logs/resnet_16_2_basic --depth 16 --width 2
python rocket_bottle.py --save logs/resnet_bottle_16_1_basic --depth 16 --width 1
```

Ninth column, train booster only:

```
python rocket_interval.py --save logs/resnet_40_1_booster --depth 40 --width 1
python rocket_interval.py --save logs/resnet_40_2_booster --depth 40 --width 2
```

Fouth column, train attention transfer:
```
python rocket_interval.py --save logs/at_16_1_40_1 --width 1 --teacher_id resnet_40_1_booster --beta 1e+3
python rocket_interval.py --save logs/at_16_2_40_2 --width 2 --teacher_id resnet_40_2_booster --beta 1e+3
```

Fifth column, train with kd:
```
python rocket_interval.py --save logs/kd_16_1_16_2 --width 1 --teacher_id resnet_40_1_booster --alpha 0.9
python rocket_interval.py --save logs/kd_16_2_16_2 --width 2 --teacher_id resnet_40_2_booster --alpha 0.9
python rocket_bottle.py --save logs/kd_bottle_16_1_40_1 --teacher_id resnet_40_1_booster --alpha 0.9
```

Sixth column and eighth column, train rocket launching:
```
python rocket_interval.py --save logs/rocket_interval_16_1_40_1 --width 1 --student_depth 16  --depth 40 --gamma 0.03 
python rocket_interval.py --save logs/rocket_interval_16_2_40_2 --width 2 --student_depth 16  --depth 40 --gamma 0.03 
python rocket_bottle.py --save logs/rocket_bottle_16_1_40_1 --width 1 --student_depth 16  --depth 40 --gamma 0.03 
```

Seventh column, train rocket launching with kd:
```
python rocket_interval.py --save logs/rocket_interval_16_1_40_1 --width 1 --student_depth 16  --depth 40 --gamma 0.03 --alpha 0.9
python rocket_interval.py --save logs/rocket_interval_16_2_40_2 --width 2 --student_depth 16  --depth 40 --gamma 0.03 --alpha 0.9
python rocket_bottle.py --save logs/rocket_bottle_16_1_40_1 --width 1 --student_depth 16  --depth 40 --gamma 0.03 --alpha 0.9
```

### Table 2
rocket launching without gradient block
```
python rocket_interval.py --save logs/rocket_interval_16_1_40_1_no_gb --width 1 --student_depth 16  --depth 40 --gamma 0.03 --grad_block False
```
rocket launching without parameter sharing
```
python rocket_interval.py --save logs/rocket_interval_16_1_40_1_no_ps --width 1 --student_depth 16  --depth 40 --gamma 0.03 --param_share False
```

### Table 4 (only Cifar 100)
The same running command with Table 1
just add parameter --dataset CIFAR100

For example: rocket-launching
```
python rocket_interval.py --save logs/rocket_interval_16_1_40_1_cifar100 --width 1 --student_depth 16  --depth 40 --gamma 0.03 --dataset CIFAR100
```


