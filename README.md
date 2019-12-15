# Airbnb-Sentiment-Analysis

This is the final project for CS 410 at UIUC. Created by Kashif Khan.

## Table of Contents

1. [ Background and Purpose ](#background)
2. [ Implementation ](#implementation)
3. [ Contents ](#contents) 
4. [ Installation ](#installation)
5. [ Running the Program ](#execution)
6. [ Examples of Runtime Arguments ](#examples)
7. [ Source of Data ](#datasource)

<a name="background"></a>
## Background and Purpose

When we look at customer reviews for products or services, we can notice two things:

1. There exists an _overall_ sentiment expressed by the reviewer.
2. There exist sentiments expressed towards certain features of the product/service that are independent of the overall sentiment of the entire comment.

Essentially, not all aspects of a positive review are positive, and not all aspects of a negative review are negative. This is also true for Airbnb reviews. For example, a review could say:

> I thoroughly enjoyed my time here. The location was amazing and the apartment was clean. The only small complaint I had was that the room was cold at night and we had to use multiple blankets. Otherwise, it was an excellent stay!

We can see that the above comment is generally positive. However, there is a negative sentiment expressed about the room being cold at night. Thus, by classifying all the comments of a listing as either positive/negative (on a scale, not just binary classification as 1 or 0), we can get a good idea of how well-regarded a listing is by reviewers through their comments. 

Next, we may want to extract the **features** that are referred to as being positive or negative for various analytical purposes either by customers or owners of the listing. 

In particular, sometimes users may want to see _specifically_ the negative aspects of an Airbnb listing that they are going to stay at. In general most comments on Airbnb are positive, so it is usually more interesting to inspect what the problems with a location are and whether they would be something critical for us as customers. By extracting features, we can easily see the problems that people had with multiple listings without having to manually go through every single comment to search for negative aspects.

This is the purpose for which I've developed this tool - to make it much easier for Airbnb customers to compare and contrast the multiple listings in order to determine the best fit for them.

<a name="purpose"></a>
## Implementation

This tool utilizes the large amounts of Airbnb reviews provided as input to generate a **sentiment score** for each Airbnb listing. Then, all of the listings will be ranked in descending order of score with the most positively reviewed listings at the top and the most negatively reviewed listings at the bottom of an output csv file. Each listing will be assigned a sentiment score between [-1, 1]. Anything below 0 is classified as **negative** and anything above 0 is classified as **positive**.

Furthermore, the tool will then extract the most popular features (identified as nouns) mentioned in the reviews on a per-listing basis, as well as the adjectives that appear alongside them. These features and adjectives are extracted from the comments of each listing and then seperated based on their sentiment. This way, users can easily see the most frequent features referenced by reviewers in a **positive** sense, as well as the most frequent features referenced in a **negative** sense.

### Data Cleaning

Before any sentiment scores are generated, we clean the data by removing null rows and reviews that are not in English (to the best of our ability) by utilizing the pandas and [langdetect](https://pypi.org/project/langdetect/) libraries.

### NLTK VADER Sentiment Tool
To generate the sentiment score for each review/noun phrase, the NLTK lexicon-based [VADER sentiment analysis tool](http://www.nltk.org/howto/sentiment.html) is used. The benefit of using this tool is that it has specifically been designed to identify sentiments expressed in social media. The features in particular that I found most applicable to Airbnb comments were that it accounts for common acronyms expressed in social media, as well as adjusting sentiment based on capitalization and punctuation (i.e., "BAD!!" would have a much lower polarity than "bad"). Due to the informal nature of Airbnb comments, they are usually phrased similarly to social media text and thus this module was perfect for my requirements.

### POS Tagging and Noun Chunk Sentiment Analysis
Once the overall review sentiment score is generated, we utilize the [NLTK](https://www.nltk.org/book/ch07.html) library once again to identify noun and adjective associations within the review. First, we tokenize the comment and use POS-tagging to tag each word in the review. Then, a regex is used as a rule to define a noun chunk as any sequence of words that satisfy that regex requirement. For example, this requirement could be: \<Adjective\>\<Noun\>. Once these noun chunks are identified, we once again use the VADER sentiment analyzer on each noun phrase to classify each noun as being referenced in a positive or negative sense. The adjectives extracted are crucial in this step as they provide most of the sentiment information. These adjectives are stored alongside each noun for reference.
  
These nouns are treated as the **features** of the listing that reviewers are discussing. 
  
### Data Generation
Finally, the data is accumulated and can then be visualized or exported as a csv file. For this, the matplotlib, seaborn and wordcloud libraries are used.

One output file is _always_ generated: listings_ranked.csv. This file contains all the listings specified at runtime and ranks them based on an overall sentiment score. The most well-reviewed listings will be at the top, and the most poorly-reviewed listings will be at the bottom. 

Furthermore, this program can generate optional bar graphs, wordclouds and csv files about the features of each listing that are either positive/negative:

1.  **Bar graphs** that highlight the frequency of features mentioned in a positive/negative sense.

![Example of Bargraph](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_bargraph.png)

2. **Wordclouds** that visually highlight the most frequently occurring words in a positive/negative sense.

![Example of Wordcloud](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_wordcloud.png)

3. **CSV files** that show the number of times each noun was mentioned as well as providing a list of adjectives that were associated with the extracted features.

![Example of Noun+Adjectives csv](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/example_csv.png)

The listings_ranked.csv file will be generated in the root directory, and the other files will all be placed in a /results subfolder.

<a name="contents"></a>
## Contents of Repository
This repository has a few components:

1. **analyzer.py**: This is the main python program that users will run through the command line.

2. **requirements.txt**: This is a requirements file for pip to easily download all required libraries.

3. **/misc/ folder**: This folder can be ignored as it holds img files for this README only.

4. **/data/ folder**: This folder is intended to hold the input files. A demo file has been provided reviews_chicago.csv.

5. **/results/ folder**: This folder will be generated upon program execution. All graphs and the noun+adjective.csv files will be placed here.

<a name="installation"></a>
## Installation

### System Requirements
The only requirements needed to run this program:

1. Windows / MacOS / Linux (Ubuntu) operating system
  
    a. This code was tested on all three of these operating systems
  
2. Python 3.6+: This program was developed on Python 3.7 and tested successfully on Python 3.6.

### Installation
To run this program, this repository must be cloned locally:

```
$ git clone https://github.com/Kudoes/Airbnb-Sentiment-Analysis.git
$ cd Airbnb-Sentiment-Analysis
```

Before running the program, we need to install the libraries required. To do so, simply run the following command in the main directory:

```$ pip install -r requirements.txt```

This will automatically install the libraries defined within the requirements file.

<a name="execution"></a>
## Running the Program

To run the program, the following commands will work:

  ```python analyzer.py source_file output_type listing_id_restrictions```

1. **source_file**: The location of the input file. This must be a .csv file with **at least** two columns with exact names 'listing_id' and 'comments'. A default test input is provided in /data/reviews_chicago.csv.

2. **output_type**: Indicates what kind of output the user desires. Valid options are as follows:

   1. **'1':** listings_ranked.csv file
   
   2. **'2':** listings_ranked.csv file + bar graphs
   
   3. **'3':** listings_ranked.csv file + nouns & adjectives .csv files
   
   4. **'4':** listings_ranked.csv file + positive/negative feature word clouds
   
   5. **'5:'** All the above
   
3. **listing_id_restrictions** (Optional): Indicates which listing id's in particular are to be used. Added as an option since some input files could have up to hundreds of thousands of listings and generating graphs for all of them could quickly consume disk space. Valid options are as follows:
  
    1. **blank**: Leaving the third argument blank will mean that all listing ids in the file are to be used.
  
    2. **[x,y,z,...]**: Comma-separated listing ids inside square brackets. Only these specific listing ids will be used for analysis. **Important: do not add spaces between listing ids!**
  
    3. **[x:y]**: A range of rows to use in the analysis from the input file.
      
        a. For example, [0:100] would only use the first 100 rows/reviews in the input file.
      
<a name="examples"></a>
### Examples of Runtime Arguments

```python analyzer.py data/reviews_chicago.csv 5 [25269,37738,46151]```

This will use **data/reviews_chicago.csv** as the input file to generate the listings_ranked.csv file, the bar graphs, the wordclouds and the noun+adjectives csv files for the listings with ids 25269, 37738 and 46151.

```python analyzer.py data/reviews_chicago.csv 1```

This will use **data/reviews_chicago.csv** as the input file to generate **only** the listings_ranked.csv file for **all** listings provided in the input file.

```python analyzer.py data/reviews_chicago.csv 2 [1000:2000]```
      
This will use **data/reviews_chicago.csv** as the input file to generate only the listings_ranked.csv file and the bar graphs for **only** the listings on rows 1000 - 2000 of the input file.

<a name="datasource"></a>
## Source of Data

The data used in this tool was retrieved from http://insideairbnb.com/get-the-data.html. 

The specific files that will work with this tool as is are the 'Detailed Review Listings for X city' files, as shown below. Any similar input file can be used in this program, as long as it contains the "listing_id" and "comments" columns.

![insideairbnb.com/get-the-data.html](https://github.com/Kudoes/Airbnb-Sentiment-Analysis/blob/master/misc/insideairbnb_example.png)
