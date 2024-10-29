Role Name
=========

A brief description of the role goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

A description of the settable variables for this role should go here, including any variables that are in defaults/main.yml, vars/main.yml, and any variables that can/should be set via parameters to the role. Any variables that are read from other roles and/or the global scope (ie. hostvars, group vars, etc.) should be mentioned here as well.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
```bash
vault write auth/ldap/config \                  
    url="ldap://openldap:389" \
    userdn="ou=Users,dc=nifi,dc=com" \
    groupdn="ou=Groups,dc=nifi,dc=com" \
    binddn="cn=admin,dc=nifi,dc=com" \
    bindpass="secret" \
    userattr="uid" \
    groupattr="cn" \
    insecure_tls=true \
    starttls=false

```

# Creating secret path for groups in ldap 
```bash
vault secrets enable -path=gryphin -version=2 kv 
vault secrets enable -path=it kv -version=2 kv   
```
# Creating policies
### Policy for Gryphin Admin
* File Name: `gryphinadmin.hcl` 
```hcl                                                                  
path "gryphin/*" {
    capabilities = ["create", "read", "update", "delete", "list"]
}
path "gryphin/metadata/*" {
    capabilities = ["read", "delete","list"]  # Optional for managing metadata
}
```
```bash
vault policy write gryphinadmin gryphinadmin.hcl 
vault write auth/ldap/groups/gryphinadmin policies=gryphinadmin

```
### Policy for IT Admin
* File Name: `itadmin.hcl` 
```hcl                                                                  
path "it/*" {
    capabilities = ["create", "read", "update", "delete", "list"]
}
path "it/metadata/*" {
    capabilities = ["read", "delete","list"]  # Optional for managing metadata
}
```
```bash
vault policy write itadmin itadmin.hcl 
vault write auth/ldap/groups/itadmin policies=itadmin

```
* https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/index.html
* https://docs.ansible.com/ansible/latest/collections/community/hashi_vault/docsite/migration_hashi_vault_lookup.html
* https://github.com/lrakai/vault-ldap-auth
* https://www.youtube.com/watch?v=rJCsh298d8o