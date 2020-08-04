# Machine-Leanrning-Project1

## Part One: Logistic Regression for Digit Classication

You have been given data corresponding to images of handwritten digits (8 and 9 in particular). 
Each row of the input data consists of pixel data from a (28 * 28) image with gray-scale values between 0.0 (black) and 1.0 (white); 
this pixel data is thus represented as a single feature-vector of length 28*28 = 784 such values. 
The output data is a binary label, with __0 representing an 8, and 1 representing a 9.__

__Tasks__
1. Build a logistic regression model with max_iter set ( i = 1,2,...,40 ), then produce two plots showing the track of accuracy and the log loss.
2. Using __coef\_ atrribute__ you can access the weights which the model assigns to each feature in the data. Produce a plot with the values of iter as x-axis and eature weight as y.
3. Build and evaluate model for each value C, the inverse penalty strength. Record the result and also to confusion matrix on test data.
4. Plot the images that are false positive or false negative.
5. Analyze all of the final weights produced by your classier. Reshape the weight coefficients into a matrix, corresponding to the pixels of the original images, and plot the result.


## Part Two: Sneakers vs Sandals

You have been given data from [Fashion MNIST](https://github.com/zalandoresearch/fashion-mnist/) data-set, your task is to build a logistic regression classiffer for this data. You should explore different *feature transformations*, transforming the input features (in any way you see fit) that are given to the regression classiffer.

### 2.1  Introduction:
The data provided has sparse matrix form, which means there are lots of number 0 in the CSV but little number above 0 can we find. This is the characteristic of data in image processing. 

The problems caused by sparse matrix are apparently, our model can not fit the test data well enough,and the processing or training part is annoying and wasting time. We need to wait for a long time but do lots of useless counting work to train our model, since there are many meaningless 0 in the data, but we need to consider every point in the data set.

As I known, there are two useful patterns to process the data, which means standardization and normalization. Python gives us some useful tools to lower the dimension of data set or say compress the information, but consider we need to use logistic regression in this work, I will not try those tricky tools.

In this part, I tried to use two kinds of standardization ways, one is Scalar from sklearn preprocessing part, the other is simple but useful, the 01 standardization.As for feature preprocessing part, I consider to add the square terms, cubic terms, histogram information and spatial information such as the local maximum and average.

### 2.2 Data Training and Comparision

__2.2.1 01 Standardization__<br>
Idea: If the input value is greater than 0, then set it as 1.<br>
This can increase the characters of the data and make it easier for machine to learn.

__2.2.2 StandardScaler (with_std=False)__<br>
Idea: By this method, we can center and scale independently to each feature by computing the relevant statistics on the samples in the training set. <br>
Consider the data in this situation only range 0 to 1 so we set the parameter with_std as false.<br>
But after some counting and comparison, I found this way is not good enough as 01-way, the scalar performances bad after many kinds of preprocessing. So for the best training model, I did not choose it as my final model.

__2.2.3 Best C__<br>
Surprisingly, I found the parameter C plays a great role with standard scaler data, I randomly tried so number of C to train my logistic regression model and they performs well in the result. <br>
I think for future study, we can combine part one and part two, which means we can use the X_test data in order to look for the best C and then we might get higher accuracy in the end.

__2.2.4 Add Histogram Information X = [X, c^🇹]__<br>
Idea: In this way we compute every positive feature and add this to the end of the data set.<br>
This way performs really well, it reduce the error rate apparently. I was amazed by its performance but don’t know exactly why. I think it is a good way to empersize every feature in every vector, so for this kind of data, the training model can find out the “special characters” well.

__2.2.5 Add Spatial Information X_max or X_avg__<br>
Add the local maximum or average is a basic idea for image processing. Since we found that for every pixel in a image, the point near by will look likely as the point itself, which is background CNN model considered. <br>
Just as the histogram method below, we compute the neighbors for every pixel and add this information into the vector.

__2.2.6 Extend with Square. X = [ X, X² ]__<br>
Idea: for every X_i, we compute its square and add it to the end in order to extend the data.<br>
This and the following method is an initial way to extend the data set. But I found it is not good for spare matrix. There are 2 reasons:<br>
1) since there are many 0 in data set,most of the computing part it’s just a waste of time
2) extend information by only adding the square does not help to explore the features’ characteristics. The number is range 0 to 1, which means the square one will be less than the original one, that doesn’t help a lot for machine to learn.

__2.2.7 Extend with Square and Cube. X = [ X , X², X³]__<br>
idea is the same as writen before.
