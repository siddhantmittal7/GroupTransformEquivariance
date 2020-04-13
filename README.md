# Group Equivariance Convolution Nerual Network

In my class presentation we talked about colen and Bekkers work on GCNN. In this is demo code which try to achive equivaraince by gourp transformation which are not limited ot discrete tranformation groups(Colen 2016) and rotaion and transaltion (Bekkers 2018). The advances of this system is

1. Can you used for continous groups transformation.
2. Avoid data argumenation which eats up network complexity.
3. Garaunties equivarance that is input after group tansformation are mapped to the same output result.
4. Other transformation can be implemented eaily in this network.

This code work is extended work of Equivariant tranformer network by Kai Sheng Tai. More details can be found in our ICML 2019 paper: https://arxiv.org/abs/1901.11399.

## Motivation

Consider, for example, the problem of classifying street signs in real-world images. In this domain, we know that the appearance of a sign in an image is subject to various deformations: the sign may be rotated, its scale will depend on its distance, and it may appear distorted due to perspective in 3D space. Regardless, the identity of the street sign should remain invariant to these transformations.

## Transformation considers for this demo

Transformation considered are affine tranformations

<p align="center">
  <img align="middle" src="./assets/translation.png" alt="Predicted transformations" width="500" />
</p>

<p align="center">
  <img align="middle" src="./assets/reflection.png" alt="Predicted transformations" width="500" />
</p>

<p align="center">
  <img align="middle" src="./assets/scale.png" alt="Predicted transformations" width="500" />
</p>

<p align="center">
  <img align="middle" src="./assets/rotation.png" alt="Predicted transformations" width="500" />
</p>

<p align="center">
  <img align="middle" src="./assets/sheer.png" alt="Predicted transformations" width="500" />
</p>

## Datasets

Dataset I have used for this is MNIST. Which is data of hand written digits. I have transformed images in this dataset to be used for my demo.

<p align="center">
  <img align="middle" src="./assets/dataset.png" alt="Examples of MNIST digits distorted by projective transformations" width="500" />
</p>

Dataset is availabe at: http://yann.lecun.com/exdb/mnist/

## Network Architectures

1. Used a CNN architecture based on the Z2CNN from Cohen & Welling (2016), with 7 layers of 3×3 convolutions with 32 channels, batch normalization after convolutional layers, and dropout after the 3rd and 6th layers. 
2. Heuristic approach of using a sequence of tranformation layers, each of which aims to remove the effect of its corresponding subgroup of the full transformation group.
3. Each tranformer layer predicts a pose parameter whose inverse transformation is applied to the input image.
4. The pose predictor networks are 3-layer CNNs with 32 channels in each layer. We selected the transformation order, dropout rate, and learning rate schedule based on validation accuracy.

## Results

Various networks which different transformation goup Vs Accuarcy

| Tranformations  | Accuracy  |
| ---| ---|
| Translation, Rotation  | 83.68%  |
| Translation, Rotation, Scale  | 86.27% |`
| Translation, Rotation, Scale, Sheer |  85.68% |
| Translation, Rotation, Scale, Sheer, reflection |  |
| Translation, Rotation, Scale, Sheer, reflection, funny distortion | 22%  | 

## Conclusion and future directions

1. Stacking tranformation layers is ultimately a heuristic approach as it does not guarantee equivariance with respect to the full transformation group.
2. Higher-dimensional groups require the use of long sequences of tranformation layers, resulting in high computational cost. 
3. In such cases, we could employ a hybrid approach where “difficult” subgroups are handled by tranformation layers, while the remaining degrees of freedom are handled by G-CNNs as suggested by Bekkers (2018).
4. In general, enforcing equivariance guarantees for higher-dimensional transformation groups in a computationally scalable fashion remains an open problem.

