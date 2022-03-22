# Demo Spring Cloud Vault

## Terminal 1

### Start Server
`vault server -config vault-server.hcl`

```
==> Vault server configuration:

                     Cgo: disabled
              Go Version: go1.17.7
              Listener 1: tcp (addr: "127.0.0.1:8200", cluster address: "127.0.0.1:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: false, enabled: false
           Recovery Mode: false
                 Storage: file
                 Version: Vault v1.9.4
             Version Sha: fcbe948b2542a13ee8036ad07dd8ebf8554f56cb

==> Vault server started! Log data will stream in below:

2022-03-22T21:10:11.886+0800 [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
2022-03-22T21:10:11.944+0800 [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
2022-03-22T21:10:11.945+0800 [INFO]  core: Initializing VersionTimestamps for core
2022-03-22T21:11:50.214+0800 [INFO]  core: security barrier not initialized
2022-03-22T21:11:50.227+0800 [INFO]  core: security barrier initialized: stored=1 shares=5 threshold=3
2022-03-22T21:11:50.238+0800 [INFO]  core: post-unseal setup starting
2022-03-22T21:11:50.251+0800 [INFO]  core: loaded wrapping token key
2022-03-22T21:11:50.255+0800 [INFO]  core: Recorded vault version: vault version=1.9.4 upgrade time="2022-03-22 21:11:50.2510909 +0800 +08 m=+98.992013401"
2022-03-22T21:11:50.256+0800 [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2022-03-22T21:11:50.257+0800 [INFO]  core: no mounts; adding default mount table
2022-03-22T21:11:50.266+0800 [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2022-03-22T21:11:50.267+0800 [INFO]  core: successfully mounted backend: type=system path=sys/
2022-03-22T21:11:50.268+0800 [INFO]  core: successfully mounted backend: type=identity path=identity/
2022-03-22T21:11:50.288+0800 [INFO]  core: successfully enabled credential backend: type=token path=token/
2022-03-22T21:11:50.288+0800 [INFO]  rollback: starting rollback manager
2022-03-22T21:11:50.289+0800 [INFO]  core: restoring leases
2022-03-22T21:11:50.294+0800 [INFO]  expiration: lease restore complete
2022-03-22T21:11:50.301+0800 [INFO]  identity: entities restored
2022-03-22T21:11:50.301+0800 [INFO]  identity: groups restored
2022-03-22T21:11:50.302+0800 [INFO]  core: usage gauge collection is disabled
2022-03-22T21:11:50.306+0800 [INFO]  core: post-unseal setup complete
2022-03-22T21:11:50.317+0800 [INFO]  core: root token generated
2022-03-22T21:11:50.318+0800 [INFO]  core: pre-seal teardown starting
2022-03-22T21:11:50.319+0800 [INFO]  rollback: stopping rollback manager
2022-03-22T21:11:50.322+0800 [INFO]  core: pre-seal teardown complete
```


## Terminal 2

### Init

`vault operator init`

```
Unseal Key 1: gf4c2W7svDXx5oBd/g8cdDgpOVad/UkH+SNKodLBeaW1
Unseal Key 2: 9N6OCcc6RHnzOfSIBIEfDpE50QKrxynSz1mcd36fUQIR
Unseal Key 3: wKobfDyw/3TcNMT/Rjf3t/LjzjFVqNqN0+WM6xqR3Pea
Unseal Key 4: p/RgSfqglYLItNp3zfvx4RbW+E61mFjk70d4wLdnlTYb
Unseal Key 5: nU1oTg/VIg8qVdMuZc4qjA1B3EOOrLMs9tCHFAh4R1Fu

Initial Root Token: s.FZTp0c0fJV1D05D1qYDZsAtM

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 keys to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```

### 
`set VAULT_TOKEN=s.FZTp0c0fJV1D05D1qYDZsAtM`

`vault status`

```
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             false
Total Shares       5
Threshold          3
Unseal Progress    0/3
Unseal Nonce       bc94403e-2af2-8256-ca9d-b3b80a6e1168
Version            1.9.4
Storage Type       file
HA Enabled         false
```


### Unseal

`vault operator unseal gf4c2W7svDXx5oBd/g8cdDgpOVad/UkH+SNKodLBeaW1`

`vault operator unseal 9N6OCcc6RHnzOfSIBIEfDpE50QKrxynSz1mcd36fUQIR`

`vault operator unseal wKobfDyw/3TcNMT/Rjf3t/LjzjFVqNqN0+WM6xqR3Pea`

```
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    5
Threshold       3
Version         1.9.4
Storage Type    file
Cluster Name    vault-cluster-739cb75a
Cluster ID      dd510672-e9a0-e7c7-f766-c4b09525328a
HA Enabled      false
```

### Enable Secrets
`vault secrets enable -path=secret/ kv`

```
Success! Enabled the kv secrets engine at: secret/
```


### Store secrets

`vault kv put secret/spring-cloud-vault/demo db.name=demo_spring_cloud_vault db.username=demo db.password=demo@123`

```
Success! Data written to: secret/spring-cloud-vault/demo
```

### Retrieve secrets

`vault kv get secret/spring-cloud-vault/demo`

```
======= Data =======
Key            Value
---            -----
db.name        demo_spring_cloud_vault
db.password    demo@123
db.username    demo
```

