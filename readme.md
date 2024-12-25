# Package Used

[GREsau/localstack-persist](https://github.com/GREsau/localstack-persist?tab=readme-ov-file)

# AWS Commands

## Start

```sh
docker-compose up -d
```

## Stop

```sh
docker-compose stop
```

## logs

```sh
docker-compose logs -f
```

## Get localstack bash shell

```sh
docker exec -it localstack-persist_localstack_1 bash
```

## To set configuration

```sh
aws configure

# This will ask you to set following things:
# - AWS Access Key ID [****]:
# - AWS Secret Access Key [****]:
# - Default region name [us-west-1]: us-west-2
# - Default output format [None]:
```

## S3 Bucket

### Create s3 bucket

```sh
aws --endpoint-url=http://localhost:4566 s3 mb s3://maze-storage
# Note: 'maze-storage' is bucket-name
```

### Attach an ACL to the bucket so it is readable

```sh
aws --endpoint-url=http://localhost:4566 s3api put-bucket-acl --bucket maze-storage --acl public-read
```

### List s3 bucket

```sh
aws s3 ls --endpoint-url http://localhost:4566

aws --endpoint-url=http://localhost:4566 s3 ls s3://maze-storage

# OR using awslocal
awslocal s3 ls

awslocal s3 ls maze-storage
```

### Remove directory/file from s3 bucket

```sh
# To remove dir:
aws --endpoint-url=http://localhost:4566 s3 rm s3://maze-storage/53779c28-3dd7-4e7d-bc2c-95aab8c1a67e/temp/ --recursive 

# To remove file:
aws --endpoint-url=http://localhost:4566 s3 rm s3://maze-storage/53779c28-3dd7-4e7d-bc2c-95aab8c1a67e/temp.txt
```

## AWS queue

### Create queue in localstack

```sh
aws --endpoint-url=http://localhost:4576 sqs create-queue --queue-name <queue-name>

# example
aws --endpoint-url=http://localhost:4576 sqs create-queue --queue-name notifyqueue
```

### List queue

```sh
aws --endpoint-url=http://localhost:4576 sqs list-queues
```

### Create SNS topic

```sh
aws --endpoint-url=http://localhost:4575 sns create-topic --name my-topic-name

# example
aws --endpoint-url=http://localhost:4575 sns create-topic --name notifyTopic
#  output: "TopicArn": "arn:aws:sns:us-east-1:000000000000:notifyTopic"
```

### Subscribing to a Topic

```sh
aws --endpoint-url=http://localhost:4575 sns subscribe --topic-arn arn:aws:sns:us-east-1:000000000000:notifyTopic --protocol sqs --notification-endpoint http://localhost:4576/queue/notifyqueue

# output: "SubscriptionArn": "arn:aws:sns:us-east-1:000000000000:notifyTopic:ccb720ca-18cb-4e71-bd9e-bd385c226d32"
```

### Get messages

```sh
aws --endpoint-url=http://localhost:4576 sqs receive-message --queue-url http://localhost:4576/queue/notifyqueue
```

## Generate pre-signed url from terminal

```sh
aws s3 presign s3://maze-storage/presentation.ppt --endpoint-url https://s3.wasabisys.com
```

## ***s3 - setup for minio and localstack:***

-----------------------------------

```dotenv
AWS_S3_ACCESS_KEY_ID=minio
AWS_S3_SECRET_ACCESS_KEY=minio123
AWS_S3_DEFAULT_REGION=us-east-1
AWS_S3_MAZE_BUCKET=maze
AWS_S3_URL=http://192.168.17.214:9001/maze
AWS_S3_ENDPOINT=http://192.168.17.214:9001
```

```dotenv
AWS_S3_ACCESS_KEY_ID=foo
AWS_S3_SECRET_ACCESS_KEY=bar
AWS_S3_DEFAULT_REGION=us-east-1
AWS_S3_MAZE_BUCKET=maze-storage
AWS_S3_URL=http://localhost:4566/maze-storage
AWS_S3_ENDPOINT=http://localhost:4566
```

## Docker build command

```sh
# Build command
docker-compose -f docker-compose.schedule.yml build

# Up command
docker-compose -f docker-compose.schedule.yml up

# Get bash shell command
docker exec -it mazetec-back_backend-schedule_1 bash

# Stop command
docker-compose -f docker-compose.schedule.yml stop
```
