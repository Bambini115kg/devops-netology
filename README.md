## Курсовая работа по итогам модуля "DevOps и системное администрирование"
Добрый день! Немного о курсовой. Она сделана по методичке, ничего лишнего.
Инструменты: vagrant+VB+Ubuntu+ufw+vault+nginx
Cтолкнулся с проблемами, может прокомментируете:
- при запуске vault в dev-режиме, не смог добиться запуска UI на хосте. 
  Порты пробрасывал по всякому (8200-8200, 80-8200 и т.д.)
- при запуске vault в боевом режиме, получалось работать в UI, нормально 
  генерировались все секреты, но на стадии написании скрипта в консоле, я получал ошибку связанную с тем, что мой запрос HTTP
  получает ответ HTTPS. Возможно из-за того, что я сертификат делал под nginx, который 
  работает на 127.0.0.1, а vault никак не хочет работать с HTTPS?
Как выяснилось, можно сертификат и privat key для хоста, помещать в один файл, поэтому я
генерировал портянку и всю в один файл помещал, нормально?
### Результат
1. Процесс установки и настройки ufw
```
vagrant@vagrant:~$ sudo ufw status
Status: inactive

vagrant@vagrant:~$ sudo ufw allow 22
Rules updated
Rules updated (v6)

vagrant@vagrant:~$ sudo ufw allow 443
Rules updated
Rules updated (v6)

vagrant@vagrant:~$ sudo ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup

vagrant@vagrant:~$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
22                         ALLOW       Anywhere
443                        ALLOW       Anywhere
22 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
```
2. Процесс установки и выпуска сертификата с помощью hashicorp vault

