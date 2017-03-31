# Ansible: openvpn

Installation and Configuring of OpenVPN

Available on Ansible Galaxy: [pgkehle.openvpn](https://galaxy.ansible.com/pgkehle/openvpn)

## Variables

Host Definitions typically contain the following:

```yaml
group_name              Name of Ansible group of servers that are configured as OpenVPN portals

target_servers          Listing of servers that are directed through the server

- target_servers:
  - description:    "description"
    server_name:    "short name"
    server_fqdn:    "fqdn"
    ip_address:     "ip address (or range)"
    netmask:        "255.255.255.255 or similar"


dns_servers             Listing of DNS servers that are published when the client is connected

sc_country              Country for the cert                        
sc_locality             Locality for the cert
sc_state                State for the cert
sc_organization         Organization for the cert
sc_ou                   Organizational Unit for the cert
sc_email                Email for the cert
sc_common_name          Common name for the cert, usually fqdn
sc_domain_comp          Domain Component for the cert, if any

```

## Tags

```yaml
server_init:         Server Initialization
server_config:       Server Configuration
server_cert_gen:     Server Certificate Generation
server_cert_pull:    Server Certificate Pull
easy_ovpn_init:      Initialize the Easy OpenVPN directoryu
client_cert_gen:     Client Certificate Generation (specify only one portal machine)
client_cert_sync:    Client Certificate synchronization between servers
easy_ovpn_kill:      Kill the Easy OpenVPN directoryu
client_deploy:       Install the packages and the certificate on the client
```

## Flags for which sections to run
```yaml
force:                  Force the command to happen

    For client cert generate:
      - Local keys directory is cleaned of the one being generated in `index` 
      - `serial` contains an appropriate number.  
      - serialized `.pem` files are removed
      
```

## Examples

```yaml
- hosts: all
    vars: 
        target_servers: []
        dns_servers:    [] 
    roles:
      - { name: pgkehle.openvpn }
```

```bash
ansible-playbook myplaybook.yml -t server_init
ansible-playbook myplaybook.yml -t server_config
ansible-playbook myplaybook.yml -t easy_ovpn_init
ansible-playbook myplaybook.yml -t server_cert_gen
ansible-playbook myplaybook.yml -t server_cert_pull
ansible-playbook myplaybook.yml -t client_cert_gen -e 'target_host=localhost'
ansible-playbook myplaybook.yml -t client_cert_gen -e 'target_host=myhost'
ansible-playbook myplaybook.yml -t client_cert_sync
ansible-playbook myplaybook.yml -t client_deploy
```
## License

MIT

## Author Information

Paul Kehle  
@pgkehle ([twitter](https://twitter.com/pgkehle), [github](https://github.com/pgkehle), [linkedin](https://www.linkedin.com/in/pgkehle))

## For local development testing

```bash
rsync -av ~/code/ansible-openvpn/* ~/.ansible/roles/pgkehle.openvpn
```

### References

* https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-14-04
* https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-16-04
* https://help.ubuntu.com/14.04/serverguide/openvpn.html
* http://superuser.com/questions/457020/openvpn-only-route-a-specific-ip-addresses-through-vpn
* http://www.linuxfunda.com/2013/09/14/how-to-install-and-configure-an-open-vpn-with-nat-server-inside-aws-vpc/
* http://serverfault.com/questions/497438/how-to-reset-ubuntu-12-04-iptables-to-default-without-locking-oneself-out
* http://serverfault.com/questions/631037/how-to-route-only-specific-openvpn-traffic-through-a-openvpn-based-on-ip-filteri

* https://forums.openvpn.net/viewtopic.php?t=16862
* https://community.openvpn.net/openvpn/wiki/Hardening
* https://blog.g3rt.nl/openvpn-security-tips.html
* https://wiki.mozilla.org/Security/Server_Side_TLS
* https://www.privateinternetaccess.com/forum/discussion/3478/pfsense-openvpn-not-connected

```
TLS-DHE-RSA-WITH-AES-256-GCM-SHA384
TLS-DHE-RSA-WITH-AES-256-CBC-SHA256
TLS-DHE-RSA-WITH-AES-128-GCM-SHA256
TLS-DHE-RSA-WITH-AES-128-CBC-SHA256

commands:
openvpn --show-digests
openvpn --show-tls
openssl x509 -text -in ca.crt
```
