## Possible reasons of getting deleted data in cassandra database

* Tombstones Not Yet Compacted - When data is deleted in Cassandra, it is not immediately removed—instead, a tombstone (a marker indicating deletion) is written. These tombstones are not immediately removed from disk. They're cleared during compaction. Stale data might still be returned during queries if QUORUM or lower consistency level on reads is being used or a node missed a delete write

* Hinted Handoff or Node Down During Delete - If a node was down during the delete, it will not have the tombstone. Upon coming back online, unless repair is run or hinted handoff succeeds, it may serve stale data.

* Inconsistent Consistency Levels - If writes (deletes) are done at a lower consistency level such as 'ONE', and reads are done at an equally or less strict level, that a read from a replica may serve stale data that did not get the delete yet. Furthermore, cassandra does not guarantee linearizability unless writes and reads use QUORUM or ALL.


## To avoid serving of stale data following steps can be taken.

1. Use of proper consistency levels - Using QUORUM or ALL for both reads and writes (including deletes). The ensure majority of the replicas agrees on a data.


2. Run Regular Repairs - Use 'nodetool repair' regularly, which synchronizes data across replicas and ensures deleted data is marked across all nodes.


3. Monitor Cluster Health - Ensure no nodes are down during deletes or writes. Use monitoring tools such as Prometheus and  Grafana  to watch for replication lag, failed writes etc.
