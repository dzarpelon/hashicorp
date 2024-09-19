# Initial exercise - AWS Engine

# Troubleshooting

Question is two folded, "how to translate" and also "what would enable us to run the translated command."

## How to make a CLI command into an API command

First step here is to understand exactly what the command used does. With that we can, then, make it a Vault API command.

From what I see at first glance, this is what's happening

* Vault is managing access to AWS via secrets engine and is adding a role to the role "my-role"
* Still at first glance, it allows access to every action in ec2 and every resource on it.

To confirm the above, I've checked the following docs:

* [AWS secrets engine](https://developer.hashicorp.com/vault/docs/secrets/aws)
  * In this I can see that what we are actually doing is not simply "adding a policy in the role", but actually adding a new role
  * this role will generate IAM users that have the permissions provided in the EOF portion of the file.
  * My assumption of what type of acces it give, seems to be correct.

Once that confirmation is done, we are ready to check the command translation to Vault API syntax.

For this, I've checked doc [AWS secrets engine (API)](https://developer.hashicorp.com/vault/api-docs/secret/aws) and what I could verify is this:

* The endpoint we are hitting is "https://`<Vault-Address>`/v1/aws/roles/my-role"
* On this we are doing a POST with the following data:

```json
{
  "credential_type": "iam_user",
  "policy_document": "{\"Version\": \"2012-10-17\", \"Statement\": [{ \"Effect\": \"Allow\", \"Action\": \"ec2:*\", \"Resource\": \"*\" }]}"
}
```

- To actually run that command we can use tools like "curl" (command line aproach) or Postman (GUI aproach).

## How to enable someone to create a role

For the second part of the question, we are looking onto how to even be able to create the role in the first place.
This is fairly simple, as we can see in our doc about [Policies](https://developer.hashicorp.com/vault/docs/concepts/policies)

Basically, policies are "a declarative way to grant or forbid access to certain paths and operations in Vault".

Here, we want a policy that allow us to create roles.

So, we have two possibilities:

### Via Vault CLI

Here we have a few steps that need to be done:

First, there's two ways to go via CLI.

- Via policy file - this is an interesting path so we can keep track of all policies we use and also keep them properly organized. This can be added to a CI/CD pipeline as well if need be.
- Via direct command - a faster way of doing it. Personally I would only go this route when I need to solve something fast.

#### Policy file approach

Here we first create the policy file in our directory, I'll call this "aws_roles_create.hcl".

As it's content, we will add the following:

```
path "aws/roles/*" {
  capabilities = ["create", "update"]
}

```

That done, we then can apply the policy with this command:

```
vault policy write aws-roles-create aws_roles_create.hcl
```

What the comand does is to write a policy called  "aws_roles_create" using the data from file "aws_roles_create.hcl"

#### Direct command approach.

Here is a bit simpler. We just run the following command:

```
vault policy write aws-roles-create -<<EOF
path "aws/roles/*" {
  capabilities = ["create", "update"]
}
EOF

```

So, I think we are now - finally - ready to answer the customer!

# [Customer Response](./InitialExercise_customer_response.md)
