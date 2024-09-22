# Hashicorp-Vault

## docker deployment
```Shell
    docker run --name my-vault \
    -d \
    --cap-add=IPC_LOCK \
    -e 'VAULT_DEV_ROOT_TOKEN_ID=root' \
    -e 'VAULT_DEV_LISTEN_ADDRESS=0.0.0.0:8200' \
    -p 8200:8200 \
    -v ./config:/vault/config \
    -v ./file:/vault/file \
    -v ./logs:/vault/logs \
    hashicorp/vault server
```

> **Error initializing listener of type tcp: listen tcp4 0.0.0.0:8200: bind: address already in use**
>
> don't use -config options when running use docker
>
{style="warning"}

## config file
```json
{
    "storage": {
        "file": {
            "path": "/vault/file"
        }
    },
    "listener": [
        {
            "tcp": {
                "address": "0.0.0.0:8200",
                "tls_disable": true
            }
        }
    ],
    "default_lease_ttl": "168h",
    "max_lease_ttl": "720h",
    "ui": true
}
```


## create userpass token(create user and config policy in ui)

get token with bob user policy
```Shell
curl --request POST --data '{"password": "bob123"}' http://127.0.0.1:8200/v1/auth/userpass/login/bob
```

use bob token
```Shell
curl --header "X-Vault-Token: xxx" \
--request GET \
http://127.0.0.1:8200/v1/secret/data/my-secret
```

