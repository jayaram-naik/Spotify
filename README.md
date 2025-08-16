# Spotify Advanced SQL Project
Project Category: Advanced
[Click Here to get Dataset](https://www.kaggle.com/datasets/sanjanchaudhari/spotify-dataset)

![Spotify Logo](https://github.com/najirh/najirh-Spotify-Data-Analysis-using-SQL/blob/main/spotify_logo.jpg)

## Overview
This project focuses on analyzing a Spotify dataset containing various attributes of tracks, albums, and artists using SQL. It follows an end-to-end approach, including data preparation and multi-level exploratory analysis (easy, medium, and advanced queries). The goal is to strengthen SQL skills while uncovering meaningful insights from the dataset.

```sql
-- create table
DROP TABLE IF EXISTS spotify;
CREATE TABLE spotify (
    artist VARCHAR(255),
    track VARCHAR(255),
    album VARCHAR(255),
    album_type VARCHAR(50),
    danceability FLOAT,
    energy FLOAT,
    loudness FLOAT,
    speechiness FLOAT,
    acousticness FLOAT,
    instrumentalness FLOAT,
    liveness FLOAT,
    valence FLOAT,
    tempo FLOAT,
    duration_min FLOAT,
    title VARCHAR(255),
    channel VARCHAR(255),
    views FLOAT,
    likes BIGINT,
    comments BIGINT,
    licensed BOOLEAN,
    official_video BOOLEAN,
    stream BIGINT,
    energy_liveness FLOAT,
    most_played_on VARCHAR(50)
);
```
## Project Steps

### 1. Data Exploration
Before diving into SQL, it’s important to understand the dataset thoroughly. The dataset contains attributes such as:
- `Artist`: The performer of the track.
- `Track`: The name of the song.
- `Album`: The album to which the track belongs.
- `Album_type`: The type of album (e.g., single or album).
- Various metrics such as `danceability`, `energy`, `loudness`, `tempo`, and more.


## 15 Practice Questions

### Easy Level
1. Retrieve the names of all tracks that have more than 1 billion streams.
```sql
select * from spotify
where stream >1000000000;
```
2. List all albums along with their respective artists.
```sql
select distinct album, artist from spotify;
```
3. Get the total number of comments for tracks where `licensed = TRUE`.
```sql
select sum(comments) as Total_number_of_comments_are from spotify 
where licensed=TRUE;  --497015695
```
4. Find all tracks that belong to the album type `single`.
```sql
select * from spotify
where album_type ilike 'single';
```
5. Count the total number of tracks by each artist.
```sql
select 
	artist, count(*) as total_songs
from spotify
group by artist
order by 2;
```

### Medium Level
1. Calculate the average danceability of tracks in each album.
```sql
select
	album,
	avg(danceability) as avg_danceability
from spotify
group by 1
order by 2 desc ;
```
2. Find the top 5 tracks with the highest energy values.
```sql
select 
	track,
	energy
from spotify
order by 2 desc
limit 5;
```
3. List all tracks along with their views and likes where `official_video = TRUE`.
```sql
select 
	track,
	sum(views) as total_views,
	sum(likes) as total_likes
from spotify
where official_video='true'
group by 1
order by 2 desc  ,3 desc
limit 5;
```
4. For each album, calculate the total views of all associated tracks.
```sql
select 
	album,
	track,
	sum(views) as total_views
from spotify
group by 1,2
order by 3 desc;
```
5. Retrieve the track names that have been streamed on Spotify more than YouTube.
```sql
select * from
(select 
	track,
	coalesce(sum(case when most_played_on='Youtube' then stream end),0) as streamed_on_Youtube,
	coalesce(sum(case when most_played_on='Spotify' then stream end),0) as streamed_on_Spotify
from spotify
group by 1
) as t1
	where 
	streamed_on_Spotify >  streamed_on_Youtube
	and
	streamed_on_Youtube <> 0
```

### Advanced Level
1. Find the top 3 most-viewed tracks for each artist using window functions.
```sql
select * from
(select 
	track,
	coalesce(sum(case when most_played_on='Youtube' then stream end),0) as streamed_on_Youtube,
	coalesce(sum(case when most_played_on='Spotify' then stream end),0) as streamed_on_Spotify
from spotify
group by 1
) as t1
	where 
	streamed_on_Spotify >  streamed_on_Youtube
	and
	streamed_on_Youtube <> 0
```
2. Write a query to find tracks where the liveness score is above the average.
```sql
select 
	track,
	artist,
	liveness
from spotify
where liveness > (select avg(liveness) from  spotify);
```
3. Use a `WITH` clause to calculate the difference between the highest and lowest energy values for tracks in each album.
```sql
with calculate
as
(select
	album,
	max(energy) as highest,
	min(energy) as lowest
from spotify
group by 1
)

select 
	album,
	highest-lowest as difference
from calculate
order  by 2 desc;
```


