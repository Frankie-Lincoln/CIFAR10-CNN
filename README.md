# Project Overview
This project implements a specific architecture of convolution neural network (CNN) for the CIFAR-10 classification problem.
## Model Requirement
<img width="1281" alt="Screenshot 2024-01-11 at 11 39 33 AM" src="https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/47e9e3de-3d64-4143-8f5f-e47a37ec5cc5">
<img width="1281" alt="Screenshot 2024-01-11 at 11 39 23 AM" src="https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/3ec96b4c-7e34-46d4-ab68-3c88aa29b88e">
<img width="1281" alt="Screenshot 2024-01-11 at 11 39 50 AM" src="https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/98f90b0f-fc46-48e6-9139-9eb300d61c7d">

# Model Architecture
The following diagrams show the whole model architecture and the structure of the block.  
![net](https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/6b43eb1c-0356-4787-9779-3f0789c2a8ff)
![block](https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/e9ae03de-677a-4f1f-9850-2602e3e00fbd)  
As a whole picture, the image is first passed through a 3x3 convolution layer, which outputs 16 channels.  
Then it goes to the backbone, which consists of 4 blocks for feature extractions and between them is a max pooling layer to reduce spatial dimension.  
After the final block, there is an adaptive average pooling to compute the average of each channel, which will be passed into the Softmax regression classifier for the final output.  
Inside each block, there are 4 separated convolution paths, each with a different kernel size to recognise details at different extents.
On another path, the input passes through an adaptive average pooling to compute the average of each channel, followed by an MLP to give a vector of 4 elements. 
As shown in the diagram, output of each convolution path is multiplied with an element of the vector and they are summed to be the output of this block.  

# Training and Hypermeters
## Weight Initialization
After constructing the model, the weights are initialized. For linear and convolution layers, Xavier uniform initialisation is applied. For batch normalisation layer, it is initialised by default.
## Training Hypermeters
A learning rate of 0.005 and a batch size of 32 is used to train the model for 60 epochs. The loss function is Cross-entropy loss and Adam is used as the optimization algorithm.
## Data Augmentation
Transformation is applied to the training set to randomly crop, rotate, flip horizontally, performs actions like zoom and change shear angles. Brightness, contrast and saturation are also slightly modified.
## Training Details and Results
In each epoch, the data loader will pass 32 samples into the model. 
For each batch, the training loss is calculated.  
Then backpropagation computes the gradients with respect to weights to minimise the loss. 
Before each backpropagation, the gradients are reset to 0. 
The loss in each batch is accumulated and the total loss is divided by the number of batches to return an average loss for that epoch. 
Meanwhile, the training accuracy is calculated by dividing the total number of correct predictions with the total number of training samples. 
For every 200 batches, the loss and progress is shown to keep track of the training process.  
In evaluation mode, the test loss and accuracy are obtained in the same way for each epoch. 
The training loss, training and test accuracies are appended to a separate list for plotting the curves at the end.
![9137](https://github.com/Frankie-Lincoln/CIFAR10-CNN/assets/77485565/0e0f8547-7fad-4c55-8f47-17d1bfaa75a5)
## Model saving mechanism
At a later stage of the training, the validation accuracy and loss always fluctuate. 
The result from the last epoch does not guarantee to be the best. Thus conditions need to be set to save the best model during the process. 
In this project, the model is saved when it reaches a higher accuracy over 90%.

   

