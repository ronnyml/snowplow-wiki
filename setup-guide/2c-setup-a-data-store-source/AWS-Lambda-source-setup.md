[**HOME**](Home) » [**SNOWPLOW SETUP GUIDE**](Setting-up-Snowplow) » [**Step 2c: Setup a Data Store Source**](Setting-up-a-Data-Store-Source) » **AWS-Lambda-source-setup**

##Overview

This tool builds into a single zip file which can be uploaded as a new [AWS Lambda](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html) function. The description you give your Lambda will be used as your collector endpoint (an URL). This means the Lambda you create will need the ability to perform `lambda:GetFunctionConfiguration` on itself, amongst the other normal AWS lambda permissions.

##Building the zip artifact

For either method, you'll first need to build the zip artifact for uploading to AWS Lambda.

Executing the following:

```
./gradlew buildZip
```

in either Vagrant or your host will build a zipped archive ready for deployment in `build/distrubutions/`.

##Using the `install.sh` script

Providing you have the [aws-cli](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) tools installed (and [jq](https://stedolan.github.io/jq/download/)) you can use the install script provided as `install/install.sh`. This script is interactive and will set up everything required to start monitoring an existing s3 bucket.

To run:

```
cd install
chmod +x install.sh
./install.sh
```

##Using the AWS web UI

It's recommended you use the `install.sh` script, however if you're familiar with AWS Lambda, or prefer the AWS web console you can upload this tool yourself manually with the following steps:

1.&nbsp;Create an IAM role for the Lambda - a sample policy is provided below:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents"
            ],
            "Resource": "arn:aws:logs:*:*:*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "lambda:GetFunctionConfiguration"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

and the initial trust policy to invoke AWS Lambda:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
```

2.&nbsp;Create a new Java8 Lambda

- Ensure the `description` field is set to the collector you wish to use, e.g. `http://mycollector.com` (NB the 'http://' is required)

3.&nbsp;Upload the zip file  
4.&nbsp;Add the event sources for your S3 bucket(s)

More information on uploading a AWS Lambda function manually is available [here](http://docs.aws.amazon.com/lambda/latest/dg/with-s3.html).