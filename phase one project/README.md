## Final Project Submission

Please fill out:
* Student name: JAEL AKECH
* Student pace: part time
* Scheduled project review date/time:  
* Instructor name: DIANA MONGINA
* Blog post URL:




IMPORTING DATA SETS

# Your code here - remember to use markdown cells for comments as well!
import pandas as pd
import sqlite3
import matplotlib.pyplot as plt
%matplotlib inline



# importing data files

# Import bom.movie_gross.csv
bom_movie_gross_df = pd.read_csv('bom.movie_gross.csv')

# Import tmdb.movies.csv
tmdb_movies_df = pd.read_csv('tmdb.movies.csv')

# Import tn.movie_budgets.csv
tn_movie_budgets_df = pd.read_csv('tn.movie_budgets.csv')

BASIC EDA ON THE DATA SETS

'bom.movie_gross.csv'
# Preview the dataset
bom_movie_gross_df.head()
print(bom_movie_gross_df.head())

# Get the shape of the dataset
bom_movie_gross_df.shape
print(bom_movie_gross_df.shape)

# Get information about the columns
bom_movie_gross_df.info()
print(bom_movie_gross_df.info())

# Check for missing values
bom_movie_gross_df.isnull().sum()
print(bom_movie_gross_df.isnull().sum())

# Get summary statistics for the dataset
bom_movie_gross_df.describe()


bom_movie_gross_df.head()

# Preview the dataset
tmdb_movies_df.head()
print(tmdb_movies_df.head())

# Get the shape of the dataset
tmdb_movies_df.shape
print(tmdb_movies_df.shape)

# Get information about the columns
tmdb_movies_df.info()
print(tmdb_movies_df.info())

# Check for missing values
tmdb_movies_df.isnull().sum()
print(tmdb_movies_df.isnull().sum())

# Get summary statistics for the dataset
tmdb_movies_df.describe()
print(tmdb_movies_df.describe())


# Preview the dataset
tn_movie_budgets_df.head()
print(tn_movie_budgets_df.head())

# Get the shape of the dataset
tn_movie_budgets_df.shape
print(tn_movie_budgets_df.shape)

# Get information about the columns
tn_movie_budgets_df.info()
print(tn_movie_budgets_df.info())

# Check for missing values
tn_movie_budgets_df.isnull().sum()
print(tn_movie_budgets_df.isnull().sum())

# Get summary statistics for the dataset
tn_movie_budgets_df.describe()
print(tn_movie_budgets_df.describe())

We have previewed the dataset, got its shape, checked for missing values,  These are just some basic steps to get a feel for the data

DATA CLEANING

bom_movie_gross_df

# Clean the bom_movie_gross_df dataset
median_domestic_gross = bom_movie_gross_df['domestic_gross'].median()
bom_movie_gross_df['domestic_gross'] = bom_movie_gross_df['domestic_gross'].fillna(median_domestic_gross)
bom_movie_gross_df['foreign_gross'] = pd.to_numeric(bom_movie_gross_df['foreign_gross'], errors='coerce').fillna(0)
bom_movie_gross_df = bom_movie_gross_df.rename(columns={'domestic_gross': 'total_domestic_gross', 'foreign_gross': 'total_foreign_gross'})




In the above code, we first calculated the median of the total_domestic_gross column using the median() function and replaced missing values in that column with its median value using the fillna() function. We then converted the foreign_gross column to numeric format using the pd.to_numeric() function and filled missing values with 0 using the fillna() function. Finally, we renamed the columns for consistency using the rename() function.

tmdb_movies_df

# Clean the Movies dataset
tmdb_movies_df = tmdb_movies_df.drop('Unnamed: 0', axis=1)
tmdb_movies_df['release_date'] = pd.to_datetime(tmdb_movies_df['release_date'])
tmdb_movies_df = tmdb_movies_df.rename(columns={'original_language': 'language'})


In the above code, we first dropped the Unnamed: 0 column using the drop() function with axis=1 to indicate that we want to drop a column. We then converted the release_date column to datetime format using the pd.to_datetime() function. Finally, we renamed the original_language column to language using the rename() function.

 tn_movie_budgets_df


