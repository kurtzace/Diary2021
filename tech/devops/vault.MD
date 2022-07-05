## Reference
HashiCorp Certified Vault Associate: Getting Started by @ned1313 - Pluralsight

## What
- Secrets lifecycle manager - Written in Go
- Secrets engines are plugins used by Vault to handle sensitive data- could be Static, dynamic or just transit services to encrypt /decrypt/ gen hash, sign , verify
- Categories of Secret engine
a. Cloud
b. DB
c. KV, transit
d. Cert (ssh,pki)
e Tokens of consul


Dynamic DB secrets example:  Database administrators want to provide
applications and developer access to a
MySQL database. Credentials should be dynamically generated and short-lived. TTL should be based on client type


### KV secrets 
- v1 has no versioning and it is default. v2 has but can be slower and impossible to downgrade.
- The kv secrets engine is a generic Key-Value store used to store arbitrary secrets within the configured physical storage for Vault. 

### Cubbyhole/response wrapping
A management server could transmit secret data to a cluster of worker servers. It could download that secret from vault and then transmit that secret over to each member of the cluster.
Or it could generate a single use token that has access to that secret in a cubbyhole.


### Policies

In Vault, policies specify permissions.
many alternatives for assignment: tokens, identities, and authentication methods



Most precise triumphs



There is none



default procedure



Root strategy





Capabilities

Enumerate keys in a list.



No access to important data



- Not suggested by the text



Special permissions for sudo



rootshielded routes



- Avoid suggesting other actions.



Disable access by denying



- Preempts all other operations



- Rejects the entire path







policy responsibility

immediately after token creation

applied using a means of authentication



through the identity secrets engine and assigned to an entity
