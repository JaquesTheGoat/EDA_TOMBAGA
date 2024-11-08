# <h1 align="center"> <a href="https://open.spotify.com/user/exll9wa5yql2llqyi1k5h56qm?si=YkkYuaD7SN60DMjXWo7eTQ&utm_source=copy-link" target="_blank"> <img src="https://github.com/mrankitgupta/Spotify-Data-Analysis-using-Python/blob/main/images/social-spotify.svg" alt="Spotify" width="55" height="40"/> </a> Exploratory Data Analysis: Spotify Top Songs 2023🎶



### About the Project

Spotify is a Swedish audio streaming and media services provider founded in April 2006. It is the world's largest music streaming service provider and has over 381 million monthly active users, which also includes 172 million paid subscribers.

<p align="center">
<img src="https://github.com/user-attachments/assets/ebc3f8a8-09b6-4cd9-9fe7-f2472afca0ab" width="500" alt="GIF Description">
</p>



This project performs an in-depth Exploratory Data Analysis (EDA) on the *Most Streamed Spotify Songs 2023* dataset, aiming to extract meaningful insights into popular music trends, artist performance, and platform preferences. This repository was created as part of an incentive-based assignment to showcase data analysis skills using Python.

---

## 📊 Project Overview

With streaming platforms shaping the music industry, understanding the characteristics of popular songs has become essential for artists, producers, and record labels. This analysis aims to provide insights into:
- **Musical Trends**: Analyzing factors that drive popularity, such as tempo, danceability, and energy.
- **Temporal Patterns**: Understanding the frequency of releases and popularity trends over time.
- **Platform Insights**: Investigating the relationship between playlist placements and track success.

Dataset source: [Most Streamed Spotify Songs 2023](https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)

---


## Importing Datasets

```python

import pandas as pd # Use for Data exploration and analysis
import numpy as np  # Performing mathematical and logical operations on arrays
import matplotlib.pyplot as plt # Creating line plots, scatter plots, bar charts, histograms, and more
import seaborn as sns # Visualizing distributions and relationships in data
import matplotlib.cm as cm  # Color mapping for generating color gradients and color schemes in plots
```

```python
data = pd.read_csv('Data_Source.csv', encoding='ISO-8859-1')  # Reading the CSV file with the specified encoding
data
```

##### `ISO-8859-1` (Latin-1) is a character encoding used for Western European languages.Specifying it can prevent UnicodeDecodeError when reading files with special characters (like accents in European languages) that aren't supported by the default encoding (UTF-8).

---

<img width="763" alt="Screen Shot 2024-11-07 at 9 53 47 PM" src="https://github.com/user-attachments/assets/8873d076-9c8c-40df-b8fa-9d3a1be74281">

#### The dataset contains 953 rows and 24 columns.

---

## Overview of Dataset
#### Questions to Ponder:

- What are the data types of each column?
- Are there any missing values?

```python
# Counts the number of missing values in each column 
# Checking null values

pd.isnull(data).sum()
```

<img width="246" alt="Screen Shot 2024-11-07 at 10 31 51 PM" src="https://github.com/user-attachments/assets/cdc110cd-cdda-4688-9bda-b658c9ddfa8a">
<img width="252" alt="Screen Shot 2024-11-07 at 10 32 09 PM" src="https://github.com/user-attachments/assets/86e8523b-b5d3-403b-867d-c801ca5c1b40">
<img width="251" alt="Screen Shot 2024-11-07 at 10 32 21 PM" src="https://github.com/user-attachments/assets/0baf2a5a-5494-4a0e-8f8c-4cc0ef8849fe">

#### Missing values in the dataset:
##### - 'in_deezer_playlists' has 79 missing values.
##### - 'in_shazam_charts' has 50 missing values.
##### - 'key' has 95 missing values.**
##### Addressing these missing values is recommended before proceeding with analysis,as they could affect the accuracy of insights.</b>



```python
# Data types of each column

data.info()
```

<img width="444" alt="Screen Shot 2024-11-07 at 10 35 06 PM" src="https://github.com/user-attachments/assets/fa505cc8-1705-42a9-85df-877ec10b2a64">
<img width="442" alt="Screen Shot 2024-11-07 at 10 40 19 PM" src="https://github.com/user-attachments/assets/e5981d41-945e-4144-8387-25b36c247129">
<img width="435" alt="Screen Shot 2024-11-07 at 10 40 46 PM" src="https://github.com/user-attachments/assets/26163061-ab58-4a56-b63e-d9cb64d40f4c">

