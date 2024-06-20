# data_warehouse_udacity
Building a Cloud Data Warehouse, Udacity Data Engineer Project 


# Sparkify Song Play Analysis

## Purpose
The primary purpose of this database is to enable Sparkify, a music streaming startup, to analyze the data they've been collecting on songs and user activity on their app. By understanding what songs users are listening to, Sparkify can optimize the user experience on their platform, make data-driven decisions regarding their music library, and better tailor their marketing strategies.

## Database Schema Design
The database utilizes a star schema optimized for queries on song play analysis. This schema includes one main fact table (`songplays`), which stores records of user song plays, and four dimension tables (`users`, `songs`, `artists`, and `time`) that store attributes related to the song plays.

### Fact Table
- `songplays` - Records log data associated with song plays. Includes songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent.

### Dimension Tables
- `users` - Information about users. Fields include user_id, first_name, last_name, gender, level.
- `songs` - Information about songs in the music database. Fields include song_id, title, artist_id, year, duration.
- `artists` - Information about artists. Fields include artist_id, name, location, latitude, longitude.
- `time` - Timestamps of records broken down into specific units. Fields include start_time, hour, day, week, month, year, weekday.

This schema design was chosen for its simplicity and effectiveness in handling queries related to song play analysis, as it allows for fast retrieval of data pertaining to user activity, song popularity, and user demographics.

## ETL Pipeline
The ETL pipeline extracts data from two main sources stored in S3: `song_data` and `log_data`. The data is first loaded into staging tables in Redshift (`staging_songs` and `staging_events`), then processed into the analytical tables.

1. **Extract**: Data is extracted from JSON files stored in S3.
2. **Transform**: During the transformation process, time data is extracted from timestamps, and data types are cast appropriately for SQL storage.
3. **Load**: Data is loaded into the fact and dimension tables using appropriate insert queries.

This ETL pipeline is fully automated and can handle large volumes of data efficiently.

## Example Queries and Results

### Most Played Song
```sql
SELECT s.title, COUNT(*) as play_count
FROM songplays sp
JOIN songs s ON sp.song_id = s.song_id
GROUP BY s.title
ORDER BY play_count DESC
LIMIT 1;
```
*Expected Result: This query returns the most played song in the database.*

### Highest Usage Time of Day
```sql
SELECT t.hour, COUNT(*) as plays
FROM songplays sp
JOIN time t ON sp.start_time = t.start_time
GROUP BY t.hour
ORDER BY plays DESC
LIMIT 1;
```
*Expected Result: This query identifies the hour of the day with the highest number of song plays, helping understand user listening habits.*

These queries offer valuable insights into user behavior and song popularity, which can aid business decisions at Sparkify.

## Conclusion
The structured database and ETL pipeline allow Sparkify to effectively analyze their data, helping to drive strategic business decisions and improve user engagement on their platform.


