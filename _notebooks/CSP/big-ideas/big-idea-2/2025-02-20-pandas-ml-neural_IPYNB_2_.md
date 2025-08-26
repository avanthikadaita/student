---
layout: post
categories: ['CSP Big Idea 2']
title: ML | Neural Network | Handwritting Detection
description: Handwritting detection using Tensor Flow
courses: {'csp': {'week': 25}}
type: ccc
---

## Sample Neural Network
Here's an example of building a simple neural network using Python and Pandas to classify images of handwritten digits from the MNIST dataset.

This example solves the problem of classifying images of handwritten digits from the MNIST dataset. The goal is to build a neural network that can accurately predict the digit shown in the input image. The problem is a classic machine learning problem and is often used as a benchmark for evaluating different algorithms and models. The example uses a simple neural network with two fully connected layers to achieve high accuracy in classifying the digits.


```python
# uncomment the following lines to install the required packages
# !pip install tensorflow
# !pip install pandas
# !pip install numpy

import pandas as pd
import numpy as np
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

# Load the MNIST dataset, a dataset of handwritten digits
# The MNIST dataset is a dataset of 60,000 28x28 grayscale images of the 10 digits
# Data is split into a training set of 50,000 images and a test set of 10,000 images 
# The training and test data is structured as a 3-dimensional array of instance, image width and image height.
# X_train: is the image data, 50000 images, each element is 28x28 gray scale image
# y_train: is the label data, 50000 labels, each label corresponds to a handwritten digit
# X_test: is the image data, 10000 images, each element is 28x28 gray scale image 
# y_test: is the label data, 10000 labels, each label corresponds to a handwritten digit
mnist = tf.keras.datasets.mnist
(X_train, y_train), (X_test, y_test) = mnist.load_data()

# Flatten the input images
# Neural networks expect the input to be a 1D array of numbers
# X_train and X_test are reshaped to 1D array of 784 from 2D array of 28x28
X_train = X_train.reshape(X_train.shape[0], -1)  # X_train.shape[0]: 50000, -1: 784, 28x28
X_test = X_test.reshape(X_test.shape[0], -1) #`X_test.shape[0]: 10000, -1: 784, 28x28`

# Normalize the data
# X_train, X_test are normalized from and integer between 0-255 to a float32 between 0-1
X_train = X_train.astype('float32') / 255
X_test = X_test.astype('float32') / 255

# Convert the target variable to one-hot encoded vectors
# For each label all elements are 0 except for the index of the number which is 1
# For example, 5 is converted to [0, 0, 0, 0, 0, 1, 0, 0, 0, 0], starting from left counting 0,1,2,3,4,5 in this example
# y_train, y_test are converted to one-hot encoded vectors
# num_classes: 10, number of elements in the one-hot encoded vectors
y_train = tf.keras.utils.to_categorical(y_train, num_classes=10)
y_test = tf.keras.utils.to_categorical(y_test, num_classes=10)

# Define a new Sequential model, for a Neural Network (NN)
# Sequential model, provide by Keras, is a linear stack of layers
# Each layer has exactly one input tensor and one output tensor, a tensor is a multi-dimensional array
# Dense is the regular densely-connected NN layer, each neuron is connected to every neuron in the previous layer
# ... 128: number of neurons in the layer, also called nodes or units; this is a hyperparameter and can be tuned for better performance or accuracy
# ... relu: rectified linear unit, a common choice in neural networks
# ... input_shape: shape of the input data, 
# ... 784: 28x28
# Dense layer with 10 neurons, one for each digit
# ... 10: number of output units
# ... softmax is activation function, it is used to convert the output to a probability distribution
model = Sequential()
model.add(Dense(128, activation='relu', input_shape=(784,)))
model.add(Dense(10, activation='softmax'))

# Compile the model
# loss: loss function to minimize, categorical_crossentropy is a good default for classification
# optimizer: adam is the algorithm to used to minimize the loss function, it combines the advantages of two other extensions of stochastic gradient descent
# metrics: judge the performance of the model, accuracy is appropriate for classification problems 
model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

# Train the model, for a number of epochs, in this case 10
# X_train, y_train: the training data and corresponding labels
# epochs: number of times the model will passed through the model
# batch_size: means to model will be trained on 32 images at a time
# validation_data: is used to monitor the test loss and accuracy during training
model.fit(X_train, y_train, epochs=10, batch_size=32, validation_data=(X_test, y_test))

# Evaluate the model, the test loss and accuracy
# X_test: test data, the images
# y_test: target data, the labels
# test_loss: scalar test loss, float between 0 and 1, close to 0 is good
# test_acc: scalar test accuracy, float between 0 and 1, close to 1 is good  
test_loss, test_acc = model.evaluate(X_test, y_test)
print('Test loss:', test_loss)
print('Test accuracy:', test_acc)
```

