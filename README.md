# Airbnb-Sentiment-Analysis

This is the final project for CS 410 at UIUC. Created by Kashif Khan.

## Table of Contents

1. [ Purpose ](#purpose)
2. [ Installation ](#installation)
3. [ Run the Program ](#execution)
4. [ Examples of Runtime Arguments ](#examples)
5. [ Background ](#background)
6. [ Source of Data ](#datasource)

<a name="purpose"></a>
## Purpose

The purpose of this tool is to use the large amounts of Airbnb reviews provided as input to generate a **sentiment score** for each Airbnb listing. Then, all of the listings will be ranked in descending order of score with the most positively reviewed listings at the top and the most negatively reviewed listings at the bottom. Each listing will be assigned a sentiment score between [-1, 1]. Anything below 0 is classified as **negative** and anything above 0 is classified as **positive**.

Furthermore, the tool can then attempt to extract the most popular features (identified as nouns) mentioned in the reviews on a per-listing basis, as well as the adjectives that appear alongside them. These features and adjectives are extracted from the comments of each listing and then seperated based on their sentiment. This way, users can easily see the most frequent features referenced by reviewers in a **positive** sense, as well as the most frequent features referenced in a **negative** sense.

Lastly, this program can also generate bar graphs, wordclouds and csv files about the features of each listing that are either positive/negative:

1.  **Bar graphs** that highlight the frequency of features mentioned in a positive/negative sense.

![Example of Bargraph](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_bargraph.png)

2. **Wordclouds** that visually highlight the most frequently occurring words in a positive/negative sense.

![Example of Wordcloud](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_wordcloud.png)

3. **CSV files** that show the number of times each noun was mentioned as well as providing a list of adjectives that were associated with the extracted features.

![Example of Noun+Adjectives csv](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_csv.png)

<a name="installation"></a>
## Installation

Before running the program, we need to install the libraries required. To do so, simply run the following command in the main directory:

```pip install -r requirements.txt```

This will automatically install the libraries defined within the requirements file.

<a name="execution"></a>
## Run the Program

To run the program, the following commands will work:

  ```python analyzer.py source_file output_type listing_id_restrictions```
  
Bold indicates **mandatory** arguments while italic indicates _optional_ arguments.

1. **source_file**: The location of the input csv file. This must be a .csv file with **at least** two columns with exact names 'listing_id' and 'comments'.

2. **output_type**: Indicates what kind of output the user desires. Valid options are as follows:

   1. **'1':** listings_ranked.csv file
   
   2. **'2':** listings_ranked.csv file + bar graphs
   
   3. **'3':** listings_ranked.csv file + nouns & adjectives .csv files
   
   4. **'4':** listings_ranked.csv file + positive/negative feature word clouds
   
   5. **'5:'** All the above
   
3. **listing_id_restrictions** (Optional): Indicates which listing id's in particular are to be used. Added as an option since some input files could have up to hundreds of thousands of listings and generating graphs for all of them could quickly consume disk space. Valid options are as follows:
  
    1. **blank**: Leaving the third argument blank will mean that all listing ids in the file are to be used.
  
    2. **[x,y,z,...]**: Comma-separated listing ids inside square brackets. Only these specific listing ids will be used for analysis.
  
    3. **[x:y]**: A range of rows to use in the analysis from the input file.
      
        a. For example, [0:100] would only use the first 100 rows/reviews in the input file.
      
<a name="examples"></a>
### Examples of Runtime Arguments

```python analyzer.py data/reviews_chicago.csv 1 [25269, 37738, 46151]```

This will use **data/reviews_chicago.csv** as the input file and generate **only** the listings_ranked.csv file for the listings with ids 25269, 37738 and 46151.

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

The data used in this tool was retrieved from http://insideairbnb.com/get-the-data.html. 

The specific files that will work with this tool as is are the 'Detailed Review Listings for X city' files, as shown below. Any similar input file can be used in this program, as long as it contains the "listing_id" and "comments" columns.

![insideairbnb.com/get-the-data.html](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/insideairbnb_example.png)
