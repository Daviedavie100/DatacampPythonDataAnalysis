# DatacampPythonDataAnalysis

## Exploratory Data Analysis

- Exploratory Data Analysis, or EDA, is the process of cleaning and reviewing data to derive insights such as descriptive statistics and correlation and generate hypotheses for experiments. 
- EDA results often inform the next steps for the dataset, whether that be generating hypotheses, preparing the data for use in a machine learning model, or even throwing the data out and gathering new data!
- Methods used: `.info(), .value_counts(), .describe()`

## Why perform EDA?

EDA is performed for a variety of reasons, such as detecting patterns and relationships in data, generating questions or hypotheses, or to prepare data for machine learning models.

### Data validation

- Data validation is an important early step in EDA. We want to understand whether data types and ranges are as expected before we progress too far in our analysis! Let's dive in.
- Use `.dtypes` to check data type and `.astype(type)` to convert to type
- We can select and view only the numerical columns in a DataFrame by calling the *`.select_dtypes`* method and passing "number" as the argument e.g. `data.select_dtypes('number')`

### Aggregating ungrouped data

- The dot-agg function, short for aggregate, allows us to apply aggregating functions. 
- By default, it aggregates data across all rows in a given column and is typically used when we want to apply **more than one function**. 
- Usage `.agg(['mean', 'std', 'min', 'max'])`, 
- or `.agg({col_1_name:['mean', 'std'], col_2_name:['min', 'max']})` use of dictionary
- or `groupby('col').agg(col_mean=('col_to_find_mean',mean'), col_std=('col_to_find_std', 'std'))`

### Addressing missing data

- Why is missing data a problem? It can affect distributions, less representative of the popultion hence leading us to draw incorrect conclusions
- Check missing data `data.isna().sum()`
- There are various strategies for addressing missing data
**(a)** One rule of thumb is to remove observations if they amount to five percent or less of all values. 
**(b)** If we have more missing values, instead of dropping them, we can replace them with a summary statistic like the mean, median, or mode, depending on the context (imputation)
- **(c)** Impute by sub-group 

#### process

- Multiply the lenth og the data by 5% `threshold=len(data)*0.05`
- Filter columns with missing values less than the threshold `columns_to_drop=data.columns[data.isna().sum()<=threshold]`
- Drop the columns with missing values `data.dropna(subset=columns_to_drop, inplace=True)`
- Impute summary statistics for the new data `cols_with_missing_values=new_data.columns[new_data.isna().sum()>0]` 
- then fill with mode `for col in cols_with_missing_values[:-1]: new_data[col].fillna(new_data[col].mode()[0])` and if there are still mising values;
- impute using subgroup meian `new_data_dict=new_data.groupby('col_with_missing_vals')['col_whose_median_is_used'].median().to_dict()` 
- then fill `new_data['col_whose_median_is_used']=new_data['col_whose_median_is_used'].fillna(new_data['col_with_missing_values'].map(new_data_dict))`

### Converting and analyzing categorical data

- Filtering/selecting data with non-numric columns `data.select_dtypes('object')`
- Count number of unique values `data.nunique()` gives integer-exact number of uniques
- Extracting value from categories. We can use the **pandas series-dot-string-dot-contains method**, which allows us to search a column for a specific string or multiple strings. `data.str.contains()` for single and `data.str.contains('str1|str2')` for multiple phrases and `data.str.contains('^new')` for string starting with phrase 'new'
#### process
- create caterories `cat_list=[]`
- create phrases `val1='str1|str2', val2='str3|str4', val3='str5|str5'`
- create condition list `conditions=[(data['col'].str.contains(val1)), (data['col'].str.contains(val2)), (data['col'].str.contains(val3))]`
- create category in the data `data['category']=np.select(condinditions, cat_list, default='Other')` 

### Working with numeric data

- Converting strings to numbers
- First, we need to remove the commas from the values,use pandas `.str.replace()`
- Next, we change the data type to float usin `.astype(float)`
- Lastly, we'll make a new column in the dataframe `data['new_col']=`

### Adding summary statistics into a DataFrame

- sometimes we might prefer to add summary statistics directly into our DataFrame, rather than creating a summary table.
- using pandas .transform and lambda function `data['std_dev']=data.groupby()[].transform(lambda x: x.std())`

