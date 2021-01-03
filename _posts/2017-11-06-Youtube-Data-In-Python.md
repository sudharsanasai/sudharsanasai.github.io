---
title: "Youtube Data in Python"
categories:
  - Layout
tags:
  - Data Collection
  - APIs
last_modified_at: 2018-01-31T14:28:50-05:00
---

I have been hunting for weeks for a specific kind of dataset. The dataset I care about. Even though there are thousands of public datasets out there, they all seem to be where I have very little or no stake in.

Even though working through Titanic or German Credit Scoring is a very good flex of your data crunching muscle, sometimes you feel like indulging yourself.

![left-aligned-image]({{ '/images/youtube-data-in-python/fifa.jpeg' | absolute_url }}){: .align-left}One such indulgence was with [FIFA18 players dataset](https://www.kaggle.com/thec03u5/fifa-18-demo-player-dataset) in Kaggle. Having played FIFA for 15 hours a day in madness, working with this dataset has been yummy so far. Having a dataset you know a lot about, will help you test all crazy hypotheses, you have ever dreamed off.

My next attempt in this series is to get some YouTube data into my playground. The objective of this below tutorial is to get the video statistics and show you around Google’s YouTube Data API.

## YouTube Data api V3

The YouTube Data api v3 gives us the access to YouTube videos, channels, search, captions, comments and playlists. From a python kernel you can call the Google’s API, store the data in a dataframe and then further analyze it.

For using Google’s API,

1. You have to register your project as Google Project at your [Google Developer](http://console.developers.google.com/) account

    ![center-aligned-image]({{ '/images/youtube-data-in-python/google-api-1.png' | absolute_url }}){: .align-center}

2. Goto Library -> Search for YouTube Data API v3 and enable it.

    ![center-aligned-image]({{ '/images/youtube-data-in-python/google-api-2.png' | absolute_url }}){: .align-center}

3. You will be given a Developer Key to authenticate your API calls.

    ![center-aligned-image]({{ '/images/youtube-data-in-python/google-api-3.png' | absolute_url }}){: .align-center}

4. Install Google API Python Client in your machine using the following command. Type into your command prompt.

    ```pip install --upgrade google-api-python-client```

   Google has given sample code to use their APIs. The guide explains all the type of api calls it supports.

5. The below code calls the API, stores the data I need in a dictionary.

    {% gist 590f38d0d67ccee80934ca7c551bfd2c %}

6. Save this script at your working directory and change the variable DEVELOPER KEY to the developer key you have received.

    Now Let’s Get some Imagine Dragons into picture

    ![center-aligned-image]({{ '/images/youtube-data-in-python/imagine-dragons.jpeg' | absolute_url }}){: .align-center}


7. Now, open your Jupyter Notebook and use the above function. The youtube_search function takes in a search query string as a parameter and returns a dictionary. You can store in Pandas DataFrame and start your analysis.

    {% gist ae49797f19f5ee2f3f95a20198df0653 %}

There is so much scope for analysis with the YouTube data especially with the comments data available as an API. I would continue this series with analysis on this data.

Please, comment below on some of the crazy hypotheses YOU have, which can be cleared with this YouTube Data.