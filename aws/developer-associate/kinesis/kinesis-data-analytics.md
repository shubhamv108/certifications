# SQL Applications
Streams/Firehose -> (DataAnalytics(SQl Statements)[<- S3]) -> Streams/Firehose

# for ApacheFlink
use Flink (Java, Scala, SQl) to process & analyze streaming data
- run on a managed cluster on aws
  - provisioning compute resources, parallel computing, auto scaling
- Flink does not read from Firehose use (for KinesisDataAbalytics for SQL)