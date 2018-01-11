# mongo-docker-compose
This repository provides a fully sharded mongo environment using docker-compose and local storage.

The MongoDB environment consists of the following docker containers. It's a simplified version of original [upstream](https://github.com/singram/mongo-docker-compose). 

 - **mongosrs(1-2)n1**: Mongod data server with two replica sets containing 1 nodes each
 - **mongocfg1**: Stores metadata for sharded data distribution 
 - **mongos1**: Mongo routing service to connect to the cluster through

In addition, also creates an admin user `mongo-admin/mongo-admin`.

## Caveats

 - This is designed to have a minimal disk footprint at the cost of durability.
 - This is designed in no way for production but as a cheap learning and exploration vehicle.

## Usage

### Check out the repository

    git clone git@github.com:singram/mongo-docker-compose.git
    cd mongo-docker-compose


### Setup Cluster
This will pull all the images from [Docker index](https://index.docker.io/u/jacksoncage/mongo/) and run all the containers.

    docker-compose up

Please note that you will need docker-compose 1.4.2 or better for this to work due to circular references between cluster members.
You will need to run the following *once* only to initialize all replica sets and shard data across them

    ./initiate

You should now be able connect to mongos1 and the new sharded cluster via mongCLI or any other client like Robo 3T

    mongo localhost:27017 -u mongo-admin -p mongo-admin --authenticationDatabase admin

## Persistent storage
Data is stored at `./data/` and is excluded from version control. Data will be persistent between container runs. To remove all data `./reset`

## TODO

 - Add local Ops Mananger
 - Implement authentication across cluster.  http://docs.mongodb.org/manual/tutorial/deploy-replica-set-with-auth/

## Built upon
 - [Docker-compose wait to start](http://brunorocha.org/python/dealing-with-linked-containers-dependency-in-docker-compose.html)
 - [Bi directional docker commuication](http://abdelrahmanhosny.com/2015/07/01/3-solutions-to-bi-directional-linking-problem-in-docker-compose/)
 - [MongoDB Sharded Cluster by Sebastian Voss](https://github.com/sebastianvoss/docker)
 - [MongoDB](http://www.mongodb.org/)
 - [Mongo Docker ](https://github.com/jacksoncage/mongo-docker)
 - [DnsDock](https://github.com/tonistiigi/dnsdock)
 - [Docker](https://github.com/dotcloud/docker/)
