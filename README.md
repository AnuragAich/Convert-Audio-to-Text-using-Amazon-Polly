# Step-by-Step Guide to Convert Audio to Text using Amazon Polly


### Overview:

The Text-to-Speech (TTS) Converter project is designed to convert written text into spoken words using a serverless architecture, leveraging AWS Lambda and Amazon Polly. This solution is ideal for creating scalable, efficient, and cost-effective applications that require natural-sounding speech synthesis.

<img width="1104" alt="Arc-Convert Audio to Text using Amazon Polly" src=![Arc-Convert Audio to Text using Amazon Polly](https://github.com/user-attachments/assets/2c2e03bf-0534-4e65-a3f6-758c54361b97)

Use Cases: 

Content Accessibility: Provides audio versions of written content for visually impaired users. \
Learning: Enables users to listen to educational materials, enhancing learning experiences. \
Content Distribution: Offers 
an additional medium for content consumption, increasing engagement. \
Convenience: Allows users to listen to articles or books while multitasking, such as during commutes or workouts. 





### Steps to Build the Project:

* Step 1: Set Up an AWS Account
  Sign Up: If you don't have an AWS account, go to AWS and sign up for a free account.
  Log In: Access the AWS Management Console by logging in with your AWS credentials.

* Step 2: Create Two S3 Buckets
  Go to S3 Service:
  Navigate to the S3 Management Console by searching for "S3" in the AWS Management Console.
  Create Source Bucket:
  Click Create bucket.
  Enter amc-polly-source-bucket as the bucket name.
  Choose a region for your bucket.
 Leave other settings as default or configure according to your needs.
 Click Create bucket.
 Create Destination Bucket:
 Repeat the steps to create another bucket named amc-polly-destination-bucket.


* Step 3: Create an IAM Policy
  Go to IAM Service:
  Navigate to the IAM Management Console by searching for "IAM" in the AWS Management Console. \
  Create Policy
  Click Policies in the left-hand menu, then click Create policy.
  Go to the JSON tab and enter the following policy (replace YOUR_REGION with your AWS region):

  ```bash
  {
   "Version": "2012-10-17",
   "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject"
      ],
      "Resource": [
        "arn:aws:s3:::amc-polly-source-bucket/*",
        "arn:aws:s3:::amc-polly-destination-bucket/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": "polly:SynthesizeSpeech",
      "Resource": "*"
      }
    ]
  }

  


* Step 4: Create an IAM Role
  Create Role:
  In the IAM Management Console, click Roles in the left-hand menu, then click Create role.
  Choose Lambda as the trusted entity type and click Next: Permissions.
  Attach Policies:
  Search for amc-polly-lambda-policy and check it.
  Also, search for “AWSLambdaBasicExecutionRole” and check it.
  Click Next: Tags (optional) and then Next: Review.
  Name and Create Role:
  Enter amc-polly-lambda-role as the role name.
  Click Create role.


* Step 5: Create and Configure the Lambda Function
  Go to Lambda Service:
  Navigate to the Lambda Management Console by searching for "Lambda" in the AWS Management Console.
  Create Lambda Function:
  Click Create function.
  Choose Author from scratch.
  Enter “TextToSpeechFunction” as the function name.
  Choose Python 3.8 as the runtime.
  Under Permissions, select Use an existing role and choose amc-polly-lambda-role.
  Add Environment Variables:
  Scroll down to Environment variables and add:
  SOURCE_BUCKET with the value amc-polly-source-bucket.
  DESTINATION_BUCKET with the value amc-polly-destination-bucket.


* Step 6: Configure S3 Event Notification
  Go to Source Bucket:
  Navigate to the S3 Management Console, click on amc-polly-source-bucket.
  Configure Event Notification:
  Click the Properties tab. 
  Scroll down to Event notifications and click Create event notification.
  Enter a name for the notification.
  For Event types, select All object create events.
  For Suffix, enter .txt to filter text files.
  Under Destination, choose Lambda function and select TextToSpeechFunction.
  Click Save changes.

* Step 7: Write Lambda Function Code
  Edit Function Code:
  In the Lambda Management Console, go to the Code tab.
  Replace the default code with the following Python script in the [TextToSpeechFunction.py](https://github.com/AnuragAich/Convert-Audio-to-Text-using-Amazon-Polly/blob/0ee7625d7e3a70e392c8af2e36823cd0dfcfdca9/TextToSpeechFunction.py)
  Save Changes:
  Click Deploy to save and deploy the Lambda function.


* Step 8: Test the System
  Upload a Test File:
  Upload a .txt file to the amc-polly-source-bucket.
  Check Execution:
  Go to the Monitor tab in the Lambda Management Console to view logs and ensure the Lambda function executed successfully.
  Check the amc-polly-destination-bucket for the generated .mp3 file.
  Verify:
  Ensure the .mp3 file is correctly created and can be played back.




