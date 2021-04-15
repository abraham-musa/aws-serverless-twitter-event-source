# aws-serverless-twitter-event-source
AWS Serverless Twitter Event Source



1. The TwitterSearchPoller lambda function is periodically triggered by a CloudWatch Events Rule.
2. In stream mode, a DynamoDB table is used to keep track of a checkpoint, which is the latest tweet timestamp found by past searches.
3. The poller function calls the Twitter Standard Search API and searches for tweets using the search text provided as an app parameter.
4. The TweetProcessor lambda function (provided by the app user) is invoked with any new tweets that were found after the last checkpoint.
5. If stream mode is not enabled, the TweetProcessor lambda function will be invoked with all search results found, regardless of whether they had been seen before.




Twitter API Key Setup
The app expects to find the Twitter API keys as encrypted SecureString values in SSM Parameter Store. You can setup the required parameters via the AWS Console or using the following AWS CLI commands:


```bash
aws ssm put-parameter --name /twitter-event-source/consumer_key --value <your consumer key value> --type SecureString --overwrite
aws ssm put-parameter --name /twitter-event-source/consumer_secret --value <your consumer secret value> --type SecureString --overwrite
aws ssm put-parameter --name /twitter-event-source/access_token --value <your access token value> --type SecureString --overwrite
aws ssm put-parameter --name /twitter-event-source/access_token_secret --value <your access token secret value> --type SecureString --overwrite
```
