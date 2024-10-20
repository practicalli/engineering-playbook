# RDBMS Clients

Connect to a relational database instance and manage the structure and data within a database.


## Recommended Clients

- Command Line
- DBeaver - desktop application
- pgAdmin - desktop application and web app



## Manage Databases

Create Database – create a new database using CREATE DATABASE statement.

Alter Database – modify the features of an existing database using the ALTER DATABASE statement.

Rename Database – change the name of the database to a new one.

Drop Database – removes a database permanently using the DROP DATABASE statement.

Copy a Database – copy a database within a database server or from a server to another.

Get Database Object Sizes – introduce you to various handy functions to get the size of a database, a table, and indexes.


## Manage Schema

Schema – introduce the schema concept and explains how the schema search path works in PostgreSQL.

Create Schema – show you how to create a new schema in a database.

Alter Schema – rename a schema or changes its owner to the new one.

Drop schema – delete one or more schemas with their objects from a database.


## Managing Tablespaces

PostgreSQL tablespaces allow you to control how data stored in the file system. The tablespaces are very useful in many cases such as managing large tables and improving database performance.

Creating Tablespaces – introduce you to PostgreSQL tablespaces and shows you how to create tablespaces by using CREATE TABLESPACE statement.

Changing Tablespaces – show you how to rename, change owner and set the parameter for a tablespace by using ALTER TABLESPACE statement.

Delete Tablespaces – learn how to delete tablespaces by using DROP TABLESPACE statement.


## Section 4. Roles & Privileges
PostgreSQL represents accounts as roles. Roles that can log in called login roles or users. Roles that contain other roles are called group roles. In this section, you will learn how to manage roles and groups effectively.

Create role: introduce you to roles concept and show you how to create roles and groups by using the create role statement.

Grant – show you how to grant privileges on database objects to a role.

Revoke – guide you on revoking granted privileges on database objects from a role.

Alter role – show you how to use the alter role statement to modify the attributes of roles, rename roles, and set the configuration parameters.

Drop role – learn how to drop a role especially a role that has dependent objects.

Role membership – learn how to create group roles to better manage role membership.

List user roles – show you how to list all roles on the PostgreSQL server.


## Backup & Restore

PostgreSQL backup and restore tools including pg_dump, pg_dumpall, psql,  pg_restore and  pgAdmin to backup and restore databases.

Backup – introduce you to practical ways to back up your databases by using PostgreSQL backup tool including pg_dump and pg_dumpall.

Restore –  show you various ways to restore PostgreSQL databases by using psql and pg_restore tools.


> Backup and restore useful for local development with Docker Compose and volumes


## General

Reset Password – show you how to reset the forgotten password of the postgres user.
psql Commands – give you the most common psql command to help you query data from PostgreSQL faster and more effectively.
Describe Table – get information on a particular table.
Show Databases – list all databases in the current database server
Show Tables – show all tables in the current database.