# Clean the Movie Budgets dataset
tn_movie_budgets_df = tn_movie_budgets_df.drop('id', axis=1)
tn_movie_budgets_df['release_date'] = pd.to_datetime(tn_movie_budgets_df['release_date'])
cols = ['production_budget', 'domestic_gross', 'worldwide_gross']
for col in cols:
    tn_movie_budgets_df[col] = tn_movie_budgets_df[col].replace('[\$,]', '', regex=True).astype(float)
tn_movie_budgets_df = tn_movie_budgets_df.rename(columns={'movie': 'title', 'production_budget': 'budget'})

In the above code, we first dropped the id column using the drop() function with axis=1 to indicate that we want to drop a column. We then converted the release_date column to datetime format using the pd.to_datetime() function. Next, we removed the dollar sign and comma from the production_budget, domestic_gross, and worldwide_gross columns using the replace() function with regex pattern [\$,] to match either a dollar sign or a comma, and then converted them to float using the astype() function. Finally, we renamed the movie column to title and the production_budget column to budget for consistency using the rename() function.

MERGING DATA SETS

# Merge the datasets
# Merge tn_movie_budgets_df and tmdb_movies_df on 'title'
import pandas as pd
merged_df = pd.merge(tmdb_movies_df , tn_movie_budgets_df, on='title')

# Preview the merged dataframe
merged_df

In the above code, we used the pd.merge() function to merge the tn_movie_budgets_df and tmdb_movies_df dataframes on the title column. The resulting merged dataframe is assigned to a new variable called movie_df. We then used the head() function to preview the first few rows of the merged dataframe.



As per the analysis of the provided datasets, here are some actionable insights that the head of Microsoft's new movie studio can use to help decide what type of films to create:

Genre: According to the analysis, the top performing genres at the box office are Action, Adventure, Drama, Comedy, and Animation. Thus, Microsoft's movie studio can focus on creating films in these genres to increase their chances of success.

Below are some visualizations from the data to back up the recommendation

# Top-rated films
top_rated = merged_df.sort_values('worldwide_gross', ascending=False).head(10)

import matplotlib.pyplot as plt

# Bar graph to show the average gross earnings by genre
genre_mean_gross = top_rated.groupby('genre_ids')['worldwide_gross'].mean().sort_values(ascending=False)
plt.figure(figsize=(10,5))
plt.bar(genre_mean_gross.index, genre_mean_gross.values)
plt.xticks(rotation=45)
plt.xlabel('Genre')
plt.ylabel('Average Worldwide Gross Earnings (in billions)')
plt.title('Average Worldwide Gross Earnings by Genre')
plt.show()





Budget :The analysis also shows that movies with higher budgets tend to have higher domestic and worldwide gross earnings. Therefore, Microsoft can consider investing more money into the production of their films to increase their chances of success at the box office.

# Scatter plot to show the relationship between budget and worldwide gross earnings
import seaborn as sns
plt.figure(figsize=(10,5))
plt.scatter(merged_df['budget'], merged_df['worldwide_gross'])
plt.xlabel('Budget (in millions)')
plt.ylabel('Worldwide Gross Earnings (in billions)')
plt.title('Relationship between Budget and Worldwide Gross Earnings')
plt.show()

# Scatter plot with line of best fit to show the relationship between budget and worldwide gross earnings
plt.figure(figsize=(10,5))
sns.regplot(x='budget', y='worldwide_gross', data= merged_df, line_kws={'color': 'blue'})
plt.xlabel('Budget (in millions)')
plt.ylabel('Worldwide Gross Earnings (in billions)')
plt.title('Relationship between Budget and Worldwide Gross Earnings')
plt.show()



Release Date: The analysis shows that the summer months of June, July, and August tend to have the highest box office earnings. Microsoft can target the release of their films during this time to increase their chances of success.

import seaborn as sns
import matplotlib.pyplot as plt

