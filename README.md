# PANDAS.tutorial.py

Display number of columns for df

pd.set_option(‘display.max_columns’, 85)

Selecting Info

Df.iloc[[0,1], 2] 
This is looking up the first two indexes of the df and proceeding the 3rd item of those two data points

Df.loc[[0,1], ‘email’]
Get the first two items in a df and then the ‘email’ column

Df.loc[[0,1], [‘email’, ‘name]]
Get the first two items in a df and then the ‘email’ and ‘name column

loc vs iloc

Loc grabs by index, iloc by column name
Value_Counts

df[‘Hobbyist’].value_counts()

This will give us the count of the different values in the hobbyist columb. I.e. 12yes, 13no

Loc slicing (IT IS INCLUSIVE!)

df.loc[0:3, ‘Hobbyist’]

df.set_index(‘last’)

This will set the df index to last 
It can be accessed by df.index
Now, df.loc will be: df.loc[‘datapointlastname’, ‘email’]
So we will get the email of the last name entered

df.reset_index(inplace=True)

THis is how to reset the index to the default

Set the default index at the beginning when setting df:

df = pd.read_csv(‘data/ETH.csv’, index_col=’date’) 
To see infomation that is truncated by df.loc, do this: 

schema_df.loc[‘MgrIdiot’, ‘QuestionTest’]

Here the first argument is the name of the index to access, and the second is the output that was truncated. 

Sort index 

schema_df.sort_index(ascending=False)

ascending=False will override the default and show info in a descending order

Filtering 

Df[‘last’] == ‘Doe’

This will return a series, not a dataframe. The series will be bools of weather the filter is true. So 0 false
1 true 
Etc..

Creating the filter…

filt = (df[‘last’] == ‘Doe’)

Df[filt]

This will return a dataframe will all objects that are in the filter, for this example, all objects with the last name of doe

.loc is used to look up columns and rows by label, so this will also work the same way as the df[filt]: df.loc[filt]. With .loc we can further filter. So we can get only the emails of the returned values in the filtered dataframe by entering in df.loc[filt, ‘email]

Filtering with AND and OR

Filtering for all the entereis with a last name of Doe AND a first of John:
filt = (df[‘last’] == ‘Doe) & (df[‘first’] == ‘John’)

Filtering for all the entereis with a last name of Doe OR a first of Jane:
filt = (df[‘last’] == ‘Doe) | (df[‘first’] == ‘Jane’)

Negating filter results

We can add a tilda before filter to get all results that DO NOT meet the filter requirements: 
filt = (df[‘last’] == ‘Doe) & (df[‘first’] == ‘John’):

Df.loc[~filt, ‘email’]  - this will give us the email of everyone whos name is not doe and first is not john in the dataframe

Using a filter

Filt = (df[‘Salary’] > 70000)
Df.loc[filt]

This will give us a new filtered df will all salaries over 70k

Df.loc[filt, [‘Country’, ‘LanguagesWorkedWith’]]

Now we can only display the columns displayed with the filter applied

Filtering with lists

Countries = [‘US’, ‘Can’, ‘UK’]
Filt df[‘country’].isin(countries)

Now we display only countries in this list 

Sorting by CONTAINS

Filt = df[‘LangugaesworkedWith’].str.contains(‘python’, na=False)

THis allows us to search a column by a string, ‘python’ in this example

Look at Columns

Df.columns
This will show us the columns as a list

Update column name

Using a list comprehension 

This will change all column names to uppercase: Df.columns = [x.upper() for x in df.columns]


This will replace all spaces in the names of columns with underscores: Df.columns = df.columns.str.replace(‘ ‘, ‘_’)

Renaming specific columns 

THis will change column names at once: df. rename(columns={‘first_name: first’, ‘last_name: ‘last’}, inplace=True)

Updating a single column

This will update the item under the 3rd index to these new values

All values: 

Df.loc[3] = [‘john’, ‘smith’, ‘cool@cool.com’]

Some values; 

Df.loc[3, [‘last, ‘email]] = [‘john’, ‘doe’]

One value: 
Df.loc[3, ‘last] = ‘Smith’

Updating a column found with filter

Filt = (df[‘email’] = ‘doe)
Df.loc[filt, ‘last’] = ‘smith’

Change all values of one column

Df[‘email’] = df[‘email’].str.lower()

Apply, applymap, map, replace

-Apply

Apply applies a function to all instances in a series

This will give us the lengths of all emails:

df[‘email’].apply(len)

This will apply a function you’ve made to the column: *do not execute the function you apply it!

Def update_email(email): 
	Return email.upper()

Df[‘email’] = df[‘email].apply(update_email)

Here is an alternate using a lambda function:
Df[‘email’] = df[‘email’].apply(lambda x: x.lower())

This will give us the number of objects in each row: 
df.apply(len, axis=’columns’)

Get minimum values in each column: 

df.apply(pd.Series.min)

Alternative to the previous code with lambda: 
df.apply(lambda x: x.min())

Applymap

APply map can apply a function to every individual element in a dataframe. This only works on a df, not on a series. 

This gives us the length of every object in our df: 

df.applymap(len)

Make every value in the dataframe lowercase: 

df.applymap(str.lower)

