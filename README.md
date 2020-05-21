# Project: Data Warehouse
## Cloud-Data-Warehouse
 In this project, you'll apply what you've learned on data warehouses and AWS to build an ETL pipeline for a database hosted on Redshift. To complete the project, you will need to load data from S3 to staging tables on Redshift and execute SQL statements that create the analytics tables from these staging tables.
## Introduction
A music streaming startup, Sparkify, has grown their user base and song database and want to move their processes and data onto the cloud. Their data resides in S3, in a directory of JSON logs on user activity on the app, as well as a directory with JSON metadata on the songs in their app.

As their data engineer, you are tasked with building an ETL pipeline that extracts their data from S3, stages them in Redshift, and transforms data into a set of dimensional tables for their analytics team to continue finding insights in what songs their users are listening to. You'll be able to test your database and ETL pipeline by running queries given to you by the analytics team from Sparkify and compare your results with their expected results.

## Project Datasets
Here, I'll be working with two datasets that reside in S3. Here are the S3 links for each:

Song data: s3://udacity-dend/song_data
Log data: s3://udacity-dend/log_data
Log data json path: s3://udacity-dend/log_json_path.json

## Song Dataset
The first dataset is a subset of real data from the [Million Song Dataset](https://labrosa.ee.columbia.edu/millionsong/). Each file is in JSON format and contains metadata about a song and the artist of that song. The files are partitioned by the first three letters of each song's track ID. For example, here are filepaths to two files in this dataset.

song_data/A/B/C/TRABCEI128F424C983.json
song_data/A/A/B/TRAABJL12903CDCF1A.json

And below is an example of what a single song file, TRAABJL12903CDCF1A.json, looks like

'''{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}'''

## Log Dataset
The second dataset consists of log files in JSON format generated by this [event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above. These simulate app activity logs from an imaginary music streaming app based on configuration settings.

The log files in the dataset you'll be working with are partitioned by year and month. For example, here are filepaths to two files in this dataset.

log_data/2018/11/2018-11-12-events.json
log_data/2018/11/2018-11-13-events.json
And below is an example of what the data in a log file, 2018-11-12-events.json, looks like.
![](log-data.png)

## Schema for Song Play Analysis

Using the song and event datasets, you'll need to create a star schema optimized for queries on song play analysis. This includes the following tables.

### Fact Table
1. songplays - records in event data associated with song plays i.e. records with page NextSong
    * songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent
### Dimension Tables
2. users - users in the app
    * user_id, first_name, last_name, gender, level
3. songs - songs in music database
    * song_id, title, artist_id, year, duration
4. artists - artists in music database
    * artist_id, name, location, lattitude, longitude
5. time - timestamps of records in songplays broken down into specific units
    * start_time, hour, day, week, month, year, weekday

## Project Template
The project template includes four files:

* create_table.py is where I'll create my fact and dimension tables for the star schema in Redshift.
* etl.py is where I'll load data from S3 into staging tables on Redshift and then process that data into my analytics tables on Redshift.
* sql_queries.py is where I'll define you SQL statements, which will be imported into the two other files above.
* README.md is where I'll provide discussion on my process and decisions for this ETL pipeline.

## Data Pipeline
1. To run this project write down configuration, and save it as dwh.cfg in the project root folder.
2. Create a list of requirements for pythoon environment need for project infrastructure.
3. Run the *create_cluster* script to setup AWS Redshift service.

       `$ python create_cluster.py`

4. Run the *create_tables* script to setup the staging schema and analytical tables.

       `$ python create_tables.py`
       
5. Run the *etl* scrip to extract data from file stored in S3, stage it at Redshift and store the data in dimensional tables.

       `$ python etl.py`
       
6. Run the *analytics* script to check the count of data into dimensional table or try using AWS Redshift to check count upon run queries for analytical table.

       '$ python analytics.py'

  ## Queries and Results

Number of rows in each table:

| Table            | rows  |
|---               | --:   |
| staging_events   | 8056  |
| staging_songs    | 71    |
| dim_artist       | 69    |
| fact_songplay    | 1     |
| dim_song         | 14896 |
| dim_time         |  8023 |
| dim_user         |  105  |
