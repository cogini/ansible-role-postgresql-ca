# PostgreSQL CA

This Ansible role manages a CA and server SSL certs for PostgreSQL servers.

# Configuration

Passphrase for the CA's private key. Set it in a vault variable. 

    postgresql_ssl_server_ca_pass

Hostname of server

    postgresql_ssl_hostname_fqdn: db.example.com

Base DN for certificates

    postgresql_ssl_subj_base: "/C=US/O=Example"

Target location of certs. By default, this role puts the certs on the server
under a new directory, `/etc/ssl/postgresql`, owned by root. 

    postgresql_ssl_dir: /etc/ssl/postgresql
    postgresql_ssl_user: root
    postgresql_ssl_group: postgres

You will then need to set variables `ssl_cert_file`, `ssl_key_file`,
and `ssl_ca_file` in the PostgreSQL config pointing to them

You can also put them under the PostgreSQL data dir, owned by postgres. That's
the default location for PostgreSQL.

    postgresql_ssl_dir: /var/lib/pgsql/10/data
    postgresql_ssl_user: postgres
    postgresql_ssl_group: postgres

See `config/main.yml` for more unusual vars.

# Example Playbook

Here is a typical playbook:

```yaml
- name: Configure Posst
  hosts: '*'
  become: true
  vars:
    # Configuration for postgresql_ssl
    postgresql_ssl_subj_base: "/C=US/O=Example"
    postgresql_ssl_server_ca_pass: "mysecret" # set this in vault
    postgresql_ssl_hostname_fqdn: db.example.com

    # Configuration for ANXS.postgresql
    postgresql_ssl: on
    postgresql_ssl_cert_file: "/etc/ssl/postgresql/server.crt"
    postgresql_ssl_key_file: "/etc/ssl/postgresql/server.key"
    postgresql_ssl_ca_file: "/erc/ssl/postgresql/root.crt"
  roles:
    - postgresql-ssl
    - ANXS.postgresql
```

## License

Apache 2.0

## Author

Jake Morrison at [Cogini](http://www.cogini.com/)
