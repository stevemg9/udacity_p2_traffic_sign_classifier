## Project: Build a Traffic Sign Recognition Program

## Dataset Summary
The dataset provided consisted of 51839 images containing one of 43 different types of German traffic signs.  The data were 32x32x3 RGB images.  The data were broken up into a training set containing 34799 images, a validation set containing 4410 images, and a testing set containing 12630 images.  Unfortunately the data were not uniformly distributed over the 43 different classes.  Therefore, I performed some data augmentation to create a more uniform and robust dataset.  This data augmentation consisted of performing random transformations on the images.  The four transformations used were: translation, rotation, blurring, and zooming.  I left the testing dataset untouched.

My final dataset consisted of:
- 125338 training images
- 22119 validation images
- 147,457 total images

## Data Preprocessing
In order to preprocess the data, I performed the aforementioned data augmentation on the original RGB images.  Once I had a uniform and robust dataset, I normalized the data from [0,255] to [0,1].  I also experimented with converting the data to grayscale.  Since I have the benefit of a powerful GPU, I ended up leaving the data in RGB format because computation time was not an issue and the results seemed a little bit better.

## Model Architecture
Before I arrived at my final network architecture, I played around with a lot of different networks and changed the hyper parameters a lot.  Unfortunately I did not log all of this data for the numerous model architectures.  The final architecture that I set on was as follows:

![Model Architecture](model_arch.png)

### Network Architecture:
* **Input Image**
  * 32x32x3
* **Layer 1: Convolution**
  * Filter Size: 3x3x8
  * Stride: 1x1
  * Padding: Valid
  * Output 30x30x8
* **Layer 2: Convolution**
  * Filter Size: 3x3x16
  * Stride: 1x1
  * Padding: Valid
  * Output 28x28x16
* **Max Pooling With Dropout**
  * Filter Size: 2x2
  * Stride: 2x2
  * Padding: Valid
  * Output: 14x14x16
  * Keep Rate: 0.65
* **Layer 3: Convolution**
  * Filter Size: 3x3x32
  * Stride: 1x1
  * Padding: Valid
  * Output 12x12x32
* **Layer 4: Convolution**
  * Filter Size: 3x3x64
  * Stride: 1x1
  * Padding: Valid
  * Output 10x10x64
* **Max Pooling With Dropout**
  * Filter Size: 2x2
  * Stride: 2x2
  * Padding: Valid
  * Output: 5x5x64
  * Keep Rate: 0.65
* **Flatten**
  * Input: 5x5x64
  * Output: 1x1600
* **Layer 5: Fully Connected**
  * Input: 1x1600
  * Output: 1x800
* **Layer 6: Fully Connected**
  * Input: 1x800
  * Output: 1x400
* **Layer 7: Fully Connected**
  * Input: 1x400
  * Output: 1x43 (Logits)
* **Network Training:**
  * Optimizer: Adam Optimizer
  * Learning Rate: 0.001
  * Batch Size: 1024
  * Epochs: 35
  * Dropout Keep Rate: 0.65

I was able to train the network on an Nvidia Quadro M500, so the training was relatively fast.  After trying quite a few different network architectures I began to notice a few things that stood out.  I noticed that 3x3 convolutions seem to work slightly better than 5x5 convolutions.  I also started out using a network with far more filters than I needed.  Once I hammered out the flaws, I realized that this problem did not require a massive network.

After training for 35 epochs on the augmented training data. I ended up with:
* Training Accuracy: 1.00
* Validation Accuracy: 0.993
* Test Accuracy: 0.939
* 5 New Image Accuracy: 1.00

With the 5 new images that I found online, I used GIMP to scale the images to 32x32 and ran them through the network.  The network was 100% accurate on these five images and was extremely certain of it’s predictions. 4 of the 5 images were 100.00000% (up to 5 decimal places) and the fifth image was 99.99986%.  This tells me that the network is not only accurate, but confident in it’s predictions.
