# Customer response

Hello Agatha.

Thanks for your question on this! 

In your message you informed that you are documenting the process of creating a root token via API and also that our documentation is missing the info on how to decode it via API. 

Finally, you finish your message asking how that can be done. 

Well, there's a lot to unpack here. So please bear with me. 

To make my line of taught a bit clearer, as we may have a big conversation on this, I'll outline my response to you on this as follows:

1. [Security concerns about creating and decoding a root token via API](#security-concerns-about-creating-and-decoding-a-root-token)
2. [A brief discussion of &#34;Production Hardening&#34; Vault](#a-brief-discussion-of-production-hardening-vault)
3. [Questions about the documenting procces and how can we help on getting that better.](#questions-about-the-documenting-process-and-how-can-we-help-on-getting-that-better)
4. [Some final considerations.](#some-final-considerations)

TLDR. of all this is the following: "Creating a root token should be done rarely, if ever, and should not be done via API."

I'll try to explain this better in the lines that follow.

### Security concerns about creating and decoding a root token via API

As you may know the root token is the most powerfull token in Vaul. Having unrestricted access to everything within it. So, if it get's compromised, it have a enourmous risk to the security of your system as it can read all other tokens and change anything that's needed.

Also, if audit logging is not enabled, there's a big risk that all actions done by that token may go un-noticed. Which can make the root token potential for breach even worse as you would only know that is breached when it's way too late.

When being created, the token has a One-time-password (OTP) which have the risk, via API - if mishandled - to be leaked. Specially if we save it in a storage without the proper security.

That's why, we recommend that the root token creation is done via CLI ( `vault operator generate-root`) as it adds quite a few safeguards for the token creation/decoding. 

A lot of this steps - if not all of them - would need to be handled by the developer if using API, which can be a security flaw for it.

### A brief discussion of "[Production Hardening](https://developer.hashicorp.com/vault/tutorials/operations/production-hardening)" Vault

Hardening Vault is of major importance for the security of your system. As Vault will contain all tokens and access to your apps and users, this shouldn't be taken lightely. (and that's why this big text :D ).

As "baseline recommendations" here are a few of the actions that should be in place for a safe Vault implementation:

- **Allow minimal write privileges**
- **End-to-End TLS**
- **Avoid Root Tokens**
- **Enable audit logging**
- **Restrict Storage Access**
- **Use Short TTLs**

The above are just a subset of those baselines, but I believe it ilustrates the root token creation importance on the security of the system.

### Questions about the documenting process and how can we help on getting that better

All that aside, I would like to ask you if you think our documentation is clear enough to help out on understanding the above points. 

If it isn't, we can certainly try to make it better. Just give me the pain points you had with it and I'll work on updating it.

Also, you can always add your own requests/PRs to our documentation in our [Documentation Github Repository](https://github.com/hashicorp/vault/tree/main/website/content/docs "Go to Github Repo")

### Some final considerations

Well, that was quite a bit text!

Sorry if that was a bit too much, but I would like to enphasize the importance of the root token so we keep your system as safe as possible.

That said, I'm more than happy to go on a call or even creating a presentation of some of the security steps that should be in mind at all times for Vault and get that to your team. 

Thanks for the patience on reading this.

Diego Zarpelon
