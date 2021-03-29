# Prerequisite
1. [Install Vault](https://learn.hashicorp.com/tutorials/vault/getting-started-install?in=vault/getting-started#install-vault)
2. [Install jq](https://stedolan.github.io/jq/). This is optional for converting json key value to environment variables.

# Configuration

Launch a new terminal session.

Copy this following snippet to your `~/.bashrc` or `~/.zshrc` if you are running zshell. This will configure the Vault client to talk to the dev server. 


```shell-session
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN="vault-token-value-here"
```

You can consult to your leads regarding these values.

## Verify vault connection

Verify the server is running by running the `vault status` command. If it ran successfully, the output should look like the following:

```shell-session
$ vault status

Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.5.0
Cluster Name    vault-cluster-4d862b44
Cluster ID      92143a5a-0566-be89-f229-5a9f9c47fb1a
HA Enabled      false
```

# Getting environment variables
With the client connection to vault server estabilished you can get the service config. Let's try getting module secrets with this following command

```shell-session
vault kv get -format=json platform/module
```

Output:
```
{                                                                                            
  "request_id": "516b95c8-b6c4-5e9a-313c-69a69d126a58",       
  "lease_id": "",                                                                            
  "lease_duration": 0,                        
  "renewable": false,                                                                        
  "data": {                                   
    "data": {                                 
      "APP_PORT": "8006",                                                                    
      "DEFAULT_SENDER": "noreply@kata.ai",
      "EMAIL_FOOTER": "templates/footer.html", 
	 ......
	 }
  }
}
```

To make life easier, we created simple bash script to generate json format result from above command to enviroment variables format.  You will need [jq](https://stedolan.github.io/jq) for this to work. 

Simply copy this following function to `~/.bashrc` or `~/.zshrc` file.

```shell-session
kataenv() {
    vault kv get -format=json $1 | jq '.data.data' | sed -E '1d; $d;s/[[:space:]]+//g;s/,$//;s/^"/export /;s/":/=/'
}
```

And then source the file to apply the changes.
```shell-session
source ~/.zshrc
# source ~/.bashrc
```

Run this command to get environment variables format.
```shell-session
kataenv platform/module

# Write output to the file
# kataenv platform/module > .env
```

Output:
```shell-session
export APP_PORT="8006"
export DEFAULT_SENDER="noreply@kata.ai"
export EMAIL_FOOTER="templates/footer.html"
...
```

# Supported Services
This is the growing lists of supported kata services in vault as of now. The lists might be changed over time.

1. [Zaun](https://gitlab.com/kata-ai/quelle/-/tree/master/zaun)
2. [Projekt](https://gitlab.com/kata-ai/quelle/-/tree/master/projekt)
3. [Inhalt](https://gitlab.com/kata-ai/quelle/-/tree/master/inhalt)
4. [Diaenne](https://gitlab.com/kata-ai/quelle/-/tree/master/diaenne)
5. [Wache](https://gitlab.com/kata-ai/quelle/-/tree/master/wache)
6. [Postbote](https://gitlab.com/kata-ai/quelle/-/tree/master/postbote)
7. [Einsatz](https://gitlab.com/kata-ai/quelle/-/tree/master/einsatz)
8. [Kanal](https://gitlab.com/kata-ai/quelle/-/tree/master/kanal)
9. [Scheduler2](https://gitlab.com/kata-ai/quelle/-/tree/master/scheduler2)
10. [Module](https://gitlab.com/kata-ai/quelle/-/tree/master/module)
11. [Empfang-Backend](https://gitlab.com/kata-ai/quelle/-/tree/master/empfang-backend)

# References
- [Vault - Getting Started](https://learn.hashicorp.com/collections/vault/getting-started)
