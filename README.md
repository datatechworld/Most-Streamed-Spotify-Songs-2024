# Most-Streamed-Spotify-Songs-2024
This dataset presents a comprehensive compilation of the most streamed songs on Spotify in 2024.
It provides extensive insights into each track's attributes, popularity, and presence on various music platforms, offering a valuable resource for music analysts, enthusiasts, and industry professionals. 
The dataset includes information such as track name, artist, release date, ISRC, streaming statistics, and presence on platforms like YouTube, TikTok, and more.


### Comprehensive Analysis Report and Code Breakdown

#### Introduction

This report aims to provide a detailed analysis of music streaming data from various platforms, focusing on identifying trends, comparing platform popularity, understanding artist impact, analyzing temporal trends, and investigating cross-platform performance. The analysis leverages data from platforms such as Spotify, YouTube, TikTok, Pandora, and SoundCloud.

#### Dataset Overview

The dataset contains various features for each song, including:
- Track Name
- Album Name
- Artist
- Release Date
- ISRC (International Standard Recording Code)
- All Time Rank
- Track Score
- Streaming and popularity metrics from multiple platforms such as Spotify, YouTube, and TikTok
- Other metrics such as AirPlay Spins, SiriusXM Spins, Shazam Counts, and TIDAL Popularity

#### Data Preparation

1. **Loading and Understanding the Data**:
   - We loaded the dataset to get an initial understanding of its structure and the type of data it contains.

2. **Cleaning Data**:
   - Non-numeric characters in numeric columns (like commas in stream counts) were removed to ensure accurate calculations.
   - Columns were converted to the appropriate data types for analysis.

#### Analysis

1. **Top 10 Most Streamed Songs**:
   - We identified and listed the top 10 most streamed songs based on Spotify Streams. This helps to recognize the most popular songs in the dataset.

2. **Descriptive Statistics**:
   - We calculated descriptive statistics such as mean, median, and standard deviation for key features to understand the distribution and central tendencies in the data.

3. **Trends Over the Years**:
   - By extracting the release year from the release date, we analyzed the total streams per year. This helps to identify any trends or changes in streaming patterns over time.
   - A line chart was used to visualize the total streams per year, showing how streaming activity has evolved.

4. **Distribution of Streams**:
   - We examined the distribution of streams using a histogram, which helps to understand how streams are spread across different songs.

5. **Most Popular Artists**:
   - We identified the top 10 artists with the most songs in the dataset, indicating which artists are the most prolific in terms of the number of tracks.
   - A bar chart was used to visualize the top 10 artists by the number of songs.

6. **Summary Statistics for Key Features**:
   - We provided summary statistics for key features such as Spotify Streams, YouTube Views, TikTok Views, Spotify Popularity, and Track Score. This gives an overview of the central tendencies and variability in these important metrics.

7. **Platform Comparison**:
   - We compared the total streams/views across different platforms (Spotify, YouTube, TikTok, Pandora, SoundCloud). This comparison highlights which platforms are driving the most traffic.
   - A bar chart was used to visualize the total streams/views by platform.

8. **Artist Impact**:
   - We analyzed the relationship between artist attributes (such as followers and debut year) and their streaming success. This helps to understand how an artist's popularity and experience impact their streaming numbers.
   - Scatter plots were used to visualize the relationships between artist followers vs. Spotify streams and debut year vs. Spotify streams.

9. **Cross-Platform Presence**:
   - We exploded the data to analyze each streaming service separately, providing a detailed view of performance across platforms.
   - A bar chart was used to show the total streams/views by streaming service, emphasizing the cross-platform presence of songs.

#### Conclusions

- **Top Songs and Artists**:
  - We identified the top 10 most streamed songs and the top 10 artists with the most songs, providing insights into current music popularity and prolific artists.

- **Temporal Trends**:
  - The analysis of streams over the years showed how music streaming has grown and evolved, reflecting industry trends and changing consumer behavior.

- **Platform Performance**:
  - Comparing different platforms revealed which ones contribute the most to total streams/views, guiding strategic decisions for marketing and content distribution.

- **Artist Influence**:
  - The relationship between artist attributes and streaming success highlighted the importance of an artist's fan base and career duration in driving streams.

- **Cross-Platform Presence**:
  - The analysis of performance across multiple platforms provided a holistic view of where songs are most successful, helping to optimize cross-platform strategies.

#### Recommendations

- **Focus on Top Platforms**:
  - Given the high traffic on platforms like Spotify and YouTube, focusing marketing efforts on these platforms can maximize reach and engagement.

- **Leverage Artist Popularity**:
  - Collaborating with popular artists with a large follower base and long career span can enhance the success of new releases.

- **Monitor Trends**:
  - Keeping an eye on temporal trends can help in planning releases to align with peak streaming periods.

- **Cross-Platform Strategies**:
  - Ensuring a strong presence across multiple streaming services can maximize a song’s reach and discoverability.



# Most Songs Streamed on Spotify 2024 - Code Breakdown


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



This breakdown should give you a clear understanding of what each part of the code does and how it contributes to the overall analysis of the dataset.



### Last words
I hope this comprehensive analysis provides valuable insights into the music streaming landscape, guiding strategic decisions to enhance business performance and maximize audience engagement.
