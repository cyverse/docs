# Portal Database

This Database is used for CyVerse User portal Service.

## Initialize database

### Access Database

```bash
ssh root@DB_HOST.com
psql -U postgres
```

### Create required Database and User

See also [portal2/setup-database](https://gitlab.com/cyverse/portal2#setup-database)


```bash
# create user
create user portal_db_reader with password '********';

# create portal database with the owner portal_db_reader
create database portal with owner portal_db_reader;

## Grant user to member of postgres
# psql -U postgres
GRANT postgres TO portal_db_reader;
```

### Restore from dump
For this we have to download [portal.sql](https://gitlab.com/cyverse/portal2/-/blob/master/portal.sql) file.

```bash
# restore to user portal_db_reader and database portal
psql -U portal_db_reader -d portal -f portal.sql
```

### Populate Database

#### make sure session Table exist

```bash
CREATE TABLE public.session (
    sid character varying NOT NULL,
    sess json NOT NULL,
    expire timestamp(6) without time zone NOT NULL
);
ALTER TABLE public.session OWNER TO portal;
CREATE INDEX "IDX_session_expire" ON public.session USING btree (expire);
```

#### Import GRID institutions

##### download required grid file

[Official website](https://digitalscience.figshare.com/articles/dataset/GRID_release_2021-09-16/16685428?backTo=/collections/GRID/3812929)

```bash
# download grid
wget https://digitalscience.figshare.com/ndownloader/files/30895309

# unzip
unzip 30895309
```

##### import to the database
For imorting this grid file we will use the script from [portal2/scripts/import_grid_institutions.py](https://gitlab.com/cyverse/portal2/-/blob/master/src/scripts/import_grid_institutions.py).

```bash
./import_grid_institutions.py --host root@DB_HOST.com --user portal_db_reader --database portal grid.csv
```

#### Populate these Tables
**These sql files can be found [here](https://github.com/cyverse-austria/portal2-db)**.

```bash
psql -U portal_db_reader -d portal -f ./account_country.sql
psql -U portal_db_reader -d portal -f ./account_region.sql
psql -U portal_db_reader -d portal -f ./account_gender.sql
psql -U portal_db_reader -d portal -f ./account_occupation.sql
psql -U portal_db_reader -d portal -f ./account_ethnicity.sql
psql -U portal_db_reader -d portal -f ./account_fundingagency.sql
psql -U portal_db_reader -d portal -f ./account_awarechannel.sql
psql -U portal_db_reader -d portal -f ./account_researcharea.sql
```

## Extra 

### Give Admin privilege to a user

```sql
--update is_superuser
UPDATE account_user SET is_superuser = true WHERE username='USERNAME';

---update is_staff 
UPDATE account_user SET is_staff = true WHERE username='USERNAME';
```

### Verify User email

```sql
-- check if its verified
select has_verified_email from account_user where username='USERNAME';

--Verify email
UPDATE account_user SET has_verified_email = true WHERE username='USERNAME';
```


## Migrate Database

**TODO**