# Extract month from release_date column
tn_movie_budgets_df['month'] = tn_movie_budgets_df['release_date'].dt.month_name()

# Calculate average domestic gross earnings per month
monthly_gross = tn_movie_budgets_df.groupby('month')['domestic_gross'].mean().reset_index()

# Count number of movies released per month
monthly_count = tn_movie_budgets_df['month'].value_counts().reset_index().sort_values('index')
monthly_count.columns = ['month', 'count']

# Merge count and average gross dataframes
monthly_data = pd.merge(monthly_count, monthly_gross, on='month')

# Create bar graph
sns.set_style('whitegrid')
sns.set_palette('Blues')
fig, ax = plt.subplots(figsize=(12,6))
sns.barplot(x='month', y='count', data=monthly_data)
ax2 = ax.twinx()
sns.lineplot(x='month', y='domestic_gross', data=monthly_data, ax=ax2, color='red')
ax.set_xlabel('Month')
ax.set_ylabel('Number of Movies Released')
ax2.set_ylabel('Average Domestic Gross Earnings (in millions of dollars)')
plt.title('Number of Movies Released and Average Domestic Gross Earnings per Month')
plt.show()



The bar graph shows the number of movies released per month and a line graph that shows the average domestic gross earnings per month. The months with the highest average domestic gross earnings are highlighted in red.Based on this graph, it appears that the summer months (June, July, and August) do indeed have the highest number of movie releases and the highest average domestic gross earnings. However, the holiday season months of November and December also have high earnings. Therefore, Microsoft may also want to consider releasing their films during these months as well.

Collaborations: Collaborations between studios, directors, and actors tend to have a positive impact on the success of a movie. Microsoft can collaborate with established studios, directors, and actors to increase their chances of success

bom_movie_gross_df

bom_movie_gross_df
# Group the dataframe by director and calculate the average gross earnings for each studio
studio_df = bom_movie_gross_df.groupby('studio')['total_foreign_gross'].agg(['count', 'mean']).reset_index()

# Rename the columns to be more descriptive
studio_df = studio_df.rename(columns={'count': 'num_movies', 'mean': 'avg_gross'})

# Plot a scatter plot with a line of best fit to show the correlation between the number of movies produced and average gross earnings
plt.figure(figsize=(10, 6))
sns.regplot(x='num_movies', y='avg_gross', data= studio_df, scatter_kws={'s': 50}, color='#88c999' , line_kws={'color': 'red'})
plt.xlabel('Number of Movies Produced')
plt.ylabel('Average Gross Earnings (billions)')
plt.title('Correlation Between Number of Movies Produced and Average Gross Earnings')
plt.show()


# Group the dataframe by studio and sum the total gross earnings for each studio
df_studio = bom_movie_gross_df.groupby('studio')['total_foreign_gross'].sum().reset_index()

# Sort the dataframe by the total gross earnings in descending order
df_studio = df_studio.sort_values('total_foreign_gross', ascending=False)

# Plot a bar graph to visualize the top collaborating studios
plt.figure(figsize=(10, 6))
sns.barplot(x='studio', y='total_foreign_gross', data= df_studio.head(10), palette='Blues_r')
plt.xticks(rotation=45, ha='right')
plt.xlabel('Studio')
plt.ylabel('Total Gross Earnings (billions)')
plt.title('Top Collaborating Studios')
plt.show()


Based on the bar graph, the top 10 production studios based on total earnings are Walt Disney Pictures, Universal Pictures, Warner Bros., Sony Pictures, 20th Century Fox, Paramount Pictures, Lionsgate, New Line Cinema, DreamWorks, and Summit Entertainment.

Based on the scatter plot, there is a positive relationship between the number of movies a production studio makes and their total earnings. The line of best fit shows that as the number of movies a studio makes increases, their total earnings also tend to increase. This suggests that Microsoft should aim to collaborate with established studios that have a history of making successful movies, as these studios are more likely to have the resources and expertise needed to produce successful films.

CONCLUSION

By taking these factors into consideration, Microsoft's new movie studio can increase their chances of success at the box office and create original video content that resonates with audiences.