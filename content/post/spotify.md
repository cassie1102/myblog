---
title: "Spotify"
date: 2020-10-06T18:08:52+08:00
draft: False
---

Spotify is one of the most popular music streaming and media services provider. This post uses the [Spotify songs](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-01-21/readme.md) dataset provided by tidytuesday and aims at building a 3NF normalized database using sqlite.

Let's start by looking at the dataset. Below is the columns of the dataframe.  

['track_id', 'track_name', 'track_artist', 'track_popularity', 'track_album_id', 'track_album_name', 'track_album_release_date', 'playlist_name', 'playlist_id', 'playlist_genre', 'playlist_subgenre', 'danceability', 'energy', 'key', 'loudness', 'mode', 'speechiness', 'acousticness', 'instrumentalness', 'liveness', 'valence', 'tempo', 'duration_ms']

We can see that there are partial and transitive dependencies in the dataframe. So we will go through the following normalization process.

### 1NF Normalization

There are three types of normalizations for general purposes. By 1NF normalization, every table should not have any duplication or dependencies that are not key or domain constraints. We can see that there are no composite entries in the dataframe, so we are already 1NF-normalization-good.

### 2NF Normalization

For 2NF normalization, we need to:  
- Break partial dependencies
    - Identify candidate Primary Key for each row
    - If there is a composite PK, see if other columns have partial dependencies


```{python}
# composite PK: track_id, track_album_id, playlist_id
track_df = spotify_songs[['track_id', 'track_name', 'track_artist', 'track_popularity',
                       'danceability', 'energy', 'key', 'loudness', 'mode', 'speechiness',
                       'acousticness', 'instrumentalness', 'liveness', 'valence', 'tempo', 'duration_ms']]
track_album_df = spotify_songs[['track_album_id', 'track_album_name', 'track_album_release_date']]
playlist_df = spotify_songs[['playlist_name', 'playlist_id', 'playlist_genre', 'playlist_subgenre']]
```

### 3NF Normalization

For 3NF normalization:  
- Remove transitive dependencies, which means everything should depend on PK directly.

```{Python}
track = track_df[['track_id', 'track_name', 'track_popularity', 'danceability', 'energy',
                  'key', 'loudness', 'mode', 'speechiness', 'acousticness', 'instrumentalness',
                  'liveness', 'valence', 'tempo', 'duration_ms']].drop_duplicates(subset=['track_id'])
album = track_album_df.drop_duplicates(subset=['track_album_id'])
track_album = spotify_songs[['track_id', 'track_album_id']].drop_duplicates()
playlist = playlist_df.drop_duplicates(subset=['playlist_id'])
track_playlist = spotify_songs[['track_id', 'playlist_id']].drop_duplicates()
artist = track_df[['track_id', 'track_artist']].drop_duplicates()
```
The normalized tables are described as follows:   

- `track`: track_id, name, popularity and track characteristics;  
- `album`: track_album_id, name and release_date;
- `artist`: track_id, track_artist;  
- `playlist`: playlist_id, name, genre and subgenre;  
- `track_album`: track_id and track_album_id;  
- `track_playlist`: track_id and playlist_id;  


### Creating database

After cleaning and transforming the dataset into several normalized tables, I built a database called 'spotify.db' to store them.

```{Python}
# Create database
con = sqlite3.connect('spotify.db')

# Create tables in the database
track.to_sql(name='track', con=con, index = False)
album.to_sql(name='album', con=con, index = False)
artist.to_sql(name='artist', con=con, index = False)
playlist.to_sql(name='playlist', con=con, index = False)
track_album.to_sql(name='track_album', con=con, index = False)
track_playlist.to_sql(name='track_playlist', con=con, index = False)
```
Next I am going to extract the names of playlists that contain instrumental tracks. Below is the sql command to generate the names. Note that I will classify those whose instrumentalness greater than 0.1 to be containing instrumentals.

```{Python}
## find the names of all playlists that contain instrumentals.
%%sql
SELECT COUNT(DISTINCT(track.track_id)) AS num_tracks, playlist.playlist_id, playlist.playlist_name
FROM track
INNER JOIN track_playlist ON track_playlist.track_id = track.track_id
INNER JOIN playlist ON track_playlist.playlist_id = playlist.playlist_id
WHERE track.instrumentalness > 0.1
GROUP BY playlist.playlist_id
ORDER BY num_tracks DESC
```
Please see below the result of the query (first 10 records).
![fig](/spotify_fig.png)

The complete script of this project can be found [here](https://github.com/cassie1102/mini-project/blob/master/spotify.ipynb). 
