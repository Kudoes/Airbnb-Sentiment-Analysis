# Airbnb-Sentiment-Analysis

A tool that will analyze popular sentiments expressed in Airbnb user reviews.

The data used in this tool was retrieved from http://insideairbnb.com/. Any similar file can be used in this program, as long as it contains "listing_id" and "comments" columns.

Arguments:

1. The first argument must be the location of the .csv file containing the Airbnb review data.
2. The second argument must be an integer between 1 - 5, selected as follows (for more specific combinations the code can be modified):
   a. 1 = Only listings_ranked.csv file
   b. 2 = listings_ranked.csv file + positive/negative feature bar graphs
   c. 3 = listings_ranked.csv file + positive/negative features and their adjectives .csv files
   d. 4 = listings_ranked.csv file + positive/negative feature word clouds
   e. 5 = All of the above
3. The third argument can either be blank, or two integers in a list:
   a. blank = This will analyze all listing id's in the input file
   b. [x,y,...] = This will only analyze the listings as defined in this array (NOTE: No spaces between elements)
   c. [x:y] = This will only analyze the rows of the input file as defined by the range of x and y (i.e., '[0, 100]' would only analyze the first 100 rows/reviews, or [100:101] would only analyze the 100th row)
