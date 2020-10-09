# WeRateDogs
I wrangled WeRateDogs Twitter data to create interesting and trustworthy analyses and visualizations. 

# Project background
This project is the final deliverable of the Wrangle and Analyze Data section for Udacity's Data Analyst Nanodegree. To start, I gathered data from multiple different data sources -- the WeRateDogs Twitter archive (csv file), tweet image predictions (hosted on Udacity's servers and was downloaded programmatically using the Requests library), each tweet's retweet count and like count (by querying the Twitter API for each tweet's JSON data using Python's Tweepy library). 

# My wrangling efforts for the WeRateDogs project consists of three major steps: gathering data, assessing data, and cleaning data. 

## Gathering data

The data used in this project were collected from the following sources:

* The WeRateDogs Twitter archive: dog name, dog “stage”, tweet text, as well as ratings, etc. I gathered this data by downloading the “twitter_archive_enhanced.csv” file and saved it as “archive_df”.
* tweet image predictions: predictions of dog breeds based on the tweet image. I pulled this data by downloading programmatically from Udacity servers using Requests and the provided URL and saved the data as “predictions_df”.
* JSON data: each tweet’s favorite and retweet count. I downloaded the data by querying the Twitter API for each tweet’s JSON data and stored it in tweet_json.text, which was then read as a pandas dataframe and stored into “tweet_json_data”.

## Assessing & Cleaning data

I have assessed the data visually and programmatically for quality and tidiness. Below, I have listed out 8 quality issues and 4 tidiness issues. I will address the “Key Points” listed in the project requirements. 

I created copies of the original files using copy() prior to cleaning just in case I need to restore some of the original contents. After that, I used functions (ie. info(), sample(), describe(), and value_counts()) to examine the data and identify potential issues. 

### Quality:
* archive_df table: Not all records are original tweets. There are 181 retweets and 78 replies but we only want original ratings. I dropped those rows containing retweets and replies.
* archive_df table: Not all records have images. I dropped the rows with missing expanded_url values from the archive table.
* archive_df table: Messy data types. I converted timestamp columns from string to datetime and tweet id columns from int64 to string.
* archive_df table: Some name values look abnormal (e.g. “a”, “such”). Dog names usually start with an uppercase letter, so I used a regex function to only look at names starting with a lowercase letter and replaced these names with “None” with loc.
* archive_df table: ratings seem inaccurate. Rating numerators could be 1776. Further, denominators should be 10 but we also find values like 170. I extracted ratings from tweet text to see if the numerators and denominators match "rating_numerator" and "rating_denominator". For mismatches, I manually corrected some values with the loc function by referencing the tweet text (more info in the ipynb doc). I also fixed some large numerators and denominators and dropped the denominator column.
* archive_df table: floofer has different ways of spelling in the tweet text. (e.g. "floofiest"), which were not picked up as "floofer". Same with the other three dog stages. I used regex to identify similar spelling and categorized them under the correct dog stages.
* archive_df table: Changed the long text strings of the source column to short abbreviations by using the def function.
* predictions_df table: some tweets' predictions do not belong to breeds of dogs. I dropped the rows with the three predictions being all false.

### Tidiness:
* archive_df table: the four dog stage columns represent one variable, which doesn’t follow the tidy data rule. I melted the four dog stages into one column -- “dog_stage_value_name”.
* tweet_json_data table: I used the merge function to merge the json data and the archive data together on the tweet id to have retweets+likes data in archive.
* archive_df table: I dropped replies and retweets columns from the archive table as they only contain NA now.
* predictions_df table: I merged the predictions data with the archive data on tweet_id. 

Finally, I stored the clean dataframes in a csv file.

