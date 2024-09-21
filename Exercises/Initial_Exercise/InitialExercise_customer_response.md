# Customer Response

Hello Agatha!

Great to hear from you again.

I see you sent us two questions:

- First one is making the command below a Vault API call:

```bash
vault write aws/roles/my-role 
	credential_type=iam_user policy_document=-<<EOF
	{  "Version": "2012-10-17",  "Statement": [    {      "Effect": "Allow",      "Action": "ec2:*",      "Resource": "*"    }  ]}EOF
```

- Second one is, "how to write an ACL policy that give you the permissions to run that command."

So, let's briefly go through them.

***Command translation:***

The command above can be sent using our [AWS secrets engine (API)](https://developer.hashicorp.com/vault/api-docs/secret/aws) by using a POST method. 

To show you the call, I'll use "curl" as our tool, but you can also use any other tool you prefer for that. 

Here's the command:

```bash
curl --header "X-Vault-Token: <VAULT_TOKEN>" \
     --request POST \
     --data '{
       "credential_type": "iam_user",
       "policy_document": "{\"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\": \"ec2:*\", \"Resource\": \"*\" }]}"
     }' \
     http://<VAULT_ADDRESS>/v1/aws/roles/<ROLE_NAME>

```

Please take notice and change the following fields with your data:

- `<VAULT_TOKEN>`: This will be your Vault token to autenticate and make the request.
- <VAULT_ADDRESS> : The IP addres or DNS name of your Vault server.
- <ROLE_NAME> : the name of the role you are creating.

***Enable the user to create a policy***

Here you have multiple options, and you can learn more about it in our [Policies](https://developer.hashicorp.com/vault/docs/concepts/policies) document:

- Run via CLI directly - this definitely works, but has the drawback of not being so flexible to be added as an automation or part of a CI/CD pipeline for example.

  ```
  vault policy write <POLICE_NAME> -<<EOF
  path "aws/roles/*" {
    capabilities = ["create", "update"]
  }
  EOF
  ```

  - In this command you nee to change <POLICE_NAME> to a name of your preference.
  - What it does is to create a policy that give permissions to a user to create and update roles in the AWS secrets engine.
- Another possibility is to this via a policy file. This gives you more control on the versioning and authoring of the policy creation.
  The action on this is two folded: first you create the file defining the policy, then you apply it to the Vault. Bellow the steps to to it:

  - create a file and name it like this: `<policy_name.hcl>`
  - On that file, add the data:

    ```json
    path "aws/roles/*" {
    capabilities = ["create", "update"]
    }
    ```
  - Finally, apply the policy to the Vault with the command:

    ```
    vault policy write <policy_name> <policy_file_name.hcl>
    ```

Please note that the commands above will only create the roles and policies.
For a user to make those actions you need to apply those to that user.

And there you have it! 

I hope that it is clear enough on this but if there's any doubts about the process, we can organise a call or even a quick training session so this is clearer for the entire team. 

Let me know what you prefer! I'm here to help out as much as you need.

Those put aside, I'm curious to know why the need to run that as an API call instead of via command?

I ask that, because if you are looking for an automation for those steps, I can suggest the usage of our Terraform tool for example, which can help a lot on the management both of Vault and, in fact of your infra-structure. If you wanna know more aobut this let me know and we can organize a quick demo. 

Or if that's not needed, we can always help on anything else you need to automate this process.

Well, thanks for this and I hope you have a great day ahead.

Best Regards.

Diego Zarpelon
