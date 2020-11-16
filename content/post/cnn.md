---
title: "Image Classification Using CNN"
date: 2020-11-17T03:05:08+08:00
draft: False
---

Neural network is an algorithmic mathematical model that imitates the behavioral characteristics of animal neural networks to perform distributed parallel information processing. This kind of network depends on the complexity of the system, and adjusts the interconnected relationship between a large number of internal nodes to achieve the purpose of processing information. The Neural network model was first established by Warren McCulloch and Walter Pitts in 1943. Their model is entirely based on mathematics and algorithms, which could not be tested due to lack of computing resources at that time. After years of evolution, neural network has become one of the most popular machine learning methods. Convolutional Neural Network (CNN) is the most popular neural network for dealing with pictures and sound.

## Neural Network

To begin with, let’s consider how human process information to make a decision. For example, one needs to determine whether to play tennis outside today. He/she needs to take weather, temperature, companions, physical status and many other factors into consideration. But the degree to which these factors might influence the final result are different. Specifically, our brain will put different weights on each factor, then do some hidden computing to activate certain neurons in determining if we are going to play or not. In simple terms, neural network is a complex function which can output desired data (decision) from the input data (information). This is exactly how artificial neural nets work. Artificial neural network consists of several elements: neurons, weights, biases and activation functions. The model is illustrated in Figure 1:

![fig 1](/Picture2.png)

## Convolutional Neural Network

Knowing the structure of image input, it is intuitive why we cannot use classic neural network to process images. The first difficulty is too many parameters. For a 32*32*3 image, a single fully connected first hidden layer of the normal neural network will have 32*32*3 = 3072 weights. This number is still controllable, but clearly this fully connected structure will not expand to larger images. For an image of a more appreciable size, such as 200*200*3, would result in neurons with a number of weights of 200*200*3 = 120,000. In addition, we will almost certainly have several such neurons which will surge the parameters. Obviously, this complete connection is wasteful, and a large number of parameters will quickly lead to overfitting. Overfitting is a modeling error which occurs when a model is too closely fitted to a limited set of training data, which will result in an over-complex model that loses power when applied to new data sets.   

The second difficulty involves the characteristics of image information. For image recognition tasks, the connection between each pixel and its surrounding pixels is relatively close, and the connection with pixels far away may be very small. In classic neural network, all neurons are fully connected with neurons in the previous layer. It is equivalent to treating all pixels of the image equally and independently. The location information between pixels is not used. When we finish learning the weight of each connection, we may eventually find that there are a lot of weights with very small values. Such learning of unimportant weights will be very inefficient.  

The third obstacle is network layer limit. We know that the more network layers, the stronger the expressive ability. However, it is difficult to train a deep artificial neural network by conventional gradient descent algorithm. Therefore, we cannot build a deep fully connected neural network, which limits its ability.  

So how does CNN solve the problem? The idea is also three-fold.  
1.	Local connection. This is the easiest idea one can think of. Each neuron is no longer connected to all neurons in the previous layer, but only to a small number of neurons. This reduces the number of parameters.
2.	Weight sharing. A group of connections can share the same weight, instead of each connection having a different weight.  
3.	Down-sampling. We can use pooling (pool several digits into one) to reduce the number of samples per layer, further reduce the number of parameters and improve the robustness of the model. For image recognition tasks, convolutional neural networks achieve better learning results by retaining important parameters as much as possible and removing a large number of unimportant parameters.  

The structure of CNN is similar to that of conventional neural network. Each layer of CNN converts one activated value to another through a differentiable function. Generally speaking, CNN has three types of layers: a convolution layer, a pooling layer and a fully connected layer FC (as seen in conventional neural networks). There will generally be an activation function before the layer, we will stack these layers to form a complete architecture. Figure 2 roughly illustrates the architecture of CNN:

![fig 2](/Picture1.png)

In sum, CNN is an efficient machine learning method with regard to image and sound processing. It directly takes the original image as input, and uses the final regression or classification problem as the output. It has both internal filtering image processing and function fitting, where all parameters are trained together.  The convolution layer and pooling layer are equivalent to feature engineering, which extract useful features from raw data. The full connection behind is equivalent to feature weighting. The last fully connected layer acts as a "classifier" in the entire convolutional neural network.  

Below is a little demo of implementing CNN model in Python to classify images of beetles and dragonflies.


We start by reading all images from the folder and save it in numpy arrays.

```{Python}
# read all images for beetles dataset
import os

def load_images_from_folder(folder):
    images = []
    for filename in os.listdir(folder):
        imgg = Image.open(os.path.join(folder,filename))
        img = np.asarray(imgg.resize((100, 100)))
        if img is not None:      
    # append to list of all images
            images.append(img)

    return images

```
After some transformations of the original data, we can design our CNN model by adding layers.

```{Python}
# set model constants
num_classes = 1

# define model as Sequential
model = Sequential()

# first convolutional layer with 32 filters
# 1 input is 1 pixel
model.add(Conv2D(32, kernel_size=(3, 3), activation='relu', input_shape=(100, 100, 3)))
# add a second 2D convolutional layer with 64 filters
model.add(Conv2D(64, kernel_size = (3, 3), activation = 'relu', input_shape = (100, 100, 3)))
# reduce dimensionality through max pooling
model.add(MaxPooling2D(pool_size = (2, 2)))

# third convolutional layer with 64 filters
model.add(Conv2D(64, kernel_size=(3, 3), activation='relu'))
# add dropout to prevent over fitting
model.add(Dropout(0.25))
# necessary flatten step preceeding dense layer
model.add(Flatten())
# fully connected layer
model.add(Dense(128, activation='relu'))

# add additional dropout to prevent overfitting
model.add(Dropout(0.5))

# prediction layers
model.add(Dense(num_classes, activation='sigmoid', name='preds'))

# show model summary
model.summary()

model.compile(
    # set the loss as binary_crossentropy
    loss=keras.losses.binary_crossentropy,
    # set the optimizer as stochastic gradient descent
    optimizer=keras.optimizers.SGD(lr=0.001),
    # set the metric as accuracy
    metrics=['accuracy']
)

# train the model using the first ten observations of the train and test sets
model.fit(X, y, epochs=5, verbose=1
)
```

After training the model, we can get the training accuracy. The full code can be found [here](https://github.com/cassie1102/mini-project).
