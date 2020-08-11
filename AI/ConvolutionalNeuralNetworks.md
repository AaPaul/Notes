### Week 3
#### Object Localization
1. The upper left corner of a picture is (0, 0), while the lower right is (1, 1) in localication.
2. There are 4 parameters for this task which are (x, y, w, h). x and y are coordinates the center of this bounding box and w and h are corresponding for the width and height of that box respectively.
3. The label y consists of 3 parts. The first part (assume its $p_c$) is determine whether there is an object or not. The following part includes 4 parameters for marking the position of the bounding box. The last is composed of classes we gonna predict. **Note that if $p_c$ is 0, the rest are "don't care".**
4. The lost function is,

$$L(\hat{y}-y) = \sum(\hat{y_i}-y)^2$$ 
Same as the above, if $p_c$ is 0, we don't care the rest parameters in loss function as well.

#### Landmark Detection
1. Examples: face landmark detection, pose landmark annotation.

#### Object Detection
1. Sliding windows detection. Firstly, it trains a *ConvNet* with cropped images where there are pretty much only objects to detect whether there is the desired objects or not. Then it feed it to an input. There is a window with specific shape. It will be used to pass through the whole input and in each slides the *ConvNet* will determine whether there is the aimed object or not. After passing through whole image, the window size will be changed and the new one will do the same process.


### Week 4
#### One shot learning
We cannot train the ConvNet which is for face verification again when adding a new data. 
The key to solve face verification is to carry out the similarity between the data in the database and the object. 

$d(img1, img2) :=$ degree of difference between images \
$d \leq \tau:$ same \
$d > \tau:$ different

#### Siamese Network

#### Nerual style transfer
Change the style of a picture to the style of anther one. For example, a actual picture can be changed to the style like a manga or an anima.

#### Cost function
The meaning of $J(C, G)$ is the difference between the Content image $C$ and the generated image $G$. $J(S, G)$ is to determine the similarity of the Style image $S$ (the aimed style) and the generated image $G$.

$$J(G) = \alpha J(C, G) + \beta J(S, G) $$

####


## 面试记录（2020-06-04）
问了:\
Dropout及其数学原理, AlexNet, VGG的结构
Kmeans的原理以及DBSCAN的原理
什么是yolo以及其loss function

不记概念，不记原理，别人就会认为你不会 \
滚去看论文

## Week 1
#### Data set split
In the case where the data set includes billion or million data, we may split the train/dev/test set into 98%/1%/1% and even much more imbalanced as the purpose of the dev set and the test set are to estimate the performance of models. Note the distribution of the dev set and test set should be the same.

#### Bias / Variance
Bias: It is resprented by the error rate on training set. If the error rate is high, it means the algorithm is in high Bias (under-fitting) Note: Because of wrong learning method, the performance on testing set would also be awful.

Variance: It reflects the performance on the testing set. With higher error rate, the algorithm is more likely in high variance (over-fitting: not good on generalization). 

#### Basic Recipe for ML
First,  
High Bias Solution:
- Bigger network
- Training longer
- New NN architecture.

Second,  
High Variance Solution:
- Getting more data
- Regularizition
- New NN architecture.

Note: Different methods can be not useful on different problems

#### Regularization
`w` is high dimentional number parameters while `b` is just a single number so that we often omit the `b` and only focus on `w`.
Typically, we use 'hold-out' to do fine-tune on $\lambda$
In logistic regression,

In Neural network,

- The reason why regularizations work on preventing overfitting is that it reduces the proportion of neural units who have positive effects on training model. Because of ignoring (transfer these units into inactive status) them, the model becomes simpler as it becomes like a linear model. Of course, it is still more complex than the linear model in logistic regression.