#### Columns with data stored as 'object' type:
##### - 'track_name'
##### - 'artist(s)_name'
##### - 'streams'
##### - 'in_deezer_playlists'
##### - 'in_shazam_charts'
##### - 'key'
##### - 'mode'

---
## Basic Descriptive Statistic
#### Questions to Ponder:

- What are the mean, median, and standard deviation of the streams column?
- What is the distribution of released_year and artist_count? Are there any noticeable trends or outliers?
  
```python
data.describe()
```
<img width="667" alt="Screen Shot 2024-11-07 at 10 53 19 PM" src="https://github.com/user-attachments/assets/3fca0174-b5f5-4097-887a-9254ffc63efd">

```python
# switching the rows and columns, resulting in a more readable format:

data.describe().transpose()
```
<img width="752" alt="Screen Shot 2024-11-07 at 10 54 49 PM" src="https://github.com/user-attachments/assets/a9c6bd07-7a42-46f3-8d64-4f72c23fe001">

#### Mean, median, and standard deviation of the streams
```python
# Calculate mean, median, and standard deviation of the 'streams' column
mean_streams = data['streams'].mean()
median_streams = data['streams'].median()
std_dev_streams = data['streams'].std()

# Display the results
print("Mean of Streams:", mean_streams)
print("Median of Streams:", median_streams)
print("Standard Deviation of Streams:", std_dev_streams)
```
<img width="448" alt="Screen Shot 2024-11-07 at 11 04 33 PM" src="https://github.com/user-attachments/assets/835b8a7c-3056-4edd-8cee-1501378fd4a5">

#### Distribution of released_year and artist_count
```python
# Create a new column for decade
data['decade'] = (data['released_year'] // 10) * 10

plt.figure(figsize=(14, 6))

# Boxplot for Artist Count by Decade
plt.subplot(1, 2, 1)
sns.boxplot(x=data['decade'], y=data['artist_count'], color=sns.color_palette("viridis", as_cmap=True)(0.7))
plt.title("Boxplot of Artist Count by Decade")
plt.xlabel("Decade")
plt.ylabel("Number of Artists")

# Adding the number of artists by decade as a bar plot
plt.subplot(1, 2, 2)
artist_counts = data['decade'].value_counts().sort_index()
sns.barplot(x=artist_counts.index, y=artist_counts.values, palette="viridis")
plt.title("Number of Artists by Decade")
plt.xlabel("Decade")
plt.ylabel("Number of Artists")

plt.tight_layout()
plt.show()
```
<img width="759" alt="Screen Shot 2024-11-07 at 11 05 52 PM" src="https://github.com/user-attachments/assets/8ad72ded-784f-4d70-867c-d8e96963da82">

### Analysis of Artist Count by Decade
#### Boxplot of Artist Count by Decade (Left Graph)
##### - The boxplot shows that tracks from earlier decades (1930s–1970s) typically feature 1–3 artists, with few outliers. In contrast, tracks from the 1990s–2020s have a wider range of artist involvement, indicating an increase in collaborations. The 2020s stand out with more outliers, highlighting a trend toward even more collaborative music.

#### Bar Plot of Number of Artists by Decade (Right Graph)
##### - The bar chart reveals an upward trend in the number of artists across decades, with a sharp increase in the 2020s. Earlier decades (1930s–1990s) show low artist counts, while the 2010s see a rise. The 2020s show a significant spike, likely due to the influence of streaming platforms and playlists, which have helped more artists gain visibility and reach broader audiences.

#### Summary Insights
##### - The data highlights a growing trend of artist collaborations and visibility, particularly in the 2020s, likely driven by the rise of digital streaming, which has increased opportunities for artists to be featured and recognized.

---

## Top Performers
#### Questions to ponder:
- Which track has the highest number of streams? Display the top 5 most streamed tracks.
- Who are the top 5 most frequent artists based on the number of tracks in the dataset?

