# CustomerSegmentation_Berthelsmann

1. Installation
	- Anaconda  Navigator 1.8.7
	- Python 3.6
	- Packages: pandas, numpy, scikitlearn, matplotlib, seaborn
	
2. Project motivation
In this project, Arvato Bertelsmann provides real live data describing the German population and a customer data set representing the customers of an mail-order company called AZ Direkt.
The goal is to identify customers from AZ Direkt in the general population and create a model that predicts if a person is likely to turn into a customer.

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
The accuracy will be measured with the AUC-ROC curve. This curve is constructed by plotting the false positive rate against the true positive rate. A perfect classifier creates a dot in the top left corner (100% correct predicted with 0 false positives). The worse the model, the closer to the diagonal. We use this accuracy measurement because the Kaggle training data is extremely unbalanced.

Models
I decided to use ensemble methods because they combine different models to one. For example the basis model that RandomForest/Adaboost/GradientBoosting-Classifiers build on is decision trees. They can be used for regression as well as for classification that is needed here.
Beside these three models, I added LogisticRegression because I was interested into that performance.

Result kaggle
As expected, the kaggle score is not very good, circa 60,4%. I guess that I dropped an important column...

My main goal is to pass this project and the nano degree program. Until 15th january (deadline for this project), I will try to find the column or columns and to improve my results.
