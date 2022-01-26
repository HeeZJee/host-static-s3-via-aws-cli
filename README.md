# Introduction

The goal of this project is to Host an HTML Static Website on AWS S3 completely using AWS CLI.

## Getting Started

First clone the repository from Github and switch to the new directory:

    git clone https://github.com/HeeZJee/static-s3-via-aws-cli.git
    
    cd static-s3-via-aws-cli.git


Create S3 Bucket:

    aws s3 mb s3://your-bucket-name 


Before assigning a GetObject policy, your have to update your bucket arn on `bucket-policy.json` policy configuration file,

    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "PublicReadGetObject",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::your-bucket-name/*"
            }
        ]
    }


Assign the above policy to the bucket you have created.

    aws s3api put-bucket-policy --bucket your-bucket-name  --policy file://bucket-policy.json


Let's copy our html files and images to our bucket.

    aws s3 sync ./ s3://your-bucket-name --exclude '.git*' --exclude '*.json'


Now we will host our static website on s3.

    aws s3 website s3://your-bucket-name --index-document index.html --error-document error.html


Your website is live now you can verify it by replacing bucket and region name via following url

    http://your-bucket-name.s3-website-your-region-name.amazonaws.com/
