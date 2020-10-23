---
title: "Cassandra Data modeling"
excerpt_separator: "A introduction to Data Modeling in Cassandra."
last_modified_at: 2020-10-15T16:20:02-05:00
categories:
  - Data Modeling Cassandra (NOSQL)
tags:
  - nosql
  - data modeling
  - cassandra
header:
  image: "https://images.pexels.com/photos/772803/pexels-photo-772803.jpeg?auto=compress&cs=tinysrgb&dpr=2&h=750&w=1260"
  caption: "Photo credit: [**Pexel**](https://images.pexels.com/)"
author: tchiring
---


# Cassandra Data Modeling
---
Before we start with our modeling, we must first educate ourselves with at least the rudimentary knowledge of Cassandra’s Architecture. Don’t worry we’re not going to dive deep in the how’s and what’s, but just get the gist of what’s happening under the hood.

#### A Gist of Cassandra Architecture
As you must already know, Cassandra is based on a distributed system architecture. That’s it, that’s all we need to know about the architecture. But what happens in a distributed system architecture you may ask? Well, a distributed system architecture for example, in our case Cassandra DB can have multiple machines (nodes) running together to serve a task or request. One thing that makes Cassandra unique from other distributed systems is that it is peer to peer. That means each node is connected to all other nodes. Each node can perform all database operations and serve client requests. You must’ve guessed it by now, yes, it is **master-less**. While there are various components that nodes in a cluster use to communicate two main components are Seeds and Gossip. Again, we’re not going to go in detail with these two for now but it is highly recommended to do some prior research.

#### What is Data Modeling?
Data Modeling is the process of creating a data model for the data to be stored in a database whereas a Data Model is an abstract model that organizes data description, data semantics, and consistency constraints of data. The data models are used to represent the data and how it is stored in the database and to set the relationship between data items. But in Cassandra Data models are made completely different compared to other relational databases which brings us to the topic Cassandra vs Relational Databases. 
Relational databases prioritize space over response time, that means only one copy of data is stored, but many joins are used which ultimately may hinder the response time as table size increases and lead to slow performance. On the other hand, Cassandra prioritizes response time over space, that means a few duplicate copies of data are stored and a real-time return of a result set. With relational databases, DBA’s and designers usually store data in a normalized form to save disk space and maintain data integrity. In Cassandra **joins do not exist** and the same data may **redundantly** be stored in multiple tables to serve a specific query which is not only expected but also tends to be a feature of a good data model.
#### Understanding how data is stored in Cassandra?
A Cassandra cluster may have many nodes running and data is usually stored redundantly across them according to a configurable replication factor. Increasing the replication factor generally means that incase certain nodes go down, we will still be able to request and fetch data even though some nodes are unreachable. Tables in Cassandra are very similar to RDBMS tables. RDBMS have databases where as in Cassandra we call them **keyspaces**. Physical records in the table are spread across the cluster at a location determined by a **partition key**. So whenever a query is executed it first looks for the partitions and retrieves the data from matching partitions. In case, we do not provide a partition key in our query it will prompt us to use **allow filtering** which is a huge no-no. Allow filtering, scans the entire table and retrieves the records matching condition provided in the query which is very costly in terms of processing and time.

#### Let's Get Started
For better understanding we will look at an example, lets say in RDBMS we have a simple twitter database model with two tables that hold user and tweet information as follows:

![userinfotweeinfo]({{ site.url }}{{ site.baseurl }}/assets/images/Data-Modeling-Cassandra/userinfotweeinfo.png)


A few of the common queries that we would apply are:
1) Get all users
`select * from userinfo`

2) Get all tweets.
`select * from tweetsinfo`

3) Get date wise user tweet by country.
`select * from tweets join user on tweetsinfo.user_id = userinfo.id where
created_at > ‘2020-01-01’ and created_at < ‘2020-02-01’`

4) Get tweets that came from a specific country.
`select * from tweetsinfo where country = ‘<country_name>’`

5) Get date wise tweets based on matching characters in the text column.
`select * from tweets join user on tweetsinfo.user_id = userinfo.id where
created_at > ‘2020-01-01’ and created_at < ‘2020-02-01’ and text like
‘% matching_pattern %’`

6) Get tweets written in a specific langauge
`select * from tweetsinfo where lang = ‘<language_code>’`

The key to using and modeling cassandra data lies in the queries. We use a query first approach while modeling our tables. Now that we have a list of queries that we want to model our tables on, how do we do so in Cassandra? Let us see.

1) Starting with the **query no (6)** we use the lang column as the partition key and id as the clustering key to preserve uniqueness. We can also use this model to fetch all of our tweets simultaneously tackling **query no (2)** with the same model.Hence our model would look like:
```
CREATE TABLE twitterdb.tweets_by_lang (
lang text,
id bigint,
coordinates text,
country text,
created_at timestamp,
hashtags text,
in_reply_to_status_id bigint,
in_reply_to_user_id bigint,
reply_count bigint,
retweet_count bigint,
source text,
text text,
user_id bigint,
user_mentions_id text,
user_mentions_name text,
PRIMARY KEY (lang, id))
```
2) For **query no (1)** we can simply create a table with partition key as name and clustering key as id. By doing this, we will sort of future proof our model by being able to query users based on their names as well as fetch all users in the table.
```
CREATE TABLE twitterdb.userinfo (
id bigint,
name text,
description text,
followers_count bigint,
friends_count bigint,
location text,
profile_image_url_https text,
screen_name text,
PRIMARY KEY (name , id))
```

