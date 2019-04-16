# Real Time Analytics with Azure Machine Learning Studio y Stream Analytics

In this session, you will going to use an experiment already created in the gallery for you, https://gallery.cortanaintelligence.com/Experiment/Classification-Twitter-sentiment-text-analysis to try to predict the sentiment of the twitts containing specific tags. Using Python, we will query the Twitter API. This twitts will then be sent to an Event hub account to be stored for further processing in the form of a queue. The stream of events will be consumed by a Stream analytics Job which will in turn will query an Azure ML Studio model published as a web service to predict the sentiment of the twitt. Finally, the average sentiment is posted in PowerBI in realtime.

## Requirements to complete this lab

<ul>
<li>An Azure subscription</li>
<li>A Twitter Developer Account. If you don't have one. Worry do not. We have a backup plan.You will need to grab the <a href="https://dev.twitter.com/oauth/overview/application-owner-access-tokens" data-linktype="external">OAuth access token</a> for accessing the Twitter API from you dev account.</li>
<li>PowerBI</li>
</ul>


<h2>Architecture</h2>
<img src="https://github.com/msftargentina/analyticsinaday/blob/master/doc/docs_m03_solution.png?raw=true">

<h2>The model</h2>
We are going to use a model from the gallery https://gallery.cortanaintelligence.com/Experiment/Classification-Twitter-sentiment-text-analysis. Open the experiment in your own workspace. It should look something like the following:
<br /><br />
<img src="https://github.com/msftargentina/analyticsinaday/blob/master/doc/docs_m03_pipeline.png?raw=true">

In this model we are going to use a SVM to classify text into two classes: Possitive or negative in a supervised way. You are going to create features based on the text using preprocessing techniques for text and feature hashing. In this case, the classification algorithm will return 0 meaning negative sentiment or 4 meaning positive sentiment.

<h2>The web service</h2>
We are going to construct a web service that can be called from the Azure Stream Analytics job. In order to do that, you have to setup a web predictive web service. When you do that, ensure that it looks like the following one. Run the experiment and the publish the web service.
<br /><br />
<img src="https://github.com/msftargentina/analyticsinaday/blob/master/doc/docs_m03_service.png?raw=true">

<h2>Streaming tweets to Azure Event Hub</h2>
<p>In order to submit the tweets for further processing, you will need to collect them. You will need a twitter developer account in order to do that. (You can skip this part if you want to use the twitts I am collecting for you in the demostration of this lab). To collect the tweets we are gonna use a Python library. This can be executed in your local computer if you have Python installed or in a Jupyter Notebook of your choice.
</p>
<p>
Install the library twitter2eh by running the following code in your python environment. Note: Azure Service Bus current version is 0.5. twitter2eh needs version 0.21.1 otherwise will fail
</p>

```bash
pip install azure-servicebus==0.21.1
pip install twitter2eh 
```

<p>
Create a twitter2eh.json.json file with the following structure. You will need to have an Event Hub component created in your Azure Subscription.
</p>

```
{
    "twitter_consumer_key" : "FROM_YOUR_TITTER_ACCOUNT",
    "twitter_consumer_secret" : "FROM_YOUR_TITTER_ACCOUNT",
    "twitter_access_token" : "FROM_YOUR_TITTER_ACCOUNT",
    "twitter_access_secret" : "FROM_YOUR_TITTER_ACCOUNT",
    "track_keywords" : [
        "#CosmosDB",
        "#ApacheSpark",
        "#MachineLearning",
        "#DataScience"
    ],
    "eventhub_namespace" : "EVENT_HUB_NAMESPACE",
    "eventhub_entity" : "EVENT_HUB_NAME_INSIDE_NAMESPACE",
    "eventhub_sas_key" : "EVENT_HUB_SHARED_ACCESS_KEY_POLICY_NAME",
    "eventhub_sas_value" : "EVENT_HUB_SHARED_ACCESS_KEY_POLICY_VALUE"
}
```
<p>
Run the process by running the following command. This process will start collecting the tweets using you tweeter account and start to stream it to the Azure Event Hub you configured.
</p>

```bash
twitter2eh --conf ./twitter2eh.json
```

<h2>Processing the Events with Stream Analytics</h2>
<p>You will need to specify a query to process the tweets with the machine learning model you created. One you register you model web service as a function inside Stream Analytics, use the following query to process them and output the result. Note that TwitterStream is the name of the input stream, PowerBIStream is the name of the output stream and InferSentiment is the name of the Azure Machine Learning Function we constructed.
</p>

```sql
WITH sentiment AS (  
    SELECT text, InferSentiment(text) as result 
    FROM TwitterStream  
)  

SELECT System.TimeStamp AS WindowEnd, result.[Scored Labels], Count(*) AS Twitts
INTO PowerBIStream
FROM sentiment  
GROUP BY result.[Scored Labels], TumblingWindow(second, 5)
```