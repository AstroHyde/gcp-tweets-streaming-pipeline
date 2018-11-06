### This repo

# Real time tweets pipeline using GCP
Forked from https://github.com/GoogleCloudPlatform/kubernetes-bigquery-python and then dutifully hacked.

## Architecture
Twitter -> App Engine Flex -> PubSub -> Dataflow -> BigQuery --> Datalab --> NLP API

## Prerequisites
 - A PubSube topic
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
