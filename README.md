# challenge-evaluation
This rep showcases model evaluation.

We focus on one model, **RandomForest**, and find the best metrics, evaluations and hyperparameters.

## 0. Setting-up the environment

Link to repository: https://github.com/AmauryvanKeste/challenge-evaluation

No trello PM set-up for this project.

## 1. Loading the data and early strategies

The data stems from the US census and consists of 48842 instances. 

The data has been cleaned by the coaches as the purpose is solely model evaluation.

The dataset has been divided in 2 subsets: "train" and "test" according to 1/3, 2/3 ratio. 

First we will perform model evaluation on the original split of the data. 

Afterwards we will merge the two datasets and train/split them according to a 1/5 ratio and see how the model evolves.

## 2. Create a function that returns some metrics evaluating the model

We select and obtain the following evaluation results:

Internal and external model evaluations:

![2021-08-16_12h14_17](https://user-images.githubusercontent.com/84380197/129548292-241dbdb6-2187-4d03-b751-14c79617ee71.png)

The confusion matrix:

![2021-08-16_12h15_16](https://user-images.githubusercontent.com/84380197/129548419-20e46619-4d5d-4579-a9a7-a1ed38c81810.png)

The ROC curve:

![2021-08-16_12h15_24](https://user-images.githubusercontent.com/84380197/129548448-2573bb32-dd24-4a24-a7e3-2b7f6ef1dc31.png)

We observe high scores. Let's see if we can improve them with grid search.

## 3. Hyperparameter tuning

### 3.1 Random Search

We select the following hyperparameters to finetune:
    n_estimators = number of trees in the foreset
    max_features = max number of features considered for splitting a node
    max_depth = max number of levels in each decision tree
    min_samples_split = min number of data points placed in a node before the node is split
    min_samples_leaf = min number of data points allowed in a leaf node
    bootstrap = method for sampling data points (with or without replacement)

We first execute a Random Search on the model to see if we can already have a rough idea of the hyperparameter values.

After 48 minutes we obtain the following first hints:

    n_estimators: 1600
    min_samples_split: 10
    min_samples_leaf: 1
    max_features: sqrt
    max_depth: 20
    bootstrap: True

### 3.2 GridSearch

Now let's finetune even more granularly. 

We spcecify the following parameters based on the Random Search info we obtained:

    'bootstrap': [True],
    'max_depth': [15,20,25],
    'max_features': [3,4],
    'min_samples_leaf': [1, 2],
    'min_samples_split': [8, 10, 12],
    'n_estimators': [1500,1600,1700]

From this we obtain the following best GridSearch parameters:
    max_depth=15
    max_features=4
    min_samples_leaf=2
    min_samples_split=10
    n_estimators=1500

Now, let's have a look at how our model scores with these specific hyperparameters:

Internal and external model evaluations:

![2021-08-16_15h05_51](https://user-images.githubusercontent.com/84380197/129568417-4d27d280-9a32-4c1b-ac03-71bfc61adfbc.png)

The confusion matrix:

![2021-08-16_15h06_00](https://user-images.githubusercontent.com/84380197/129568419-1eb88033-e88e-4b4d-bcb1-3e0d31b60ea4.png)

The ROC curve:

![2021-08-16_15h06_11](https://user-images.githubusercontent.com/84380197/129568420-cf7f065c-281f-4b02-bf36-fc66e2c651eb.png)

The model scores slightly better on the test size while the training score goes down.
We can conclude that the the previous model was overfitted.

## 4. Merging and splitting differently

Now let's have a look at our model if we merge the two datasets into one and split according to a 1/5, 4/5 ratio.

We then obtain the following metrics.

### 4.1 Without hypertuning:

Internal and external model evaluations:

![2021-08-16_15h10_16](https://user-images.githubusercontent.com/84380197/129568946-8f7eb8d1-b7ca-4e22-8ecb-927d5bf13d39.png)

The confusion matrix:

![2021-08-16_15h10_27](https://user-images.githubusercontent.com/84380197/129568950-d7417bc1-e865-4699-bff2-103f56044e2a.png)

The ROC curve:

![2021-08-16_15h10_36](https://user-images.githubusercontent.com/84380197/129568951-b2a31836-ff16-4e79-be4e-6163bc747050.png)

### 4.2 With hypertuning:

We use the previously obtained hyperparameters.

Internal and external model evaluations:

![2021-08-16_15h12_51](https://user-images.githubusercontent.com/84380197/129569300-057f63ae-5501-46d2-be8a-db4fd491242e.png)

The confusion matrix:

![2021-08-16_15h13_00](https://user-images.githubusercontent.com/84380197/129569304-52b1745d-b4f7-4c81-9492-d201fcecfb42.png)

The ROC curve:

![2021-08-16_15h13_06](https://user-images.githubusercontent.com/84380197/129569307-c6cc9ce9-b56b-4675-83ce-c8284762e797.png)

## 5. Conclusion

We obtain the highest scores accross all boards with a 1/5, 4/5 split together with hypertuned parameters.

