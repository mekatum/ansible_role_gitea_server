# Ansible role for deploy self-hosted Gitea server

This Ansible role deploy and configurate self-hosted [Gitea](https://gitea.io) 
server with MariaDB on Alpine Linux.

## System requirements

So to run the playbook you will need:
* Ansible 2.12 on management node
 * Collections [community-general](https://github.com/ansible-collections/community.general) and [community-mysql](https://github.com/ansible-collections/community.mysql)
* Alpine Linux >= 3.16 on managed node

## How to use it

Create the playbook `gitea.yaml` like below and override some variables

```yaml
- hosts: gitea.your-domain.local
  roles:
    - gitea
  become: True
  become_method: su
  vars:
    - alpine_version: 'v3.16'
    - alpine_repo_mirror: 'https://mirror.yandex.ru/mirrors/alpine'

      #CHANGE_THIS_PASSWORDS
    - mysql_root_pass: 'VerySecr3tP@s$worD' 
    - gitea_database_passwd: 'SuperStR0ngP@s$word'

    - gitea_server_domain: 'gitea.your-domain.local'
    - gitea_server_ssh_domain: 'gitea.your-domain.local'
    - gitea_server_root_url: 'http://gitea.your-domain.local/'
    - gitea_server_http_port: '80'
    - gitea_server_root_url: 'http://gitea.your-domain.local/'

     # FOR HTTPS
    - gitea_server_root_url: 'https://gitea.your-domain.local/'
    - gitea_server_http_port: '443'
    - gitea_server_cert_file: '/etc/gitea/ssl_cert.cer'
    - gitea_server_key_file: '/etc/gitea/ssl_cert.key'
    - ssl_key: 
-----BEGIN RSA PRIVATE KEY-----
BLAH-BLAH
-----END RSA PRIVATE KEY-----
    - ssl_cer: 
-----BEGIN CERTIFICATE-----
BLAH-BLAH-BLAH
-----END CERTIFICATE-----
```
### HTTPS

You have to put your certificate (fullchain!) and key in encrypted ansible vault and pass them to the playbook as variables (in extra vars or in vars in playbook).

And run this playbook with `ansible-playbook`
```console
user@host:~$ ansible-playbook gitea.yaml
```

## More Gitea options
Gitea server configurates in INI-config (default in `/etc/gitea/app.ini`).
This options you must overwrite in variables of playbook.
For details of options see [Configuration Cheat Sheet ](https://docs.gitea.io/en-us/config-cheat-sheet/)
