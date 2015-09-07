# Ansible Role: Redis

[![Build Status](https://travis-ci.org/geerlingguy/ansible-role-redis.svg?branch=master)](https://travis-ci.org/geerlingguy/ansible-role-redis)

Installs [Redis](http://redis.io/) on RHEL/CentOS or Debian/Ubuntu.

## Requirements

On RedHat-based distributions, requires the EPEL repository (you can simply add the role `geerlingguy.repo-epel` to install ensure EPEL is available).

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    redis_port: 6379
    redis_bind_interface: 127.0.0.1

Port and interface on which Redis will listen. Set the interface to `0.0.0.0` to listen on all interfaces.

    redis_unixsocket: ''

If set, Redis will also listen on a local Unix socket.

    redis_timeout: 300

Close a connection after a client is idle `N` seconds. Set to `0` to disable timeout.

    redis_loglevel: "notice"
    redis_logfile: /var/log/redis/redis-server.log

Log level and log location (valid levels are `debug`, `verbose`, `notice`, and `warning`).

    redis_databases: 16

The number of Redis databases.

    # Set to an empty set to disable persistence (saving the DB to disk).
    redis_save:
      - 900 1
      - 300 10
      - 60 10000

Snapshotting configuration; setting values in this list will save the database to disk if the given number of seconds (e.g. `900`) and the given number of write operations (e.g. `1`) have occurred.

    redis_rdbcompression: "yes"
    redis_dbfilename: dump.rdb
    redis_dbdir: /var/lib/redis

Database compression and location configuration.

    redis_maxmemory: 0
    
Limit memory usage to the specified amount of bytes. When the memory limit is reached Redis will try to remove keys according to the eviction policy selected (see redis_maxmemory_policy).
Leave at 0 for no limit.

    redis_maxmemory_policy: "allkeys-lru"
    
How Redis will select what to remove when maxmemory is reached. You can select among five behaviors:

- "volatile-lru" > remove the key with an expire set using an LRU algorithm
- "allkeys-lru" > remove any key according to the LRU algorithm
- "volatile-random" > remove a random key with an expire set
- "allkeys-random" > remove a random key, any key
- "volatile-ttl" > remove the key with the nearest expire time (minor TTL)
- "noeviction" > don't expire at all, just return an error on write operations

<!-- comment to allow code block after bullet list -->
    redis_maxmemory_samples: "5"

Number of samples to use to approximate LRU. The default of 5 produces good enough results. 10 Approximates true LRU very closely but costs a bit more CPU. 3 is very fast but not very accurate.

    redis_appendonly: "no"

The appendonly option, if enabled, affords better data durability guarantees, at the cost of slightly slower performance.

    redis_appendfsync: "everysec"

Valid values are `always` (slower, safest), `everysec` (happy medium), or `no` (let the filesystem flush data when it wants, most risky).

    # Add extra include files for local configuration/overrides.
    redis_includes: []

Add extra include file paths to this list to include more/localized Redis configuration.

## Dependencies

None.

## Example Playbook

    - hosts: all
      roles:
        - { role: geerlingguy.redis }

## License

MIT / BSD

## Author Information

This role was created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