3) In **query no (3)** traditionally we would use join, but remember, joins are not supported by Cassandra so we need to handle this before insertion. The most efficient and convenient way would be to use spark to join say two dataframesuserinfo, tweet info and insert the joined table into the database. For this, our partition key could be country and clustering keys created_at and id.

```
CREATE TABLE twitterdb.tweets_user_by_country_date (
country text,
created_at timestamp,
id bigint,
coordinates text,
description text,
followers_count bigint,
friends_count bigint,
hashtags text,
in_reply_to_status_id bigint,
in_reply_to_user_id bigint,
lang text,
location text,
name text,
profile_image_url_https text,
reply_count bigint,
retweet_count bigint,
screen_name text,
source text,
text text,
user_id bigint,
user_mentions_id text,
user_mentions_name text,
PRIMARY KEY (country, created_at, id))
```
4) **Query no (4)** is an exercise you can do and guess what the model could be? Mainly what would the partition and clustering keys be? Answers are in the end.

5) In **query no (5)** we have a lot of things going on but it is quite similar to query no (3) except we’re using wildcards to query matching patterns in the column text. The best approach to model time series data is to fully utilize the data given and maybe add a few columns of our own. What this means is that for qno (5) we can add and use two new columns created_year and  created_month as partition keys and the columns created_at (date range), text (search matching characters) and id (to maintain uniqueness) for clustering keys. Hence finally our data model would look like:
```
CREATE TABLE twitterdb.tweets_user_by_date_client (
created_year text,
created_month text,
created_at timestamp,
id bigint,
coordinates text,
country text,
description text,
followers_count bigint,
friends_count bigint,
hashtags text,
in_reply_to_status_id bigint,
in_reply_to_user_id bigint,
lang text,
location text,
name text,
profile_image_url_https text,
reply_count bigint,
retweet_count bigint,
screen_name text,
source text,
text text,
user_id bigint,
user_mentions_id text,
user_mentions_name text,
PRIMARY KEY ((created_year, created_month), created_at, id)
) WITH CLUSTERING ORDER BY (created_at ASC, id ASC)
```
```
CREATE CUSTOM INDEX string_search_idx ON
twitterdb.tweets_user_by_date_client (text) USING
'org.apache.cassandra.index.sasi.SASIIndex' WITH OPTIONS =
{'tokenization_locale': 'en', 'tokenization_skip_stop_words': 'true',
'analyzer_class': 'org.apache.cassandra.index.sasi.analyzer.StandardAnalyzer',
'tokenization_normalize_lowercase': 'true', 'mode': 'CONTAINS', 'analyzed': 'true',
'tokenization_enable_stemming': 'true'};
```
(We create a custom index using SASI. It implements three types of indexes, PREFIX, CONTAINS, and SPARSE. It is suggested to look up these custom indexes for more information but for now we just need to know that it acts as wildcard in Cassandra. Note: It is still experimental and not suggested for production use.)

#### Query No. 5 Explanation
When we query tweets, users and matching patterns between certain dates, we’re making use of a **join**, **time series condition** as well as **wildcard condition**. In the early stages when we have limited data this might not seem as a problem but as our dataset increases and later if we join multiple tables our query time increases as it has to first join the tables and scan the entire joined table and return records matching the conditions. Following a query first based model removes the need to scan for unnecessary partitions. In this case, say we have data from 2016 to 2020 and want to query data from 01-Jan-2020 to 15-Jan-2020 and tweets with text that contain the word ‘Corona’, we would be avoiding scanning a large chunk of data with our current partition keys. According to our model, internally Cassandra first looks for the matching year then the matching month and finally the date range and matching pattern. Hence our query would not be bothered with what is not needed.

#### Points to Remember
Denormalization and data redundancy are characteristics of a good data model. At present storage is a lot cheaper than what it used to be, so it is better in a sense to have redundant data and add storage than to upgrade your servers. One question that might have struck your mind after all of this must be how on earth do we maintain **data integrity** with all these duplicate data. Well have no fear as **Apache Spark** is here, also not to mention Cassandra provides extremely fast writes depending on your cluster specifications and data models. So **inserting** to multiple tables through spark to maintain integrity should not be a problem. Lastly also do look at batch inserts in Cassandra for more knowledge on maintaining data integrity.

**Almost, forgot the answer to query no** (4) 
Our model for **query no (4)** would be:
The same as the model for **query no (3)**. Tadaa! We’re already using a table with a single partition key i.e country column hence we do not need to create another one and can use the same table for two queries now.