#### Top 5 most streamed tracks
```python
top_10_songs = data[['track_name', 'artist(s)_name', 'streams']].sort_values(by='streams', ascending=False).head(5)

# Display the top 5 songs as a formatted table
print("Top 10 Songs by Streams:")
top_10_songs.reset_index(drop=True, inplace=True)  # Reset the index for a cleaner look
top_10_songs.style.set_table_styles(
    [{'selector': 'th', 'props': [('text-align', 'center')]},
     {'selector': 'td', 'props': [('text-align', 'center')]}]
)
```
<img width="635" alt="Screen Shot 2024-11-07 at 11 19 51 PM" src="https://github.com/user-attachments/assets/3b730651-667f-4a60-a1c2-cd814066d822">

#### Top 5 frequent artists
```python
# Separate the 'artist(s)_name' column into individual artists and place each in its own row
artists = data['artist(s)_name'].str.split(',').explode().str.strip()

# Show the 5 most frequent artists based on the number of tracks in the dataset
topartists = artists.value_counts().nlargest(5)

# Convert the Series to a DataFrame for a more organized table format
top_artists_df = pd.DataFrame(topartists).reset_index()
top_artists_df.columns = ['Artist', 'Track Count']

# Display the organized table
print("Top 5 Most Streamed Artists:")
print(top_artists_df)
```
<img width="328" alt="Screen Shot 2024-11-08 at 12 43 47 PM" src="https://github.com/user-attachments/assets/24130e28-221c-43f9-98a0-368417b73ce7">




---

## Temporal Trends
#### Questions to ponder:
- Does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?
- What are the trends in the number of tracks released over time, and how can we visualize the number of tracks released per year?"

```python
tracks_per_year = data['released_year'].value_counts().sort_index()

# Create a DataFrame for easier plotting
tracks_per_year_data = tracks_per_year.reset_index()
tracks_per_year_data.columns = ['released_year', 'track_count']

# Plotting the number of tracks released per year
plt.figure(figsize=(14, 6))
sns.barplot(x='released_year', y='track_count', data=tracks_per_year_data, palette='viridis')
plt.title("Number of Tracks Released Per Year")
plt.xlabel("Released Year")
plt.ylabel("Number of Tracks")
plt.xticks(rotation=45)
plt.grid(axis='y')

plt.show()
```
<img width="762" alt="Screen Shot 2024-11-07 at 11 37 40 PM" src="https://github.com/user-attachments/assets/c339f883-2d58-42d2-b46a-c007d6cb434a">

-  The chart shows that the number of tracks released per year has increased drastically in the last few years. The number of tracks released in 2023 is significantly higher than in any other year. This could be due to a variety of factors, such as the increasing popularity of music streaming services or the rise of independent artists.

```python
# Group by 'released_month' to count the number of tracks released per month
monthly_releases = data['released_month'].value_counts().sort_index()

# Plot the distribution of track releases per month
plt.figure(figsize=(10, 6))
sns.barplot(x=monthly_releases.index, y=monthly_releases.values, palette="viridis")
plt.xlabel("Month")
plt.ylabel("Number of Tracks Released")
plt.title("Number of Tracks Released per Month")
plt.xticks(range(12), ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'])
plt.show()

# Identify the month with the most releases
most_releases_month = monthly_releases.idxmax()
most_releases_count = monthly_releases.max()

# Convert month number to month name
month_names = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
most_releases_month_name = month_names[most_releases_month - 1]  # Subtract 1 since month is 1-based

print(f"The month with the most releases is {most_releases_month_name} with {most_releases_count} tracks.")

# Summary of pattern based on the descriptive statistics
mean_month = data['released_month'].mean()
median_month = data['released_month'].median()

# Convert mean and median months to month names
mean_month_name = month_names[int(round(mean_month)) - 1]
median_month_name = month_names[int(median_month) - 1]

print(f"On average, most tracks are released around {mean_month_name} (mean), with the median release month being {median_month_name}.")

```

<img width="677" alt="Screen Shot 2024-11-07 at 11 43 55 PM" src="https://github.com/user-attachments/assets/ce9bb39c-aaf6-490d-a43e-7bee5a162626">

- The graph shows the number of tracks released per month. January has the highest number of releases with 134 tracks released that month. The month with the lowest number of tracks released is August, with 46. Overall the months with the most releases are clustered around June with the average number of releases for the year being 76. The median month is June with 84 tracks released that month.

