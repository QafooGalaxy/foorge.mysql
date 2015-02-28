Qafoo MySQL
===========

MySQL role for Ansible

Partly ported from [ANXS/mysql](https://github.com/ANXS/mysql) role.

Requirements
------------

- Ubuntu Server
- Servers with this role must have the Ansible group `mysql`

Environment Variables
---------------------

The MySQL role provides resources to application and therefore registers
an environment variable for each one based on the database name:

    export MYSQL_CONNECTION_{DBNAME}=mysql://username:password@host:port/dbname

Example:

    export MYSQL_CONNECTION_TESTING=mysql://testing:foobar@localhost:3306/testing

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

Example Playbook
----------------

Minimal MySQL configuration to setup with root password and one database with a user of the same name + password.

    - hosts: database
      roles:
        - role: "qafoo.mysql"
          mysql_root_password: "{{ lookup('password', 'credentials/mysql_root') }}"
          mysql_databases:
            - name: "testing"
              password: "{{ lookup('password', 'credentials/mysql_app') }}"

The ``lookup('password')`` calls create a folder named ``credentials/`` next to
your Ansible provisioning playbook containing the mysql root and application
passwords.

License
-------

Licensed under the MIT License. See the LICENSE file for details.

Author Information
------------------

Benjamin Eberlei <benjamin@qafoo.com>
https://github.com/QafooGalaxy
