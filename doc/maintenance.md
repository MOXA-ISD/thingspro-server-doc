# DB Matainace

### Prerequsite

- Install postgresql client version 9.6
for Ubuntu 16.04:
```
add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
apt-get update
apt-get install postgresql-client-9.6
```

### Backup/Restore database

#### Backup

- Backup single table (devices)

```
export DATABASE_PORT=32767
export DATABASE_USERNAME=moxa
pg_dump -h localhost -p $DATABASE_PORT -U $DATABASE_USERNAME \
	--column-inserts --data-only --table=device devices > devices.pg
```

#### Restore

- Restore from pg file

```
psql -h localhost -p $DATABASE_PORT -U $DATABASE_USERNAME devices < devices.pg
```

