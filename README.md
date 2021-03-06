# CNN

This repository is a note directory for CNN - which covers the basic ideas of how CNN works.The second part of the notebook is on classic networks, the 3rd about object detection and YOLO Algorithim and the final part is face recognistion and Neural style transfer. 

The Notes are picked from Deep learning course by Andrew NG on Coursera for understanding purposes and reading.  
For Object detection there are two YOLO papers: Redmon et al., 2016 (https://arxiv.org/abs/1506.02640) and Redmon and Farhadi, 2016 (https://arxiv.org/abs/1612.08242).

We will be implementing the building blocks of a convolutional neural network! Each function we implement will have detailed instructions that will walk us through the steps needed - Convolution functions, including:
Zero Padding
Convolve window
Convolution forward
Convolution backward 
Pooling functions, including:
Pooling forward
Create mask
Distribute value
Pooling backward 

<img width="604" alt="model" src="https://user-images.githubusercontent.com/30907520/35636467-d1eab914-0676-11e8-8155-34a9a4e0114a.png">


The main benefits of padding are the following:

- It allows you to use a CONV layer without necessarily shrinking the height and width of the volumes. This is important for building deeper networks, since otherwise the height/width would shrink as you go to deeper layers. An important special case is the "same" convolution, in which the height/width is exactly preserved after one layer.

- It helps us keep more of the information at the border of an image. Without padding, very few values at the next layer would be affected by pixels as the edges of an image.

<img width="574" alt="pad" src="https://user-images.githubusercontent.com/30907520/35636604-4fd60900-0677-11e8-87ae-33f1ac5e6117.png">

The pooling (POOL) layer reduces the height and width of the input. It helps reduce computation, as well as helps make feature detectors more invariant to its position in the input. The two types of pooling layers are:

- Max-pooling layer: slides an ( f,f) window over the input and stores the max value of the window in the output.
- Average-pooling layer: slides an ( f,f) window over the input and stores the average value of the window in the output.

<img width="775" alt="max_pool1" src="https://user-images.githubusercontent.com/30907520/35638763-a44c13ac-067d-11e8-8a35-84845c49b48c.png">
<img width="757" alt="a_pool" src="https://user-images.githubusercontent.com/30907520/35638775-ad778f1a-067d-11e8-9ffe-b40f07aed584.png">

In TensorFlow, there are built-in functions that carry out the convolution steps for you.

- tf.nn.conv2d(X,W1, strides = [1,s,s,1], padding = 'SAME'): given an input  X
  and a group of filters  W1
 , this function convolves  W1
 's filters on X. The third input ([1,f,f,1]) represents the strides for each dimension of the input (m, n_H_prev, n_W_prev, n_C_prev). You can read the full documentation here

- tf.nn.max_pool(A, ksize = [1,f,f,1], strides = [1,s,s,1], padding = 'SAME'): given an input A, this function uses a window of size (f, f) and strides of size (s, s) to carry out max pooling over each window. You can read the full documentation here

- tf.nn.relu(Z1): computes the elementwise ReLU of Z1 (which can be any shape). You can read the full documentation here.

- tf.contrib.layers.flatten(P): given an input P, this function flattens each example into a 1D vector it while maintaining the batch-size. It returns a flattened tensor with shape [batch_size, k]. You can read the full documentation here.

- tf.contrib.layers.fully_connected(F, num_outputs): given a the flattened input F, it returns the output computed using a fully connected layer. You can read the full documentation here.

Model: CONV2D -> RELU -> MAXPOOL -> CONV2D -> RELU -> MAXPOOL -> FLATTEN -> FULLYCONNECTED. You should use the functions above.
In detail, we will use the following parameters for all the steps:
 - Conv2D: stride 1, padding is "SAME"
 - ReLU
 - Max pool: Use an 8 by 8 filter size and an 8 by 8 stride, padding is "SAME"
 - Conv2D: stride 1, padding is "SAME"
 - ReLU
 - Max pool: Use a 4 by 4 filter size and a 4 by 4 stride, padding is "SAME"
 - Flatten the previous output.
 - FULLYCONNECTED (FC) layer: Apply a fully connected layer without an non-linear activation function. Do not call the softmax here. This will result in 6 neurons in the output layer, which then get passed later to a softmax. In TensorFlow, the softmax and cost function are lumped together into a single function, which you'll call in a different function when computing the cost. 
 
 
#OBJECT DETECTION AND LOCALIZATION : 

Summary for YOLO:

- Input image (608, 608, 3)

- The input image goes through a CNN, resulting in a (19,19,5,85) dimensional output.

- After flattening the last two dimensions, the output is a volume of shape (19, 19, 425):

- Each cell in a 19x19 grid over the input image gives 425 numbers.425 = 5 x 85 because each cell contains predictions for 5 boxes, corresponding to 5 anchor boxes, as seen in lecture.85 = 5 + 80 where 5 is because  (pc,bx,by,bh,bw)(pc,bx,by,bh,bw)  has 5 numbers, and and 80 is the number of classes we'd like to detect

- You then select only few boxes based on:
    Score-thresholding: throw away boxes that have detected a class with a score less than the threshold
    Non-max suppression: Compute the Intersection over Union and avoid selecting overlapping boxes.This gives you YOLO's final output.


References: The ideas presented in this notebook came primarily from the two YOLO papers. The implementation here also took significant inspiration and used many components from Allan Zelener's github repository. The pretrained weights used in this exercise came from the official YOLO website.

- Joseph Redmon, Santosh Divvala, Ross Girshick, Ali Farhadi - You Only Look Once: Unified, Real-Time Object Detection (2015)
- Joseph Redmon, Ali Farhadi - YOLO9000: Better, Faster, Stronger (2016)
- Allan Zelener - YAD2K: Yet Another Darknet 2 Keras
- The official YOLO website (https://pjreddie.com/darknet/yolo/)


# Transfer Learning
Neural Style Transfer (NST) uses a previously trained convolutional network, and builds on top of that. The idea of using a network trained on a different task and applying it to a new task is called transfer learning.

# Face Recognition 
Face recognition problems commonly fall into two categories:

- Face Verification - "is this the claimed person?". For example, at some airports, you can pass through customs by letting a system scan your passport and then verifying that you (the person carrying the passport) are the correct person. A mobile phone that unlocks using your face is also using face verification. This is a 1:1 matching problem.

- Face Recognition - "who is this person?". For example, the video lecture showed a face recognition video (https://www.youtube.com/watch?v=wr4rx0Spihs) of Baidu employees entering the office without needing to otherwise identify themselves. This is a 1:K matching problem.

FaceNet learns a neural network that encodes a face image into a vector of 128 numbers. By comparing two such vectors, you can then determine if two pictures are of the same person.



