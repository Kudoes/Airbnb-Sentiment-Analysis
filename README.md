# Airbnb-Sentiment-Analysis

This is the final project for CS 410 at UIUC. Created by Kashif Khan.

## Table of Contents

1. [ Purpose ](#purpose)
2. [ Installation ](#installation)
3. [ Examples of Runtime Arguments ](#examples)
4. [ Background ](#background)
5. [ Source of Data ](#datasource)

<a name="purpose"></a>
## Purpose

The purpose of this tool is to analyze large amounts of Airbnb reviews stored in a csv file and use them to generate a **sentiment score** for each listing indicating the overall sentiments expressed in all the reviews for that particular listing. 

Furthermore, it will then attempt to extract the most popular features (identified as nouns) expressed on a per-listing basis, as well as the adjectives that appear alongside it. These features and adjectives are seperated based on their sentiment. This way, users can easily see the most frequent features referenced in a positive sense, as well as the most frequent features referenced by reviewers in a negative sense.

For example, if all the reviews for a listing with an id of 1050 are provided in a csv file, this tool will generate a sentiment polarity score between -1 and 1. Anything below 0 is classified as **negative** and anything above 0 is classified as **positive**.

After classifying the listing as positive/negative, this program can also generate graphs and tables about the features of each listing that are either positive/negative:

1.  **Bar graphs** that highlight the frequency of features mentioned in a positive/negative sense.

![Example of Bargraph](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_bargraph.png)

2. **Wordclouds** that visually highlight the most frequently occurring words in a positive/negative sense.

![Example of Wordcloud](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_wordcloud.png)

3. **CSV files** that show the number of times each noun was mentioned as well as providing a list of adjectives that were associated with the extracted features.

![Example of Noun+Adjectives csv](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_csv.png)

<a name="installation"></a>
## Installation

(install instructions)

  ```python analyzer.py source_file output_type listing_id_restrictions```
  
Bold indicates **mandatory** arguments while italic indicates _optional_ arguments.

**source_file**: The location of the input csv file. This must be a .csv file with **at least** two columns with exact names 'listing_id' and 'comments'.

**output_type**: Indicates what kind of output the user desires. Valid options are as follows:

   1. listings_ranked.csv file
   
   2. listings_ranked.csv file + bar graphs
   
   3. listings_ranked.csv file + nouns & adjectives .csv files
   
   4. listings_ranked.csv file + positive/negative feature word clouds
   
   5. All of the above
   
_listing_id_restrictions_: Indicates which listing id's in particular are to be used. Added as an option since some input files have hundreds of thousands of listings and generating graphs for all of them could eat up disk space quickly. Valid options are as follows:
  1. **blank**: Leaving the third argument blank will mean that all listing ids in the file are to be used.
  
  2. **[x,y,z,...]**: Comma-separated listing ids inside square brackets. Only these specific listing ids will be used for analysis.
  
  3. **[x:y]**: A range of rows to use in the analysis from the input file.
      
      a. For example, [0:100] would only use the first 100 rows/reviews in the input file.
      
<a name="examples"></a>
### Examples of Runtime Arguments

```python analyzer.py data/reviews_chicago.csv 1 [25269, 37738, 46151]```

This will use **data/reviews_chicago.csv** as the input file and generate **only** the listings_ranked.csv file.

```python analyzer.py data/reviews_boston.csv 5```

This will use **data/reviews_boston.csv** as the input file and generate the listings_ranked.csv file, the bar graphs, the wordclouds and the adjectives csv files for **all** listings provided in the input file.

```python analyzer.py data/reviews_seattle.csv 2 [1000:2000]```
      
This will use **data/reviews_seattle.csv** as the input file and generate only the listings_ranked.csv file and the bar graphs for **only** the listings on rows 1000 - 2000 of the input file.
      
<a name="background"></a>
## Background

Not all aspects of a positive review are positive, and not all aspects of a negative review are negative. This is true especially for Airbnb reviews. For example, a review could say:

> I thoroughly enjoyed my time here. The location was amazing and the apartment was clean. The only small complaint I had was that the room was cold at night and we had to use multiple blankets. Otherwise, it was an excellent stay!

We can intuitively see that the above comment is generally positive. Thus, by classifying all the comments of a listing as either positive/negative (on a scale, not just binary classification as 1 or 0), we can get a good idea of how well-regarded a listing is by reviewers. 

Secondly, we can extract the **features** that are referred to as being positive or negative. My tool utilizes POS-tagging and noun phrase extraction to identify nouns and adjectives associated with that noun within each review. In general, we will consider nouns to be the features that we are extracting and the adjectives will provide us with **context** about that feature. Then, we will analyze the sentiments of each noun phrase to identify whether the noun is positive or negative and then classify it as so.

<a name="datasource"></a>
## Source of Data

The data used in this tool was retrieved from http://insideairbnb.com/. Any similar file can be used in this program, as long as it contains "listing_id" and "comments" columns.
