## February 12, 2023
## The difference between RDS and Aurora

This article [Aurora vs. RDS: An Engineer’s Guide to Choosing a Database](https://www.lastweekinaws.com/blog/aurora-vs-rds-an-engineers-guide-to-choosing-a-database/) did a good job of explaining the key differences between [Amazon RDS](https://aws.amazon.com/rds/) and [Amazon Aurora](https://aws.amazon.com/rds/aurora/) and [Aurora Serverless](https://aws.amazon.com/rds/aurora/serverless). I decided to tabularize the differences to make it easier to scan:

| Trait | Amazon RDS | Amazon Aurora | Amazon Aurora Serverless |
|-------|-------------|--------------|--------------------------|
| Most cost-effective use case | Sustained workloads | Sustained and spiky workloads | Spiky workloads |
| Cluster split | Uses a symmetric split: compute and storage are integrated into each node | Uses an asymmetric split: compute and storage are split across different nodes | Similar to Aurora |
| Cache replication | Synchronous | Asynchronous | Similar to Aurora |
| Replication traffic | Flows between all nodes | Flows between storage nodes only | Similar to Aurora |
| Replica size | Up to 5 replicas | Up to 15 replicas | Similar to Aurora |
| Database flavors (Wire-compatibilty) | Five: MySQL, PostgreSQL, MariaDB, SQL Server and Oracle | Two: MySQL and Postgres only | Similar to Aurora |





