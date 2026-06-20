# Storage And Database
# AWS Storage and Database Services

# AWS Storage and Database Services
#### Storage Services


| Service Type | AWS Service | Description |
|---|---|---|
| [Object Storage](storage_services/object_storage%28s3%29/s3.md) | Amazon S3 | Highly scalable object storage for any type of data. |
| Block Storage | Amazon EBS | High-performance block storage for EC2 instances. |
| File Storage | Amazon EFS / Amazon FSx | Managed file storage for shared access. |
| Archive Storage | Amazon S3 Glacier | Low-cost archive storage for long-term data retention. |
| Hybrid Storage | AWS Storage Gateway | Connects on-premises storage systems with AWS cloud storage. |




#### Database Services


# Amazon AWS Database Services

| Database Type              | AWS Service                        | Supported Engine / Compatibility                                     | Main Purpose                                                         | Common Use Cases                                                              |
| -------------------------- | ---------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Relational Database        | Amazon RDS                         | MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server and IBM Db2 | Managed relational database service                                  | ERP systems, CRM systems, websites and transactional applications             |
| Relational Database        | Amazon Aurora                      | MySQL-compatible and PostgreSQL-compatible                           | High-performance cloud-native relational database                    | Enterprise applications, SaaS platforms and high-traffic websites             |
| Distributed SQL Database   | Amazon Aurora DSQL                 | PostgreSQL-compatible                                                | Serverless distributed SQL database                                  | Globally distributed and highly available applications                        |
| Custom Relational Database | Amazon RDS Custom                  | Oracle and Microsoft SQL Server                                      | Managed database with operating-system-level access                  | Legacy and customised enterprise applications                                 |
| Simple Relational Database | Amazon Lightsail Managed Databases | MySQL and PostgreSQL                                                 | Simple managed database for small applications                       | Small websites, blogs, prototypes and learning projects                       |
| Key-Value Database         | Amazon DynamoDB                    | AWS proprietary NoSQL engine                                         | Serverless key-value and document database                           | Shopping carts, gaming, mobile applications and user sessions                 |
| Document Database          | Amazon DocumentDB                  | MongoDB-compatible API                                               | Stores JSON-like documents                                           | Product catalogues, content-management systems and user profiles              |
| Wide-Column Database       | Amazon Keyspaces                   | Apache Cassandra-compatible                                          | Serverless wide-column database                                      | IoT systems, event data, fleet management and Cassandra migrations            |
| Graph Database             | Amazon Neptune                     | Property Graph and RDF                                               | Stores highly connected data and relationships                       | Social networks, recommendation systems, fraud detection and knowledge graphs |
| Time-Series Database       | Amazon Timestream for InfluxDB     | InfluxDB-compatible                                                  | Stores time-stamped data                                             | IoT monitoring, application metrics, financial data and patient monitoring    |
| In-Memory Database         | Amazon MemoryDB                    | Valkey and Redis OSS compatible                                      | Durable in-memory database                                           | Real-time applications, leaderboards and low-latency services                 |
| In-Memory Cache            | Amazon ElastiCache                 | Valkey, Redis OSS and Memcached                                      | Improves application performance by caching frequently accessed data | Session storage, query caching and application acceleration                   |
| Data Warehouse             | Amazon Redshift                    | SQL-based analytical engine                                          | Large-scale data warehousing and analytics                           | Business intelligence, reporting, dashboards and analytical queries           |
| Self-Managed Database      | Amazon EC2                         | Any supported database installed by the user                         | Provides virtual servers for running databases                       | MongoDB, Neo4j, Cassandra, PostgreSQL, MySQL and other custom databases       |
| Container-Based Database   | Amazon ECS or Amazon EKS           | Any containerised database                                           | Runs databases using containers                                      | Development environments, testing and specialised database deployments        |

## Simple AWS Database Selection Guide

| Requirement                                   | Recommended AWS Service            |
| --------------------------------------------- | ---------------------------------- |
| Standard SQL database                         | Amazon RDS                         |
| High-performance MySQL or PostgreSQL database | Amazon Aurora                      |
| Globally distributed serverless SQL database  | Amazon Aurora DSQL                 |
| Highly scalable key-value database            | Amazon DynamoDB                    |
| MongoDB-compatible document database          | Amazon DocumentDB                  |
| Apache Cassandra-compatible database          | Amazon Keyspaces                   |
| Graph and relationship-based database         | Amazon Neptune                     |
| Time-series and monitoring database           | Amazon Timestream for InfluxDB     |
| Durable in-memory database                    | Amazon MemoryDB                    |
| Application caching                           | Amazon ElastiCache                 |
| Data warehouse and business intelligence      | Amazon Redshift                    |
| Simple database for a small website           | Amazon Lightsail Managed Databases |
| Fully customised or self-managed database     | Amazon EC2                         |

> **Note:** Amazon S3 is an object-storage service, not a traditional database. However, it is commonly used as a data lake for storing structured, semi-structured and unstructured data.
|