--- 

## Genre and Music Characteristics
#### Questions to ponder:
- Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?
- Is there a correlation between danceability_% and energy_%? How about valence_% and acousticness_%?

```python
columns_of_interest = ['streams', 'bpm', 'danceability_%', 'energy_%', 'valence_%', 'acousticness_%']
correlation_matrix = data[columns_of_interest].corr()

# Display the correlation matrix as a heatmap with viridis color palette and transparency
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='inferno', vmin=-1, vmax=1, fmt=".2f", linecolor='Black')
plt.title("Correlation between Streams and Musical Attributes")
plt.show()
```
<img width="713" alt="Screen Shot 2024-11-08 at 10 26 14 AM" src="https://github.com/user-attachments/assets/586e7b11-4fb7-4a8e-b0ec-25a511d46763">

### Correlation of Streams with Musical Attributes

```python
# Calculate correlations between 'streams' and the specified musical attributes
correlationstreams = data[['streams', 'bpm', 'danceability_%', 'energy_%']].corr()['streams'].drop('streams')

# Convert the Series to a DataFrame for a larger, more readable table format
correlation_df = pd.DataFrame(correlationstreams).reset_index()
correlation_df.columns = ['Musical Attribute', 'Correlation with Streams']

# Display the correlation table in a larger format
print("Correlation of Streams with Musical Attributes:\n")
print(correlation_df)
```
<img width="443" alt="Screen Shot 2024-11-08 at 10 33 50 AM" src="https://github.com/user-attachments/assets/c69df075-0e4d-4127-8f85-1920d6e0b2ff">

### Correlation between Selected Musical Attribute Pairs

```python
# Calculate the correlation between specific pairs of musical attributes
attribute_pairs = data[['danceability_%', 'energy_%', 'valence_%', 'acousticness_%']].corr()

# Extract specific pairs for clarity
danceability_energy_corr = attribute_pairs.loc['danceability_%', 'energy_%']
valence_acousticness_corr = attribute_pairs.loc['valence_%', 'acousticness_%']

# Create a DataFrame to display these correlations in a larger table format
correlation_df = pd.DataFrame({
    'Attribute Pair': ['Danceability vs Energy', 'Valence vs Acousticness'],
    'Correlation': [danceability_energy_corr, valence_acousticness_corr]
})

# Display the organized table of correlations
print("Correlation between Selected Musical Attribute Pairs:\n")
print(correlation_df)
```
<img width="496" alt="Screen Shot 2024-11-08 at 10 37 43 AM" src="https://github.com/user-attachments/assets/74c6d272-e9e3-465b-948c-c412dd617657">

#### Analysis on Correlation between Streams and Musical Attributes

##### Observed Correlation Values:

#### Streams and BPM:</b>
- The correlation coefficient is close to zero, indicating no significant linear relationship between the number of streams and BPM. This suggests that tempo (measured by BPM) does not meaningfully impact the popularity of a track.

#### Streams and Danceability:</b>
- There is a slightly negative correlation of -0.11 between streams and danceability_%, which implies a very weak inverse relationship. However, this relationship is minimal, indicating that a track's danceability score has little to no effect on the number of streams.

#### Streams and Energy:</b>
- The correlation coefficient between streams and energy_% is -0.03, suggesting an extremely weak negative correlation. This means that the energy level of a track also does not strongly influence the streaming numbers.

#### Danceability and Energy:</b>
- The correlation coefficient between danceability_% and energy_% is 0.20. This is a weak positive correlation, suggesting that as the danceability of a song increases, its energy level tends to slightly increase as well. However, this relationship is not very strong, indicating that danceability and energy are somewhat independent of each other.

#### Valence and Acousticness:</b>
- The correlation coefficient between valence_% and acousticness_% is -0.08. This is a very weak negative correlation, implying that there is almost no relationship between the valence (musical positivity) of a song and its acousticness. In this case, songs with high acousticness levels are not strongly associated with either high or low valence.

#### Summary:</b>

The analysis shows that musical attributes (BPM, Danceability, Energy, Valence, Acousticness) have minimal impact on streaming numbers, with low correlation values indicating they are weak predictors of a track's popularity. Other factors likely play a more influential role in streaming success.