Map
Change values in the series: * This will return NAN for all objects not changed in our map function
df[‘first’].map({‘cory’: ‘calvin’, ‘jane’: ‘jen’})

Replace
This will NOT change other objects when applied like map does

df[‘first’].replace({‘cory’: ‘calvin’, ‘jane’: ‘jen’})

Add and remove columns from the dataframe

This will add a new column of ‘full_name’: 

Df[‘full_name’] = Df[‘first’] + ‘ ‘ + df[‘last]

This will remove columns: 

df.drop(columns=[‘first’, ‘last’], inplace=True)

This will split full_name in to first and last name columns: 
Df[[‘first’, ‘last’]] = df[‘full_name’].str.split(‘ ‘, expand=True) 

Add a new object to the series with only the info from one column: 

df.append({‘first’: ‘Tony’}, ignore_index=True)* NAN will fill other column values


Add two dfs together with their indexes assigned properly: 

Df = df.append(df2, ignore_index=True)

Remove Row from df: 

df.drop(index=4, inplace=True)

Remove all rows with a conditional: 
df.drop(idex=df[df[‘last’] == ‘Doe’.index)
Or
Filt = df[‘last’] == ‘Doe’
df.drop(index=df[filt].index)

Sort data: 

df.sort_values(by=’last’, ascending=False)

Sort with changing ascending and decending values: 

df.sort_values(by=[‘last’, ‘first’], ascending=[False, True], inplace=True)

df.sort_value(by=[‘country’, coveted comp’], ascending=[True, False], inplace=True)

Get the values of a column: 

Get just the top 10 in the convert comp column
df[‘ConvertedComp’].nlargest(10)

Get the whole dataset for the lowest 10 in converted comp
df.nsmallest(10, ‘ConvetedComp’)

Get calculated values for all numerical values in the df

df.median()

Get a broad overview of the different stats that can be calcualted in your df
df.describe()

Get percentages of the whole for each possible value in a column: 

df[‘socialmedia’].value_counts(normalize=True)

Aggregation - group by: SPLIT, APPLY FUNCTION, COMBINE RESULTS

SPLIT

df.groupby([‘country’]) - this returns an object

Country_grp = df.groupby([‘country’])

country_grp.get_group(‘United States’) * this is similar to creating a filter

This gives us a series of all socia media counts for each country in the df with multiple indexes. 
country_grp[‘socialmedai’].value_counts()

Break that group down for an individual country
country_grp[‘socialmedai’].value_counts().loc[‘russian federation’]


Get multiple aggregate functions on our group: 

Coutry_grp[‘converted comp’].agg([‘meidan’, ‘mean’)]

For one country: 
Coutry_grp[‘converted comp’].agg([‘meidan’, ‘mean’)].loc[‘canada’]

APPLY FUNCTION

Find who uses python in each country: 

country_group[‘langwokredwith’].apply(lambda x: x.str.contains(‘Python’).sum())

Get percentage of developers who know python in each country: 

Grab number of respondents from each country: 
Country_respondants = df[‘country’].value_counts()
Country_uses_python = country_group[‘langwokredwith’].apply(lambda x: x.str.contains(‘Python’).sum())

Python_df = pd.concat([country_respondants, country_uses_python], axis=’columsn’, sort=False)

python_df.rename(columsn={‘country’: ‘numrespondnets’, ‘langworkedwith’: numknowspython}, inplace=True)

pythong_df[‘pct_knows_python’]  = python_df[‘numknowspython’] / python_df[‘numknowspython’] * 100

python_df.sort_values(by=’pct_kows_python’, ascending=False, inplace=True) 

Dropping data: 

This drops any rows with any NaN values
df.dropna(axis=’index’, how=’any’)

This drops any rows with all NA values
df.dropna(axis=’index’, how=’all’)

This drops any columns with all NA values
df.dropna(axis=’columns’, how=’all’)

-droping rows missing values in a specific column or columns

df.dropna(axis=’index’, how=’any’, subset=[‘email’, ‘last’])

Replace values from the onset with NAN

df.replace(‘NA’, np.nan, inplace=True)

Find what is na: 

df.isna(na)

Replace NA value: 

df.fillna(‘missing’)

Calculating numeral data with NANs in the set. 

Df[‘age’] = df[‘age’].astype(float)

Now df[‘age’].mean() will work

Wtih CSV, replace NA values on the import: 

Na_vals = [‘NA’, ‘MISSING’]
Df = pd.read_csv…...

Get all unique values in a column: 

df[‘yearscode’].unique()


Replace values in a column: 

df[‘yearscode’].replace(‘less than one year’, 0, inplace=True)

Correct dats in teh df on the import: 

D_parser = lambda x: pd.datetime.strptime(x, ‘%Y-%m-%d)
Df = pd.read_csv(‘ETH.csv’, parse_dates=[‘Date’], date_parser = d_parser)

Get the mean of a set of dates on close for price: 

Df[‘2020-1-1’: ‘2019-1-1’][‘Close’].mean()

Get the numerical calculations for all dates resampled by week: 

df.resample(‘W’).agg({‘Close’: ‘mean’, ‘High’: ‘median’})

Write to excel: 

india_df.to_excel(‘newexcel.xlsx’)

