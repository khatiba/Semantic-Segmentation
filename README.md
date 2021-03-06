# Semantic Segmentation

### Introduction
The goal of this project is to build a Fully Convolutional Neural Network (FCN) starting from the VGG16 image classifier architecture. The network performs semantic segmenation on images classifying pixels as either drivable road or non-road. The KITTI data set is used for training and testing as well as tested on previous videos of highway driving.

#### Architecture
The conversion to FCN follows the procedures laid out in [this paper (Long and Shelhamer)](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf).

1. Layer 7, Pool 4 and Pool 3 are extracted from VGG16
1. 1x1 convolution is run on Layer 7
1. Upsample by 2x
1. 1x1 convolution is run on Pool 4 and added to result
1. Upsample by 2x
1. 1x1 convolution is run on Pool 3 and added to result
1. Upsample by 8x

The result combines coarse and fine features from pool 3, 4 and 7 to produce high resulution and more accurate boundaries.

##### Training Settings
* Optimizer: Adam
* Loss: Cross Entropy
* Learning Rate: 5e-4
* Keep Prob: 0.5
* Epochs: 40
* Batch Size: 5


#### Results on Sample Images
![Sample Image](./sample_images/um_000013.png)
![Sample Image](./sample_images/um_000014.png)
![Sample Image](./sample_images/um_000029.png)
![Sample Image](./sample_images/uu_000026.png)
![Sample Image](./sample_images/uu_000081.png)


#### Results on Lane Finding Videos
Not great, needs more work.

[![Video](./thumbnail.jpg)](https://youtu.be/kWfq41DDfzk)


##### Frameworks and Packages
Make sure you have the following is installed:
 - [Python 3](https://www.python.org/)
 - [TensorFlow](https://www.tensorflow.org/)
 - [NumPy](http://www.numpy.org/)
 - [SciPy](https://www.scipy.org/)

##### Dataset
Download the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php) from [here](http://www.cvlibs.net/download.php?file=data_road.zip).  Extract the dataset in the `data` folder.  This will create the folder `data_road` with all the training a test images.

### Tips
- The link for the frozen `VGG16` model is hardcoded into `helper.py`.  The model can be found [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/vgg.zip)
- The model is not vanilla `VGG16`, but a fully convolutional version, which already contains the 1x1 convolutions to replace the fully connected layers. Please see this [forum post](https://discussions.udacity.com/t/here-is-some-advice-and-clarifications-about-the-semantic-segmentation-project/403100/8?u=subodh.malgonde) for more information.  A summary of additional points, follow. 
- The original FCN-8s was trained in stages. The authors later uploaded a version that was trained all at once to their GitHub repo.  The version in the GitHub repo has one important difference: The outputs of pooling layers 3 and 4 are scaled before they are fed into the 1x1 convolutions.  As a result, some students have found that the model learns much better with the scaling layers included. The model may not converge substantially faster, but may reach a higher IoU and accuracy. 
- When adding l2-regularization, setting a regularizer in the arguments of the `tf.layers` is not enough. Regularization loss terms must be manually added to your loss function. otherwise regularization is not implemented.