## Handling outliers

-  an outlier is an observation that is far away from other data points.

1. Using descriptive statistics

- identifying outliers with the pandas dot-describe method. `data.describe()`
- when the maximum value is more than four times the mean and median. 

2. Using the interquartile range

- We can define an outlier mathematically. First, we need to know the interquartile range, or IQR, which is the difference between the 75th and 25th percentiles. 
- check IQR in box plots. You can also calculate IQR, `75th _percentile=data.quantile(0.75)` while `25th_percentile=data.quantile(0.25)` IQR is the difference
- Then use IQR to finde an **upper outlier** `upper_threshold=75th_percentile + 1.5 * IQR` and **lower outlier** `lower_threshold=25th_percentile - 1.5 * IQR`
-  Subsetting our data to obtain values beyond the threshold `(data<lower) | (data>upper)`

-  **Why look for outliers?** during EDA

- These are extreme values and may not accurately represent the data.
- They can skew the mean and standard deviation. 

- **What to do about outliers?** ask yuorself why these outliers exist. If they are a representative of a subset of the data, we could just **leave the values** however if we do know the values are not accurate, error occured during data collection then **remove the values** `(data>lower) & (data<upper)`

##  Patterns over time

- **Importing DateTime data**

- adding the *parse_dates* keyword argument to the CSV import and setting it equal to a list of column names that should be interpreted as DateTime data. `parse_dates=['col']`
- we may wish to update data types to DateTime data after we import the data using **pd.to_datetime** `data[col]=pd.to_datetime(data[col])`
- we can also use pd-dot-to_datetime to combine "month", "day", and "year" into single date `data[col]=pd.to_datetime(data[["month", "day", and "year"]])`
- we might also want to extract just the month, day, or year from a column containing a full date, we can append **.dt.month** to extract the month attribute `data[col]=data[col].dt.year`

- **Visualizing patterns over time**

- use line plots

## Correlation

- Correlation describes the direction of the relationship between two variables as well as its strength
- it can help us use variables to predict future outcomes. 
- pairwise correlation of numeric columns in a DataFrame, use pandas' `.corr` method, which calculates the Pearson correlation coefficient, and measures the linear relationship between two variables. 
- A negative correlation coefficient indicates that as one variable increases, the other decreases. 
- A value **closer to zero** imply a weak relationship, while values **closer to one** imply stronger relationships. 
- Pearson coefficient only describes the linear correlation between variables. 
- Variables can have a strong non-linear relationship and a Pearson correlation coefficient of close to zero. 
- Data can also have a correlation coefficient indicating a strong linear relationship when quadratic is actually a better fit for the data. 
- so it's always important to complement correlation calculations with scatter plots

- **Visualizing patterns over time**

- use **seaborn heatmap** plots `sns.heatmap(data.corr(), annot=True)` setting the annot argument to True labels the correlation coefficient inside each cell
- use **Scatter plots** `sns.scatterplot(data=data, x='', y='')`
- use **Pairplots**, `sns.pairplot(data=data)` which plots all pairwise relationships between numerical variables in one visualization.
- However, having this much information in one visual can be difficult to interpret, especially with big datasets
- remedy is to limit the number of plotted relationships by setting the vars argument equal to the variables of interest `sns.pairplot(data=data, vars=['cols selected'])`

## Factor relationships and distributions

- Categorical variables are harder to summarize numerically, so we often rely on visualizations to explore their relationships. 

- use  `sns.histplot(hue='cat_col')` but categories overlap with each other
- Kernel Density Estimate (KDE) plots address this overlapping issue `sns.kdeplot(hue='cat_col')`
- Similar to histograms, KDEs allow us to visualize distributions. 
- They are considered more interpretable
- However, due to the smoothing algorithm used in KDE plots, the curve can include values that don't make sense, so it's important to set good smoothing parameters.
- use cut argument `sns.kdeplot(hue='cat_col', cut=0)` cut tells Seaborn how far past the minimum and maximum data values the curve should go when smoothing is applied.
- Cumulative KDE plots `sns.kdeplot(hue='cat_col', cut=0, cumulative=True)`, which is a graph describes the probability
