# Project: Data Modeling with Cassandra

This project is part of Udacity Data Engineering Nanodegree and was therefore lead for educational purposes. Its main goals are :

- Creating a data model with Apache Cassandra with relevant tables that allow queries to be run efficiently.
- Building an ETL pipeline using Python that transfers data from a set of CSV files within a directory to create a streamlined CSV file and then insert the data into Apache Cassandra tables.

## Context (fictional)

A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analysis team is particularly interested in understanding what songs users are listening to. Currently, there is no easy way to query the data to generate the results, since the data reside in a directory of CSV files on user activity on the app.

## Data before modeling

The raw data consists of csv files located inside a directory. The files themselves are formatted as below :

| artist                     | auth      | firstName | gender | itemInSession | lastName |   length | level | location                                     | method | page     |      registration | sessionId | song                                             | status |                ts | userId |
| -------------------------- | --------- | --------- | ------ | ------------- | -------- | -------- | ----- | -------------------------------------------- | ------ | -------- | ----------------- | --------- | ------------------------------------------------ | ------ | ----------------- | ------ |
|                            | Logged In | Walter    | M      |             0 | Frye     |          | free  | San Francisco-Oakland-Hayward, CA            | GET    | Home     | 1 540 920 000 000 |        38 |                                                  |    200 | 1 541 110 000 000 |     39 |
|                            | Logged In | Kaylee    | F      |             0 | Summers  |          | free  | Phoenix-Mesa-Scottsdale, AZ                  | GET    | Home     | 1 540 340 000 000 |       139 |                                                  |    200 | 1 541 110 000 000 |      8 |
| Des'ree                    | Logged In | Kaylee    | F      |             1 | Summers  | 246,308… | free  | Phoenix-Mesa-Scottsdale, AZ                  | PUT    | NextSong | 1 540 340 000 000 |       139 | You Gotta Be                                     |    200 | 1 541 110 000 000 |      8 |
|                            | Logged In | Kaylee    | F      |             2 | Summers  |          | free  | Phoenix-Mesa-Scottsdale, AZ                  | GET    | Upgrade  | 1 540 340 000 000 |       139 |                                                  |    200 | 1 541 110 000 000 |      8 |
|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|...|

## Data Model

The analytics team wants to run the following 3 queries :

1. Retrieve `artist name`, `song title` and `song's length` in the music app history that was heard during *sessionId = 338*, and *itemInSession = 4*
2. Retrieve `artist name`, `song title` (sorted by itemInSession) and `user` (first and last name) for *userid = 10* and *sessionid = 182*
3. Retrieve every `user` (first and last name) in the music app history who listened to the song *'All Hands Against His Own'*

To achieve this, we're going to model our data this way : **1 query = 1 table**. We're doing this because there are no joints in NoSQL, denormalized data is necessary to ensure fast reads.

### Tables :

|**music_query1**||
|-|-|
|`session_id`|`INT`|
|`item_in_session`|`INT`|
|artist_name|TEXT|
|song_title|TEXT|
|song_length|FLOAT|

|**music_query2**||
|-|-|
|`user_id`|`INT`|
|`session_id`|`INT`|
|artist_name|TEXT|
|song_title|TEXT|
|user_firstname|TEXT|
|user_lastname|TEXT|

|**music_query3**||
|-|-|
|`song_title`|`TEXT`|
|`user_id`|`INT`|
|user_firstname|TEXT|
|user_lastname|TEXT|

`*primary keys`




