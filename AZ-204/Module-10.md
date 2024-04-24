# Explore Azure Cosmos DB

## Key Benefits of Azure Cosmos DB

- Low latency: Guaranteed sub-10ms reads and writes at the 99th percentile.
- Elastic scalability: Scale throughput and storage independently.
- Global distribution: Distribute data globally for low-latency access.
- High availability: 99.999% read/write availability for multi-region configurations.
- Consistent data model: Support for multiple data models with defined consistency.
- Automatic indexing: No manual index management required.
- Multi-master replication: Unlimited elastic write and read scalability.

## Azure Cosmos DB Resource Hierarchy

- Account: Fundamental unit of global distribution and high availability.
- Databases: Logical namespaces for organizing data.
- Containers: Scalable units for throughput and storage.
  - Dedicated or shared provisioned throughput.
- Items: Automatically partitioned and indexed.

You can create up to 50 Cosmos DB accounts per Azure subscription (soft limit).

## Choosing the Right Consistency Level in Azure Cosmos DB

- You can configure the default consistency level on your Azure Cosmos DB account, which applies to all databases and containers under that account.
- Azure Cosmos DB guarantees that 100% of read requests will meet the consistency guarantee for the chosen consistency level.

### Consistency Level Guarantees

1. **Strong Consistency**:
   - Offers a linearizability guarantee, ensuring reads return the most recent committed version of an item.
   - Clients never see an uncommitted or partial write.

2. **Bounded Staleness Consistency**:
   - Reads honor the consistent prefix and lag behind writes by at most:
     - "K" versions (updates)
     - "T" time interval
   - Whichever is reached first
   - Minimum values for K and T depend on whether the account is single-region or multi-region.

3. **Session Consistency**:
   - Within a single client session, reads honor the consistent prefix, monotonic reads, monotonic writes, read-your-writes, and write-follows-reads.

4. **Consistent Prefix Consistency**:
   - Updates made as a batch within a transaction are always returned consistent to the transaction.
   - Individual document writes see eventual consistency.

5. **Eventual Consistency**:
   - Provides the weakest guarantee, with no ordering promise for reads.
   - In the absence of further writes, the replicas will eventually converge.
   - Ideal for use cases that don't require strict ordering, such as counting retweets or likes.

By understanding these consistency levels and their tradeoffs, you can make an informed decision on the right consistency level for your Azure Cosmos DB-powered application.

## Supported APIs in Azure Cosmos DB

Azure Cosmos DB offers multiple database APIs to model data using various data models:

- **SQL (Core) API**: Native API for document-oriented data
- **API for MongoDB**: Wire-protocol compatible with MongoDB
- **API for PostgreSQL**: Managed PostgreSQL service with distributed tables
- **API for Apache Cassandra**: Wire-protocol compatible with Cassandra
- **API for Apache Gremlin**: For graph-oriented data
- **API for Table**: For key-value (table) data

### Considerations when Choosing an API

- **SQL (Core) API**: Best end-to-end experience, supports SQL querying
- **API for MongoDB, PostgreSQL, Cassandra, Gremlin**: Leverage existing ecosystem and skills
- **API for Table**: Overcomes limitations of Azure Table Storage

Key factors to consider:

- Existing application architecture and data model
- Desire to use open-source ecosystem and skills
- Need for specific data modeling capabilities (document, graph, column-family, etc.)

The choice of API depends on your application requirements and the tradeoffs you're willing to make.

## Discovering Request Units in Azure Cosmos DB

In Azure Cosmos DB, you pay for the throughput you provision and the storage you consume on an hourly basis. Throughput must be provisioned to ensure sufficient system resources are available for your database.

### Request Units (RUs)

- Request units (RUs) represent the system resources (CPU, IOPS, memory) required to perform database operations.
- The cost of a database operation is normalized and expressed in RUs.
- A point read (fetch single item by ID and partition key) of a 1KB item costs 1 RU.
- All other database operations are assigned a cost in RUs, regardless of the API used.

### Provisioning Throughput

Azure Cosmos DB offers three modes for provisioning throughput:

1. **Provisioned Throughput Mode**:
   - Provision RUs per second in increments of 100 RUs.
   - Adjust provisioned throughput programmatically or in the Azure portal.
   - Provision throughput at the container or database level.

2. **Serverless Mode**:
   - No need to provision throughput upfront.
   - Get billed for the RUs consumed at the end of the billing period.

3. **Autoscale Mode**:
   - Automatically and instantly scale throughput (RUs) based on usage.
   - Suitable for mission-critical workloads with variable traffic patterns.
   - Maintains performance and scale SLAs.

Understanding RUs and throughput provisioning is crucial for managing the cost and performance of your Azure Cosmos DB-powered applications.
