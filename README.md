# Analyzing Amazon Fire Tablet Reviews from 2010-2017
For my CIS 9665 Applied Natural Language Processing graduate course, my group and I analyzed Amazon Fire Tablet reviews ranging from 2010-2017. My contributions to this project include cleaning and transforming the data, downsampling the skewed data, fitting the Bernoulli Naive Bayes classifier model, and fitting the Decision Tree model. Majority of this project we used the nltk library and sklearn library.

# Introduction & Research Question
For many online shoppers, the consumer reviews for products are a crucial part of their shopping journey; checking reviews have become an integral part for shoppers to decide whether or not they will purchase a product based on previous buyer's recommendations. Our goal for this project was to understand whether keywords or features predict if a product like the Amazon Fire Tablet will be recommended or not? And if so, which keywords or features are the most influential?

# Data Preprocessing
The Amazon reviews are found on Kaggle as 3 separate CSV files. In total there are 67,992 customer reviews with 21-24 columns. After importing them to Jupyter Notebook and storing them in 3 separate dataframes, we dropped unneccesary columns for the purposes of this analysis and merged all 3 dataframes to 1 overarching dataframe. Additionally, we converted columns to strings and recoded columns. The next step in the preprocessing step was to clean the reviews text by lowercasing and dropping stopwords except for "not"/"no" which will be needed in the next data exploratory section.

Since there were many Amazon products in the dataset, we decided to only focus on 1 product because we believe we'll be able to perform the same analysis for all other products once we can do so on one. We deicded to focus our project on analyzing the Amazon Fire Tablet as it had the most reviews; there are 12,691 reviews for Fire Tablets.

# Downsampling
An issue our team noticed with the dataset is that of the 12,691 reviews for the Amazon Fire Tablet, 95.1% recommend the product while the remaining 4.9% do not as you can see in the graph below:

![downsampling1](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/797f70e1-a19e-49be-bcc9-3a9d5f5e9caa)

The data was heavily skewed, thus we needed to apply downsampling methods to our dataset. To do so, I utilized the resample() function from the Pandas library to balance the number of reviews for the Fire Tablet that recommend it and the number of reviews that do not recommend it. This evened out the imbalance and the final dataset our team used for the analysis is a 50% split between the 2 types of reviews with 626 reviews each.

![downsampling2](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/7ed31255-bc66-452e-b681-d38eff074fe9)

# Data Exploration
To explore the reviews for the Fire Tablet, we used term frequency-inverse document frequency (TF-IDF) to identify important words in reviews that recommend and don't recommend the product. TF-IDF is a scoring metric that's used to determine how important a word is to a document in a collection. In the reviews that recommend the product, the words that received the highest scores are "great", "use", "price", "bought", "good" and "easy". In the reviews that didn't recommend the product, the words that received the highest scores are "not", "tablet", "amazon", "would", and "apps". We did the same analysis with bigrams which looked at 2 words most frequently appearing together; however, there did not appear to be a significant sentiment difference between the 2 types of reviews. Next, we also used part-of-speech tagging (POS tagging) to identify what words were the most popular for nouns, adjectives, adverbs, and verbs to explore the relationships between the different parts of speech used.

# Feature Engineering & Feature Selection
For our analysis, we needed to extract the necessary features; we chose sentiment analysis, review length, and TF-IDF to transform review text to predict desired response of either "recommended" or "not recommneded". Sentiment Analysis is the process of analyzing text to determine if the emotional tone of the message is positive, negative, or neautral; we extracted the polarity score of each review and used the compound score; the compound score is the normalized score of the positive and negative sentiments for each review. Review length is considered as a factor as well because according to studies done, they indicate review length is highly associated with review ratings. TF-IDF is also calculated for each word and obtained 2,802 unique words then utilized them as features. These set of feature engineering processes resulted in a total of 2,804 features extracted from our dataset.

