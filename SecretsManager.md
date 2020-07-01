# Question

### __1. How can I immediately delete a Secrets Manager secret so that I can create a new secret with the same name?__

__Details:__

I deleted an AWS Secrets Manager secret. Then I tried to recreate the secret using the same name. However, I received the error "You can't create this secret because a secret with this name is already scheduled for deletion"

__Short Description__

When you delete a secret, Secrets Manager deprecates it with a seven-day recovery window. This means that you can't recreate a secret using the same name using the AWS Management Console until seven days have passed. You can permanently delete a secret without any recover window using the AWS Command Line Interface (AWS CLI). For more information, see [Deleting and Restoring a Secret][Deleting_and_Restoring_a_Secret]


__Resolution__

Run the [DeleteSecret][API_DeleteSecret] API call with the [ForceDeleteWithoutRecovery][API_DeleteSecret_RequestSyntax] parameter to delete the secret permanently.

`Notes:`

 - Before you begin, be sure that you [installed][cli_chap_install] and [configured][[cli_chap_configure]] the AWS CLI.
- Secrets deleted using the `ForceDeleteWithoutRecovery` parameter can't be recovered or restored.

In this example, replace `your-secret` with your Secrets Manager secret ID and `your-region` with your AWS [Region][regional_endpoints].

    $ aws secretsmanager delete-secret --secret-id your-secret  --force-delete-without-recovery --region your-region

Run the [DescribeSecret][API_DescribeSecret] API call to verify that the secret is permanently deleted.

`Note:` The deletion is an asynchronous process. There might be a short delay.  

    $ aws secretsmanager describe-secret --secret-id your-secret --region your-region

You receive an error similar to the following:

    An error occurred (ResourceNotFoundException) when calling the DescribeSecret operation: Secrets Manager can't find the specified secret.

This error means that the secret is successfully deleted.  

<!-- Link -->
[Deleting_and_Restoring_a_Secret]:[https://docs.aws.amazon.com/zh_tw/secretsmanager/latest/userguide/manage_delete-restore-secret.html]

[API_DeleteSecret]:[https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_DeleteSecret.html]

[API_DeleteSecret_RequestSyntax]:[https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_DeleteSecret.html#API_DeleteSecret_RequestSyntax]

[cli_chap_install]:[https://docs.aws.amazon.com/zh_tw/cli/latest/userguide/cli-chap-install.html]

[cli_chap_configure]:[https://docs.aws.amazon.com/zh_tw/cli/latest/userguide/cli-chap-configure.html]

[regional_endpoints]:[https://docs.aws.amazon.com/zh_tw/general/latest/gr/rande.html#regional-endpoints]

[API_DescribeSecret]:[https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_DescribeSecret.html]