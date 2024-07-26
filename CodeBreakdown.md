# Code Breakdown of Most Songs Streamed on Spotify 2024 Analysis


### Step 1: Load Needed Libraries and Dataset

1. **Import Libraries**:
    ```python
    import pandas as pd
    import numpy as np
    import matplotlib.pyplot as plt
    import seaborn as sns
    ```
    - Start by importing necessary libraries. `pandas` and `numpy` are used for data manipulation, `matplotlib` and `seaborn` for data visualization.


2. **Load the Dataset**:
    ```python
    df = pd.read_csv(r"C:\Users\HP\Downloads\Most Streamed Spotify Songs 2024\Most Streamed Spotify Songs 2024.csv", encoding='latin1')
    ```
    - Load our dataset from a CSV file into a `pandas` DataFrame.


### Step 2: Understanding the Data

1. **Let's Display the First Few Rows**:
    ```python
    df.head()
    ```
    - This shows the first 5 rows of the dataset, giving us an initial look at the data.

2. **Dataset Overview**:
    ```python
    print("Dataset Overview:")
    print(df.info())
    print("\nDataset Head:")
    print(df.head())
    ```
    - `df.info()` provides a summary of the dataset, including the number of entries, column names, and data types.
    - Now, we print the first few rows again to see the structure.



### Step 3: Data Cleaning

1. **Check and Clean 'Spotify Streams' Column**:
    ```python
    print(df['Spotify Streams'].head())
    df['Streams'] = df['Spotify Streams'].str.replace(',', '').astype(float)
    ```
    - Look at the 'Spotify Streams' column to see if there are any non-numeric characters (e.g., commas).
    - Remove these characters and convert the column to a numeric type for analysis.


2. **Convert Other Columns to Numeric**:
    ```python
    def convert_to_numeric(column):
        if column.dtype == 'object':
            column = column.str.replace(',', '', regex=False)
        return pd.to_numeric(column, errors='coerce')
    ```
    - Define a function to clean and convert other columns to numeric types as needed.


### Step 4: Data Analysis

1. **Top 10 Most Streamed Songs**:
    ```python
    top_10_songs = df.nlargest(10, 'Streams')
    print("\nTop 10 Most Streamed Songs:")
    print(top_10_songs[['Track', 'Artist', 'Streams']])
    ```
    - Identify the top 10 most streamed songs and display them along with their artists.


2. **Descriptive Statistics**:
    ```python
    print("\nDescriptive Statistics:")
    print(df.describe())
    ```
    - Generate summary statistics (e.g., mean, median, etc.) for all numeric columns in the dataset.


3. **Trends Over the Years**:
    ```python
    df['Release Date'] = pd.to_datetime(df['Release Date'])
    df['Year'] = df['Release Date'].dt.year
    streams_per_year = df.groupby('Year').agg({'Streams': 'sum'}).reset_index()
    print("\nStreams Per Year:")
    print(streams_per_year)
    ```
    - Convert the 'Release Date' column to a datetime type and extract the year.
    - Group the data by year and calculate the total streams per year.


4. **Visualizing Trends**:
    ```python
    plt.figure(figsize=(16, 6))
    sns.lineplot(data=streams_per_year, x='Year', y='Streams', marker='o')
    plt.title('Total Streams Per Year')
    plt.xlabel('Year')
    plt.ylabel('Streams (in billions)')
    plt.grid(True)
    plt.show()
    ```
    - Create a line plot to visualize the total number of streams each year.


5. **Distribution of Streams**:
    ```python
    plt.figure(figsize=(16, 6))
    sns.histplot(df['Streams'], bins=30, kde=True)
    plt.title('Distribution of Streams')
    plt.xlabel('Streams (in billions)')
    plt.ylabel('Frequency')
    plt.grid(True)
    plt.show()
    ```
    - Create a histogram to visualize the distribution of streams across songs.


