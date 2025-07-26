# Monitoring Root Account Activities Using CloudTrail, KMS, CloudWatch, SNS
 Root Account Monitoring Using CloudTrail, CloudWatch and SNS to Create Topic and Set an Event Rule for notification.
OBJECTIVE:
Simulate sensitive activity from the AWS root account and detect it using CloudTrail, CloudWatch, and SNS alerts.

Prerequisite:
- AWS CloudTrail
- SNS (Simple Notification Service)
- IAM (Identity and Access Management)
- AWS Management Console

STEP 1: Log in Using Root Account
NOTE: Avoid using root in production unless absolutely necessary.
- Go to the AWS Console
- Sign in with the root credentials

STEP 2: Perform a Sensitive Action On Root Account
- Choose any of the following examples:
- Disable MFA (Multi-Factor Authentication)
- View or access the billing dashboard
- Modify account settings

STEP 3: Enable CloudTrail
- Open the CloudTrail console, If CloudTrail is not already enabled:
- Click on Create trail
- Name your trail (In this case RootAccountTrail)
- Choose Apply trail to all regions
- Select Create new S3 bucket or use an existing one
- Click Create trail

STEP 4: Configure the S3 Bucket (Log Storage)
- Ensure that the S3 bucket selected is properly configured:
- Enable bucket versioning
- Enable access logging (optional but recommended)
- Set bucket policies to allow CloudTrail to write logs

STEP 5: Create SNS Topic
- Open the SNS console
- Choose Topics > Create topic
- Select Standard
- Enter a name (In this case, RootAlertTopic)
- Create the topic

Step 5B: Subscribe to the Topic
- While viewing your SNS topic:
- Click Create subscription
- Choose protocol (Email or SMS), I prefer and choose Email
- Enter your email or phone number(In this case Email)
- Click Create subscription
- Confirm the subscription in your email/SMS (In this case Email)

Step 6: Create CloudWatch Event Rule
- Open Amazon CloudWatch
- Go to Rules > Create rule
- Select Event Source as Event Pattern
- Use the following pattern:

{
  "source": ["aws.signin"],
  "detail-type": ["AWS Console Sign In via CloudTrail"],
  "detail": {
    "userIdentity": {
      "type": ["Root"]
    }
  }
}

- For Target, choose SNS topic
- Select the SNS topic you created (RootAlertTopic)
- Click Create rule

STEP 7: Testing the Setup
- Log in again as the root user
- Perform a sensitive action again
- Wait for a few minutes
- You should receive an alert via email/SMS
