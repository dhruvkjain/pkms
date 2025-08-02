[[ML/index|ML/index]]

## Sources
- [Great for learning concepts, functions and different methods - Hands on Machine Learning with ScikitLearn, Keras & TensorFlow](http://14.139.161.31/OddSem-0822-1122/Hands-On_Machine_Learning_with_Scikit-Learn-Keras-and-TensorFlow-2nd-Edition-Aurelien-Geron.pdf)


## Research Articles
- [How Deepseek used MultiToken Prediction](https://youtu.be/4BhZZYg2_J4?si=JkppbxMu1gMyqETy)
- [Understanding the difficulty of training deep feedforward neural networks](https://proceedings.mlr.press/v9/glorot10a/glorot10a.pdf)
- [A DISCIPLINED APPROACH TO NEURAL NETWORK HYPER-PARAMETERS](https://arxiv.org/pdf/1803.09820)
- [Bitwise Neural Networks](https://arxiv.org/pdf/1601.06071)





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
- **starting with a very low learning rate (e.g., 10-5) and gradually increasing it up to a very large value (e.g., 10).**
- **This is done by multiplying the learning rate by a constant factor at each iteration (e.g., by exp(log(10^6 )/500) to go from 10^-5 to 10 in 500 iterations).** 
- If you plot the **loss as a function of the learning rate (using a log scale for the learning rate),** you should see it dropping at first. But after a while, the learning rate will be too large, so the loss will shoot back up: the optimal learning rate will be a bit lower than the point at which the loss starts to climb (typically about 10 times lower than the turning point).

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

#### Activation functions
- biological neurons => roughly sigmoid activation functions, but not good for artificial neurons
- different layers may learn at widely different speeds.
- this is mostly caused by activation functions (logistic sigmoid) and the weight initialization techniques (i.e., a normal distribution with a mean of 0 and a standard deviation of 1)
- vanishing gradients problem (gradients grow smaller and smaller)
	- gradients often get smaller and smaller as the back propagation algorithm progresses down to the lower layers
- exploding gradients problem (gradients grow larger and larger)
- **Glorot and He Initialization**: 
	- variance of the outputs of each layer to be equal to the variance of its inputs
	- number of inputs in a layer = fan-in
	- number of neuron in a layer = fan-out
	-  **Glorot initialization** or Xavier initialization  
		- fan-avg = (fan-in + fan-out) / 2
		- Normal distribution with mean 0 and variance σ^2 = 1 / fan-avg
		- Or a uniform distribution between −r and  + r, with r = sqrt( 3 / fan-avg )
	- **LeCun initialization**
		- when fan-avg = fan-in is used in Glorot initialization
		- Normal distribution with mean 0 and variance σ^2 = 1 / fan-in
		- Or a uniform distribution between −r and  + r, with r = sqrt( 3 / fan-in )
	- **He initialization** (initialization of ReLU layers and it's variants)
		- Normal distribution with mean 0 and variance σ^2 = 2 / fan-in

Initialization parameters for each type of activation function:
![[Pasted image 20250729095932.png]]

for the uniform distribution, just compute r = sqrt ( 3 * σ^2 )

- ReLU better than sigmoid function but suffers from dying ReLU where they only output 0, never activating that side of neural network
- Use Leaky ReLU-α (z) = max(αz, z) instead 
	- α = 0.2 (a huge leak) better than α = 0.01 (a small leak)
- Randomized Leaky ReLU (RReLU)
	- α is picked randomly in a given range during training and is fixed to an average value during testing
- Parametric Leaky ReLU (PReLU)
	- α is authorized to be learned during training
	- making α an parameter instead of hyperparameter
	- good for large dataset
- Exponential Linear Unit (ELU) (good but heavy to compute so use Leaky instead)
	- ELU-α (z) = α * (e^z − 1)       if z < 0 
	-                      z                         if z ≥ 0
	- α = 1 is a good start
- Scaled ELU (SELU) (conditional best)
	- SELU-α (z) = λ* α * (e^z − 1)       if z < 0 
		-                λ * z                         if z ≥ 0
		- α = λ = 1 is a good start
	- network will self-normalize: the output of each layer will tend to preserve a mean of 0 and standard deviation of 1 during training only if:
		- input features must be standardized (mean 0 and standard deviation 1)
		- LeCun normal initialization is done
		- network’s architecture must be sequential
- **SELU > ELU > leaky ReLU (and its variants) > ReLU > tanh > logistic**
- **Batch Normalization**
	- zero-centers and normalizes each input, then scales and shifts the result using two new parameter vectors per layer: one for scaling, the other for shifting.
	- ![[Pasted image 20250729132157.png]]
	- Here x are inputs of that layer and z are output x-hat is normalized vector
	- To sum up, four parameter vectors are learned in each batch-normalized layer: γ (the output scale vector) and β (the output offset vector) are learned through regular backpropagation, and μ (the final input mean vector) and σ (the final input standard deviation vector) are estimated using an exponential moving average.
	- clip the gradients during backpropagation so that they never exceed some threshold. This is called **Gradient Clipping**.
		- ideal is to clip for -1 to 1 after normalizing the values i.e. the vector (0.9, 100.0) will be clipped to (0.00899964, 0.9999595)

#### Optimizers
- **Momentum Optimization**: Imagine a bowling ball rolling down a gentle slope on a smooth surface: it will start out slowly, but it will quickly pick up momentum until it eventually reaches terminal velocity (if there is some friction or air resistance).
	- Gradient Descent: θ = θ – η * ∇θ(J(θ))
		- gradient of the cost function J(θ) with regard to the weights (∇θ(J(θ)))
	- Momentum algorithm: 
		- m = βm - η * ∇θ(J(θ))
		- θ = θ + m
		- here m is momentum vector (initially all 0).
		- β is momentum which must be set between 0 (high friction) and 1 (no friction), 0.9 mostly used.
- **Nesterov Accelerated Gradient**: measures the gradient of the cost function not at the local position θ but slightly ahead in the direction of the momentum, at θ + βm
	- m = βm − η * ∇θ(J(θ + βm))
	- θ = θ + m
- **AdaGrad**: Consider the elongated bowl problem again: Gradient Descent starts by quickly going down the steepest slope, which does not point straight toward the global optimum, then it very slowly goes down to the bottom of the valley. It would be nice if the algorithm could correct its direction earlier to point a bit more toward the global optimum. The AdaGrad algorithm achieves this correction by **scaling down the gradient vector along the steepest dimensions.**
	- **often stops too early when training neural networks so don't use this**
	- s = s + ∇θ(J(θ)) ⊗ ∇θ(J(θ))
	- θ = θ − η * ∇θ(J(θ)) ⊘ sqrt(s + ε)
	- ⊗ means element-wise multiplication, ⊘ means element-wise division
	- ε is a smoothing term to avoid division by zero, typically set to 10^(–10)
	- **this algorithm decays the learning rate, but it does so faster for steep dimensions than for dimensions with gentler slopes**
	- This is called an **adaptive learning rate**.
- **RMSProp**: fix for AdaGrad slowing down a bit too fast and never converging to the global optimum. RMSProp algorithm fixes this by accumulating only the gradients from the most recent iterations (as opposed to all the gradients since the beginning of training). **It does so by using exponential decay in the first step**.
	- s = βs + (1-β) * ( ∇θ(J(θ)) ⊗ ∇θ(J(θ)) )
	- θ = θ − η * ∇θ(J(θ)) ⊘ sqrt(s + ε)
	- The decay rate β is typically set to 0.9
	- ε is a smoothing term to avoid division by zero, typically set to 10^(-7) or 10^(–10)
- **Adam Optimization (adaptive moment estimation)**: momentum optimization (exponentially decaying average of past gradients) + RMSProp (exponentially decaying average of past squared gradients)
	- m = (β1 * m) − (1-β1) * ∇θ(J(θ))
	- s = (β2 * s) + (1-β2) * ( ∇θ(J(θ)) ⊗ ∇θ(J(θ)) )
	- m-hat = m / (1 - (β1^t))
	- s-hat = s / (1 - (β2^t))
	- θ = θ + η * (m-hat ⊘ sqrt(s-hat + ε))
	- t represents the iteration number (starting at 1)
	- β1 is typically initialized to 0.9, β2 is often initialized to 0.999
	- ε is a smoothing term to avoid division by zero, typically set to 10^(–7) or 10^(-10)
	- two variants of Adam:
		- **AdaMax Optimization**:
			- m = (β1 * m) − (1-β1) * ∇θ(J(θ))
			- s = max(   (β2 * s)   ,   ∇θ(J(θ))   )
			- m-hat = m / (1 - (β1^t))
			- θ = θ + η * (m-hat ⊘ s)
		- **Nadam Optimization**:
			- Adam + Nesterov Accelerated Gradient
- All the optimization techniques discussed so far only rely on the first-order partial derivatives (Jacobians). The optimization literature also contains amazing algorithms based on the second-order partial derivatives (the Hessians, which are the partial derivatives of the Jacobians). Unfortunately, these algorithms are very hard to apply to deep neural networks because there are n 2 Hessians per output (where n is the number of parameters), as opposed to just n Jacobians per output. Since DNNs typically have tens of thousands of parameters, the second-order optimization algorithms often don’t even fit in memory, and even when they do, computing the Hessians is just too slow.
- **RMSProp ~ Nadam > AdaMax > Adam > Nestrov > Momentum > SGD > GD** 

#### Classifiers
- Some algorithms (such as SGD classifiers, Random Forest classifiers, and naive Bayes classifiers) are capable of handling multiple classes natively. 
- Others (such as Logistic Regression or Support Vector Machine classifiers) are strictly binary classifiers.

### Final Deep Neural Network Configs
- ![[Pasted image 20250731013706.png]]
- ![[Pasted image 20250731013731.png]]