6. **Top Artists by Number of Songs**:
    ```python
    top_artists = df['Artist'].value_counts().head(10)
    print("\nTop 10 Artists By Number of Songs in the Dataset:")
    print(top_artists)
    plt.figure(figsize=(16, 6))
    sns.barplot(x=top_artists.values, y=top_artists.index)
    plt.title('Top 10 Artists By Number of Songs')
    plt.xlabel('Number of Songs')
    plt.ylabel('Artist')
    plt.grid(True)
    plt.show()
    ```
    - Identify and visualize the top 10 artists based on the number of songs in the dataset.


### Step 5: Advanced Analysis

1. **Summary Statistics for Key Features**:
    ```python
    key_features = ['Spotify Streams', 'YouTube Views', 'TikTok Views', 'Spotify Popularity', 'Track Score']
    feature_summary = df[key_features].describe()
    print(f"Key Features Summary:\n{feature_summary}")
    ```
    - Generate summary statistics for key features related to different platforms.


2. **Sample the Dataset**:
    ```python
    df_sample = df.sample(n=1000, random_state=42)
    ```
    - Take a random sample of 1000 rows from the dataset for more manageable analysis and visualization.


3. **Pairplot for Relationships Between Key Features**:
    ```python
    sns.pairplot(df_sample, vars=key_features)
    plt.show()
    ```
    - Create a pairplot to visualize relationships between key features.


4. **Platform-wise Streams/Views**:
    ```python
    platform_features = ['Spotify Streams', 'YouTube Views', 'TikTok Views', 'Pandora Streams', 'Soundcloud Streams']
    for feature in platform_features:
        df[feature] = pd.to_numeric(df[feature].str.replace(',', '', regex=False), errors='coerce')
    df.dropna(subset=platform_features, inplace=True)
    platform_totals = df[platform_features].sum().reset_index()
    platform_totals.columns = ['Platform', 'Total Streams/Views']
    plt.figure(figsize=(16, 6))
    sns.barplot(data=platform_totals, x='Platform', y='Total Streams/Views')
    plt.title('Total Streams/Views by Platform')
    plt.xlabel('Platform')
    plt.ylabel('Total Streams/Views')
    plt.show()
    ```
    - Clean and convert platform-specific columns to numeric types, drop rows with missing values, and visualize total streams/views by platform.



### Step 6: Artist Impact

1. **Scatter Plots for Artist Attributes**:
    ```python
    if 'Artist Followers' in df.columns:
        plt.figure(figsize=(10, 6))
        sns.scatterplot(data=df, x='Artist Followers', y='Spotify Streams')
        plt.title('Artist Followers vs. Spotify Streams')
        plt.xlabel('Artist Followers')
        plt.ylabel('Spotify Streams')
        plt.show()
    if 'Debut Year' in df.columns:
        plt.figure(figsize=(10, 6))
        sns.scatterplot(data=df, x='Debut Year', y='Spotify Streams')
        plt.title('Debut Year vs. Spotify Streams')
        plt.xlabel('Debut Year')
        plt.ylabel('Spotify Streams')
        plt.show()
    ```
    - Create scatter plots to explore the relationship between artist attributes (followers and debut year) and their song streams.


### Step 7: Cross-Platform Presence

1. **Explode Data for Streaming Services**:
    ```python
    streaming_services = ['Spotify Streams', 'YouTube Views', 'TikTok Views', 'Pandora Streams', 'Soundcloud Streams']
    exploded_data = df.melt(id_vars=['Track'], value_vars=streaming_services, var_name='Streaming Service', value_name='Streams/Views')
    streams_by_service = exploded_data.groupby('Streaming Service')['Streams/Views'].sum().reset_index()
    plt.figure(figsize=(16, 6))
    sns.barplot(data=streams_by_service, x='Streaming Service', y='Streams/Views')
    plt.title('Total Streams/Views by Streaming Service')
    plt.xlabel('Streaming Service')
    plt.ylabel('Total Streams/Views')
    plt.show()
    ```
    - Transform the data to analyze the total streams/views across different streaming services and visualize the results.

### Last Words
I hope this code breakdown should give you a clear understanding of what each part of the code does and how it contributes to the overall analysis of the dataset.
