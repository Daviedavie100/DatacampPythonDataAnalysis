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
