# Getting Started

## Installation

* Install the repository RPM:   

  * `yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm`

* Install the client packages:

  * `yum install postgresql11`

* Optionally install the server packages:

  * `yum install postgresql11-server`

* Optionally initialize the database and enable automatic start:

  * ```shell
    /usr/pgsql-11/bin/postgresql-11-setup initdb
    systemctl enable postgresql-11
    systemctl start postgresql-11
    ```

## Basic Commands

* Create Database: `createdb`
* Remove Database: `dropdb`
* psql [mydb]     // accesss DB: mydb by psql.   if the db name is not given then it will default to your user account name. 
* mydb=> \h    //get help 
* mydb=> \q   // get out of psql

*we need to change to user 'postgres' before type these above commands*



# Server Setup and Operation

## Initial a Database Cluster

**Database Cluster**: A DB Cluster is a collection of databases that is managed by a single instance of a running database server.  In file system terms, it's a single directory under which all data will be stored.

* **Initial a database cluster**:
  * `initdb -D /user/local/pgsql/data`
  * or
  * `pg_ctl -D /user/local/pgsql/data`



## Starting the DB Server

`$ postgres -D /usr/local/pgsql/data`

Normally it is better to start postgres in the background. For this, use the usual Unix shell syntax:

`$ postgres -D /usr/local/pgsql/data >logfile 2>&1 &`

# Server Configuration

## Parameter types:

* **Boolean**:  on off true false yes no 1 0
* **integer**: 0 1 2 3 4 5 ...
* **floating**:  2.3  4.5...
* **string or enum**:  String

## Parameter unit:

* **memory units**: kB  MB  GB  ...
* **time units**: ms  s  min h  d ...

*default units can be found by referencing `pg_settings.unit`*

##Setting parameters via Configuration File

`postgresql.conf`   (default path: /var/lib/pgsql/11/data/postgresql.conf) 

*if the configuration file is not in the default path, we can use `find / -name postgresql. conf` command to find the configuration file.

## Setting parameters via Command-line

`postgres -c log_connections=yes -c log_destination=’syslog’`

**we can also examine the parameters by check the virtual table `pg_settings`

​	`select * from pg_settings`

## Important Configuration in `postgresql.conf`

``data_directory (string)``

Speciﬁes the directory to use for data storage. This parameter can only be set at server start.

`config_file (string)`

Speciﬁes the main server conﬁguration ﬁle (customarily called postgresql.conf). This parameter can only be set on the postgres command line.

`hba_file (string)`

Speciﬁes the conﬁguration ﬁle for host-based authentication (customarily called pg_hba.conf). This parameter can only be set at server start.

`ident_file (string)`

Speciﬁes the conﬁguration ﬁle for user name mapping (customarily called pg_ident.conf). This parameter can only be set at server start. See also Section 19.2.

`external_pid_file (string)`

Speciﬁes the name of an additional process-ID (PID) ﬁle that the server should create for use by server administration programs. This parameter can only be set at server start.



# Client Authentication

* **Configuration File**: `pg_hba.conf`

* Config Record:

  * ```shell
    # local      DATABASE  USER  METHOD  [OPTIONS]
    # host       DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
    # hostssl    DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
    # hostnossl  DATABASE  USER  ADDRESS  METHOD  [OPTIONS]
    ```

    *these configure records like ACL: if one record is chosen and the authentication fails, subsequent records are not considered. If no record matches, access is denied.  Since the pg_hba.conf records are examined sequentially for each connection attempt, the order of the records is signiﬁcant.*




