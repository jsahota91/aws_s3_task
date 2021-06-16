# README for AWS s3

##Create EC2 instance
- Go through the configuration process of launchin a new instance just like the app instance
- Naming convention `devops_bootcamp_yourname_s3` ensure your tags are labelled
- In Security group make sure you have your SSH rule, at port 22, with source as My IP
- You need a HTTP port at 80 with the source set to Anywhere
- Launch the instance, ensure it is running.


## SSH into your EC2 instance
- Once you see your instance select Connect
- Use the SSH client tab, to get the code to SSH into your new EC2 instance
- Make sure you are in ~/.ssh directory before you paste the command
- ssh -i "devop_bootcamp.pem" ubuntu@ec2(yourip).amazonaws.com
- you should be in your localhost now `ubuntu@ip...`

## Configure aws with required dependencies
- once you have ssh into your app run these commands
- `sudo apt-get update -y`
- `sudo apt-get install python` When prompted enter `Y`
- `sudo apt-get install python-pip` install the python dependencies, when prompted enter Y
- `sudo apt-get install awscli` install dependencies for aws, enter `Y` when prompted
- `aws configure` now we need to configure aws, which we need our public and private keys for
- Enter the access key
- Enter the secret access key
- For region enter: `eu-west-1`
- For output formet enter: `json`
- `aws s3 ls` - see all the buckets available in the regio eu-west-1. The bucket is a folder on the cloud to store whatever you want. These can be accessed on AWS website under buckets tab.
- `aws s3 mb s3://devops-bootcamp-jaspreet-s3bucket` - mb means make bucket this creates our bucket, then after `s3://name_of_your_bucket`
- certain naming conventions for buckets is you can't use underscore or capital letters in the bucket name
- you will get a notification message saying make_bucket and your bucket name.
- `sudo nano devops_bootcamp_newtest.text` - create a file and give it a name
- `aws s3 cp devops_bootcamp_newtest.text s3://devops-bootcamp-jaspreet-s3bucket` - cp means copy, you are copying the file in current location in the localhost to the bucket you created. So `aws s3 cp` then `file name` then `bucket location`
- You will get a nification that it has been uploaded
- If you go to aws-> buckets in search -> you should see your bucket -> inside it should be your file

## CRUD commands
- `rm -rf devops_bootcamp_test.text` to remove a file
- `aws s3 sync s3://devops-bootcamp-jaspreet-bucket/ devops_bootcamp_text.text` this will bring everything down (download) from your bucket so your local host is up to date
- `aws s3 rm s3://devops-bootcamp-jaspreet-bucket/devops_bootcamp_test.text` delete the file from the bucket
- doing the above command your file won't exist in your bucket, go to aws and refresh buckets, you will see file is not there
- `aws s3 rb s3://devops-bootcamp-jaspreet-bucket` - rb means remove bucket, doing this your bucket will no longer exist

## s3 Glaciers
A service that accounts for non-frequently used data, which helps to save on cost as files or buckets that don't need to be accessed frequently can be kept in the s3 glacier. So they exist but just put to one side. If someone does need to access it notification has to be given.

## Edit S3 Block public access
- To make the bucket file globally available
- Open the Amazon S3 console at https://console.aws.amazon.com/s3/.
- Choose the name of the bucket that you have configured as a static website.
- Choose Permissions.
- Under Block public access (bucket settings), choose Edit.
- Clear Block all public access, and choose Save changes.

## Change Bucket Policy
- Under Buckets, choose the name of your bucket.
- Choose Permissions.
- Under Bucket Policy, choose Edit.
- To grant public read access for your website, copy the following bucket policy, and paste it in the Bucket policy editor.
- `{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
   }`
- Update the Resource to your bucket name.
- In the preceding example bucket policy, Bucket-Name is a placeholder for the bucket name. Replace with your own bucket name.
- Choose Save changes.
- A message appears indicating that the bucket policy has been successfully added.
- If you see an error that says Policy has invalid resource, confirm that the bucket name in the bucket policy matches your bucket name.
