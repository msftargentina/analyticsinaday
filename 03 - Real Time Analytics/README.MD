# Real Time Analytics with Azure Machine Learning Studio y Stream Analytics

In this session, you will going to use an experiment already created in the gallery for you, https://gallery.cortanaintelligence.com/Experiment/Classification-Twitter-sentiment-text-analysis to try to predict the sentiment of the twitts containing specific tags. Using Python, we will query the Twitter API. This twitts will then be sent to an Event hub account to be stored for further processing in the form of a queue. The stream of events will be consumed by a Stream analytics Job which will in turn will query an Azure ML Studio model published as a web service to predict the sentiment of the twitt. Finally, the average sentiment is posted in PowerBI in realtime.

## Requirements to complete this lab

<ul>
<li>An Azure subscription</li>
<li>A Twitter Developer Account. If you don't have one. Worry do not. We have a backup plan.You will need to grab the <a href="https://dev.twitter.com/oauth/overview/application-owner-access-tokens" data-linktype="external">OAuth access token</a> for accessing the Twitter API from you dev account.</li>
<li>PowerBI</li>
</ul>


<h2>Architecture</h2>
<img src="https://notebooks.azure.com/fasantia/projects/hol-mls-twittersentiment/raw/docs%2FStreamingSolution.png">

<h2>The model</h2>
We are going to use a model from the gallery https://gallery.cortanaintelligence.com/Experiment/Classification-Twitter-sentiment-text-analysis. Open the experiment in your own workspace. It should look something like the following:
<br /><br />
<img src="https://notebooks.azure.com/fasantia/projects/hol-mls-twittersentiment/raw/docs%2Fpredictivemodel.png">

In this model we are going to use a SVM to classify text into two classes: Possitive or negative in a supervised way. You are going to create features based on the text using preprocessing techniques for text and feature hashing.

<h2>The web service</h2>
We are going to construct a web service with the following shape.
<br /><br />
<img src="https://notebooks.azure.com/fasantia/projects/hol-mls-twittersentiment/raw/docs%2Fpredictiveservice.png">

