# Convolutional Autoencoder for Image Denoising

## AIM

To develop a convolutional autoencoder for image denoising application.

## Problem Statement and Dataset
In this experiment, we use an autoencoder to process handwritten digit images from the MNIST dataset. The autoencoder learns to encode and decode the images, reducing noise through layers like MaxPooling and convolutional. Then, we repurpose the encoded data to build a convolutional neural network for classifying digits into numerical values from 0 to 9. The goal is to create an accurate classifier for handwritten digits removing noise.
![376165230-d1075826-836b-44e1-8ed4-330e3899a494](https://github.com/user-attachments/assets/3a11bc6e-853a-4c2e-b266-741b7199bc8e)
## Convolution Autoencoder Network Model
![376165283-ce6d8c1c-5d8f-44d3-b02d-639c26ff9ba1](https://github.com/user-attachments/assets/4dd52497-9e17-43e3-a835-d2d62e206767)

## DESIGN STEPS

### STEP 1:
Import TensorFlow, Keras, NumPy, Matplotlib, and Pandas.
### STEP 2:
Load MNIST dataset, normalize pixel values, add Gaussian noise, and clip values.
### STEP 3:
Plot a subset of noisy test images.
### STEP 4:
Define encoder and decoder architecture using convolutional and upsampling layers.
### STEP 5:
Compile the autoencoder model with optimizer and loss function.
### STEP 6:
Train the autoencoder model with specified parameters and validation data.
### STEP 7:
Plot training and validation loss curves.
### STEP 8:
Use the trained model to reconstruct images and visualize original, noisy, and reconstructed images.

## PROGRAM
### Name: KRISHNARAJ D
### Register Number: 212222230070

```python
from tensorflow import keras
from tensorflow.keras import layers
from tensorflow.keras import utils
from tensorflow.keras import models
from tensorflow.keras.datasets import mnist
import numpy as np
import matplotlib.pyplot as plt
(x_train, _), (x_test, _) = mnist.load_data()
x_train.shape
x_train_scaled = x_train.astype('float32') / 255.
x_test_scaled = x_test.astype('float32') / 255.
x_train_scaled = np.reshape(x_train_scaled, (len(x_train_scaled), 28, 28, 1))
x_test_scaled = np.reshape(x_test_scaled, (len(x_test_scaled), 28, 28, 1))
noise_factor = 0.5
x_train_noisy = x_train_scaled + noise_factor * np.random.normal(loc=0.0, scale=1.0, size=x_train_scaled.shape)
x_test_noisy = x_test_scaled + noise_factor * np.random.normal(loc=0.0, scale=1.0, size=x_test_scaled.shape)

x_train_noisy = np.clip(x_train_noisy, 0., 1.)
x_test_noisy = np.clip(x_test_noisy, 0., 1.)
n = 10
plt.figure(figsize=(20, 2))
for i in range(1, n + 1):
    ax = plt.subplot(1, n, i)
    plt.imshow(x_test_noisy[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
plt.show()
input_img = keras.Input(shape=(28, 28, 1))

x = layers.Conv2D(16, (3, 3), activation='relu', padding='same')(input_img)
x = layers.MaxPooling2D((2, 2), padding='same')(x)
x = layers.Conv2D(8, (3, 3), activation='relu', padding='same')(x)
encoded = layers.MaxPooling2D((2, 2), padding='same')(x)

x = layers.Conv2D(8, (3, 3), activation='relu', padding='same')(encoded)
x = layers.UpSampling2D((2, 2))(x)
x = layers.Conv2D(16, (3, 3), activation='relu', padding='same')(x)
x = layers.UpSampling2D((2, 2))(x)
decoded = layers.Conv2D(1, (3, 3), activation='sigmoid', padding='same')(x)

autoencoder = keras.Model(input_img, decoded)
print('Name: Aakash P  \nRegister Number:212222110001\n')
autoencoder.summary()
autoencoder.compile(optimizer='adam', loss='binary_crossentropy')
autoencoder.fit(x_train_noisy, x_train_scaled,
                epochs=2,
                batch_size=128,
                shuffle=True,
                validation_data=(x_test_noisy, x_test_scaled))
decoded_imgs = autoencoder.predict(x_test_noisy)
history=autoencoder.fit(x_train_noisy, x_train_scaled,
                epochs=2,
                batch_size=128,
                shuffle=True,
                validation_data=(x_test_noisy, x_test_scaled))
decoded_imgs = autoencoder.predict(x_test_noisy)
n = 10
print('Name: Aakash P\nRegister Number:212222110001\n')
plt.figure(figsize=(20, 4))
for i in range(1, n + 1):
    # Display original
    ax = plt.subplot(3, n, i)
    plt.imshow(x_test_scaled[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)

    # Display noisy
    ax = plt.subplot(3, n, i+n)
    plt.imshow(x_test_noisy[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)

    # Display reconstruction
    ax = plt.subplot(3, n, i + 2*n)
    plt.imshow(decoded_imgs[i].reshape(28, 28))
    plt.gray()
    ax.get_xaxis().set_visible(False)
    ax.get_yaxis().set_visible(False)
plt.show()
print("Aakash P\n212222110001")
plt.plot(history.history['loss'], label='Training Loss')
plt.plot(history.history['val_loss'], label='Validation Loss')
plt.title('Training and Validation Loss vs Iterations')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.grid(True)
plt.show()

```
## OUTPUT
![DL 7 OUTPUT](https://github.com/user-attachments/assets/64c2beb5-3a46-48d6-a7af-133183ec0f9e)

![DL 7 OUTPUT 2](https://github.com/user-attachments/assets/21f06258-5e60-42ee-b493-5b527b303675)

## RESULT:
Thus, the convolutional autoencoder for image denoising application has been successfully developed.
