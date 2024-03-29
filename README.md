# ansible-role-postgresql
Ansible role to install PostgreSQL

# Supported Platforms
- RedHat 8
- RedHat 9

# Requirements
- The role supports the installation of PostgreSQL version 10 and higher. Default is 15.
- Make sure you have installed locale data for your language to be able to set `postgresql_locale` other than `en_US.UTF-8`. See `molecule/default/prepare.yml` for details.

# Example Inventory
```
[postgresql]
pg1.local
```

# Example Variables
```
postgresql_version: "14"
postgresql_listen_addresses:
  - "0.0.0.0"
  - "::"

postgresql_users:
  - name: test_user
    password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          39383334383236663665373965626434316131666564626362653765663638643664323961336166
          6363323037366638393635303131626261613332666666320a613565386662633762643631653165
          65663733653231346336363663643633643438663136316433613863326539316362316539656435
          3038343330616166340a306134363664616266316564343537633263656630376462616637646366
          3762

postgresql_databases:
  - name: test_db
    owner: test_user

postgresql_tables:
  - name: test_table
    owner: test_user
    db: test_db
    columns:
      - id bigserial primary key
      - num bigint
      - stories text

postgresql_extensions:
  - name: hstore
    db: test_db

postgresql_hba:
  - type: local
    method: peer
  - type: host
    databases:
      - test_db
    users:
      - test_user
    address: "0.0.0.0/0"
    method: scram-sha-256
  - type: host
    databases:
      - test_db
    users:
      - test_user
    address: "::/0"
    method: scram-sha-256
```

# Example Playbook
```
---
- hosts: postgresql
  roles:
    - role: dborisov.postgresql
```

# Role Variables
Please see `defaults/main.yml`

# Dependencies
None

# License
MIT