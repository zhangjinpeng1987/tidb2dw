# tidb2dw

A tool to replicate data changes from TiDB to Data Warehouse in real-time.

> **Note**
> Only support TiDB v7.1.0 or later, and only support Snowflake as target Data Warehouse now.

## Build

```bash
make build
```

## Getting Started

To replicate snapshot and incremental data of a TiDB Table to Snowflake:

```shell
export AWS_ACCESS_KEY_ID=<ACCESS_KEY>
export AWS_SECRET_ACCESS_KEY=<SECRET_KEY>
export AWS_SESSION_TOKEN=<SESSION_TOKEN>  # Optional

./tidb2dw snowflake full \
    --storage s3://my-demo-bucket/prefix \
    --table <database_name>.<table_name> \
    --snowflake.account-id <organization>-<account> \
    --snowflake.user <username> \
    --snowflake.pass <password> \
    --snowflake.database <database> \
    --snowflake.schema <schema> \

# Note that you may also need to specify these parameters:
#   --cdc.host x.x.x.x
#   --tidb.host x.x.x.x
#   --tidb.user <user>
#   --tidb.pass <pass>
# Use --help for details.
```

> **Warning**
> We do not support ddl replication yet. Any ddl operation will cause the incremental replication stop. You need to manually run the DDL on target table and then restart the incremental replication.
> Restart incremental replication without manually runing DDL on target table may cause data loss.