```
vagrant@vagrant:~$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
OK
vagrant@vagrant:~$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
vagrant@vagrant:~$ sudo apt-get update && sudo apt-get install vault
vagrant@vagrant:~$ vault
Usage: vault <command> [args]

Common commands:
    read        Read data and retrieves secrets
    write       Write data, configuration, and secrets
    delete      Delete secrets and configuration
    list        List data or secrets
    login       Authenticate locally
    agent       Start a Vault agent
    server      Start a Vault server
    status      Print seal and HA status
    unwrap      Unwrap a wrapped secret

Other commands:
    audit          Interact with audit devices
    auth           Interact with auth methods
    debug          Runs the debug command
    kv             Interact with Vault's Key-Value storage
    lease          Interact with leases
    monitor        Stream log messages from a Vault server
    namespace      Interact with namespaces
    operator       Perform operator-specific tasks
    path-help      Retrieve API help for paths
    plugin         Interact with Vault plugins and catalog
    policy         Interact with policies
    print          Prints runtime configurations
    secrets        Interact with secrets engines
    ssh            Initiate an SSH session
    token          Interact with tokens

        vagrant@vagrant:~$ sudo apt-get install jq
vagrant@vagrant:~$ vault server -dev -dev-root-token-id root
vagrant@vagrant:~$ export VAULT_ADDR=http://127.0.0.1:8200
vagrant@vagrant:~$ export VAULT_TOKEN=root
vagrant@vagrant:~$ vault secrets tune -max-lease-ttl=87600h pki
Success! Tuned the secrets engine at: pki/
vagrant@vagrant:~$ vault write -field=certificate pki/root/generate/internal \
>      common_name="localhost" \
>      ttl=87600h > CA_cert.crt
vagrant@vagrant:~$ vault write pki/config/urls \
>      issuing_certificates="$VAULT_ADDR/v1/pki/ca" \
>      crl_distribution_points="$VAULT_ADDR/v1/pki/crl"
Success! Data written to: pki/config/urls
vagrant@vagrant:~$ vault secrets enable -path=pki_int pki
Success! Enabled the pki secrets engine at: pki_int/
vagrant@vagrant:~$ vault secrets tune -max-lease-ttl=43800h pki_int
Success! Tuned the secrets engine at: pki_int/
vagrant@vagrant:~$ vault write -format=json pki_int/intermediate/generate/internal \
>      common_name="localhost Intermediate Authority" \
>      | jq -r '.data.csr' > pki_intermediate.csr
vagrant@vagrant:~$ vault write -format=json pki/root/sign-intermediate csr=@pki_intermediate.csr \
>      format=pem_bundle ttl="43800h" \
>      | jq -r '.data.certificate' > intermediate.cert.pem
vagrant@vagrant:~$ vault write pki_int/intermediate/set-signed certificate=@intermediate.cert.pem
Success! Data written to: pki_int/intermediate/set-signed
vagrant@vagrant:~$ vault write pki_int/roles/example-dot-com \
>      allowed_domains="localhost" \
>      allow_subdomains=true \
>      max_ttl="720h"
Success! Data written to: pki_int/roles/example-dot-com
vagrant@vagrant:~$ vault write -format=json pki_int/issue/example-dot-com common_name="localhost" ttl="720h" > /home/vagrant/key_nginx.crt
vagrant@vagrant:~$ cat key_nginx.crt | jq -r .data.certificate > /home/vagrant/localhost.crt
vagrant@vagrant:~$ cat key_nginx.crt | jq -r .data.ca_chain[] >> /home/vagrant/localhost.crt
vagrant@vagrant:~$ cat key_nginx.crt | jq -r .data.private_key > /home/vagrant/localhost.key
```
3. Процесс установки и настройки сервера nginx
- я пробросил порты в vagrantfile, чтобы на моей машине в браузере доступ был к стартовой
страничке сервера
```
vagrant@vagrant:~$ sudo apt update
vagrant@vagrant:~$ sudo apt install nginx

vagrant@vagrant:~$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2022-01-06 18:49:42 UTC; 41s ago
       Docs: man:nginx(8)
   Main PID: 1695 (nginx)
      Tasks: 3 (limit: 1071)
     Memory: 5.3M
     CGroup: /system.slice/nginx.service
             ├─1695 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             ├─1696 nginx: worker process
             └─1697 nginx: worker process

Jan 06 18:49:42 vagrant systemd[1]: Starting A high performance web server and a reverse proxy server...
Jan 06 18:49:42 vagrant systemd[1]: Started A high performance web server and a reverse proxy server.

vagrant@vagrant:~$ sudo ufw allow 'Nginx FULL'
Rule added
Rule added (v6)
vagrant@vagrant:~$ sudo nano /etc/nginx/nginx.conf
# настройки https
       server {
  listen              80 ssl;
  listen              443 ssl;
  server_name         localhost;
  ssl_certificate     /home/vagrant/localhost.crt;
  ssl_certificate_key /home/vagrant/localhost.key;
  ssl_protocols       TLSv1 TLSv1.1 TLSv1.2;
  ssl_ciphers         HIGH:!aNULL:!MD5;
        }
vagrant@vagrant:~$ sudo service nginx reload
```
4. Скрипт записал в script_crt и дал права на выполнение
```
#! /usr/bin/env bash

vault write -format=json pki_int/issue/example-dot-com common_name="localhost" ttl="720h" > /home/vagrant/key_nginx.crt

cat key_nginx.crt | jq -r .data.certificate > /home/vagrant/localhost.crt
cat key_nginx.crt | jq -r .data.ca_chain[] >> /home/vagrant/localhost.crt
cat key_nginx.crt | jq -r .data.private_key > /home/vagrant/localhost.key

sudo service nginx reload
```
5. Crontab работает (выберите число и время так, чтобы показать что crontab запускается и делает что надо)
```buildoutcfg
20 * * * * /home/vagrant/script_crt - такое в crontab записал

vagrant@vagrant:~$ grep CRON /var/log/syslog

Jan 18 17:17:01 vagrant CRON[2366]: (root) CMD (   cd / && run-parts --report /etc/cron.hourly)
Jan 18 17:20:01 vagrant CRON[2369]: (vagrant) CMD (/home/vagrant/script_crt)
Jan 18 17:20:01 vagrant CRON[2368]: (CRON) info (No MTA installed, discarding output)
```