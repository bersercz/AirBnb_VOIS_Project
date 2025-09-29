# AIRBNB Hotels Data Analysis

# About Airbnb DataSet

### The data of Hotels suitated in US of AIRBNB is given with their specifications. This data is available as a CSV file. I'm have analyzed this data set using the Pandas, Matplotlib, Seaborn & Numpy DataFrame. This is internshipnproject and dataset is provided by VOIS.


## Analyzing & Cleaning the dataset

## To view data
```pyhton
data

data.head(5)

data.columns
```

### To get data info
```python
data.info()
data.describe()
```

## Cleaning & viewing Duplicates
```python
data[data.id.duplicated()]

data.drop_duplicates(inplace = True)

data.drop(['house_rules', 'license'], axis = 1, inplace = True)

data.dropna(inplace = True)

data.isnull().sum()

data = data.drop(data[data['availability 365'] > 500].index)
```

## To get the data index & Columns
```python
data.shape
```

## To get unique values & no of unique values
``` python
data.country.unique()
data.country.value_counts()
data['review rate number'].nunique()
data['review rate number'].unique()
```

## Changing the dtypes
```python

data['id'] = data['id'].astype(str)
data['host id'] = data['host id'].astype(str)
data['Construction year'] = data['Construction year'].astype(int)
```
# Problem Statement & Solutions

## 1- Find the randam data with highest review?
```python
data.loc[data['review rate number'].idxmax()]
```

## 2- Find the randam data with lowest review?
```python
data.loc[data['review rate number'].idxmin()]
```


## 3- Identify the host identity (verified or not) with chart 

##  --- I have divided the dataset two parts (verified and non-cinfirmed) and then found avg review, impact of service fee wrt to neighbourhood, neighbour group, host name , Hotels face value. From this user and company will get to know the best overview and estimate earning of the hotels wrt to neighbourhood and neighbourhood groups.
### (Solutions of this is in Analysis.ipynb file wisit their to view solutions with graphical representation)

## 4- Find the top estimate earners with  host name wrt to month & graphical representation

```python
graph10 = data.groupby(['Month_Name','host name'])['max_earning'].max().sort_values(ascending = False).head(10)

# unstacking df
graph10 = graph10.unstack(level = 'Month_Name')

#graphical representation
graph10.plot(kind = 'barh', stacked = True, legend = True, grid = True);
plt.title('Estimate Monthly Earning of Hosts')
plt.xlabel('Monthly Total Earnings');
plt.savefig('Est_earning.png', bbox_inches = 'tight', transparent = True);
```

## 4- Find the top estimate losses with thier host name wrt to month & graphical representation

```python

graph11 = data.groupby(['Month_Name','host name'])['max_earning'].min().sort_values(ascending = True).head(10)

# unstacking the df
graph11 = graph11.unstack(level = 'Month_Name')

#representatiaon through graph
graph11.plot(kind = 'bar', stacked = True, grid = True);
plt.title('Losses Of Hosts ')
plt.ylabel('Loss');
plt.savefig('Loss_Of host.png', transparent = True, bbox_inches ='tight');
```

## 5 Error corretion of name brookln in dataset
```python
data.loc[data['neighbourhood group'] == 'brookln' , 'neighbourhood group'] = 'Brooklyn'
```
## 6  What are the different property types in the dataset?

```python
property_types = data['room type'].value_counts().to_frame()
property_types

# graphical representation
plt.bar(property_types.index, property_types.loc[:,'count']);
plt.ylim([0, 50000])
plt.xlabel('Room Type')
plt.ylabel('Room Type Count')
plt.title('Property Types and their Count in the dataset');
```

## 7 Which neighbourhood has the heigher number of listing?
```python
hood_group = data['neighbourhood group'].value_counts().to_frame()
hood_group

#graphical representation
hood_group_bar = plt.bar(hood_group.index, hood_group.loc[:, 'count'])
plt.bar_label(hood_group_bar, labels = hood_group.loc[:,'count'], padding= 4);
plt.ylim(0,50000);
plt.xlabel('Neighbourhood Groups')
plt.ylabel('Nmber of Listings');
```


## 8 Which neighbourhood group have the highest average prices for airbnb listings?
```python
avg_price = data.groupby('neighbourhood group')['price'].mean().sort_values(ascending = False).to_frame()

# graphical represntation
avg_price_bar = plt.bar(avg_price.index, avg_price.loc[:, 'price']);
plt.ylim(0,700)
plt.bar_label(avg_price_bar, labels = round(avg_price.loc[:,'price'],2), label_type = 'edge', padding = 4)
plt.xlabel('Neighbourhood Group')
plt.ylabel('Average Price per Listings ($)')
plt.xticks(rotation = 45)
plt.title('Average price per listings ($) in eacg neighbourhood group');
```

