See also: [[Machine learning]]


![[introduction to image classification.png]]
The idea of image classification is taking an image (with pure 0 and 1 signals) and output a list of possibilities of the prediction of each class. 

The recognition of computer vision has some major challenges to considered:
- **Viewpoint variation**. A single instance of an object can be oriented in many ways with respect to the camera.
- **Scale variation**. Visual classes often exhibit variation in their size (size in the real world, not only in terms of their extent in the image).
- **Deformation**. Many objects of interest are not rigid bodies and can be deformed in extreme ways.
- **Occlusion**. The objects of interest can be occluded. Sometimes only a small portion of an object (as little as few pixels) could be visible.
- **Illumination conditions**. The effects of illumination are drastic on the pixel level.
- **Background clutter**. The objects of interest may _blend_ into their environment, making them hard to identify.
- **Intra-class variation**. The classes of interest can often be relatively broad, such as _chair_. There are many different types of these objects, each with their own appearance.
![[challenges in image classification.png]]

The image classification typically is a **data-driven approach**. 
Having an specific algorithm (such as sorting, searching) to identify an image doesn't seems like an obvious way. It is instead provided with many examples (i.e., training sets) 

The pipeline of image classification can be formalized as follows:
- **Input:** The input consists of a set of _N_ images, each labeled with one of _K_ different classes. This data is referred as the _training set_.
- **Learning:** Our task is to use the training set to learn what every one of the classes looks like. This step is referred as _training a classifier_, or _learning a model_.
- **Evaluation:** In the end, we evaluate the quality of the classifier by asking it to predict labels for a new set of images that it has never seen before. We will then compare the true labels of these images to the ones predicted by the classifier. Intuitively, we’re hoping that a lot of the predictions match up with the true answers (which we call the _ground truth_).



![[L1 and L2 distances.png]]

Hyperparameters: 

##### Nearest neighbour classifier:
The methodology is rarely used in practice, but simply allowing getting the idea of basic approach to a image classification problem. 

A list of choices of distance methods are:
**L1 distance (Manhattan distance)** $$d_1(I_1, I_2) = \sum_pI_1^p-I_2^p$$
**L2 distance (Euclidean distance)** $$d_2(I_1, I_2) = \sqrt {\sum_pI_1^p-I_2^p}$$
A visualisation of the image subtraction process:
![[intuitive image subtraction.png]]

For Nearest Neighbour model, the time complexity is **train O(1)**, **predict O(n)**, as the training processing is simply plotting the data onto scatterplot, and predicting process will need to iterate the predicting data with every trained datapoint to find the nearest neighbour. 
This is not ideal, as usually the training can be slow and prediction should be faster. 

**kNN model** is a extension of Nearest Neighbour, but instead of using the one nearest neighbour to determine the class, k Nearest Neighbour sees a bigger picture and only choose the most numbers 
Disadvantage: 
1. Actual meaning of L1 and L2 distances are not capturing perceptual meaning
2. Curse of dimensionality

![[kNN with different K.png]]
Linear classifier


Struggles






##### Regularisation
L1 regularisation
L2 regularisation
Elastic net


##### Implementation

**Load the images** 
```python
Xtr, Ytr, Xte, Yte = load_CIFAR10('data/cifar10/') # loading function 
# flatten out all images to be one-dimensional
Xtr_rows = Xtr.reshape(Xtr.shape[0], 32 * 32 * 3) # Xtr_rows becomes 50000 x 3072
Xte_rows = Xte.reshape(Xte.shape[0], 32 * 32 * 3) # Xte_rows becomes 10000 x 3072
```

**Training and predicting**
```python
nn = NearestNeighbor() # create a Nearest Neighbor classifier class
nn.train(Xtr_rows, Ytr) # train the classifier on the training images and labels
Yte_predict = nn.predict(Xte_rows) # predict labels on the test images
# and now print the classification accuracy, which is the average number
# of examples that are correctly predicted (i.e. label matches)
print 'accuracy: %f' % (np.mean(Yte_predict == Yte))
```

**Nearest neighbour class**
```python
import numpy as np

class NearestNeighbor(object):
  def __init__(self):
    pass

  def train(self, X, y):
    """ X is N x D where each row is an example. Y is 1-dimension of size N """
    # the nearest neighbor classifier simply remembers all the training data
    self.Xtr = X
    self.ytr = y

  def predict(self, X):
    """ X is N x D where each row is an example we wish to predict label for """
    num_test = X.shape[0]
    # lets make sure that the output type matches the input type
    Ypred = np.zeros(num_test, dtype = self.ytr.dtype)

    # loop over all test rows
    for i in range(num_test):
      # find the nearest training image to the i'th test image
      # using the L1 distance (sum of absolute value differences)
      distances = np.sum(np.abs(self.Xtr - X[i,:]), axis = 1)
      min_index = np.argmin(distances) # get the index with smallest distance
      Ypred[i] = self.ytr[min_index] # predict the label of the nearest example

    return Ypred
```