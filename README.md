[![Udacity - Robotics NanoDegree Program](https://s3-us-west-1.amazonaws.com/udacity-robotics/Extra+Images/RoboND_flag.png)](https://www.udacity.com/robotics)

## Deep Learning Project: Follow Me ##

In this project, I trained a deep neural network to identify and track a target in simulation. So-called “follow me” applications like this are key to many fields of robotics and the very same techniques I applied here could be extended to scenarios like advanced cruise control in autonomous vehicles or human-robot collaboration in industry.

The model is built in Tensor-flow and Keras, and the training was done using a K80 GPU enabled Amazon Web Services (AWS) compute instance.



[image_0]: ./docs/misc/followme.jpg
![alt text][image_0] 

## Archeticture
Many deep learning archetictures can be used to solve different machine proplems. SegNet is a deep learning architecture designed to solve image segmentation problem. The network consists of Convlutional layers, convolutional layers have successfully been applied to analyzing visual imagery. 


## Fully Convolutional Networks
an FCN consists of sequence of processing layers (encoders) followed by a corresponding set of reverse processing layers (decoders). By encoding and decoding convolutional layers, the model is able to preserve spatial information through the network.


### Encoder ###
The encoder portion can be of one or more encoders, each includes a separable convolution layer with batch normalization, and relu function. After that a maxpooling layer is added. 

Each encoder block allows the model to build on what it learns from the previous block. For example, the first layer distinguishes very basic characteristics in the image, such as lines, brightness and hue. The next layer is able to identify more complicated shapes, a combination of lines, curves as squares, circles and curves. until a fourth or fifth layer can identify faces and humans.


### Decoder ###

The decoder section is made of the same number as encoders, but with transposed convolution layers that reverse the regular convolution layers, with a Bilinear upsampling layer after.

Bilinear upsampling is the opposite of max pooling, it is a way to estimate the new pixel intensity value based on averaging neighbouring pixels. Bilinear upsampling has good performance, which is important for training large models quickly.


Each decoder layer is meant to reverse and to deconstruct more of the image. The final decoder layer generates an image with the same size as the original. The output image is used for guiding the quadcopter.


## Model ##
The model was made of twe encoders and two decoders, and a 1X1 convultion layer in between
The first and the seocod convolution uses a filter size of 32 and a stride of 2, and a filter size of 64 and a stride of 2 respectively.


## Neural Network parameters
These parameters were found using trial and error and knowledge gained through the labs
**learning rate** = 0.001  
**batch size** = 100  
**number of epochs** = 15  
**steps per epoch** = 250  
**validation steps** = 50  
**workers** = 2  


Increasing the epoch (one forward pass and one backward pass of all the training examples) was a sure way to increase the accuracy


## Training ##
The training was done on a p2.xlarge AWS instance. Training the model with the above hyperparameters required about 40 minutes


## Scoring ##

The final score of my model is 0.404, while the final IoU is 0.545.

## Finally:
This model was exclusively trained on the person in the simulation. the same model can be used for anything of similar complexity as a human. more layers will be needed to for more complex imagery. This model could be used to segment animals and vehicles, however, it will need the required training beforehand.