#### Insights:</b>
While musical attributes contribute to the character of a track, they are not primary drivers of streaming success. Other influential factors may include:

- Marketing and promotion: The visibility and reach of a track, often driven by marketing efforts, can greatly impact streams.
- Artist popularity: Established artists with a loyal fan base are likely to attract more listeners, regardless of specific musical attributes.
- Social media trends: Viral trends and social media platforms can dramatically boost a track's streams, independent of its intrinsic qualities.


--- 


## Platform Popularity
#### Questions to ponder:
- How do the numbers of tracks in spotify_playlists, spotify_charts, and apple_playlists compare? Which platform seems to favor the most popular tracks?

```python
# Get the descriptive statistics for the specified columns
description = data[['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']].describe().transpose()

# Display the result
print(description)
```

<img width="657" alt="Screen Shot 2024-11-08 at 10 59 25 AM" src="https://github.com/user-attachments/assets/2f3a054a-7275-4cb0-9d7c-92a33b63e22e">


```python
# Assuming 'sub' is your DataFrame
# Get the descriptive statistics
description = data[['in_spotify_playlists', 'in_deezer_playlists', 'in_apple_playlists']].describe()

# Extract the mean values for each platform
average_tracks = {
    'Spotify': description.loc['mean', 'in_spotify_playlists'],
    'Deezer': description.loc['mean', 'in_deezer_playlists'],
    'Apple Music': description.loc['mean', 'in_apple_playlists']
}

# Convert to DataFrame for visualization
average_tracks_df = pd.DataFrame(list(average_tracks.items()), columns=['Platform', 'Average Tracks'])

# Define the color palette for each platform
platform_colors = {
    'Spotify': sns.color_palette("viridis", as_cmap=True)(0.7),
    'Deezer': 'purple',
    'Apple Music': 'red',
}

# Plotting
plt.figure(figsize=(10, 6))
sns.barplot(x='Platform', y='Average Tracks', data=average_tracks_df, palette=platform_colors)
plt.title('Average Number of Tracks in Playlists by Platform')
plt.ylabel('Average Number of Tracks')
plt.xlabel('Platform')
plt.show()
```
<img width="683" alt="Screen Shot 2024-11-08 at 10 59 55 AM" src="https://github.com/user-attachments/assets/f2741ccf-79c1-40ce-8e57-f44c31d2dc2a">

### Analysis of Average Playlist Size Across Streaming Platforms

Music streaming platforms like Spotify, Deezer, and Apple Music have become central to global music consumption, offering extensive catalogs and playlist curation features. This report analyzes the average playlist sizes on these platforms, uncovering key differences in user behavior and platform characteristics and exploring potential reasons for these variations.

#### Spotify</b>
- Spotify has the largest average playlist size, with 52,898 tracks per playlist. This suggests that Spotify emphasizes expansive music collections, likely due to its strong playlist ecosystem, including popular algorithmic playlists like "Discover Weekly" and extensive user-curated lists. Additionally, Spotify's large user base and frequent playlist updates may contribute to these higher track counts.

#### Deezer</b>
- Deezer playlists average 974 tracks, fewer than Spotify but more than Apple Music, suggesting a preference for moderately sized playlists. This balance may cater to users seeking variety without excess and could reflect Deezer's unique market positioning and user playlist preferences compared to Spotify.

#### Apple Music</b>
- Apple Music playlists have the smallest average size, with 672 tracks per playlist. This suggests a preference for concise, theme-focused playlists, possibly reflecting Apple’s emphasis on quality and curated experiences. The platform’s design and sharing features may also encourage users to create smaller, more specific playlists.

##### Insight</b>
These differences in playlist sizes reveal insights into each platform's approach to user engagement. Spotify users tend toward expansive, evolving playlists, while Apple Music users may prefer more curated, selective collections. Future research could examine how playlist sizes influence user satisfaction and engagement, as well as the factors behind these platform-specific tendencies.

---

## Advance Analysis
#### Questions to ponder:
- Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?
- Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.

### Patterns with Major and Minor

