# Option1 exercise - Dev server & TLS

## Troubleshooting

- Customer is running a POC
- Starting a DEV server for testing
- When checking status got error:

  ```
  $ vault status
  Error checking seal status: Get https://127.0.0.1:8200/v1/sys/seal-status: http: server gave HTTP response to HTTPS client
  ```
- In terms of error alone, this is quite a straight forward error. The server is running http and customer is using https. So to solve this must be changed.

## Checking vault status when in DEV mode

As customer stated they are in DEV mode, I've checked our documentation about [DEV Server mode](https://developer.hashicorp.com/vault/docs/concepts/dev-server) and we can read there the following:

* > **Bound to local address without TLS** - The server is listening on `127.0.0.1:8200` (the default server address) *without* TLS.
  >

This means customer must have set the env variable `VAULT_ADDR` to https. 

The correct address would be this (assuming they are in localhost):

`export VAULT_ADDR='http://127.0.0.1:8200'`

The above command is shown in [Vault tutorials: Starting the server](https://developer.hashicorp.com/vault/tutorials/getting-started/getting-started-dev-server)

Which will establish the connection using the http protocol and hence, without TLS.

## Oportunities to bond with customer and help further

- Customer is creating a POC of the system, this is a good time to help them more by offering some training or a conversation about what are the goals of their project.
- They may need help on the planning and/or with tools on how to use Vault properly.
