# Question:

### __1. AWS Cloud Formation: role (arn:aws:iam:xxx) is invalid or cannot be assumed__

Solving an error when issuing a delete-stack (or any other related) command on AWS Cloud Formation.

`The Problem`

Ran into this issue today whilst trying to delete a CFN stack while testing some architectures:

Basically the `IAM role` that `CFN` needs to assume (to do anything to the stack) has `disappeared`.

The problem with this is now you have a defined stack that cannot be deleted, updated or redeployed.

`The Solution`

tldr; You need to `recreate` the role with the exact name as the error indicates and allow Cloud Formation access to the resources associated with the stack.

Since some stacks can span multiple services, I preferred granting admin access.

Following my philosophy of preferring the AWS CLI over the Console, I crafted a CLI command to create the required IAM role:

First create a JSON policy document as follows (and save it):

```json
{
  "Version": "2012-10-17", 
  "Statement": [
    {
      "Action": "sts:AssumeRole", 
      "Principal": {
         "Service": "cloudformation.amazonaws.com"
      }, 
      "Effect": "Allow", 
      "Sid": ""
    }
  ]
}
```

Then execute the following commands (assuming you saved the above to a file called `iam_role_cfn.json` in the current directory:

    $ aws iam create-role --role-name=PipelineStack-automaticawsdbshutdowncdkpipelineDep-18Y7YJVAYWPIS  --assume-role-policy-document file://iam_role_cfn.json --description "TEMP ROLE: Allow CFN to administer 'pipeline' stack"

And then attach the Admin policy ARN:

    $ aws iam attach-role-policy --role-name PipelineStack-automaticawsdbshutdowncdkpipelineDep-18Y7YJVAYWPIS --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

`The Result`

And trying to delete the stack again…

`Clean Up!`

We now have a rogue admin IAM role lying around. So let’s nuke it.

Detach the Admin ARN policy:

    $ aws iam detach-role-policy --role-name PipelineStack-automaticawsdbshutdowncdkpipelineDep-18Y7YJVAYWPIS --policy-arn arn:aws:iam::aws:policy/AdministratorAccess

Remove the role:

    $ aws iam delete-role --role-name PipelineStack-automaticawsdbshutdowncdkpipelineDep-18Y7YJVAYWPIS