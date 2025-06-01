To shard the collection sanfrancisco.company_name based on _id floowing are the steps

Enable sharding on the sanfrancisco databse

```bash
sh.enableSharding("sanfrancisco")
```

Creating index on the shard key

```bash
db.company_name.createIndex({ _id: 1 })
```

This allows ranged based sharding

Lastly we will shard the collection

```bash
sh.shardCollection("sanfrancisco.company_name", { _id: 1 })
```