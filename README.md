# Does everyone hate your tweets? Google Cloud Platform NLP can help!

This repo follows my Medium Blogpost: https://medium.com/@elaina.hyde/does-everyone-hate-your-tweets-google-cloud-platform-nlp-can-help-dc1c8ac6bfd3

## Real time tweets pipeline using GCP
The pipeline structure is forked from Graham Polley https://github.com/polleyg/gcp-tweets-streaming-pipeline 

The pipeline is described in his Medium post: https://medium.com/weareservian/tweets-pipelines-gcp-and-poetry-how-did-it-get-to-this-2a6e47fb3f6a

## Architecture
Twitter -> App Engine Flex -> PubSub -> Dataflow -> BigQuery --> Datalab --> NLP API

## Prerequisites
 - A PubSub topic
 - A BigQuery Dataset
 - A GCS bucket called `tweet-pipeline`

## Deployment
`gcloud builds submit --config=cloudbuild.yaml .`

## Sample Query
```
SELECT
  TIMESTAMP_MILLIS(timestamp) AS tweet_timestamp,
  JSON_EXTRACT(payload,
    '$.text') AS tweet_text,
  JSON_EXTRACT(payload,
    '$.user.screen_name') AS user_screen_name,
  JSON_EXTRACT(payload,
    '$.user.location') AS user_location,
  JSON_EXTRACT(payload,
    '$.user.followers_count') AS user_followers_count
FROM
  `twitter.tweets`
WHERE
  JSON_EXTRACT(payload,
    '$.text') LIKE '%BigQuery%'
```
