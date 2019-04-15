# Real Time Analytics with Azure Machine Learning Studio y Stream Analytics

In this session, you will going to use an experiment already created in the gallery for you, https://gallery.cortanaintelligence.com/Experiment/Classification-Twitter-sentiment-text-analysis to try to predict the sentiment of the twitts containing specific tags. Using Python, we will query the Twitter API. This twitts will then be sent to an Event hub account to be stored for further processing in the form of a queue. The stream of events will be consumed by a Stream analytics Job which will in turn will query an Azure ML Studio model published as a web service to predict the sentiment of the twitt. Finally, the average sentiment is posted in PowerBI in realtime.

## Requirements to complete this lab

<ul>
<li>An Azure subscription</li>
<li>A Twitter Developer Account. If you don't have one. Worry do not. We have a backup plan.You will need to grab the <a href="https://dev.twitter.com/oauth/overview/application-owner-access-tokens" data-linktype="external">OAuth access token</a> for accessing the Twitter API from you dev account.</li>
<li>PowerBI</li>
</ul>