## Postgresql 12 Cheatsheet - Ayan

# Starting Server and Other Local Information

1. `pg_ctlcluster 12 main start`

2. Log File at -  /var/log/postgresql/postgresql-12-main.log

3. Data Directory -  /var/lib/postgresql/12/main

4. Postgres Login - sudo -u postgres psql postgres

5. `\du`          - Lists all USERs and their ROLEs and GROUPs

6. `\dt`          - Lists all Tables

7. `psql -d ayandb -U ayan`  - Gets into Database ayandb under user ayan, doesn't have to specify user, if OS User by same name.

8. `pg_restore -U postgres -d dvdrental /path_to/dvdrental.tar` - Restores Database from a tar file. [ Has to create database under the user first ]

9. `CREATE DATABASE ayandb;` and `GRANT ALL PRIVILAGES ON DATABASE ayandb to ayan;` - Makes it possible to connect to pgAdmin/psql/wrappers.

# SQL Commands

1. `SELECT usename FROM pg_user;`                [ Lists all USERNAMEs ]

2. `CREATE USER librarian;`                      [ Create a USER ]

3.  


# Wrapper in Python - `psycopg2`

1. 

```
import psycopg2



```
