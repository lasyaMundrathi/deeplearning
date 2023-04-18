# deeplearning
cifar10 with pytorch

 ## *Read dataset and create data loaders*
 Importing the cifar10 dataset and separating it into the train and test datasets. During training and testing, the
DataLoader class is used to load data in batches. The training and testing sets are subjected to two separate
sets of data transformations. Data augmentation methods such as random cropping, horizontal flipping, colour
jittering, and normalisation are used on the training set.
Stats variable has the mean and the standard deviation of the dataset, which is used to normalise the data.

*For training*,
● RandomCrop crops the image at random to the 32 size, with padding of 4 around the edges.Now, the
image size is around 40*40 pixel image with reflection of the image.
● RandomHorizontalFlip turns the picture horizontally at random.
● ColorJitter alters the image's brightness, contrast, saturation, and hue at random.
● ToTensor transforms an image into a tensor.
● The stats variable's mean and standard deviation are used to normalise the image with Normalise .
*For testing*,
● tt.to Tensor transforms an image into a tensor.
● tt.Normalize normalises the image data defined in the stats variable.
 
 ## *Creating a Model*
 On the CIFAR-10 dataset, this code creates a convolutional neural network (CNN) for Image categorization. The
network is made up of six Cifar_Block modules, which are followed by a fully linked layer. Each Cifar_Block
module has a set of 3 convolutional layers with batch normalisation and ReLU activation, followed by a
maximum pooling layer if one is given. Each Cifar_Block's output is concatenated and processed through a linear
layer with ReLU activation and dropout. The end result is a linear layer with softmax activation that may be
classified into one of 10 classes.
The ‘BaseClassification’ class[2], which offers some training functionality for the cifar10 image classification
model, is inherited by the cifar10_backbone class. The __init__ function defines the six sequential Cifar_Block
modules (blocks 1–6), which have increasing numbers of output channels where the output of one block is given
as input to another block. The first Cifar_Block contains three input channels (for the RGB colours in the input
image), while the last Cifar_Block has 512 output channels. The nn.AdaptiveAvgPool2d layer performs global
average pooling over the output of the previous Cifar_Block, which is followed by a linear layer with 128 output
features and ReLU activation. Dropout is applied to the ReLU layer's output, and a forward linear layer with 10
output features (one for each class in CIFAR-10) and softmax activation is defined.

## *Creating the loss and optimizer*

The loss function utilised in the training phase is F.cross_entropy. This is a loss function that combines the
softmax and negative log-likelihood loss functions. It is widely used for multi-class problems with a single correct
class out of several different classes.
Adam optimizer(Torch.optim.Adam) is the optimizer utilised throughout the optimisation process. This is a
common optimisation approach that employs adaptive learning rates for each parameter, resulting in faster
convergence and improved performance on non-convex optimisation problems as compared to other standard
optimisation algorithms such as stochastic gradient descent (SGD). The Adam optimizer adjusts the model
parameters based on the magnitude and sign of the gradients of the loss function with respect to the model
parameters.
In the validation phase, the validation loss is calculated using the same loss function F.cross_entropy, and the
validation accuracy is calculated using the same accuracy function 'accuracy'. However, because there is no
backpropagation during validation, no optimizer is applied. Instead, the model's performance on the validation set
is examined to see how effectively the model generalises to new, previously unknown data.

## *Training script for training the model*

Initially fit_one_cycle() which is a training function that trains a PyTorch neural network model using the
one-cycle learning rate scheduling approach. This function takes the following arguments:
● epochs:The number of training epochs
● max_lr: the training's maximum learning rate.
● model: the to-be-trained PyTorch neural network model.
● train_loader: the training data loader that returns batches of input data and labels.
● val_loader: the data loader for validation that returns batches of input data and labels.
● weight_decay: the regularisation weight decay coefficient.
● grad_clip: The maximum gradient value to be clipped.
● opt_func: the training optimizer, which is set to torch.By default, optim.Adam is used.
Fit_one_cycle() first creates a custom optimizer with weight decay [3] and a one-cycle learning rate scheduler
[4]. It then trains the model for the number of epochs supplied using a mix of forward and backward gradient
propagation, weight updates, and learning rate scheduling.
The function begins each epoch with the training phase, in which the model is put to train mode using the
model.train() method. It then iterates over the training data loader's batches of data, computes the model's
forward pass using the input data, computes the training loss using the F.cross_entropy() function, computes the
gradients using backpropagation, applies gradient clipping (if specified), updates the model weights using the
optimizer, and records the learning rate used in the current iteration. It moves on to the validation step at the
conclusion of the period.
The model is set to evaluation mode during the validation phase by using the model.eval() function. It then
iterates over the validation data loader's batches of data, computes the model's forward pass using the input
data, and calculates the validation loss and accuracy using the F.cross_entropy() and accuracy() functions,
respectively. It then averages the losses and accuracies received from all batches to produce the total loss and
accuracy for the validation set. Finally, it produces a dictionary containing the epoch's validation loss and
accuracy.
The function computes and displays the epoch-wise training and validation losses and accuracies, as well as the
learning rate utilised in the final iteration of the epoch, following the validation phase. The outcomes for each
epoch are saved in a list named history, which the function returns at the end.
The %time magic command is used to calculate the amount of time it takes the fit_one_cycle() function to train
the model. Before the function call, the history list is initialised and updated with the outcomes of each epoch