```python
# Set seaborn style for better aesthetics
sns.set(style="whitegrid")

# Assuming `data` is a DataFrame with 'key', 'mode', and 'streams' columns
# Calculate descriptive statistics for streams by mode
descriptive_stats = data.groupby('mode')['streams'].agg(['mean', 'median', 'std', 'size'])
print(descriptive_stats)

# Calculate tracks for key and mode
key_mode_counts = data.groupby(['key', 'mode']).size().reset_index(name='Count')

# Create a pivot table for the stacked bar chart
key_mode_pivot = key_mode_counts.pivot(index='key', columns='mode', values='Count').fillna(0)

# Create a stacked bar chart
plt.figure(figsize=(15, 8))
ax = key_mode_pivot.plot(kind='bar', stacked=False, color=cm.viridis([0.2, 0.7]), ax=plt.gca())

# Customize plot labels and title
plt.title('Key vs Mode Distribution')
plt.xlabel('Keys')
plt.ylabel('Track Count')
plt.legend(title='Mode')



# Show plot
plt.show()
```

<img width="856" alt="Screen Shot 2024-11-08 at 11 25 45 AM" src="https://github.com/user-attachments/assets/333d3636-e761-4207-b20c-8bfffbe78368">


- The bar chart shows that major keys, particularly `C#`, `G`, and `D`, are the most popular choices among the top Spotify songs of 2023, likely due to their bright, uplifting tones. Minor keys are used less frequently, with `C#`, `E`, and `G` being the most common among them, offering a more introspective or somber tone. Some keys, like `A` and `F`, have a balanced presence in both major and minor modes, making them versatile. Meanwhile, keys such as `D#` (major) and `A#` (minor) are rarely used. Overall, the trend favors major keys, reflecting a preference for upbeat sounds in mainstream music, while minor keys add occasional emotional depth.
  
<img width="491" alt="Screen Shot 2024-11-08 at 12 46 18 PM" src="https://github.com/user-attachments/assets/33cd598d-5edc-438c-9c78-44d354299984">


### Most frequently appearing artists in playlists or charts

```python
# Grouping by the correct artist name column
artistplaylist_charts = data.groupby('artist(s)_name')[  # Ensure this is the correct column name
    ['in_spotify_playlists', 'in_apple_playlists', 'in_spotify_charts', 'in_apple_charts']].sum().sort_values(by='in_spotify_playlists', ascending=False)

# Get top 5 artists with highest playlist and chart appearances
artists_playlist = artistplaylist_charts[['in_spotify_playlists', 'in_apple_playlists']].head(5)
artists_chart = artistplaylist_charts[['in_spotify_charts', 'in_apple_charts']].head(5)

# Bar charts to display side by side 
fig, axes = plt.subplots(1, 2, figsize=(15, 6))

# Use the 'viridis' colormap for colors
colors_playlist = cm.viridis([0.2, 0.5])  # Adjust the values for desired shades
colors_chart = cm.viridis([0.7, 0.9])  # Adjust the values for desired shades

# Graph for Apple and Spotify playlist appearances
artists_playlist.plot(kind='bar', color=colors_playlist, ax=axes[0])
axes[0].set_title('Top 5 Artists by Playlist Appearances')
axes[0].set_xlabel('Artist')
axes[0].set_ylabel('Playlist Appearances')
axes[0].tick_params(axis='x', rotation=0)

# Graph for Apple and Spotify chart appearances
artists_chart.plot(kind='bar', color=colors_chart, ax=axes[1])
axes[1].set_title('Top 5 Artists by Chart Appearances')
axes[1].set_xlabel('Artist')
axes[1].set_ylabel('Chart Appearances')
axes[1].tick_params(axis='x', rotation=0)

# Display the two graphs
plt.tight_layout()
plt.show()
```
<img width="854" alt="Screen Shot 2024-11-08 at 12 46 48 PM" src="https://github.com/user-attachments/assets/e416ce88-416f-444f-bbed-4c11a4816127">

### Playlist Appearance</b>
- In the playlist appearances chart, Spotify heavily features popular artists such as The Weeknd, Taylor Swift, Ed Sheeran, Harry Styles, and Eminem, with each artist appearing tens of thousands of times (up to around 140,000 for The Weeknd). In contrast, Apple playlists include these artists much less frequently, showing a minimal presence compared to Spotify. This indicates that Spotify’s strategy emphasizes extensive playlist promotion for top artists, likely to attract and retain mainstream listeners.

