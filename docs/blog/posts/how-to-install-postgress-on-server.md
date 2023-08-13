---
date: 2020-12-20
authors: [nadeem]
title: How to install and setup PostgreSQL on Ubuntu Server
description: >
  The right way to quickly install and set up the PostgreSQL database.
categories:
  - Linux
  - Ubuntu
  - PostgreSQL
---

Hey, this is a tutorial to keep a note for myself and others. After countless stupid tries, I think I have finally understood the right to install and set up the PostgreSQL database.

### 1 - Install PostgreSQL

To install the PostgreSQL we need to type the following command.

```bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib
```

### 2 - Creating roles and databases

By default, Postgres uses a concept called “roles” to handle authentication and authorization. These are, in some ways, similar to regular Unix-style accounts, but Postgres does not distinguish between users and groups and instead prefers the more flexible term “role”.

Upon installation, Postgres is set up to use ident authentication, meaning that it associates Postgres roles with a matching Unix/Linux system account. If a role exists within Postgres, a Unix/Linux username with the same name can sign in as that role.

The installation procedure created a user account called Postgres that is associated with the default Postgres role. To use Postgres, you can log into that account. There are a few ways to utilize this account to access Postgres.

Switching over to Postgres account

```bash
sudo -i -u postgres
```

You can now access the Postgres prompt by typing

```bash
psql
```

### 3 - Creating a new role

A Postgres Database with a custom user account can be created by the following command in the SQL prompt

```bash
postgres-# CREATE ROLE your_username WITH LOGIN CREATEDB ENCRYPTED PASSWORD 'your_password';
```

Above command will create a new role with privileges to log in and create databases. [_Read more_](https://www.postgresql.org/docs/11/sql-createrole.html)

### 4 - Create a Linux user and access the database

If you don't have a Linux user with the role name you specified above then we need to create the user.

For creating the user following command can be used

```bash
sudo adduser <role_name>
su - <user_name>
```

Now we need to create the default database which will be used to access the database because the Postgres authentication system assumes by default that for any role used to log in, that role will have a database with the same name which it can access.

Hence, we need to create a database without a role name. Make sure you're logged in with the Linux user you want to create a database with

```bash
createdb <user_name>
```

Now, we can create any database we want by using the above command and access the psql
