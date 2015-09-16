# dokku-psql-single-container

dokku-psql-single-container is a plugin for [dokku][dokku] that provides a Postgresql server in a single container for your applications.

It uses the official Postgresql docker image (version 9.3) by default but you can provide an alternative image.

This version is compatible with dokku 0.3.26+.

## Installation

```
git clone https://github.com/Flink/dokku-psql-single-container /var/lib/dokku/plugins/psql-sc
dokku plugins-install
```

### Providing an alternative docker image for PostgreSQL

If you want to use an alternative image instead of the default `postgres:9.3`
one, you just have to set it in the global dokku config **before**
running `dokku plugins-install`.

```bash
dokku config:set --global PSQL_SC_IMAGE=postgres:9.4
```

### Setting a custom PSQL_ROOT

By default, this plugin stores its data in `$DOKKU_ROOT/.psql-sc`. If you want
to store it elsewhere, you have to set the `PSQL_SC_ROOT` in the global dokku
config. You should do it **before** running `dokku plugins-install`. **Note:** 
If you are using custom `PSQL_SC_ROOT` directory, make sure it is writable by 
`dokku` user.

```bash
dokku config:set --global PSQL_SC_ROOT=/my/custom/path
```

### Binding Postgresql port to another IP

By default, the container is not reachable from outside. The container can be
bound to an IP of your choice by setting `PSQL_SC_BIND_IP` to an IP in the
global dokku config.

```bash
dokku config:set --global PSQL_SC_BIND_IP=0.0.0.0
```

## Commands
```
$ dokku help
    psql:admin_console                              Launch a postgresql console as admin user
    psql:console     <app>                          Launch a postgresql console for <app>
    psql:create      <app>                          Create a Postgresql database for <app>
    psql:delete      <app>                          Delete Postgresql database for <app>
    psql:dump        <app> > <filename.dump>        Dump <app> database to PG dump format
    psql:link        <source> <target>              Link <source> app DB to <target> app
    psql:list                                       List all databases
    psql:restart                                    Restart the Postgresql docker container
    psql:restore     <app> < <filename.*>           Restore database to <app> from any non-plain-text format exported by pg_dump
    psql:start                                      Start the Postgresql docker container if it isn't running
    psql:status                                     Shows status of Postgresql
    psql:stop                                       Stop the Postgresql docker container
    psql:unlink      <app>                          Remove DB config for <app>
    psql:url         <app>                          Get DATABASE_URL for <app>
    psql:version                                    Output version of plugin
```

## Info
This plugin adds the following environment variables to your app automatically (they are available via `dokku config`):

* DATABASE\_URL
* DB\_HOST
* DB\_NAME
* DB\_PASS
* DB\_PORT
* DB\_TYPE
* DB\_USER
* POSTGRESQL\_URL

## Usage

### Start PostgreSQL:
```
$ dokku psql:start                 # Server side
$ ssh dokku@server psql:start      # Client side
```

### Stop PostgreSQL:
```
$ dokku psql:stop                  # Server side
$ ssh dokku@server psql:stop       # Client side
```

### Restart PostgreSQL:
```
$ dokku psql:restart               # Server side
$ ssh dokku@server psql:restart    # Client side
```

### Create a new database for an existing app:
```
$ dokku psql:create foo            # Server side
$ ssh dokku@server psql:create foo # Client side
```

### Dump database:
```
$ dokku psql:dump foo > filename.dump # Server side
```

### Restore database from dump:
```
$ dokku psql:restore foo < filename.dump # Server side
```

For plain-text format, you should do this instead:

```shell
$ dokku psql:console < dump.sql
```

### Copy database foo to database bar using pipe:
```
$ dokku psql:dump foo | dokku psql:restore bar # Server side
```

### Link an existing DB to another application
```
$ dokku psql:link app_with_db target_app # Server side
```

### Unlink an application
```
$ dokku psql:unlink foo # Server side
```

If you inadvertently unlink an app owning a DB, there is a very easy way to
recover the configuration vars:

```
$ dokku psql:link foo foo # Server side
```

## Known issues

When upgrading Dokku to a newer version, owner of `/home/dokku/.psql-sc/data` will be changed to `dokku`. It can causes errors with the container.

You’ll have to restore the previous owner by issuing the following command:
```
$ chown 999 -R /home/dokku/.psql-sc/data
```

## Acknowledgements

This plugin is based originally on the [one by Olivier Hardy](https://github.com/ohardy/dokku-psql).

## License

This plugin is released under the MIT license. See the file [LICENSE](LICENSE).

[dokku]: https://github.com/progrium/dokku
