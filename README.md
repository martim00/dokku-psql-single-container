# PostgreSQL plugin for Dokku

Project: https://github.com/progrium/dokku

## Installation

```
git clone https://github.com/Flink/dokku-psql /var/lib/dokku/plugins/psql
dokku plugins-install
```


## Commands
```
$ dokku help
    psql:admin_console                              Launch a postgresql console as admin user
    psql:console     <app>                          Launch a postgresql console for <app>
    psql:create      <app>                          Create a Postgresql database for <app>
    psql:delete      <app>                          Delete Postgresql database for <app>
    psql:dump        <app> > <filename.dump>        Dump <app> database to PG dump format
    psql:list                                       List all databases
    psql:restart                                    Restart the Postgresql docker container
    psql:restore     <app> < <filename.*>           Restore database to <app> from any format exported by pg_dump
    psql:start                                      Start the Postgresql docker container if it isn't running
    psql:status                                     Shows status of Postgresql
    psql:stop                                       Stop the Postgresql docker container
    psql:url         <app>                          Get DATABASE_URL for <app>
```

## Info
This plugin adds the following environment variables to your app automatically (they are available via `dokku config`):

* DATABASE_URL
* DB_HOST
* DB_PORT
* DB_NAME
* DB_USER
* DB_PASS

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

### Copy database foo to database bar using pipe:
```
$ dokku psql:dump foo | dokku psql:restore bar # Server side
```
