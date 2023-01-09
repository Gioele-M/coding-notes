# Deep Learning and Neural Networks
Machine learning algorithms learn from ì patterns to find the best representation of the data, which are then used to make predictions about new data that it has never seen before.
_Deep learning_ is a subfield of machine learning with similar concept of 'learning' eg we create the model, train it and make predictions with it. Deep learning is used with many different types of data such as text, images, audio and more.
*Deep* refers to the numerous 'layers' that ransform the data. This is done by successive layer attempts to learn progressively complex patterns from the data fed into the model.
Ex. facial recognition, the model takes in a photo as input, and the layers perform specific staps to identify whose face is in the picture. For example:
- Find face in the image using edge detection
- Analyse the facial features
- Compare against features in repository
- Output prediction

These models perform best with massive amount of data, if there is no availability of it, they are comparable to ML models. These models learn from raw data without human intervention, so with large amount of data perform better than ML that relies on feature extraction from developers.
Deep learning models often require GPUs to perform optimally.




## Deep learning math
*Scalar, Vectors and Matrices*
- Scalars: A scalar is a single quantity that you can think of as a number.
- Vectors: Vectors are arrays of numbers. In python are often denoted as NumPy arrays eg x = np.array([1,2,3])
- Matrices: Matrices are grid of information with row and columns. In NumPy x = np.array([[1,2,3],[4,5,6],[7,8,9]])



*Tensors*
The data structure used in deep learning is called a tensor, which is a generalised form of a vector and matrix: _a multidimensional array_. This is used to better interpret and manipulate data.
Tensors can be manipulated with matrix algebra.





## Neural networks concept
Neural networks work with _inputs_, these are the datapoints that we feed. The input can have different features, so when we give a data point we create an _input layer_ where each node represents a different input feature. Other than the input layer, a neural network has two different types of layers:
- Hidden layers:
	+ Layers between the input and the output layer. They introduce complexity in our neural network and help with the learning process. (Can vary >0)
- Output layer:
	+ It is the final layer in the neural network and prduces the final result, so there is only 1 per network

Each layer in a neural network contains _nodes_. Nodes between layers are connected by _weights_. These are the learning parameters in our neural network, determining the strength of the connection between each linked node.
The weighted sum between the nodes and weights is calculated between each layer; for example the weighted sum of the inputs and our weights is:
`weighted_sum = (inputs • weight_transpose)+bias`

At the weighted sum is applied the _activation function_. All the transformations done so far are linear, activation functions introduce nonlinearity in our learning model, creating more complexity during the learning process.
_Activation functions_ are essentials for the model as it elevates it above ML models. The activation function *decides* what is fired to the next neuron based on its calculation for the weighted sums. The most common functions applied are:
- ReLU
	+ is like on the x axis where x<0 then spikes up
- Sigmoid
	+ S shaped function 
- Softmax


The process is carried through the different neurons in _forward propagation_.


*Loss functions*
When a value is outputted, we calculate its error using a loss function. Two commonly used are:
- Mean squared error
	+ Used in linear regression
- Cross entropy loss
	+ Used for classification learning models



*Backpropagation*
When outputs are predicted inaccurately, _backpropagation_ and _gradient descent_ are used. Forward propagation dels with feeding the input values through hidden layers to find the final output layer. Backpropagation refers to the computation of gradients with the gradient descent algorithm. This continuously updates and refines the weights between neurons to minimise our loss function. (Gradient is the rate of change with respect to the parameters of our loss function, basically the function determines how much each weight is contributing to the error in our loss function and update the weights accordingly to decrease the error)


*Gradient descent*
The process of gradient descent is represented in a U curve graph, where we determine the gradient of the tangent of the point we're currently at in the U curve. If the gradient is high we should lower our input, if it's low we should raise it.
Formula
`parameter_new=parameter_old+learning_rate⋅gradient(loss_function(parameter_old))`
The learning rate affects how large the 'steps' changes in input should be. A high learning rate might overshoot while a small might require too much computation

*Stochastic gradient descent* cuts down time in the process by performing gradient descent on a random data point to use at each iteration instead of the entire data set.
Other gradient descents include:
- Adam optimisation algorithm
	+ Adaptive algorithm that finds individual learning rates for each parameter
- Mini-batch gradient descent
	+ Similar to the stochastic but iterates on a batch of fixed size insted of 1 data point










------------------------

# Perceptron
Perceptron is an algorithm that allowed an artificial neuron to simulate a biological neuron. The artificial neuron could take in an input, process it based on some rules, and fire a result. The revolutionary part is that this neuron can train itself based on its own results and fire better results in the future (just like an actual neuron). 




Perceptrons are the units or neurons we defined before; these are the building blocks of neural networks
A perceptron has 3 main components:
- Inputs
	+ Each input corresponding to a featuer
- Weights
	+ Each input has a weight which assigns a certain amount of importance to the input
- Output
	+ Finally the perceptron uses the inputs and weights to produce an output.


Building a perceptron takes:
- Perceptron class
	+ constructor
		* number inputs and array of weights of the inputs
	+ weighted sum method
	+ Activation method
		* used to constrain the weighted sum to produced a desired output (function that determines the answer)
	+ Training method
		* used to determine how bad the activation function is and what the error is 
	+ Perceptron algorithm 
		* used to tweak the weight of the inputs and reduce the error (this is inside the training method)



```py
class Perceptron:
  def __init__(self, num_inputs=2, weights=[1,1]):
    self.num_inputs = num_inputs
    self.weights = weights
    
  def weighted_sum(self, inputs):
    weighted_sum = 0
    for i in range(self.num_inputs):
      weighted_sum += self.weights[i]*inputs[i]
    return weighted_sum
  
  def activation(self, weighted_sum):
    if weighted_sum >= 0:
      return 1
    if weighted_sum < 0:
      return -1
    
  def training(self, training_set):
    #if algorithm has found line separating the two sets
    foundLine = False
    while not foundLine:
      total_error = 0
      for inputs in training_set:
        prediction = self.activation(self.weighted_sum(inputs))
        actual = training_set[inputs]
        error = actual - prediction
        total_error += abs(error)
        #adjust weight with perceptron algorithm
        for i in range(self.num_inputs):
          self.weights[i] += error*inputs[i]
      #break loop
      if total_error == 0:
        foundLine = True
     
  # To get a line you can get:
  # slope = -self.weights[0]/self.weights[1]
  #intercept = -self.weights[2]/self.weights[1]


cool_perceptron = Perceptron()
small_training_set = {(0,3):1, (3,0):-1, (0,-3):-1, (-3,0):1}
cool_perceptron.training(small_training_set)
print(cool_perceptron.weights)
```


























