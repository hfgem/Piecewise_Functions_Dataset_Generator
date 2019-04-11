Hannah Germaine

# About
This repository houses a set of functions dedicated to the study of how well neural networks can learn a piecewise function dataset of varying complexity. You will find code dedicated to generating piecewise function datasets, code dedicated to generating piecewise function descriptors that can then be used to generate datasets, and code dedicated to training neural networks on piecewise function datasets.

# Piecewise_Dataset_Generator.py
This program will create datasets for piecewise functions on the interval `[0,1)`. The generated information will be stored in a given filepath in 3 documents:
* A .csv file containing the dataset as x,y pairs
* A .txt file containing a description of the dataset
* A .png file containing an image of the plotted dataset

## Customization:
* To save data in a file path other than the desktop, line 109 will need to be modified.
* To modify the number of pieces desired, line 113 will need to be modified.
* To modify the number of datapoints desired, line 114 will need to be modified.
* The program currently randomly selects the possible degrees of the polynomials as well as the possible coefficients.
** To modify the degrees, line 34 will need to be modified.
** To modify the coefficients, lines 38, 42, and 45 will need to be modified.
* The program currently generates datapoints on the interval `[0,1)`. To adjust, line 49 will need to be modified.

# generate_supporting_txt.py
This program will generate the following four files describing a dataset of k pieces and a maximum degree of complexity n on the interval `[0,1)`:
* dataset_k_n_about.txt : this file will contain the formulas for the set of piecewise functions generated as well as the expected number of pieces
* dataset_k_n.png : this image will display the dataset, showing clearly the different pieces and marking the points of break
* points_of_break_k_n.txt : this file will contain an array of the points at which one piece's function ends and the next piece's function begins
* polynomial_coefficients_k_n.txt : this file will contain arrays of the coefficients for the piecewise functions

The main purpose of this program is to provide the points of break and polynomial coefficients to the neural network program so that, across many different trials with different activation functions, the same piecewise function is being learned.

## Customization:
* To set the directory in which to store the generated data, modify the filepath variable at line 123
* To change the how many pieces you want your piecewise function to have, modify the sizes array - if you include more than one number (as the code does), it will result in datasets being created for each size
* To modify the maximum complexity (number of degrees) a function can have within the piecewise function, modify the degrees variable. For each value in the array, the program will create a dataset
* Within the generate_data function, you can modify the range of coefficients (it is currently random.randint(-10,10) and can be modified to be constant, a larger range, etc...)

# Piecewise_NN3.py
This program will use the dataset information generated by generate_supporting_txt.py to train a neural network and determine how well it learns the piecewise function. The program leverages tensorboard for insight into the loss over time, as well as has settings for running on a GPU. Note, this means you'll need to run a tensorboard directory command so you can see the loss over time as the program runs:
                tensorboard --logdir ~/Deskt/directory_here/

To properly use this program along with the generate_supporting_txt.py program, ensure that the files variable at line 70 matches the filepath variable at line 123 of the generate_supporting_txt.py program. Additionally ensure that the sizes (line 87) and degrees (line 88) match those same variables in the generate_supporting_txt.py program (lines 126 and 127 respectively).

You'll see that this code is written for a three-layer neural network with relu activation functions and can be customized to other activation functions, other initializers, etc...

The goal of this program is to run 8 experiments for a set size neural network and see how well it learns the dataset. If it does not learn it such that the loss is below a certain loss threshold (line 84) for 7/8 experiments, then the size of the neural network is increased by an update factor (line 83). If the average loss is higher than the previous round of experiment's loss, then the neural network is decreased by the update factor.

The program will proceed with a particular combination of complexity and degree

## Customization:
Because there is quite a bit of customization possible within this program, I will defer to my comments as guidelines within the code of what can be changed and below note the largest points.
* The filepath (line 68) and files (line 70) variables should be modified to match the directories where you want to save data and access the polynomial data respectively.
* The learning rate for the neural network can be modified at line 73.
* The momentum rate can be modified at line 74.
* The initializer for the neural network's biases can be modified at line 75.
* The number of samples generated for testing can be modified at line 76.
* The number of experiments you'd like to run, q, can be modified at line 77. Note, this will cause the number of successful experiments needed to move on to be q-1.
