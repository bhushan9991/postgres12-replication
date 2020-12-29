# Postgres 12.4 replication with Docker

PostgreSQL with built-in replication is a great choice for a reliable and scalable database engine. The setup is going to be a master instance that handles both read and write operations. The other is a slave instance that will only serve read requests and that is because writing data is a whole different story than reading them.

Let’s create [Dockerfile](./docker/postgres_master/Dockerfile) for master database.

And there is a [setup-master.sh](./docker/postgres_master/setup-master.sh) file which needs to be copied to the image which makes the Postgres ready for being a master in the replication process.

Now let’s create a [Dockerfile](./docker/postgres_slave/Dockerfile) for the slave database.

We need the gosu binary to execute the postgres as a root
We also need the iputils package to be able to ping the master.

The next step is to prepare the slave images to actually be slaves. We use a [docker-entrypoin.sh](./docker/postgres_slave/docker-entrypoin.sh) file which will be the first thing that docker executes upon creating the container.

Notes:
- On the first line, we check that this instance has already been set up or not by checking the PG_VERSION file in the PG_DATA the path so as not to do it on every startup of the container.
- We copy the replication user and password in the .pgpass so Postgres can access it.
- We start pinging the Master to make sure that it’s already up and running.

It’s almost done now! :)

Now, need to create a [docker-compose.yml](.docker-compose.yml) file to spin up our databases and build our containers.



Now let’s create a [.env](.env) file to store the variables Or you can just directly put the values in docker-compose.yml

Just keep in mind that we have put the Dockerfile and setup-master.sh file the postgres_master directory and the slave’s Dockerfile and docker-entrypoint.sh inside the postgres_slave directory.
Let’s run this setup:

docker-compose up
