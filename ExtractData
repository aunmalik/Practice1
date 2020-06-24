import pandas as pd
import tweepy
import time
from datetime import datetime

auth = tweepy.OAuthHandler('llYOKVcEvNz9ZnnH2bkoC5iHx', 'kEglPmUVZ7PWPHXTrgJ1uNGNHAatieyReBgmm9k36FTuPbB5tl')
auth.set_access_token('1264591037307260933-to1Gz4pcJJBzKWtlmQyvumkiKDrwHn',
                      '2IKKglfI4UWeXQvoVspdIj4v6z5AWSn7y2iru7fyPGsEb')
api = tweepy.API(auth)


def Get_Tweets (search_words, date_since, number_of_tweets, number_of_runs):
    # Pandas DataFrame to store the date:
    datab_tweets = pd.DataFrame(
        columns=['username', 'account_description', 'location', 'following', 'followers', 'total_tweets',
                 'user_creation_time',
                 'tweet_creation_time',
                 'retweet_count', 'text', 'hashtags'])

    program_start = time.time()
    for i in range(0, number_of_runs):
        initiate_run = time.time()

        # Using Cursor object to collect tweets
        # Cursor() returns an object and you can iterate to access the data collected

        tweets = tweepy.Cursor(api.search, q=search_words, lang="en", since=date_since, tweet_mode='extended').items(
            number_of_tweets)

        # to store tweets in Python list
        tweets_list = [tweet for tweet in tweets]
        number_of_tweets = 0

    for tweet in tweets_list:
        # Getting the values
        username = tweet.user.screen_name  # Username of account
        account_description = tweet.user.description   # Description of account
        location = tweet.user.location   # Location where tweeted
        following = tweet.user.friends_count   # Following list of user
        followers = tweet.user.followers_count   # Number of followers
        total_tweets = tweet.user.statuses_count   # Total tweets
        user_creation_time = tweet.user.created_at   # Tweeter user creation time
        tweet_creation_time = tweet.created_at  # Tweet creation time
        retweet_count = tweet.retweet_count   # Number of Retweets
        hashtags = tweet.entities['hashtags']   # Hashtags in tweet

        try:
            text = tweet.retweeted_status.full_text
        except AttributeError:  # Not a Retweet
            text = tweet.full_text
            # Add the 11 variables to the empty list - ith_tweet:
        ith_tweet = [username, account_description, location, following, followers, total_tweets,
                     user_creation_time, tweet_creation_time, retweet_count, text, hashtags]
        # Append to dataframe:
        datab_tweets.loc[len(datab_tweets)] = ith_tweet
        # Count number of tweets:
        number_of_tweets += 1

    # Run end time:
    end_runtime = time.time()
    duration_runtime = round((end_runtime - initiate_run) / 60, 2)

    print('number of extracted tweets for run {} : {}'.format(i + 1, number_of_tweets))
    print('time spent {} run to complete is {} minutes'.format(i + 1, duration_runtime))

    # When all the runs are completed save all of it to csv file:
    # Also add timestamp to files
    to_csv_timestamp = datetime.today().strftime('%Y%m%d_%H%M%S')

    # Define path and filename:
    path = r'E:'
    filename = path + 'aun.csv'
    # Store dataframe in csv with creation date timestamp
    datab_tweets.to_csv(filename, index=False)

    program_end = time.time()
    print('Extracting has completed successfully')
    print('Total time to extract is {} minutes.'.format(round(program_end - program_start) / 60, 2))


search_words = "#united or #MUFC"
date_since = "2018-05-05"
number_of_tweets = 2000
number_of_runs = 6

# Call the function Get_Tweets
Get_Tweets(search_words, date_since, number_of_tweets, number_of_runs)
