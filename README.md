# CustomerSegmentation_Berthelsmann

1. Installation
	- Anaconda  Navigator 1.8.7
	- Python 3.6
	- Packages: pandas, numpy, scikitlearn, matplotlib, seaborn
	
2. Project motivation

In this project, Arvato Bertelsmann provides real live data describing the German population and a customer data set representing the customers of an mail-order company called AZ Direkt.
The goal is to identify customers from AZ Direkt in the general population and create a model that predicts if a person is likely to turn into a customer.

To achieve this, I will use KMeans. KMeans is a clustering method - an unsupervised learning algorithm - to create customer segments. In contrast to hierachical clustering, the execution time of KMeans is linear to the amount of data instead of being quadratic. Because of the huge amount of data, I decided to use KMeans. 
Fitting KMeans to the demographic data and predict clusters for both the german population and the customers, I expect that there will be clusters in the customer data with a much higher proportion of people than the demographic data (a sign that these people are persons of interest), and clusters with a lower proportion of people (indicating that these people are out of interest for the company).
But before applying this method to the data, I use principal component analysis (PCA) to reduce the dimensions of the data set. On the one hand, this will optimize the KMeans execution time, on the other hand this method calculates so called eigen vectors. An eigen vector is a vector containing weights for each feature. These weights describe how much the relevant feature increase this component. By sorting the absolute values in such a vector, the features can be summarized to a so called "latent feature" that gives a more general description of people who can be described. (Below, there is an example discussion on a latent feature describing pensioners who like to drive sport cars).
The last part of this project is to build a binary classification model that predicts a persons likeliness to become a customer or not. To achieve this, I will train several supervised learning models that can be used for binary classification in hope to find a model with a good accuracy that does not overfit or underfit. The precondition is that important/informative features have not beed dropped. If so, the model won't be able to make good predictions.

3. Problem Description

Profit and growth is the main goal of each company. Mail-order companies grow by the number of customers (among other AMAZON).
As a mail order company, persons of interest are those who have an internet connection and an email address. One of the best ways to catch attention is an email campaign. But sending an e-mail to each person on the world is "very inefficient" and not a good idea. So the list of addressed persons should consist of those persons where the probability of becoming a customer is > 0.
But how do we identify potential customers? 

4. Provided Files

DIAS Attributes - Values 2017.xlsx - description of attributes and their values

DIAS Information Levels - Attributes 2017.xlsx - description of attributes without their values

Udacity_AZDIAS_052018.csv - the German population (represented by roughly 900.000 persons)

Udacity_CUSTOMERS_052018.csv - the core customers of AZ Direct (circa 191.600 person, 366 columns (features)

Udacity_MAILOUT_052018_TRAIN.csv - response file of a campaign containing same columns plus one column giving the information if the person became a customer

Udacity_MAILOUT_052018_TEST.csv - final test file for model evaluation at kaggle.

4. Interaction

Unfortunatelly, the interaction with this notebook is not possible because the provided files from Bertelsmann are not public!

As mentioned above, I am creating a library that falilitates working with dataframes.
For example searching column names by wildcard is supported, extracting unique values out of columns with the ability to define a configuration value cleaning and preparation.
Feel free do modity parameters. 


5. Segmentation results

The customers seem to be more financial investors, no financial minimalists (over 60% in 5th cluster), but rather money savers. 
This goes along with the fact that FINANZTYP_2, 5 are almost not present, but 3 and 1 are overrepresented. They tend to be over 45 
ALTERSKATEGORIE is one-hot encoded and the fact that 3/4 are are over .35 and .58 means that these people tend to be over 45, more likely to be 60 (PRAEGENDE_JUGENDJAHRE_Ages center point is 6.3462783537449665 in original data (not pca data)). Finally, they live in regions with a low movement rate.

In contrast to these people stand those whoe are described by cluster center 5 and 7. They are younger than 45 and the tendency to be a financial minimalist is very high, being a money saver, investor or a person with a own house in focus is very low. They live in regions with a high share of households with 6 persons and more and a high movement rate. 
All these descriptions go with the fact that people in the underrepresented clusters tend to be not traditional minded, but the people in the overrepresented cluster are highly trad. minded.

6. Supervised Model

Metrics

The PCA object has an implemented score function and provides probalistic interpretation for each component. These values are stored in the attribute "explained_variance_ratio_" and represent the amount of variance each component explains regarding to the original data set. 
To determine the required amount of principal components, I will set a minimum limit (minimum % of original data that I intend to explain with the components), iterate over all explained_variance_ratio_ values and stop until the sum is bigger or equal than my limit. With that number, PCA will be fitted again to the data.
Regression metrics like Mean Average Error, Root Mean Square Error, .. cannot be used as we have a binary classification problem in the supervised part. 
For classification, there are metrics like Precison-Recall, ROC_AUC, Log-Loss but we will use ROC_AUC curve. 
AUC_ROC stands for "Area Under the Receiver Operating Characteristic curve". ROC plots the true positive rate against the false positive rate by various thresholds and the area under this curve describes the accuracy of the model as the maximum area is 1x1. A perfect classifier creates a dot in the top left corner (100% correct predicted with 0 false positives). The worse the model, the closer to the diagonal. 
In contrast to Log-Loss, F1-Score and Accuracy, ROC_AUC does not rely on the threshold set for classification. The predicted absolute values are irrelevant, the ranks of the predictions are taken into consideration.

Models

I decided to use ensemble methods because they combine different models to one. A nice example I read was this one: Assume that you are ill and the morbus is unknown, would you rather go to a single doctor who is professional in one area, or would you rather contact a group of doctors where everyone is professional in his own métier?
This is the idea behind ensemble methods. EM use different classifiers during training and those classifiers with the best accuracy will be used. That means that ensemble methods 
stabilize machine learn algorithms and increase their accuracy
and help to reduce overfitting as well as the variance of single estimators because several estimates from several models are combined.

The basis model that RandomForest/Adaboost/GradientBoosting-Classifiers build on is decision trees. They can be used for regression as well as for classification that is needed here.
Beside these three models, I added LogisticRegression because I was interested into that performance.

Result kaggle

My second approach incresed the previous score by 18% up to 78%.
