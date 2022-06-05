#  ANN-based Traffic Light Controller

An Artificial Neural Network based Traffic Light Transition Controller for intersections.

See TensorFlow implementation [here.](https://github.com/raymelon/TrafficLightNeuralNetwork---TensorFlow)


## Overview 
 
Given the previous and current light pair, the controller must predict the next light pair.

The controller must receive two inputs, 

 - Previous light pair at `t - 1`

 - Current light pair at `t`.
 
 ```
                    +-----------+ 
 TL(t - 1) -------->|           |
                    |    ANN    |-------> TL(t + 1)
 TL(t)     -------->|           |
                    +-----------+ 
 ```
 
And outputs the next pair at `t + 1`.

Transitions will be implemented for a pair of traffic lights with the following sequence of pairs:

| Light Sequence Pair | | |
| - | - | - |
| **Traffic Light 1** | **Traffic Light 2** | **Time**
| RED | GREEN | t1
| RED | AMBER | t2
| RED | RED | t3
| GREEN | RED | t4
| AMBER | RED | t5
| RED | RED | t6

## Neural Network
### Design

Having two inputs and one output, the Neural Network architecture will be based on the typical logic gate Neural Network architecture:

- Two input neurons `I1`, `I2`
- Two hidden layer neurons `H1`, `H2`
- One output neuron `O1`

![ANN Architecture](https://github.com/raymelon/TrafficLightNeuralNetwork/blob/master/misc/ANNarchi.png)

The neural network will be a Feedforward neural network, having
the Logistic Sigmoid equation as its activation function.
```
             1
S(t) =  __________
               -t
          1 + e
```

Training will be done using Gradient Descent Backpropagation.

### Training

In order to simplify training, numerical values are mapped with each pair, just like how an index is associated for each row in an array.
We made sure that these numerical values assigned are in the Logistic Sigmoid's curve ranges (`0` to `1`).

| Light Sequence Pair | | |
| - | - | - |
| **Traffic Light 1** | **Traffic Light 2** | **Numerical Value**
| RED | GREEN | 0.1
| RED | AMBER | 0.2
| RED | RED | 0.3
| GREEN | RED | 0.4
| AMBER | RED | 0.5
| RED | RED | 0.6

The training data are as follows:

| Inputs at [`input.csv`](https://github.com/raymelon/TrafficLightNeuralNetwork/tree/master/data/input.csv) | | Outputs at [`target.csv`](https://github.com/raymelon/TrafficLightNeuralNetwork/tree/master/data/target.csv) |
|-|-|-
| **t - 1** | **t** | **t + 1**
0.1 | 0.2 | 0.3
0.2 | 0.3 | 0.4
0.3 | 0.4 | 0.5
0.4 | 0.5 | 0.6
0.5 | 0.6 | 0.1
0.6 | 0.1 | 0.2

Using 50 training sets (or repetitions of these in a file) in 1000 epochs is enough for the network to learn.