### Chart Appearance</b>
- In the chart appearances, Apple shows a stronger emphasis on specific artists, particularly Taylor Swift, who leads with about 1,750 chart appearances, followed by The Weeknd with over 1,250. On Spotify, these artists also appear on charts, but their numbers are generally lower than on Apple. Taylor Swift is still prominent on Spotify, though to a lesser extent. Ed Sheeran and Harry Styles have moderate chart appearances on Apple but are less represented on Spotify. This suggests that Apple charts prioritize certain artists, reflecting their popularity among Apple listeners.

---

## Conclusions and Reflections

### Challenges Encountered
- Throughout the analysis of the Most Streamed Spotify Songs 2023 dataset, several challenges surfaced. One notable issue was dealing with inconsistent data types across multiple columns. For instance, in_deezer_playlists and in_shazam_charts were stored as objects instead of integers, which required conversion to int64 for accurate analysis. Additionally, columns such as streams, in_deezer_playlists, in_shazam_charts, and key also needed to be converted to their appropriate data types to enable meaningful calculations and visualizations.

- Below, you can find the program code used for these data type conversions, which was essential for creating a properly structured DataFrame and ensuring the accuracy of subsequent analysis.

```python
# Converting 'streams', 'in_deezer_playlists', 'in_shazam_charts', and 'key' to appropriate data types
# would improve analysis and allow for more accurate statistical and visualization results.
# Use 'Int64' for nullable integers

data['streams'] = pd.to_numeric(data['streams'], errors='coerce').astype('Int64')
data['in_deezer_playlists'] = pd.to_numeric(data['in_deezer_playlists'], errors='coerce').astype('Int64')
```

### Importance of Data Filtering
- Data filtering proved to be a critical step in preparing the dataset. By removing irrelevant or problematic data points and standardizing column formats, I was able to create a cleaner, more manageable DataFrame. This preprocessing step significantly improved the efficiency of subsequent analysis by reducing noise and focusing on the most relevant data. As a result, the insights drawn were clearer and more aligned with the analysis objectives, providing a solid foundation for meaningful exploration.


### Key Insights 

The analysis revealed several interesting trends and patterns within the music industry. For instance:

- Top-Performing Tracks: The most streamed songs in the dataset highlighted certain tracks that resonated broadly with listeners, possibly due to their genre, artists, or unique musical characteristics.
- Artist Popularity: Certain artists, such as Taylor Swift and The Weeknd, stood out not only in playlist appearances but also in chart rankings across Spotify and Apple platforms, reflecting their strong fan bases and consistent appeal.
- Release Trends: Temporal analysis showed notable trends in music releases, such as an increase in the number of tracks released during particular months, suggesting seasonal influences on music production and promotion.

### Platform-Specific Patterns
An analysis of playlist and chart appearances across Spotify and Apple revealed distinct strategies used by each platform. Spotify showed a strong focus on playlist promotion, with top artists appearing tens of thousands of times. In contrast, Apple charts displayed a more concentrated popularity for certain artists like Taylor Swift, indicating that each platform has unique promotional methods and caters to slightly different listener demographics.


### Reflections and Learnings

This exploratory data analysis provided valuable insights into the factors influencing a song’s popularity, the role of platform-specific strategies, and the seasonal trends in the music industry. One major takeaway was the importance of thorough data preprocessing—by addressing issues early on, the analysis process became smoother, and the results were more reliable. This project underscored how essential clean, well-structured data is for generating accurate insights, especially when analyzing trends in complex and varied datasets.

---

### References:

https://rakhitbari.soumitrarakshit.com/product-category/digital-products/spotify/

--- 

## For any queries/doubt 🔗👇

### Francis Tombaga

[![Gmail](https://img.shields.io/badge/Gmail-D14836?logo=gmail&logoColor=white&style=flat-square)](mailto:francischristian.tombaga.eng@ust.edu.ph)
[![GitHub](https://img.shields.io/badge/GitHub-181717?logo=github&logoColor=white&style=flat-square)](https://github.com/JaquesTheGoat)
[![Facebook](https://img.shields.io/badge/Facebook-1877F2?logo=facebook&logoColor=white&style=flat-square)](https://www.facebook.com/FrancisTombaga)








 





































