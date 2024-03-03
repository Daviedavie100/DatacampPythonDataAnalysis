# DatacampPythonDataAnalysis
Contains training materials for python data analysis

## Exploratory Data Analysis

- Exploratory Data Analysis, or EDA, is the process of cleaning and reviewing data to derive insights such as descriptive statistics and correlation and generate hypotheses for experiments. 
- EDA results often inform the next steps for the dataset, whether that be generating hypotheses, preparing the data for use in a machine learning model, or even throwing the data out and gathering new data!
- Methods used: `.info(), .value_counts(), .describe()`

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

## Addressing missing data

- Why is missing data a problem? It can affect distributions, less representative of the popultion hence leading us to draw incorrect conclusions
- Check missing data `data.isna().sum()`
- There are various strategies for addressing missing data
- **(a)** One rule of thumb is to remove observations if they amount to five percent or less of all values. 
- **(b)** If we have more missing values, instead of dropping them, we can replace them with a summary statistic like the mean, median, or mode, depending on the context (imputation)
- **(c)** Impute by sub-group 

### process

- Multiply the lenth og the data by 5% `threshold=len(data)*0.05`
- Filter columns with missing values less than the threshold `columns_to_drop=data.columns[data.isna().sum()<=threshold]`
- Drop the columns with missing values `data.dropna(subset=columns_to_drop, inplace=True)`
- Impute summary statistics for the new data `cols_with_missing_values=new_data.columns[new_data.isna().sum()>0]` 
- then fill with mode `for col in cols_with_missing_values[:-1]: new_data[col].fillna(new_data[col].mode()[0])` and if there are still mising values;
- impute using subgroup meian `new_data_dict=new_data.groupby('col_with_missing_vals')['col_whose_median_is_used'].median().to_dict()` 
- then fill `new_data['col_whose_median_is_used']=new_data['col_whose_median_is_used'].fillna(new_data['col_with_missing_values'].map(new_data_dict))`

## Converting and analyzing categorical data

- Filtering/selecting data with non-numric columns `data.select_dtypes('object')`
- Count number of unique values `data.nunique()` gives integer-exact number of uniques
- Extracting value from categories. We can use the **pandas series-dot-string-dot-contains method**, which allows us to search a column for a specific string or multiple strings. `data.str.contains()` for single and `data.str.contains('str1|str2')` for multiple phrases and `data.str.contains('^new')` for string starting with phrase 'new'
### process
- create caterories `cat_list=[]`
- create phrases `val1='str1|str2', val2='str3|str4', val3='str5|str5'`
- create condition list `conditions=[(data['col'].str.contains(val1)), (data['col'].str.contains(val2)), (data['col'].str.contains(val3))]`
- create category in the data `data['category']=np.select(condinditions, cat_list, default='Other')` 

## Working with numeric data

- Converting strings to numbers
- First, we need to remove the commas from the values,use pandas `.str.replace()`
- Next, we change the data type to float usin `.astype(float)`
- Lastly, we'll make a new column in the dataframe `data['new_col']=`

### Adding summary statistics into a DataFrame

- sometimes we might prefer to add summary statistics directly into our DataFrame, rather than creating a summary table.
- using pandas .transform and lambda function `data['std_dev']=data.groupby()[].transform(lambda x: x.std())`
