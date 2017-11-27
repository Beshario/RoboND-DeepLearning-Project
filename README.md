[//]: # (Image References)
[image_0]: ./docs/misc/Untitled.jpeg
[image_1]: ./docs/misc/1.png
[image_2]: ./docs/misc/2.png
[image_3]: ./docs/misc/3.png
[image_4]: ./docs/misc/11.png
[image_5]: ./docs/misc/22.png
[image_6]: ./docs/misc/33.png
[image_7]: ./docs/misc/44.png
[image_8]: ./docs/misc/55.png
[image_9]: ./docs/misc/66.png
[image_10]: ./docs/misc/diagram.gif
## Deep Learning Project: Follow Me ##

[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)



In this project, I trained a deep neural network to identify and track a target in simulation. So-called “follow me” applications like this are key to many fields of robotics and the very same techniques I applied here could be extended to scenarios like advanced cruise control in autonomous vehicles or human-robot collaboration in industry.

The model is built in Tensor-flow and Keras, and the training was done using an Amazon Web Services (AWS) compute instance.




![in Action][image_0] 

## Archeticture
Many deep learning archetictures can be used to solve different machine proplems. SegNet is a deep learning architecture designed to solve image segmentation problem. The network consists of Convlutional layers, convolutional layers have successfully been applied to analyzing visual imagery. 


## Fully Convolutional Networks
an FCN consists of sequence of processing layers (encoders) followed by a corresponding set of reverse processing layers (decoders). By encoding and decoding convolutional layers, the model is able to preserve spatial information through the network.


## Model ##

![in Action][image_10]:


The model was made of two encoders and two decoders, and a 1X1 convolution layer in between
The first and the second convolution uses a filter size of 32 and a stride of 2, and a filter size of 64 and a stride of 2 respectively.

### Encoder ###
The encoder portion can be of one or more encoders, each includes a separable convolution layer with batch normalization, and relu function. After that a maxpooling layer is added. 

Each encoder block allows the model to build on what it learns from the previous block. For example, the first layer distinguishes very basic characteristics in the image, such as lines, brightness and hue. The next layer is able to identify more complicated shapes, a combination of lines, curves as squares, circles and curves. until a fourth or fifth layer can identify faces and humans.

### 1X1Convolution ###
1X1 Convolution layer adds an inexpensive non-linear classifier to the model, it also adds depth and can be very powerful when used with inception. 1X1 convolution helps in reducing the depth dimensionality of the layer. A fully-connected layer of the same size would result in the same number of features. However, with convolutional layers you can feed images of any size into the trained network.

### Skip connections ###
is a way to retain some data by skipping a certain number of pairs (one encoder one decoder).The skipped connection image is used in the element-wise addition with the image going through C as shown in the image above. these connections allows the network to use data from different resolutions from the network.

### Decoder ###

The decoder section is made of the same number as encoders, but with transposed convolution layers that reverse the regular convolution layers, with a Bilinear upsampling layer after.

Bilinear upsampling is the opposite of max pooling, it is a way to estimate the new pixel intensity value based on averaging neighbouring pixels. Bilinear upsampling has good performance, which is important for training large models quickly.


Each decoder layer is meant to reverse and to deconstruct more of the image. The final decoder layer generates an image with the same size as the original. The output segmented image is used for guiding the quadcopter.

## Neural Network parameters
These parameters were found using trial and error and knowledge gained through the labs  
**learning rate** = 0.001
**batch size** = 100  
**number of epochs** = 15  
**steps per epoch** = 250  
**validation steps** = 50  
**workers** = 2  

Lowering the learning rate made it possible for us to acheive a 40% overall IoU. an attempt at 0.05 learning rate was not successfull and ended up with 37 %overall IoU.  
I also increased the batch size to 250, but all that caused was slower performance. A studied decision was made to increase both the number of epochs (epoch: one forward pass and one backward pass of all the training examples) and the steps per epoch, which acheived our current accracty.




## Training 
The training was done on a p2.xlarge AWS instance. Training the model with the aforementioned Fully Convolutional Networks required about 40 minutes.


## Scoring 

The final score of the model is 0.404, while the final Intersection over Union of  0.545

The produced Images are as follows:

**on a closed range as the quad is following the person, it had a IoU of 0.85**

![in Action][image_1]:
![in Action][image_2]:
![in Action][image_3]:

**But when the quad was patrolling and the target was not around, it had a IoU of identifying other peope 0.67 with 97 false positives. a false postives is constituted when the model determines an existence of the targed in three pixels or more with more than 50 certainty.**

![in Action][image_4]:
![in Action][image_5]:
![in Action][image_6]:


**And finally when the quad was patrolling and the target was around, it had an IoU of 0.42 of identifying people and 0.23 of identifying the target. **

![in Action][image_7]:
![in Action][image_8]:
![in Action][image_9]:

## Future enhancement
The error resulted when the quad was patrolling and the target was around is due to the lack of distant training images of the target, a better score could be gained through taking more images. In addition, a deeper layer could have been implemented instead of going wider.
Sources:

## Finally:
This model was only trained on the person in the simulation. the same model can be used for anything of similar complexity as a human. more layers are needed for more complex imagery. This model could be used to segment animals and vehicles, however, it will need the required training beforehand.


[SegNet: A Deep ConvolutionalEncoder-Decoder Architecture for Image Segmentation](https://arxiv.org/pdf/1511.00561.pdf)  
[Literature Review: Fully Convolutional Networks - David Silver](https://medium.com/self-driving-cars/literature-review-fully-convolutional-networks-d0a11fe0a7aa)  
[What does 1x1 convolution mean in a neural network? (Cross Validated)](https://stats.stackexchange.com/questions/194142/what-does-1x1-convolution-mean-in-a-neural-network)

