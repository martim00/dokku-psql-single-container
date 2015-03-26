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
    psql:console     <app>                     Launch a postgresql console for a given app
    psql:env         <app>                     Get generated environment variables for <app>
    psql:url         <app>                     Get DATABASE_URL for <app>
    psql:create      <app>                     Create a Postgresql database
    psql:delete      <app>                     Delete specified Postgresql database
    psql:link        <app> <another_app>       Give environment variable of database of <app> to <another_app>
    psql:unlink      <another_app>             Unlink <another_app> to a database
    psql:dump        <app> > <filename.dump>   Dump database to PG dump format
    psql:restore     <app> < <filename.*>      Restore database from any format exported by pg_dump
    psql:admin_console                         Launch a postgresql console as admin user
    psql:restart                               Restart the Postgresql docker container and linked app
    psql:start                                 Start the Postgresql docker container if it isn't running
    psql:stop                                  Stop the Postgresql docker container
    psql:status                                Shows status of Postgresql
    psql:list                                  List all databases
```

## Info
This plugin adds following environment variables to your app automatically:

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
..or..
$ ssh dokku@server psql:start      # Client side

```

### Stop PostgreSQL:
```
$ dokku psql:stop                  # Server side
..or..
$ ssh dokku@server psql:stop       # Client side

```

### Restart PostgreSQL:
```
$ dokku psql:restart               # Server side
..or..
$ ssh dokku@server psql:restart    # Client side

```

### Create a new database:
```
$ dokku psql:create foo            # Server side
..or..
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


## Link
You can link a database to an application :

- Create database:
```
$ dokku psql:create foo
```
- Push another_app to dokku and link it to foo with:
```
$ dokku psql:link foo another_app
```
- Environment variables for database foo will be available in another_app

## Unlink
```
$ dokku psql:unlink another_app # Server side
```
