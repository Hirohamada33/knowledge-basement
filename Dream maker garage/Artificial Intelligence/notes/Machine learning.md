Machine learning is where a computer observes some **data**, and it builds a **model** based on the data. It uses the model as both a **hypothesis** about the world and a piece of software that can solve problems. 

A difference between machine learning and classical search / decision making algorithm is the "learning component". A decision tree has a existing, unchanged model that uses prior knowledge to find the answer, whereas machine learning observes data and build the model, to infer a prediction that may not be existed in prior knowledge. 

Types of machine learning:
- **Supervised learning**:
  The agent observes input-output pairs and learns a function that maps from input to output. 
  - **Classification / categorisation (label prediction)**: the output is one of a finite set of values (e.g. true / false, sunny / cloudy / rainy, cat / dog)
  - **Regression (function approximation)**: the output is a number (or continuous variable) (e.g. tomorrow’s temperature)
- **Unsupervised learning**:
  The agent learns the patterns in the input without any explicit feedback. 
  - **Clustering**: 
  - **Dimensionality reduction**:
- **Reinforcement learning**: 
  The agent learns from a series of reinforcement (rewards and punishments). 
  - **Policy learning (learning from experience)**: 


![[types of ML learning.png]]


##### Process

1. **Data collection**
   Data sets should be split into a few partitions: **training sets**, to train the candidate model; **validation sets / development sets** (use when params and hyperparams to tune), to evaluate the candidate models to choose the best one; **test sets**, to do a final unbiased evaluation of the selected model. 
   Data bias: A sample of group that is overgeneralised / undergeneralised.   
2. **Training**
   Preprocessing
   Feature extraction
   Regression / classification
3. **Validation**
   **Holdout validation** means the dataset must be **unforeseen**, which is the specific data sets must be excluded from the training set, for better generalisation. 
   **Cross-fold validation**
   
   Q: Why is there a separate sets for validation and testing, while their purposes seems to be similar and both served as the correcting the model? 
   A: 
4. Testing
5. Inference / prediction

![[ML process.png]]




##### Classification
- Naïve Bayes classifier  
- K-Nearest Neighbours  
- Support Vector Machines (SVM) 
- Random Forest


##### Regression
- Linear Regression  
- Multiple Regression  
- Logistic Regression  
- Advanced/non-parametric Regression

In a general regression, usually the formula is described as:
![[general regression formula.png]]
Notice that there are a few parameters to be determined: $w_j$ and $M$

**Bias** is the inability for a machine learning method to capture the true relationship between the input and the output (x and y). 

**Variance** is the difference in fit (error between estimates and reality) between the datasets. Variability of results for a new dataset. 



**Sum of squares error function**

**Loss function**

- E(w) is often called the Loss function: Loss(hw) for the hypothesis hw (candidate function to approximate the true function f)
- This particular loss function (squared-error loss function) is named $L_2$
- The minimum occurs when the partial derivatives of Loss are zero
- For a linear regression problem (fit a line, i.e. M=1) there is a direct, analytical solution to this optimization problem
- For the general case, an optimization algorithm is used, such as gradient descent.


**Overfitting**

**Regularisation** penalises large coefficient values by adding an extra term to the loss function. 

![[regularisation.png]]



#### Bayes Classification

$$p(Y|X) = \frac{p(X|Y)p(Y)}{p(X)}$$
$$p(X)=\sum_Yp(X|Y)p(Y)$$

![[Bayes theorem.png]]

In a probability model, 
- $p(X|Y)$ is the **class conditional model** (i.e., observation model) which is the likelihood to observe x given we know the class is y , 
- $p(Y)$ is **prior probability** of each class
- $\sum_Y'p(X|Y')p(Y')$ is **normaliser** or **posterior** (likelihood x prior) which normalizes probabilities across observations


##### Naïve Bayes classifier  
$$v_{map} = argmax \space P(v_j|a_1,...a_n)$$
$$= argmax\space P(v_j)\prod P(a_i|v_j)$$



It is noticed that the priors have been taken into consideration. Which means the data itself should be representative and generalising, and the prior should match to the general occurrence. 

An additional notice is the probability of zero in any Bayesian processes. Once the probability of an event / instance / class is zero, it will identify the occurrence as zero, which violates the intuition 
##### Gaussian Bayes classifier
Gaussian Bayes classifier is used for dealing with continuous data. Assume that each class follows a normal distribution (i.e., $p(x|Ci) = Gaussian(\mu_x, \sigma_k)$)

The following example shows a case of classifying positive (red, 1) and negative (blue, 0) cases. The continuous data is modeled given the parameters $\mu, \sigma$. A
![[Decision boundary.png]]

Notice an overlapping occurred in the above example. 
A **decision boundary** is when, two classes are colliding with each other, a manual boundary to categorise the instances in the overlapping area. This process is totally decision-based, which can also be seen as arbitrary in terms of data reading. 


There will be **false categorisations** when a decision boundary is set, namely **false positives** and **false negatives** for this example. 

![[False categorisations.png]]


By shifting the decision boundary, the false positives and negatives can increase and decrease. Such as the first example, the false negatives have increased, and the false positives have decreased. The consideration of shifting the decision boundary could be based on reasons such as, no false alarms want to be triggered at all, or no positive cases want to be slipped away. 

![[False negatives shift.png]]


![[False positives shift.png]]



##### Nearest Neighbour Classifier


Instance-based learning:
- Store training examples and delay the processing (“lazy evaluation”) until a new instance must be classified

Example of approaches:
- K-nearest neighbour
	- Instances represented as points in a Euclidean space 
- Locally weighted regression
	- Constructs local approximation
All instances correspond to points in the n dimensions space, where n is the size of x.

One way is simply discretised an area and calculate the ratio of each classes presented in this area, then predict the data by the largest ratio. 

An variant is the distance weight


##### Other effective and popular classifier
- Support Vector Machines (SVM)
- Random forest



##### Evaluation
A confusion matrix (below is the binary classification case) is a presentation for true categorisations along with false categorisations. 


![[confusion matrix.png]]

The accuracy is quantified by $$\frac{TN+TP}{TN+TP+FN+FP}$$which is the sum of diagonal elements of the confusion matrix (successful classification), divided by the sum of all elements of the matrix (i.e. total number of samples tested). 
However, the limitations of accuracy are:
- it does not tell what sort of error is presented in the system. 
- If a class is largely represented (sometimes called data imbalanced), its error (even when numbers are significant) will not be showing in the result, creating a data bias. 
![[截圖 2024-04-28 下午5.25.46.png]]
In the above example, if a classifier determines all the cases are negative, it still receives a 95% accuracy which in fact the classifier itself is poor. 
Hence, accuracy itself tells the ambiguous story. 


**Precision**  $$P=\frac{TP}{TP+FP}$$**Recall**   $$R =\frac{TP}{TP+FN}$$
**F1 score**   $$F1 = \frac{2PR}{P+R}$$
F1 score does not account of true negative, so it is favouring the positive class (which again is a class imbalance)
![[截圖 2024-04-28 下午7.03.42.png]]


**Matthews Correlation Coefficient (MCC)**


**Normalised Confusion matrix**
Help compensate the class imbalance

**Receiver Operating characteristic (ROC) Curve**
AUC 

**Precision Recall Curve**

**Some discussion of each evaluation**
Accuracy can be **misleading** if the data is NB (imbalanced). 

