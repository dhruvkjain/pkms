[[ML/index|ML/index]]

## Sources
- [Great for learning concepts, functions and different methods - Hands on Machine Learning with ScikitLearn, Keras & TensorFlow](http://14.139.161.31/OddSem-0822-1122/Hands-On_Machine_Learning_with_Scikit-Learn-Keras-and-TensorFlow-2nd-Edition-Aurelien-Geron.pdf)


## Research Articles
- [How Deepseek used MultiToken Prediction](https://youtu.be/4BhZZYg2_J4?si=JkppbxMu1gMyqETy)


## Fine Tuning

**Hyperparameters** => Variables that are constant throughout the training for example, Neural Net architecture (number of layers, neuron in layers etc...), Learning Rate, batch size, Loss function, Optimizers, Activation functions, number of iterations etc..

**Parameters** => variables that are fine tuned during training weights and biasis


### Hyperparameters fine tuning
#### Number of Hidden Layers
- for complex problems, deep networks have a much higher parameter efficiency than shallow ones. They can model complex functions using exponentially fewer neurons than shallow nets, allowing them to reach much better performance with the same amount of training data.

#### Number of Neurons per Hidden Layer
- using the same number of neurons in all hidden layers performs just as well in most cases, or even better; plus, there is only one hyperparameter to tune, instead of one per layer. 
- depending on the dataset, it can sometimes help to make the first hidden layer bigger than the others.
- a layer with two neurons can only output 2D data, so if it processes 3D data, some information will be lost
- **In general you will get better performance by increasing the number of layers instead of the number of neurons per layer.**

#### Learning rate
- learning rate is too small, then the algorithm will have to go through many iterations to converge
- learning rate is too high, you might jump across the minimum value and end up on the other side, possibly even higher up than you were before
- **The curve of the Mean Squared Error (MSE) cost function for linear regression is typically a convex shape, which means it has no local minima, just one global minimum.**
- eliminate models that take too long to converge
- gradually reduce the learning rate, function that determines the learning rate at each iteration is called the learning schedule.

#### Number of Iterations
- **set a very large number of iterations but to interrupt the algorithm when the gradient vector becomes tiny that is, when its norm becomes smaller than a tiny number ϵ (called the tolerance)—because this happens when Gradient Descent has (almost) reached the minimum**
- it can take O(1/ϵ) iterations to reach the optimum within a range of ϵ, depending on the shape of the cost function

#### Loss functions
- **Root Mean Square Error (RMSE) is generally the preferred performance measure for regression tasks, in some contexts you may prefer to use another function. For example, suppose that there are many outlier districts. In that case, you may consider using the Mean Absolute Error (MAE)**
	- **Root Mean Square Error (RMSE)** corresponds to the Euclidean norm -> shortest distance between two points calculated using Pythagoras’ theorem. The square of the total distance between two objects is the sum of the squares of the distances along each perpendicular co-ordinate.
	- **Mean Absolute Error (MAE)** corresponds to Manhattan norm -> sum of absolute differences between points across all the dimensions calculated using sum of the absolute differences of their Cartesian coordinates. Total sum of the difference between the x-coordinates and y-coordinates.
- **Hamming Distance**: Used to Calculate the distance between binary vectors.
- **Minkowski Distance**: Generalization of Euclidean and Manhattan distance.
- **Cosine distance**: measures the similarity between two vectors of an inner product space.

#### Accuracy / Cross Validation
- **Confusion Matrix**: count the number of times instances of class A are classified as class B. For example, to know the number of times the classifier confused images of 5s with 3s, you would look in the fifth row and third column of the confusion matrix.
	- **Precision**: true positives / (true positives + false positives)
	- **Recall (sensitivity)**: true positives / (true positives + false negatives). Precision is typically used along with another metric named recall, also called sensitivity or the true positive rate (TPR). This is the ratio of positive instances that are correctly detected by the classifier.
	- both precision and recall can be calculated from confusion matrix
- F1 score is the harmonic mean of precision and recall 
- Binary classifiers => **Receiver Operating Characteristic (ROC)** curve are used that plots true positive rate / false positive rate i.e. sensitivity (recall) versus 1 – specificity, where **Sensitivity** is true negative rate.
- One way to compare classifiers is to measure the **Area Under the Curve (AUC)**. A perfect classifier will have a ROC AUC equal to 1, whereas a purely random classifier will have a ROC AUC equal to 0.5
- **you should prefer the PR curve whenever the positive class is rare or when you care more about the false positives than the false negatives. Otherwise, use the ROC curve.**

#### Classifiers
- Some algorithms (such as SGD classifiers, Random Forest classifiers, and naive Bayes classifiers) are capable of handling multiple classes natively. 
- Others (such as Logistic Regression or Support Vector Machine classifiers) are strictly binary classifiers.

