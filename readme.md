

# AWS IAM Access Key Disabler

![Image of KeyIcon]
(https://github.com/te-papa/aws-key-disabler/blob/3869f2a788d948fd962a51b51ff5389f3dbf5b3b/docs/images/GitHubTepapaKeyIcon.jpg)

The AWS Key disabler is a Lambda function that disables AWS access keys after a set amount of time in order to reduce the risk associated with old access keys.

## AWS Lambda Architecture

![Image of Arch]
(https://github.com/te-papa/aws-key-disabler/blob/master/docs/images/GitHubTepapaLambda.png)

## Current Limitations

* The key will only send to a designated address as EC2 accounts don't have an email address associated with them
* Keys are only disabled, not deleted or replaced

## Prerequisites

This script requires the following components to run.
* Node.js with NPM installed https://nodejs.org/en/
* Gruntjs installed http://gruntjs.com/
* Python installed https://www.python.org/
* aws-cli installed https://aws.amazon.com/cli/

It also assumes that you have an AWS account with SES enabled, ie domain verified and sandbox mode removed.

## Installation instructions

These instructions are for OSX. Your mileage may vary on Windows and other \*nix.

1. Grab yourself a copy of this script
2. Fill in the following information in `/grunt/package.json`
	1. Set the `aws_account_number` value to your AWS account id found on https://portal.aws.amazon.com/gp/aws/manageYourAccount
	2. Set the `first_warning` and `last_warning` to the age that the key has to be in days to trigger a warning. These limits trigger an email send to `report_to`
	3. Set the `expiry` to the age in days when the key expires. At this age the key is disabled and an email is triggered to `report_to` notifying this change
	4. Set the `report_to` value to the email address you'd like to receive deletion reports to
	5. Set the `report_from` value to the email address you'd like to use as the sender address for deletion reports. Note that the domain for this needs to be verified in AWS SES.
	6. Set the `deployment_region` to a region that supports Lambda and SES. Also ensure that the region has SES sandbox mode disabled.
		* See the AWS Region table for support https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/
3. Ensure you can successfully connect to AWS from the CLI, eg run `aws iam list-users` to verify successful connection
4. from the `/grunt` directory run `grunt bumpup && grunt deployLambda` to bump your version number and deploy the script to the selected region

## Additional configuration option

* You can choose to set the message used for each warning and the final disabling by changing the values under key_disabler.keystates.<state>.message
* You can change the length of masking under key_disabler.mask_accesskey_length. The access keys are 20 characters in length.

## Troubleshooting

This script is provided as is. We are happy to answer questions as time allows but can't give any promises.

If things don't work ensure that:
* Your connection to AWS is successful
* SES is not in sandbox mode and the sender domain has been verified
* The selected region provides both Lambda and SES https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/
