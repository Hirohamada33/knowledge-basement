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
   Used to evaluate the generalization of the ML method, and sensitivity to the training/validation/test split
• Useful to select the ‘best’ ML method (e.g. classifier)  
• Can be useful especially if limited labelled data available
   
   Q: Why is there a separate sets for validation and testing, while their purposes seems to be similar and both served as the correcting the model? 
   A: Validation step for **hyperparameters tuning**, while testing step is to evaluate the model performance. 
4. Testing
5. **Inference / prediction**
   The final step inference / prediction is conducted online, where a real-world dataset is fed into the system. 
   

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



###### **Sum of squares error function**
$$E(w)=\frac{1}{2}\sum ^N _{n=1}(y(x_n,\space w)-t_n)^2$$

####### **Loss function**

- E(w) is often called the Loss function: Loss(hw) for the hypothesis hw (candidate function to approximate the true function f)
- This particular loss function (squared-error loss function) is named $L_2$
- The minimum occurs when the partial derivatives of Loss are zero
- For a linear regression problem (fit a line, i.e. M=1) there is a direct, analytical solution to this optimization problem
- For the general case, an optimization algorithm is used, such as gradient descent.


####### **Overfitting**
In overfitting, the idea is the model fits the training dataset too well, where it overfits the geometry of the data set. When overfitting, the model performs poorly on unforeseen data set, hence a usual case is an apparent part-ways in training and testing data set. 
Overfitting occurs when the capacity of the model becomes too large, where in the case the accuracy will increased to an unreasonable amount (such as nearly 100%) and loss is to 0%, but in unforeseen case the loss function will start exploding. 
![[截圖 2024-06-18 上午7.47.56.png]]


**Regularisation** penalises large coefficient values by adding an extra term to the loss function. In this way it can control the capacity of the model, and prevent any weight explosion from overfitting. 

![[regularisation.png]]



##### Bayes Classification

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
**Advantages**:
- one number to summarise the results, easy to interpret
**Limitation**: 
- it does not tell what sort of error is presented in the system. (Type 1 or type 2 errors)
- If a class is largely represented (sometimes called data imbalanced), its error (even when numbers are significant) will not be showing in the result, creating a data bias. 
  Also referred as sensitive to class imbalance. 
- Hence, accuracy itself tells the ambiguous story. 
  
![[截圖 2024-04-28 下午5.25.46.png]]
In the above example, if a classifier determines all the cases are negative, it still receives a 95% accuracy which in fact the classifier itself is poor. 


**Precision**  $$P=\frac{TP}{TP+FP}$$**Recall**   $$R =\frac{TP}{TP+FN}$$
If a false positives (false alarms)
If a false negatives (missed detection) was to be minimised, focus on increasing the recall. 
**Advantage**:
- gives more details on type of errors, especially relevant if your classes are not to be treated the same
- better with imbalanced classes
**Limitation**:
- If looking at only precision or recall, then, for increasing precision it can simply declares every case to be negative; for increasing recall it can simply declares every case to be positive. 
- 


**F1 score**   $$F1 = \frac{2PR}{P+R} = \frac{2 \times TP}{2 \times TP + FP + FN} = 2 \times \frac{\text{sensitivity} \times \text{precision}}{\text{sensitivity+precision}}$$F1 score does not account of true negative, so it is favouring the positive class (which again is a class imbalance). 
The choice of Positive Class (vs. Negative Class) may dramatically change the value of F1 in the case of significant class imbalance. 
(Favourable value of F1 if the positive class has mores samples than the negative class)
![[截圖 2024-04-28 下午7.03.42.png]]

**Sensitivity (True positive rate)** = $\frac{TP}{TP+FN}$
**Specificity (True negative rate)** = $\frac{TN}{TN+FP}$
**Balanced accuracy** = $\frac{TPR + TNR}{2} = \frac{\text{sensitivity + specificity}}{2}$



**Matthews Correlation Coefficient (MCC)**
$$MCC = \frac{TP \times TN - FP \times FN}{\sqrt{(TP+FP)(TP+FN)(TF+FP)(TF+FN)}}$$

**Normalised Confusion matrix**
Help compensate the class imbalance

**Receiver Operating characteristic (ROC) Curve**
True Positive Rate (TPR) vs False Positive Rate (FPR) (i.e., Sensitivity vs. (1-specificity))
Area under the curve (AUC) is a way to measure the performance of a classifier. 
![[截圖 2024-06-18 下午3.18.24.png]]


**Precision Recall Curve**

**Some discussion of each evaluation**
Accuracy can be **misleading** if the data is NB (imbalanced). 


##### Multi-class
**Macro-averaging** (treats all class equally):
```python
Precision = (Precision(C1)+...+Precision(Cn)) / n 
Recall = (Recall(C1)+...+ Recall(Cn)) / n
```
- arithmetic mean across all classes.
- equal weight to each class, regardless of number of instances
Pros:
- Good when all classes **equally important**
- Handle **imbalanced datasets**
Cons:
- Performance may look worse due to low performance in a small class  
- May not see poor performance in minority class when the number of classes is large

**Micro-averaging** (treats all instances equally)
- first aggregate counts of TP, FP, FN for each class first
- equal weight to each instance
```python
Precision = (TP1+...+TPN)/(TP1+FP1+...+TPn+FPn)
Recall = (TP1+...+TPN)/(TP1+FN1+...+TPn+FNn)
```
Pros:
- Accounts for the total number of misclassifications (similar to accuracy)
Cons:
- Can overemphasize the performance of a **majority class** (highly represented in the dataset)
- Might not see poor performance of **minority class**


