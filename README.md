Qafoo MySQL
===========

MySQL role for Ansible

Partly ported from [ANXS/mysql](https://github.com/ANXS/mysql) role.

Requirements
------------

- Ubuntu Server
- "qafoo.base" role

Resource
--------

The MySQL role provides resource/service at `mysql.service.consul` domain to
application and therefore registers an environment variable for each one based
on the database name:

    export MYSQL_CONNECTION_{NAME}=mysql://{NAME}:{PASSWORD}@{APP}.mysql.service.consul:3306/{DATABASE}

Example:

    export MYSQL_CONNECTION_TESTING=mysql://testing:foobar@{APP}.mysql.service.consul:3306/testing

Connection is available from Consul as well:

    $ curl http://localhost:8500/v1/kv/apps/{APP}/mysql/connection/{DBNAME}
    [
        {
            "CreateIndex":43,
            "ModifyIndex":44,
            "LockIndex":0,
            "Key":"mysql/testing",
            "Flags":0,
            "Value":"bXlzcWw6Ly90ZXN0aW5nOnRlc3RpbmdAbXlzcWwuc2VydmljZS5jb25zdWw6MzMwNi90ZXN0aW5n"
        }
    ]

Role Variables
--------------

    ---
    mysql_port: 3306
    mysql_bind_address: "127.0.0.1"
    mysql_language: '/usr/share/mysql/'
    mysql_root_password: ""
    mysql_databases: []
    mysql_allowed_hosts: ['localhost']

    mysql_key_buffer: '16M'
    mysql_max_allowed_packet: '128M'
    mysql_thread_stack: '192K'
    mysql_cache_size: 8
    mysql_myisam_recover: 'BACKUP'
    mysql_max_connections: 100
    mysql_table_cache: 64
    mysql_thread_concurrency: 10
    mysql_query_cache_limit: '1M'
    mysql_query_cache_size: '16M'
    mysql_character_set_server: 'utf8'
    mysql_collation_server: 'utf8_general_ci'
    mysql_mysqldump_max_allowed_packet: '128M'
    mysql_isamchk_key_buffer: '16M'

    mysql_innodb_file_per_table: 'innodb_file_per_table'
    mysql_innodb_flush_method: 'fdatasync'
    mysql_innodb_buffer_pool_size: '128M'
    mysql_innodb_flush_log_at_trx_commit: 1
    mysql_innodb_lock_wait_timeout: 50
    mysql_innodb_log_buffer_size: '1M'
    mysql_innodb_log_file_size: '5M'

    mysql_consul_service: 'mysql'

Example Playbook
----------------

Minimal MySQL configuration to setup with root password and one database with a user of the same name + password.

    - hosts: database
      roles:
        - role: "qafoo.mysql"
          mysql_root_password: "{{ lookup('password', 'credentials/mysql_root') }}"
          mysql_databases:
            - name: "testing"
              app: "testapp"
              database: "testing_production"
              password: "{{ lookup('password', 'credentials/mysql_app') }}"

The ``lookup('password')`` calls create a folder named ``credentials/`` next to
your Ansible provisioning playbook containing the mysql root and application
passwords.

The following keys exist in the `mysql_databases` list:

- `name` is the name of the connection and the user created. (Required)
- `password` is the password for the user that can access this database. (Required)
- `app` is the name of the application this database is registered for in Consul, defaulting to the connection/user name (Optional)
- `database` is the name of the database and defaults to the given connection `name` (Optional)
- `collation` is the database collation and defaults to `utf8_general_ci` (Optional)
- `encoding` is the database encoding and defaults to `utf8` (Optional)

License
-------

Licensed under the MIT License. See the LICENSE file for details.

Author Information
------------------

Benjamin Eberlei <benjamin@qafoo.com>
https://github.com/QafooGalaxy
