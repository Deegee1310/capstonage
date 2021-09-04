<h1> Churn game - Predicting the churners</h1>
<img src="https://s2.ax1x.com/2019/09/27/uugh8K.png">

Sparkify is a digital music service similar to Netease Cloud Music or QQ Music. Many of the users stream their favorite songs in Sparkify service everyday, either using free tier that places advertisements in between the songs, or using the premium subscription model where they stream music as free, but pay a monthly flat rate. User can upgrade, downgrade or cancel their service at anytime.

This is a Customer Churn Prediction Problem , there are so many similar projects, such as WSDM - KKBox’s Churn Prediction Challenge competition from Kaggle.
So, our job is deep mining the customers’ data and implement appropriate model to predict customer churn as follow steps:<br>

<h2> Steps to follow</h2>
- Clean data: fill the nan values , correct the data types, drop the outliers.<br>
- EDA: exploratory data to look features’ distributions and correlation with key label (churn).<br>
- Feature engineering: extract and found customer-features and customer-behavior-features; Implement standscaler on numerical features.<br>
- Train and measure models: I choose logistic regression, linear svm classifier, decision tree and random forest classifier to train a baseline model and tuning a better model from best of them. <br>
It is worth mentioning that this data is unbalanced because of less churn customers, so we choose f1 score as a metrics to measure models’ performance.<br>


<h2>Quick Facts</h2>
A mini subset of size 125 MB of the original 12 GB customer log json data file will be used for creating the prediction model. The small dataset has 286’500 log entries with 18 unique columns.<br>

<img src="https://i.ibb.co/vq5nyX7/Bildschirmfoto-2021-09-04-um-11-23-35.png">

<h2>Exploratory Data Analysis</h2>

We use the Cancellation Confirmation events of page column to define the customer churn, and perform some exploratory data analysis to observe the behavior for users who stayed vs users who churned.<br>
<h3>Churn</h3>
<img src="https://i.ibb.co/FWz8MB3/Bildschirmfoto-2021-09-04-um-11-34-00.png"> 
So, there are 52 users have churned events in the dataset, it’s about 23.1% churned rate. The rate of churn and not churn is roughly 1:3, so this is an unbalanced dataset.

<h3>Gender</h3>
<img src="https://i.ibb.co/cc5cFsn/Bildschirmfoto-2021-09-04-um-11-34-27.png">
Can we say the gender has effect on Churn or not ? We calculate the p-value and result is 0.20 over 0.05, so, we can’t say like that.


<h3>Page</h3>
<img src="https://i.ibb.co/TM1z18W/Bildschirmfoto-2021-09-04-um-11-34-57.png">
page
We count each item in page column of different group and normalized data.
Obviously, NextSong has accounted for most of customers’ events. Thumbs Up ,Thumbs Down , Home and Add to Playlist have effect on churn too.
We count each item in page column of different group and normalized data.

<h3>userAgent</h3>
We extract the browser and platform of customers from userAgent column.
<img src="https://i.ibb.co/qR6GPm1/Bildschirmfoto-2021-09-04-um-11-35-29.png">
<img src="https://i.ibb.co/sKBtQf1/Bildschirmfoto-2021-09-04-um-11-35-55.png">
Customers using safari and iPad have more proportion in churn.

<h3>Time</h3>
We extract day-of-month, day-of-week and hour from ts column.

<img src="https://i.ibb.co/n1zJqTx/Bildschirmfoto-2021-09-04-um-11-36-26.png">
<img src="https://i.ibb.co/Z8cdr0x/Bildschirmfoto-2021-09-04-um-11-36-41.png">
<img src="https://i.ibb.co/cvkTXXM/Bildschirmfoto-2021-09-04-um-11-37-00.png">
Customers from churn group have more events after 15th in one month, and have less events in weekend.

<h2>Feature Engineering</h2>

<h3>On the basis of the above EDA, we can create features as follows:</h3>
<h4>Categorical Features</h4>
- gender<br>
- level<br>
- browser<br>
- platform<br>
<h4>Numerical Features</h4>
- mean,max,min,std of length of users<br>
- numbers of each item in page (ThumbsUp …<br>
- number of unique songs and total songs of users<br>
- number of unique artists of users<br>
- percentage of operations after 15th in a month<br>
- percentage of operations in workday<br>
<br>
We implement label encoding on categorical features and standard scaler on numerical features.

<h2>Modeling</h2>

We split the full dataset into train and test sets. Test out the baseline of four machine learning methods: Logistic Regression, Linear SVC, Decision Tree Classifier and Random Forest Classifier.

<img src="https://i.ibb.co/GHdd0h6/Bildschirmfoto-2021-09-04-um-11-37-30.png">

Though the LinearSVC spent more training time, but it can get the highest f1 score 0.702. And the LogisticRegression has a medium training time and f1 score, maybe I can tuning it to get a higher
score. So I’ll choose LinearSVC and LogisticRegression to tuning in next section, the result is as follows:

<h3>Linear SVC</h3>
- Training time:
<img src="https://i.ibb.co/fCr3V4J/Bildschirmfoto-2021-09-04-um-12-15-23.png">

- F-1 Score:
<img src="https://i.ibb.co/6Fh03CT/Bildschirmfoto-2021-09-04-um-12-15-49.png">


<h3>LogisticRegression</h3>
- Training time:
<img src="https://i.ibb.co/1GpQXs0/Bildschirmfoto-2021-09-04-um-12-19-12.png">

- F-1 Score:
<img src="https://i.ibb.co/VNSNptL/Bildschirmfoto-2021-09-04-um-12-19-31.png">

As we can see in above, the logistic regression (0.741) can get a higher f1 score as the linear svm classifier(0.705) and also saves computing-time than the latter, considering this is only a quit mini dataset and our purpose is scaling this up to the total 12G dataset, so, the logistic regression is the best model from now on in this project.

<h2>Conclusion</h2>

<h3>Reflection</h3>

In this project I set out to predict customers’ churn problem with the dataset of a music streaming service named Sparkify. This is a binary classification problem , so I choose four supervised learning algorithm to found a model. After evaluated and tuning, I find out the logistic regression is the suitable model for this project because of its balanced and high f1-score (0.741) and time spending.

By the way, I once fell into the trap of data leakage ,so that all of the models can achieve a performance that seems too good (1 for f1-score) to be true. I had to go back to check my feature engineering, and found I put the cancellation confirmation which is the flag of churn in the features, what a awkward thing! And that teach us you must be careful and patient when you are working.

<h3>Improvement</h3>

Another improvement could be to try out more features or deep learning models.

<h3>Github Repo</h3>

Hope you find this interesting and for further details on this analysis like code and process followed would be available <a href="https://github.com/Deegee1310/capstonage">click here</a>
