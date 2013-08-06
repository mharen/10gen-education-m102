# My Notes - Week 4

## Introduction to replication

* Sharding and replication
* Replication approachs: statement-based vs. binary
    * statement-based:
        * + likely to have smaller payloads to replicate
        * - each node has to do it's own querying to figure out what to do
    * binary:
        * + saves query time by running it once and replicating the effects
        * - requires the underlying data model be exact, which can make having multiple versions of the software on different nodes impossible
    * mongodb takes a hybrid approach
        * e.g. on the primary: if `db.foo.remove({ age: 30 })` affects 100 docs, gets replicated as 100 statements, each deleting by `_id`

## Replica Sets Overview
    * automatic failover
    * automatic node recovery after failover
    * writes always go to the primary, reads _can_ go to secondaries (see `read preference`)
    
## Replica Set demo
    * the node that initiates the replica set *can* have data, which will be seeded to the other nodes
    * startup commands:
        * mongod --replSet abc --dbpath 1 --port 27001 --oplogSize 50
    * rs config:
    
    cfg = {
        _id: 'abc',
        members: [
          { _id: 0, host: "machine-name:27001" },
          { _id: 1, host: "machine-name:27002" },
          { _id: 2, host: "machine-name:27003" }
        ]
    }
    
    * `rs.initiate(cfg)`
    * config is stored as a single document, e.g. `db.system.replset.find().pretty()`
    * rs.help()
    
## Replica Sets - getting information
* http interface: http://servername:(mongod port + 1000)
* `db.printReplicationInfo()`