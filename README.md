# Semantic Segmentation

### Goals of the Project
The goal of the project is to label the pixels of a road in images using a Fully Convolutional Network (FCN).

[gif1]: ./runs/kitti_semantics1.gif "Results example 1"   
[gif2]: ./runs/kitti_semantics2.gif "Results example 2"   
[gif3]: ./runs/kitti_semantics3.gif "Results example 3"   

#### Dataset
The dataset used was the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php) from [here](http://www.cvlibs.net/download.php?file=data_road.zip).  Extract the dataset in the `data` folder.  This will create the folder `data_road` with all the training a test images. This folder was not included in GitHub's repo.

## Results

Here are some examples images that were infered by the network on unseen test images. It succeeds to paint in green every pixel where the road is detected:

![alt text][gif1]
![alt text][gif2]
![alt text][gif3]

## Approach Description

In this project I used a FCN8 architecture to classify traffic scene images at the pixel level. In this case we train on two classes. 'Drivable Road' or 'Non Road' detection.
The architecture of my network is based on the paper [Fully Convolutional Networks for Semantic Segmentation](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf).
I started using the pre-trained VGG16 CNN. Then I replaced the final fully connected layer by 1x1 convolutions and upsampled the network to be able to infer back on images. I also use skip layers so that the network does not lose finer resolution capability. For details about the implementation please read the refered paper.

In the python script 'main.py' is where we implement the main part of the program. Namely, building the network, training it and then infering predictions on the test data.
We use some helper functions defined in 'helper.py' and also some test methods defined in 'project_tests.py'.

The best results were achieved training for 50 epochs and choosing a learning rate of 1e-4 with an Adam Optimizer and cross entropy loss. The loss at the beginning of training was around 1.5 and at the end around 0.02. The new layers defined on top of VGG16 were initialized with random normal distribution (standard deviation of 0.01). For these layers an L2 regularizer with a value of 1e-3 was used. Dropout probability of neural connections in VGG was set to 0.5.

Training for more epochs (up to 100) and changes to the L2 regularizer did not help to further improve the results or decrease the loss.

You can find the the complete results (all infered images out of the test images) in the './runs/Loss_0p02_epochs50_Learningrate_1e-4/' folder.

### Setup
##### Frameworks and Packages
Make sure you have the following is installed:
 - [Python 3](https://www.python.org/)
 - [TensorFlow](https://www.tensorflow.org/)
 - [NumPy](http://www.numpy.org/)
 - [SciPy](https://www.scipy.org/)

##### Run
Run the following command to run the project:
```
python main.py
```
**Note** If running this in Jupyter Notebook system messages, such as those regarding test status, may appear in the terminal rather than the notebook.