During the pre-modeling process, we wanted to simplify our significant number of features, so we utilized a random forest model. Using the RandomForestClassifier function from scikit-learn, we selected the top 10 most important features. These top 10 features now only include the following: compound, review length, "not", "great", "easy", "love", "slow", "tablet", "loves", "ok".

![top 10 words](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/eefa18a2-a0bb-41b9-a815-d9888a6f8fad)

The evaluation scores of this model were all reasonably high with an accuracy rate of 83% and precision, recall, and F1-scores all being close to or over 80% as well.

# Data Modeling: Logistic Regression, Naive Bayes, and Decision Tree
For our project we explored modeling our dataset with Logistic Regression, Naive Bayes, and Decision Tree. We split our dataset into 2 parts: 90% training dataset and 10% testing dataset; this is done to ensure that the evaluations reflect how well the models will perform with unseen data. To train and test the models with upcoming statsmodels and scikit-learn libraries, we split the dataset into x and y, which created a total of 4 variables: x_test, x_train, y_test, and y_train. Each of the x variables consisted of all 10 selected feature columns and the y variables are derived from the "Reviews.doRecommend" column.

For our logistic regression model, it revealed the association of each feature with the response through clear metrics. In this analysis, we employed the Logit() function to model the relationship. Below are the results:

![logreg results](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/1cd28d0f-7f35-4be1-abca-bf8712855743)

In this report, we can see that all 10 features selected are statistically significant with p-values all less than 5%. The coefficients indicate the correlation and considerable impact of each feature on the likelihood of a product being recommended or not.

For the Naives Bayes model, I utilized the BernoulliNB() function from the scikit-learn library. It ran with our training dataset to predict the testing dataset and the model yieled an accuracy rate of 76.19%. Results. Below are the results:

![NB results](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/19fc9196-e8af-49f1-939a-67dd39bd8c94)

For the Decision Tree, I used the DecisionTreeClassifier() function from the scikit-learn library and limited the tree depth to 3 levels. It yielded an accuracy rate of 73%. Below are the results:

![DT results](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/15683e90-b283-461c-969e-7e067cd7c04d)

Here is also a look at the 3-level decision tree:

![DT](https://github.com/sallywuhoo/CIS9665-Tablet-Reviews/assets/148400043/be253843-2cbb-4e97-82ef-6edd061ea63e)

There are 8 terminal nodes and reviews are classified as either "Recommnded" or "not recommended". It appears that the most notable independent variables that affect the prediciton results are the compound score and the review length. Taking a closer look at the first level, if a review has a compound score less than 0.44, it will continue down the branch to the left; if the review has a compound score greater than 0.44, it will continue to the right. At the second level to the left, if a review falls to this branch, the secondary decision made is based on the compound score again; so if the score is less than 0.254, it will continue to the left and if it has a value greater than that, it will continue to the right. On the second level to the right, if a review falls to this branch, the secondary decision made is based on the number of words in the review; so if the review has less than 52.5 words, it will continue to the left and if it has greater than 52.5 words, it will continue to the right. This trend continues down the branch until the final decisions are reached. Note that at this 3 level tree, words that impact the decisions include “love” and “easy” both on the third level.

When comparing the Decision Tree model and the Logistic Regression model, it appears that the results are relatively in line with each other. Since compound has a positive coefficient, reviews with higher scores are linked to being recommended; similarly, with the Decision Tree, reviews with higher compound scores continue down the right hand side branch grouping it in conclusions that the review will recommend a product. Similarly, this pattern appears with the words “easy” and “love” as well both having positive coefficients in the Logistic Regression and being in the Decision Tree that higher TF-IDF scores for these words will lead to reviews recommending products. Additionally, when looking at the variable review_length, this also shows both models are in parallel; in the Logistic Regression model, review_length has a negative coefficient indicating that shorter reviews are more likely to recommend a product, and similarly in the Decision Tree at the second level where review_length appears, reviews with less than 52.5 words continue down a path where they are classified as recommended.
