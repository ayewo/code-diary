## February 12, 2023
## The difference between RDS and Aurora

This article [Aurora vs. RDS: An Engineer’s Guide to Choosing a Database](https://github.com/ayewo/code-diary/new/main) did a good job of explain the key differences between Amazon RDS and Amazon Aurora:

| Trait | Amazon RDS | Amazon Aurora | 
|-------|-------------|--------------|
| Design | Managed-database | Managed-database and cloud-native |
| Node design | Same node for compute and storage | Different nodes for compute and storage |
| Cluster size | Up to 5 replicas | Up to 15 replicas |
| Database Flavors (Wire-compatibilty) | MySQL, PostgreSQL, MariaDB, SQL Server and Oracle | MySQL and Postgres only | 
