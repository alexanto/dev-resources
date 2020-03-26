### Supervised machine learning

- What is supervised machine learning? 

### Linear regression

- What is regression? Which models can you use to solve a regression problem?
- What is linear regression? When do we use it?
- What’s the normal distribution? Why do we care about it?
- How do we check if a variable follows the normal distribution?
- What if we want to build a model for predicting prices? Are prices distributed normally? Do we need to do any pre-processing for prices?
- What are the methods for solving linear regression do you know?
- What is gradient descent? How does it work?
- What is the normal equation?
- What is SGD  —  stochastic gradient descent? What’s the difference with the usual gradient descent?
- Which metrics for evaluating regression models do you know?
- What are MSE and RMSE?

### Validation

- What is overfitting?
- How to validate your models?
- Why do we need to split our data into three parts: train, validation, and test?
- Can you explain how cross-validation works?
- What is K-fold cross-validation?
- How do we choose K in K-fold cross-validation? What’s your favorite K?

### Classification

- What is classification? Which models would you use to solve a classification problem?
- What is logistic regression? When do we need to use it?
- Is logistic regression a linear model? Why?
- What is sigmoid? What does it do?
- How do we evaluate classification models?
- What is accuracy?
- Is accuracy always a good metric?
- What is the confusion table? What are the cells in this table?
- What is precision, recall, and F1-score?
- Precision-recall trade-off
- What is the ROC curve? When to use it?
- What is AUC (AU ROC)? When to use it?
- How to interpret the AU ROC score?
- What is the PR (precision-recall) curve?
- What is the area under the PR curve? Is it a useful metric?
- In which cases AU PR is better than AU ROC?
- What do we do with categorical variables?
- Why do we need one-hot encoding?

### Regularization

- What happens to our linear regression model if we have three columns in our data: x, y, z  —  and z is a sum of x and y?
- What happens to our linear regression model if the column z in the data is a sum of columns x and y and some random noise?
- What is regularization? Why do we need it?
- Which regularization techniques do you know?
- What kind of regularization techniques are applicable to linear models?
- How does L2 regularization look like in a linear model?
- How do we select the right regularization parameters?
- What’s the effect of L2 regularization on the weights of a linear model?
- How L1 regularization looks like in a linear model?
- What’s the difference between L2 and L1 regularization?
- Can we have both L1 and L2 regularization components in a linear model?
- What’s the interpretation of the bias term in linear models?
- How do we interpret weights in linear models?
- If a weight for one variable is higher than for another  —  can we say that this variable is more important?
- When do we need to perform feature normalization for linear models? When it’s okay not to do it?

### Feature selection

- What is feature selection? Why do we need it?
- Is feature selection important for linear models?
- Which feature selection techniques do you know?
- Can we use L1 regularization for feature selection?
- Can we use L2 regularization for feature selection?

### Decision trees

- What are the decision trees? 
- How do we train decision trees?
- What are the main parameters of the decision tree model?
- How do we handle categorical variables in decision trees?
- What are the benefits of a single decision tree compared to more complex models?
- How can we know which features are more important for the decision tree model?

### Random forest

- What is random forest?
- Why do we need randomization in random forest?
- What are the main parameters of the random forest model?
- How do we select the depth of the trees in random forest?
- How do we know how many trees we need in random forest?
- Is it easy to parallelize training of a random forest model? How can we do it?
- What are the potential problems with many large trees?
- What if instead of finding the best split, we randomly select a few splits and just select the best from them. Will it work?
- What happens when we have correlated features in our data?




