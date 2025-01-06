# DataFireHose
  Record(1MB) -> KinesisFirehose <-DataTransformation-> AWS lambda
                                 -BatchWrites-> **S3/Redshift/OpenSearch** 
                                                -3rdParty-> Splunk/newRelic/DataDog/MongoDB
                                                -> HTTP/Endpoint
                                                -> S3Bucket/FailedS3Bucket
  
 - Pay for data going through firehose
 - Near RealTime: 
   - BufferInterval: 0 seconds (no buffering) to 900 seconds
   - BufferSize: min 1MB


Streams                                       |                      Firehose
Streaming svc for ingest at scale             | Stream data into S3/redshift/Opensearch/ 3rd party/ HTTP
Write custom code (prod/cons)                 | Fully Managed
Real time (~200ms)                            | **Near Realtime**
Manage scaling (shard spliting / merging)     | AutoScaling
Data storage for 1 to 365days                 | No Data storage
Supports replay capability                    | Doesn't support replay capability