getMongoData.js
---------------

### Description

`getMongoData.js` is a utility for gathering information about how a running
MongoDB deployment has been configured and for gathering statistics about its
databases, collections, indexes, and shards.

For sample output, see [getMongoData.log](sample/getMongoData.log).

### Usage

To execute on a locally-running `mongod` or `mongos` on the default port (27017)
without authentication, run:

    mongo --quiet --norc getMongoData.js > getMongoData-output.json

To execute on a remote `mongod` or `mongos` with authentication (see the next
section for the minimum required permissions), run:

    mongo HOST:PORT/admin -u ADMIN_USER -p ADMIN_PASSWORD --quiet --norc getMongoData.js > getMongoData-output.json

### More Details

`getMongoData.js` is JavaScript script which must be run using the `mongo` shell
against either a `mongod` or a `mongos`.

Minimum required permissions (see [MongoDB Built-In Roles](https://docs.mongodb.com/manual/reference/built-in-roles)):
* A database user with the `backup`, `readAnyDatabase`, and `clusterMonitor` roles. These are essentially read-only roles except the [backup](https://docs.mongodb.com/manual/reference/built-in-roles/#backup-and-restoration-roles) role allows writes to two MongoDB system collections - `admin.mms.backup` and `config.settings`. The `backup` role is necessary in order for the script to output the number of database users and user-defined roles configured.
* A root/admin database user may be used as well.

Example command for creating a database user with the minimum required permissions:

```
db.getSiblingDB("admin").createUser({
    user: "ADMIN_USER",
    pwd: "ADMIN_PASSWORD",
    roles: [ "backup", "readAnyDatabase", "clusterMonitor" ]
  })
```

The most notable methods, commands, and aggregations that this script runs are listed below.

**Server Process Config & Stats**
* [serverStatus](https://docs.mongodb.com/manual/reference/command/serverStatus)
* [hostInfo](https://docs.mongodb.com/manual/reference/command/hostInfo)
* [getCmdLineOpts](https://docs.mongodb.com/manual/reference/command/getCmdLineOpts)
* [buildInfo](https://docs.mongodb.com/manual/reference/command/buildInfo)
* [getParameter](https://docs.mongodb.com/manual/reference/command/getParameter/)

**Replica Set Config & Stats**
* [rs.conf()](https://docs.mongodb.com/manual/reference/method/rs.conf/)
* [rs.status()](https://docs.mongodb.com/manual/reference/method/rs.status/)
* [db.getReplicationInfo()](https://docs.mongodb.com/manual/reference/method/db.getReplicationInfo)
* [db.printSecondaryReplicationInfo()](https://docs.mongodb.com/manual/reference/method/db.printSecondaryReplicationInfo)

**Database Users and User-Defined Roles (the count only)**
* [db.system.users.count()](https://docs.mongodb.com/manual/reference/system-users-collection/) in the "admin" database
* [db.system.roles.count()](https://docs.mongodb.com/manual/reference/system-roles-collection/) in the "admin" databases

**Database, Collection, and Index Config & Stats**
* [listDatabases](https://docs.mongodb.com/manual/reference/command/listDatabases/)
* [db.getCollectionNames()](https://docs.mongodb.com/manual/reference/method/db.getCollectionNames/)
* [db.stats()](https://docs.mongodb.com/manual/reference/method/db.stats/)
* [db.getProfilingStatus()](https://docs.mongodb.com/manual/reference/method/db.getProfilingStatus/)
* [db.collection.stats()](https://docs.mongodb.com/manual/reference/method/db.collection.stats/)
* [db.collection.getShardDistribution()](https://docs.mongodb.com/manual/reference/method/db.collection.getShardDistribution/)
* [db.collection.getIndexes()](https://docs.mongodb.com/manual/reference/method/db.collection.getIndexes/)
* [$indexStats](https://docs.mongodb.com/manual/reference/operator/aggregation/indexStats/)