## Visualize how this neural network works. 

Use the matplotlib library to display some of the input images and the corresponding predicted labels.  This code will create a 5x5 grid of subplots, each containing an input image from the test set and its predicted label. The np.argmax function is used to find the index of the predicted label with the highest probability.


```python
# This cell is dependent on the previous cell

# uncomment the following lines to install the required packages
# !pip install matplotlib 

import matplotlib.pyplot as plt

# Make predictions on the test set
y_pred = model.predict(X_test)

# Generate a random sample of 25 indices from the range 0 to 9999
indices = np.random.choice(range(10000), size=25, replace=False)

# Setup 25 images and their predicted labels prior to iteration
_, axs = plt.subplots(5, 5, figsize=(14, 14)) # setup 5x5 grid and scale figure
axs = axs.ravel() # convert to 1D array for easy iteration
for i in range(25):
    axs[i].set_title('test {} = {}'.format(indices[i],np.argmax(y_pred[indices[i]]))) # show test, predicted value
    axs[i].imshow(X_test[indices[i]].reshape(28, 28), cmap=plt.cm.gray_r) # show flattened 1D array as 2D array, 28x28
    axs[i].axis('off') # turn of axis

# Show occurs after the loop
plt.show()
```

## Resources
> Lesson to expand on MNIST.

TensorFlow's official MNIST tutorial: TensorFlow is a popular open-source machine learning framework, and their MNIST tutorial provides a great introduction to building neural networks with TensorFlow. You can find the tutorial here: https://www.tensorflow.org/tutorials/keras/classification.

Coursera's Deep Learning Specialization: This is a series of courses taught by Andrew Ng that provides a comprehensive introduction to deep learning. The second course in the series, "Improving Deep Neural Networks: Hyperparameter tuning, Regularization and Optimization," covers the MNIST example in detail. You can find the course here: https://www.coursera.org/specializations/deep-learning.

Neural Networks and Deep Learning book: This is a free online book by Michael Nielsen that provides an introduction to neural networks and deep learning. The book includes a chapter on the MNIST example, which provides a detailed explanation of how the neural network works. You can find the book here: http://neuralnetworksanddeeplearning.com/chap1.html.

Fast.ai: This is a set of online courses and resources that provide an introduction to deep learning. The courses include hands-on coding assignments, including an assignment on the MNIST example. You can find the courses here: https://www.fast.ai/.

## Sign Up / Login (Facial Recognition)
> Lesson to implement user sign-up with facial recognition by capturing multiple images and storing facial markers in a database.

### Frontend (JavaScript)
Handle user interactions for Sign-Up and Login using facial recognition.

#### **Sign-Up Flow:**
* Prompt the user to allow camera access.
* Capture multiple images of the user's face from different angles.
* Display a preview of the captured images to confirm.
* Send the images to the backend via an API call to `/user/facial/store`.
* Show success or error messages based on the backend response.

#### **Login Flow:**
* Automatically prompt for camera access when the login page loads.
* Capture a single image of the user's face.
* Send the image to the backend via an API call to `/user/facial/recognize`.
* If the backend returns a match, log the user in and issue a JWT.
* If no match is found, prompt the user to try again or log in manually.

### Backend (Python, Flask & TensorFlow)
Process images, extract features, and store user data.

### Import requirements
* tensorflow - Used for deep learning and extracting facial embeddings.
* opencv (cv2) - Processes images, including reading and resizing.
* face_recognition - Simplifies face detection and comparison.

### API Structure

**Facial Store Endpoint** (`/user/facial/store`)
  - Accepts image data from frontend (base64).
  - Saves the images to local storagethen.
  - Model call to extract and stores facial features.
    - Use a pre-trained model (FaceNet or similar) to extract embeddings.
    - Convert embeddings into a storable JSON format.
  - Model call to store user data and embeddings in the database for future authentication.
    - Modify SQLAlchemy `User` model to include a `facial_data` JSON attribute.
    - Store the user's embeddings alongside other user details.
  - Returns success response if stored correctly.

* **Facial Recognition Endpoint** (`/user/facial/recognize`)
  - Accepts a new image for authentication from frontend (base64).
  - Model call to compare logic to User saved embeddings.
  - Returns user identity if matched or a prompt for login if unknown.

### Model Code

* **Model Logic**
  - Preprocess images before extracting embeddings.
  - Generate facial embeddings using deep learning.
  - Compare new embeddings to stored embeddings in the database.
  - Implement similarity scoring (cosine distance, Euclidean distance, etc.).

* **Database Storage**
  - Update `User` model to store facial embeddings as JSON.
  - Ensure database indexing for efficient lookup.
  - Add migration script to update schema if necessary.




