# Post Mortem - Summary for Sentiment on social media (Twitter) goes negative due to issue with site

> * You have to write auto test for to prevent bugs on build and test stage.
> * Change release pipeline.
> * Check all changed before publish release on prod.

## Detect - Add twitter sentiment analysis widget to dashboard

We need to be aware of the sentiment of our web site / service / company and continuously monitor it. When we notice that the sentiment is trending negative then we should go and investigate our #hashtags / @accounts to find out what negative feedback we are receiving. This allows to be fast-reactive on negative feedback and can quickly find serious issues that customers are facing .

### Prerequisite

* Make sure you have provisioned the azure resources and deployed azure function, described at github.
* Deploy these resources into your teams resource group e.g. "EuropeDemo-EuropeDemo-Team02rsg" (this is listed on team details page).

### Steps

* Navigate to dashboards in your teams Azure DevOps project
* Create a new dashboard with or select an existing one
* Edit the dashboard and find "Twitter sentiment analysis widget", select Add.
* Configure the widget and add necessary keys
    * TwitterConsumerKey: find in your team information page
    * TwitterConsumerSecret: find in your team information page
    * HashTag: Your teams name (without #)
    * Azure function URL: Go to the Azure Portal, select your app service, select your function (TwitterSentimentFunction) and select get function URL, only copy the link, not the question mark and what comes after, it could look like this: https://prod-weu-teamstorage-fxapp.azurewebsites.net/api/TwitterSentimentFunction
    * Function key: Select Manage on the app service and copy Function Key
    * Cognitive Services access key:Go to Cognitive services in your resource group and "grab your keys"
    * Region, same as the location of your resource group

### What if twitter application hits rate limits or consumer keys don't work.

Follow [this guide](https://github.com/solidify/twittersentimentanalysisinazure/tree/master/twitter/create-twitter-appkeys.md) to create your own app keys in twitter. This can take 15 minutes.

### What if twitter sentiment stays at 0 on dashboard widget.

The function doing the scoring calculation is using twitter search api and seems to return fresh and active hashtag tweets. This is what twitter says "Please note that Twitter's search service and, by extension, the Search API is not meant to be an exhaustive source of Tweets. Not all Tweets will be indexed or made available via the search interface." So to mitigate, you could go and do a few tweets with your teams hashtag and the sentiment score will become active again.

## Validate challenge

If the widget shows something else than zero it works, red is negative sentiment, green is positive sentiment.

### Detect - Add twitter sentiment analysis release gate to release pipeline

We need to be aware of the sentiment of our web site / service / company and continuously monitor it. To be even more responsive to the feedback regarding the latest deployment to website we can integrate the twitter sentiment analysis with out release pipeline and let the pipeline post deployment monitor the sentiment. If sentiment remains positive, then we consider the deployment successful and if sentiment goes negative, we will see that the deployment is not complete - we can either wait and work on turning sentiment positive or consider the deployment a failure and notify team. Possibly even redeploy previous version.

### Prerequisite

* Make sure you have provisioned the azure resources and deployed azure function, described at github.
* Deploy these resources into your teams resource group e.g. "EuropeDemo-EuropeDemo-Team02rsg" (this is listed on team details page).

### Steps

* Go to your teams project in Azure DevOps
* Under pipelines select releases and edit your release pipeline
* In your selected environment select post-deployment conditions
* Enable gates and select Add, add Twitter Sentiment analysis
* Add the keys and links, they are the same ones as in the previous task: Add twitter sentiment analysis widget to dashboard
* Close the Pre-deployment conditions and save your release

### What if twitter application hits rate limits or consumer keys don't work.

Follow [this guide](https://github.com/solidify/twittersentimentanalysisinazure/tree/master/twitter/create-twitter-appkeys.md) to create your own app keys in twitter. This can take 15 minutes.

### What if twitter sentiment stays at 0 on dashboard widget.

The function doing the scoring calculation is using twitter search api and seems to return fresh and active hashtag tweets. This is what twitter says "Please note that Twitter's search service and, by extension, the Search API is not meant to be an exhaustive source of Tweets. Not all Tweets will be indexed or made available via the search interface." So to mitigate, you could go and do a few tweets with your teams hashtag and the sentiment score will become active again.

## Validate challenge

By default the time between re-evaluation of gates is 15 minutes, change it to two or three minutes. Create a couple of positive tweets about your team with #teamname hashtag and trigger a new release and see if it goes through

---

## Post Mortem step-by-step video

If you want to see an example of how to solve the challenge you watch the video.

https://www.youtube.com/watch?v=rYy4KKMFz9M

## Behind the scene

Are you interested in how we did this, then view the behind the scene video.

https://www.youtube.com/watch?v=NINNqQUB-WA