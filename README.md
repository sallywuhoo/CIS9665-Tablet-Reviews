# Analyzing Amazon Fire Tablet Reviews from 2010-2017
For my CIS 9665 Applied Natural Language Processing graduate course, my group and I analyzed Amazon Fire Tablet reviews ranging from 2010-2017. My contributions to this project include cleaning and transforming the data, downsampling the skewed data, fitting the Bernoulli NAive Bayes classifier model, and fitting the Decision Tree model. Majority of this project we used the nltk library and sklearn library.
# Introduction & Research Question
For many online shoppers, the consumer reviews for products are a crucial part of their shopping journey; checkings reviews have become an integral part for shoppers to decide whether or not they will purchase a product based on previous buyer's recommendations. Our goal for this project was to understand whether keywords or features predict if a product like the Amazon Fire Tablet will be recommended or not? And if so, which keywords or features are the most influential?
# Data Preprocessing
The Amazon reviews are found on Kaggle as 3 separate CSV files. In total there are 67,992 customer reviews with 21-24 columns. After importing them to Jupyter Notebook and storing them in 3 separate dataframes, we dropped unneccesary columns for the purposes of this analysis and merged all 3 dataframes to 1 overarching dataframe. Additionally, we converted columns to strings and recoded columns. The next step in the preprocessing step was to clean the reviews text by lowercasing and dropping stopwords except for "not"/"no" which will be needed in the next data exploratory section.
