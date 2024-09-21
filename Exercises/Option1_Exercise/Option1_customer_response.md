# Customer Response

Hello Agatha!

Thanks for the contact, I can see that you are testing Hshicorp Vault. That's great!

### Error description and troubleshooting

Unfortunately it seems that there's an error showing when you run the `vault status` command. Here's the error you mentioned:

```
$ vault status
Error checking seal status: Get https://127.0.0.1:8200/v1/sys/seal-status: http: server gave HTTP response to HTTPS client
```

That error happens because the server is receiving a request via HTTPS and is expecting it to be HTTP. 

This happens because, in DEV mode, Vault **does not** use TLS. Hence, it is listening on the HTTP port only and the error we are getting shows.

Majority of time, the root cause of the issue is that our env variable `VAULT_ADDR` is set like this:

```
VAULT_ADDR='https://127.0.0.1:8200'
```

As you can see, it's using the HTTPS protocol. 

To check in your system if that's the case please run the following command:

```
echo $VAULT_ADDR
```

If the URL is targeting the https endpoint, we need to change that. Simply run the following:

```
export VAULT_ADDR='http://127.0.0.1:8200'
```

Then confirm if it got corrected by running:

```
echo $VAULT_ADDR
```

With that done, just try the `vault status` command again and you should see something similar to this:

```
bash-3.2$ vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.17.5
Build Date      2024-08-30T15:54:57Z
Storage Type    inmem
Cluster Name    vault-cluster-472e0252
Cluster ID      7f5e8bf1-1459-97ca-2d20-8476ee0ce199
HA Enabled      false
```

If you see this output, all is now fine! 

Also, these steps and info are available in the document about [Starting the dev Server](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dev-server). 

### Some side considerations:

Assuming the steps above solve the issue (as they should) I would like to take this oportunity to understand better the POC you are running and what are its goals.

Also, I just want to make sure we have said that so you are aware:

Please understand that DEV is **NOT** to be run in production as it is a non-secure environment made exclusively for testing purposes! (sorry the caps but this is very important :D )

I say that as we want to make sure we help as much as possible here. 

If you need some training, demo or just a quick call to understand things better, please make sure to contact me! 

I'm more than happy to help out!

Hope to talk to you soon and good luck on your Vault journey!

Best regards.

Diego Zarpelon
