# Goup Equivariance Convolution Nerual Network

In my class presentation we talked about colen and Bekkers work on GCNN. In this is demo code which try to achive equivaraince by gourp transformation which are not limited ot discrete tranformation groups(Colen 2016) and rotaion and transaltion (Bekkers 2018). The advances of this system is

1. Can you used for continous groups transformation.
2. Avoid data argumenation which eats up network complexity.
3. Garaunties equivarance that is input after group tansformation are mapped to the same output result.
4. Other transformation can be implemented eaily in this network.

This code work is extended work of Equivariant tranformer network by Kai Sheng Tai. More details can be found in our ICML 2019 paper: https://arxiv.org/abs/1901.11399.

## Motivation

Consider, for example, the problem of classifying street signs in real-world images. In this domain, we know that the appearance of a sign in an image is subject to various deformations: the sign may be rotated, its scale will depend on its distance, and it may appear distorted due to perspective in 3D space. Regardless, the identity of the street sign should remain invariant to these transformations.

## Requirements

- python >=3.6
- pytorch >=1.0 (https://pytorch.org/get-started/locally/)
- fire (`pip install fire` / `conda install fire -c conda-forge`)

## Transformation considers for this demo

<p align="center">
  <img align="middle" src="./assets/translation.png" alt="Predicted transformations" width="500" />
</p>

---

## Requirements

- python >=3.6
- pytorch >=1.0 (https://pytorch.org/get-started/locally/)
- fire (`pip install fire` / `conda install fire -c conda-forge`)

---

## Datasets

<p align="center">
  <img align="middle" src="./assets/projective-mnist.png" alt="Examples of MNIST digits distorted by projective transformations" width="500" />
</p>

To download and preprocess datasets, run:

```bash
python datasets.py projective_mnist --data_dir=<PATH>
``` 

for the Projective MNIST dataset, and

```bash
python datasets.py svhn --data_dir=<PATH>
```

for the SVHN dataset.

This will download the requested dataset to the directory indicated by `PATH` and will write three files: `train.pt`, `valid.pt` and `test.pt`.
These files will be used by the experiment scripts.

---

## Pretrained Models

Pretrained models can be found in the `pretrained` directory.

These can be loaded by simply setting the `load_path` argument for the corresponding `Model` subclass:

```python
from experiment_mnist import MNISTModel
from experiment_svhn import SVHNModel

mnist_model = MNISTModel(load_path='pretrained/etn-projmnist-8x.pt')
svhn_model = SVHNModel(load_path='pretrained/etn-resnet34-svhn.pt')
```

---

## Usage

There are two scripts for running the experiments described in the paper: `experiment_mnist.py` and `experiment_svhn.py`.
These scripts come with preset hyperparameters for each task that can be overridden by setting the corresponding flags.

To train a model on the Projective MNIST dataset, run:

```bash
python experiment_mnist.py train --train_path <PATH>/train.pt --valid_path <PATH>/valid.pt [--save_path <SAVE_PATH>]
```

The `save_path` flag lets us specify a path to save the model that achieves the best validation accuracy during training.

To change the set of transformers used by the model, we can use the `tfs` flag to specify a list of class names from the `etn.transformers` module. For example:

```bash
python experiment_mnist.py train ... --tfs "[ShearX, HyperbolicRotation]"
```

To train a model without any transformers, we can simply set `tfs` to the empty list `[]`:

```bash
python experiment_mnist.py train ... --tfs []
```

We can also set the coordinate transformation that's applied immediately prior to classification by the CNN:

```bash
python experiment_mnist.py train ... --coords logpolar
```

Therefore, to train a bare-bones model without any transformer layers or coordinate transformations, we can run:

```bash
python experiment_mnist.py train ... --tfs [] --coords identity
```

Feel free to play around with different combinations of transformers and coordinate systems!

To train a non-equivariant model, we can set the `equivariant` flag to `False`:

```bash
python experiment_mnist.py train ... --equivariant False
```

To change the device used for training, we can set the `device` flag (this is set to `cuda:0`) by default:

```bash
python experiment_mnist.py train ... --device cuda:1
```

To evaluate a saved model on the test set, run:

```bash
python experiment_mnist.py --load_path <SAVE_PATH> test --test_path <PATH>/test.pt
```

The experiments on the SVHN dataset can be run in same manner by calling `experiment_svhn.py` instead of `experiment_mnist.py`.

These scripts can also be called from a Jupyter Notebook by importing the `MNISTModel` and `SVHNModel` classes.
In a notebook, we can visualize training progress using the `show_plot` parameter.
This produces a live plot of the loss and validation error as training progresses.

```python
from experiment_mnist import MNISTModel
model = MNISTModel(...)
model.train(..., show_plot=True, ...)
```

For more training options, see the `__init__` and `train` functions for the base `experiments.Model` class and its subclasses in `experiment_mnist` and `experiment_svhn`.

---

## Citation

If you've found this repository useful in your own work, please consider citing our paper:

```
@inproceedings{tai2019equivariant,
  title={{Equivariant Transformer Networks}},
  author={Tai, Kai Sheng and Bailis, Peter and Valiant, Gregory},
  booktitle={International Conference on Machine Learning},
  year={2019}
}
```


