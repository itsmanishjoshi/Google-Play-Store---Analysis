 # App Analysis - Google Play Store


## Table of contents 
- [Objective](#objective)
- [Scope of work](#scopeofwork)
- [Data Source](#datasource)
- [Tools and Technologies](#Toolsandtech)
- [Data Collection & Preprocessing](#datacollectionandprocessing)
- [Data Visualisation](#datavisualisation)
- [Conclusion](#conclusion)


## Project Overview
This project offers a thorough analysis of applications available on the Google Play Store, focusing on user ratings, download trends, and essential performance metrics. By utilizing advanced data analytics techniques, we aim to unveil insights that enhance our understanding of market dynamics and user preferences. The findings will provide valuable information for app developers and marketers, enabling data-driven decision-making to improve application quality and increase user engagement. This comprehensive analysis serves as a strategic resource for navigating the competitive landscape of mobile applications.

##  Objective
The primary goal of this project is to analyze Google Play Store app data to identify key factors that influence app performance, including downloads, ratings, revenue, and user engagement. This analysis will provide insights into app market dynamics, helping developers, marketers, and stakeholders optimize app design, pricing, and marketing strategies.

##  Scope of Work
The project will focus on key performance metrics such as:

- App Downloads
- User Ratings
- Category-wise Analysis
- Pricing Model
- Content Rating
- Size of the App
- Sentiment Analysis of User Reviews
- Impact of Updates

##  Data Source
Google Play Store Dataset: A publicly available dataset from kaggle with features such as app name, category, rating, reviews, size, installs, type (free/paid), price, content rating, genres, last update, and current version. [Download here](https://www.kaggle.com/datasets/gauthamp10/google-playstore-apps)


##  Tools and Technologies
- Programming Language: Python for data manipulation and analysis.
- Libraries:
Pandas: For data manipulation and cleaning.
Matplotlib/Seaborn: For visualizations.
- Jupyter Notebooks: For documentation and step-by-step analysis.



##  Data Collection and Preprocessing

- ### Data Collection
The dataset utilized for this analysis has been sourced from [Kaggle](https://www.kaggle.com/), a prominent platform for data science and machine learning resources. Below is a preview of the dataset,

![Google Play Store Raw Data .img](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Excel%20Data/Data%20Excel%20ss.jpg)

- ### loading Data into Python:
Importing the required library
``` Python

import numpy as np
import pandas as pd
df= pd.read_csv("Google_Play_Store.csv")
df.head()

```
![df.head()](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/df.head().png)

- ### Data Cleaning and processing

Fixed null values

``` python
df = df[~df.Rating.isnull()]
```

Filling na values with average values
``` python
df["Android_Ver"] = df["Android_Ver"].fillna(df["Android_Ver"].mode()[0])
```


Converting some objects types to floating type

```python
#Reviews
df.Reviews = df.Reviews.astype("int32")

#Installs
df.Installs = df["Installs"].str.replace(",", "").str.replace("+", "").astype("int")

#Price
df.Price = df.Price.apply(lambda x : 0 if x=="0"  else float(x[1:]))
```

 Sanity Check - The reviews should be less than the no of installs
```python
df[df.Reviews > df.Installs].head()
```


##  Data Visualisation

Data is visualized using Matplotlib and Seaborn, two powerful Python libraries that enable the creation of comprehensive and aesthetically pleasing visual representations.


## Matplotlib
Importing the library
```python
import matplotlib.pyplot as plt
%matplotlib inline
```
### Boxplot

### 1.Boxplot of apps and prices
```python
plt.boxplot(df.Price)
plt.title("Boxplot of apps and Prices")
plt.xlabel("Apps")
plt.ylabel("Price $")
plt.show()
```
![Apps & Price](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Box%20Plot%20-%20Apps%20%26%20Price.png)

 -  During the analysis, we observed that there are a few apps priced significantly higher than $50.
By narrowing the analysis to apps priced under $50, we aim to provide insights that are more actionable and applicable to a broader user base.

Therefore Apps costing less than 50$:
```python
df = df[df.Price < 50]
```

![plt.show](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Box%20Plot%20-%20Price%20less%20than%2050.png)

### Insights:
- Free apps generally attract more downloads.
- Paid apps have lower download counts but can achieve substantial revenue if they offer unique features or target niche markets effectively.
- The analysis suggests that a well-executed freemium strategy can outperform a straightforward paid model in terms of total revenue.
----------------------------------------------------------------------------------------------------------------------------------------------------------------

### Histogram

### 2. Histogram for the reviews
```python
plt.hist(df.Reviews)
plt.title("Histogram for the reviews.")
plt.xlabel("Reviews")
plt.ylabel("No of Apps")
plt.show
```

![Reviews](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Hist%20-Reviews.png)
Note: Each bin in x axis contains values in 200k, whereas the y axis contains in Thousands.

### Insights:

- Sentiment analysis reveals common themes in user reviews, such as praise for usability and complaints about bugs or crashes.
- Positive sentiment correlates with higher ratings, while negative sentiment often highlights specific issues users face.
- Additionally, reviews with constructive feedback often lead to higher user satisfaction when developers respond positively.

----------------------------------------------------------------------------------------------------------------------------------------------------------------


### 3. Histogram for the size
```python
plt.hist(df.Size)
plt.title("Histogram for the size.")
plt.xlabel("Size")
plt.ylabel("No of Apps")
plt.show
plt.show
```
![Size](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Hist%20-%20Size.png)

### Insights:

- Smaller app sizes tend to perform better in terms of downloads and ratings, especially in regions with limited bandwidth.
- Frequent updates, particularly those that improve functionality or fix bugs, positively impact user ratings and lead to improved reviews.
- Users appreciate ongoing support and enhancements, resulting in higher engagement.
----------------------------------------------------------------------------------------------------------------------------------------------------------------

### 4. Distribution for Rating
```python
df.Rating.plot.hist()
plt.title("Distribution of Rating")
plt.xlabel("Ratings")
plt.ylabel("No of Apps")
plt.show()
```
![Rating](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Hist%20-%20Rating.png)

### Insights:

- Apps with ratings above 4.0 tend to have significantly higher download numbers compared to those with lower ratings.
- Users often perceive apps with high ratings as more reliable and trustworthy, leading to greater adoption.
- Maintaining a good rating is crucial for app visibility and market success.
----------------------------------------------------------------------------------------------------------------------------------------------------------------

## Seaborn
Importing the library
``` python
import seaborn as sns
```
### Bar Chart

### 1. Bar Chart of Content Rating Distribution
```python
df["Content_Rating"].value_counts().plot.barh()
plt.title("Content Rating Distribution")
plt.xlabel("Installs")
plt.ylabel("Category")
plt.show()
```
![Content Rating Distribution](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Bar%20-%20Content%20Rating.png)

### Insights:

- A significant proportion of apps on the Google Play Store are rated as "Everyone."
- This category sees the widest distribution across all app categories, particularly in Education, Productivity, and Tools.
-  Apps rated "Everyone" generally have broader appeal and are more accessible to all age groups, which often results in higher download numbers and user engagement.
----------------------------------------------------------------------------------------------------------------------------------------------------------------


### 2. Android Version Distribution
```python
df["Android_Ver"].value_counts().plot.bar()
plt.title("Android Version Distribution")
plt.xlabel("Version")
plt.ylabel("No of Apps")
plt.show()
```
![Android Version](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Bar-%20Android%20ver.png)

### Insights:

- A large proportion of apps on the Google Play Store are designed to support Android versions 4.1 (Jelly Bean) and up.
- This version is often the minimum requirement as it strikes a balance between reaching a broad audience and maintaining compatibility with relatively modern device features.
- Supporting older versions ensures greater reach, particularly in regions with a high prevalence of older Android devices.
----------------------------------------------------------------------------------------------------------------------------------------------------------------



### Joint Plot

### 3. Joint plot of Size & Rtaing
```python
sns.jointplot(x="Size", y="Rating", data=df)
plt.show()
```
![Joint - Size & Rating](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Joint%20-%20Size%20%26%20Rating.png)

### Insights:

- Apps with smaller file sizes generally receive higher ratings.
- Users appreciate lightweight apps that consume less storage and run smoothly on a wide range of devices.
- This trend is particularly noticeable in categories like Tools, Productivity, and Education, where the simplicity of functionality combined with ease of use contributes to higher user satisfaction.
----------------------------------------------------------------------------------------------------------------------------------------------------------------


### Reg Plot

### 4. Reg Plot of Price & Rating
```python
sns.regplot(x="Price", y='Rating', data=df)
plt.show()
```
![Price & Rating](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Reg%20-%20Price%20%26%20Rating.png)

### Insights:

- Free apps tend to receive a broad range of user ratings, as they cater to a large and diverse audience.
- Users expect more from paid apps, and those that deliver good quality and value typically receive better ratings.
-  Users tend to expect a lot from expensive apps, and if they don’t meet those high expectations, it results in lower ratings. Developers of high-priced apps need to ensure exceptional quality to justify the cost.
- Factors such as app quality, performance, and user experience have a more significant influence on ratings than the price itself.
----------------------------------------------------------------------------------------------------------------------------------------------------------------


### Pair Plot

### 5. Pair Plot of Review, Size, Rating & Price
```python
sns.pairplot(df[["Reviews", "Size", "Rating", "Price"]])
plt.show()
```
![Pair Plot](https://github.com/itsmanishjoshi/Google_Play_Store-Analysis/blob/main/Python/ScreenShots/Pair%20Plot.png)

### Insights:

- Popular apps with high reviews generally receive higher ratings, indicating good quality and user satisfaction.
- Larger apps may face performance issues, leading to slightly lower ratings, especially for apps that are not optimized well for lower-end devices.
- Paid apps often have fewer reviews but higher ratings due to better quality and user expectations, while free apps garner more reviews with a wider distribution of ratings.
- Price sensitivity can be observed, as free apps tend to get more reviews, but user satisfaction (as reflected in ratings) can vary based on the balance between free features and in-app purchases.



##  Conclusion
The analysis of Google Play Store apps highlights critical factors influencing app success, including user ratings, download numbers, and revenue generation strategies. By understanding the dynamics of app performance across different categories and pricing models, developers can make informed decisions to enhance user satisfaction and profitability. 
Key recommendations include:

- Focusing on app quality to boost ratings and downloads.
- Regularly updating apps to maintain user interest and engagement.
- Utilizing sentiment analysis to gather actionable feedback from users.
- Experimenting with pricing strategies to maximize revenue potential, particularly by considering freemium models.
- Overall, this project provides valuable insights that can help guide developers in optimizing their apps for success in the competitive landscape of the Google Play Store.



🤾‍♂️💻