## 9 Is there a relatioship between the construction year of property and price?
```python
data.groupby(data['Construction year'])['price'].mean().to_frame().plot()
plt.xlabel('Construciton Year')
plt.ylabel('Average Price in ($)')
plt.title('Average price ($) for properties in each constriction year');
```

## 10 FInd the top 10 hosts names with their listngs count?
```pythoon
hosts = data.groupby('host name')['calculated host listings count'].sum().sort_values(ascending = False).nlargest(10).to_frame()

# graphical representaion
hosts_bar = plt.bar(hosts.index, hosts.loc[:,'calculated host listings count'])
plt.bar_label(hosts_bar, label = hosts.loc[:, 'calculated host listings count'], label_type = 'edge', padding =3);
plt.xlabel('Hosts Name')
plt.ylabel('Calculated hosts listings Count');
plt.xticks(rotation = 80)
plt.ylim([0,120000])
plt.title('Top 10 hosts by calculated host listings Count');
```


## 11 Are Hosts with verified identities more likely to receive positive reviews?
```python
review = data.groupby('host_identity_verified')['review rate number'].mean().sort_values(ascending = False).to_frame()

#graphical representation
review_bar = plt.bar(review.index, review.loc[:,'review rate number'])
plt.bar_label(hosts_bar, labels = round(review.loc[:, 'review rate number'], 2),padding =3);
plt.xlabel('Host Verification Status')
plt.ylabel('Average Review rate number');
plt.ylim([0,4])
plt.title('Average review for each verification statistics');
```

## 12 IS there a correration between the price of a listing and its service fee?
```python
data['price'].corr(data['service fee'])

# graphical representation

sns.regplot(data, x= 'price', y = 'service fee');
plt.xlabel(' price in $')
plt.ylabel('Service fee in $')
plt.title('A regression plot showing the correlation of the price of a listing and its service fee');
```

## 13 What is the avg review rate number for listing , and does it vary based on the neighbourhood group and room type?
```python
avgrev = data.groupby(['neighbourhood group', 'room type'])['review rate number'].mean().to_frame()

# graphical representataion
plt.figure(figsize = (10,8))
sns.barplot( data =avgrev, x= 'neighbourhood group', y = 'review rate number', hue = 'room type')
plt.xlabel('Neighbourhood group')
plt.ylabel('Average review rate')
plt.title('Average Review for each Room/Property type in each Neighbourhood Group');
plt.savefig('barplot.png', transparent = True, bbox_inches = 'tight')
```

## 14 Are hosts with the higher calculated host listings count more likely to maintian higher availability throughout the year?
```python
sns.regplot(data,x = 'calculated host listings count', y = 'availability 365');
plt.xlabel('Calculated Host listing')
plt.ylabel('Availability 365')
plt.title('A regression Plot of the relationship between calculated host listing count and availability 365');
plt.savefig('regplot', bbox_inches = 'tight')
```

## Is there any correlation between host listings count and availability 365?
```python
data['calculated host listings count'].corr(data['availability 365'])
```

# Summary
### Objective: Analyzed Airbnb hotel booking data to optimize decision-making for hosts and enhance guest satisfaction.

###Key Findings: Identified peak booking seasons, assess pricing strategies, understand guest preferences, and evaluate host performance.

### Focus Areas: Booking behaviors, pricing impacts, guest amenities, and host responsiveness.

### Future Directions: Developed advanced predictive models,personalize guest recommendations, implement dynamic pricing strategies, and integrate external data sources.

### Impact: Improved listing performance, elevate guest experiences, and strengthen Airbnb's market position.

### Outcome: Delivered actionable insights for hosts and strategic enhancements for Airbnb's platform.



# Conclusion

### This project has provided a comprehensive analysis of the United States (US) Airbnb dataset, shedding light on various aspects of the short-term lodging market. Through data wrangling, exploratory data analysis (EDA), and interpretation of summary statistics, we've uncovered valuable insights into listing distribution, pricing dynamics, host, and review analysis.

### Key findings include the dominance in counts of Entire home/apt listings, the variability in listing counts across neighborhood groups, and the downward trend between property construction year and price. Additionally, the analysis highlighted the significance of verified host status on review rates, as well as the strong correlation between listing price and service fee.

### Furthermore, conducting sentiment analysis on guest reviews to understand factors driving customer satisfaction and preferences could inform targeted marketing strategies and product improvements for Airbnb. Lastly, expanding the analysis to include predictive modeling techniques, such as regression or machine learning algorithms, could enable forecasting of listing demand, pricing trends, and customer behavior, facilitating strategic decision-making for stakeholders in the short-term lodging industry.

### __________________________________________________________________________________________________________________________________________________________________________
# By: Devansh Singh Tomar
