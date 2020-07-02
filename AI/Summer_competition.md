## A Surface Defect Detection Method Based on Positive Samples
(data is based on concentrated fabric images)
#### Reconstruction network

#### Image local contrast

#### Two-stage method
combines the region proposals

#### FCN
The structure of FCN
(based on FCN semantic segmentation network)

#### Generator $G$ and Discriminator $D$
$G$ generates an image after receiving a Gaussian random signal.
$D$ receives a true or false picture and gives the percentage of whether this image is true or not.

#### Reconstruction error 
middle layer?

by comparing the original one with the fixed one image, we can get the answer whether it is a defect based on the reconstruction error. The bigger RE is, the bigger potential it would be.

Cons:
hard to know the difference between the RE and the small defect by direct subtraction because of the error when it reconstructs and repairs the figure.

#### basis of image repair

#### Autoencoder
data-dependent or data-specific. 
The performance on compressing figure is not good while it can learn from the data and get the *characters* from the data.

#### Skip connections
(U-Net)
(Not used in this model)

#### image translation task

#### LBP


#### Method
Generate the defect samples manually and feed them to $G$ which is the generator in GAN consisting of encoder and decoder (autoencoder). Then the $G$ reconstructs or repairs the samples and feeds them to $D$ which is the discriminator.

The core is $G$ and in the test processing, the LBP algorithm is used to extract features of original figures and the repaired figures. The measure to judge whether it belongs to defect sample is based on the number of difference between original figures and generated figures.

#### Structure
(DCGAN)

#### Loss functions
two parts:
$L_{COUNTS}$ is L1-distance
$L_{GAN}$ is loss function in the part of GAN, which can improve the quality of figures and descriptions about feature details.

#### questions
1. artificial defect module. generate samples ?


## Plan
1. 问题解读
2. 提问
- 两种数据是要训练两个模型吗？还是说训练完一种数据后接着训练另一种然后交一个模型？  

- 正样本的定义：就是无缺陷

- 给定的数据中训练集是没有缺陷的数据的，需要我们自己创建缺陷数据集

- FCN效果还行，但是耗时长，还有ResNet，如果合用，不算时间的影响，会有提升吗

- 竞赛 自行标注吗？

