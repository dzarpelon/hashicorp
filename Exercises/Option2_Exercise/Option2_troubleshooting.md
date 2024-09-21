# Option 2 exercise - Generate-ROOT API

### Troubleshooting

- Customer wants to create a root token using Vault API
  - this action by itself is really not a good practice. We want fine grained permissions as that will help ensure customer security.
  - first, a root token is already part of the default tokens in Vault. So it's not necessary - initially - to create one.
  - Also, a root token can do anything within Vault, which is not advisable at best.
  - Doing this via API seems even more insecure.
- We have docs, but they seem to be missing the API for decoding.
  - [Generate root token](https://www.vaultproject.io/api/system/generate-root.html)
  - [Operations Generate root token](https://learn.hashicorp.com/vault/operations/ops-generate-root)
- It seems to me that Hashicorp may have intentionaly not added the step to decode it with an API as a security measure.
- A follow up question here is why the "root" token is necessary as customer should be following the principle of least priviles and vault already have a root token associated with it.

## Action plan for customer communication

- It's probably best to inform customer about the securtiy implications of creating a root token and also understand why they want that.
- Also, understand why the need to do those steps specifically via API.
- Inform customer about the [Production Hardening Guide](https://developer.hashicorp.com/vault/tutorials/operations/production-hardening) and specially the following lines of it:

* > **Avoid Root Tokens** . Vault provides a root token when it is first initialized. This token should be used to setup the system initially, particularly setting up auth methods so that users may authenticate. We recommend treating Vault [configuration as code](https://www.hashicorp.com/blog/codifying-vault-policies-and-configuration/), and using version control to manage policies. Once setup, the root token should be revoked to eliminate the risk of exposure. Root tokens can be [generated when needed](https://developer.hashicorp.com/vault/docs/commands/operator/generate-root), and should be revoked as soon as possible.
  >
* > **Do not run as root** . Use a dedicated, unprivileged service account to run Vault, rather than running as the root or Administrator account. Vault is designed to be run by an unprivileged user, and doing so adds significant defense against various privilege-escalation attacks.
  >

- Also stressing the point of our doc on [Tokens](https://developer.hashicorp.com/vault/docs/concepts/tokens) that says the following:

> Root tokens are useful in development but should be extremely carefully guarded in production. In fact, the Vault team recommends that root tokens are only used for just enough initial setup (usually, setting up auth methods and policies necessary to allow administrators to acquire more limited tokens) or in emergencies, and are revoked immediately after they are no longer needed. If a new root token is needed, the `operator generate-root` command and associated [API endpoint](https://developer.hashicorp.com/vault/api-docs/system/generate-root) can be used to generate one on-the-fly.

> It is also good security practice for there to be multiple eyes on a terminal whenever a root token is live. This way multiple people can verify as to the tasks performed with the root token, and that the token was revoked immediately after these tasks were completed.

- So main point of our communication should actually be that the action of creating a root token should be done very rarely. And even then the best option to do it [is via Vault cli](https://developer.hashicorp.com/vault/docs/commands/operator/generate-root).
- Maybe a call or Vault Security best practices training here would be suitable. (Maybe[ Hashicorp Sentinel](https://www.hashicorp.com/sentinel) can be helpful for them?)
- Finally customer mentioned that they are "documenting the process" which can imply that our documentation is not either easily findable or miss something. We should try to understand that and see what can be improved.
