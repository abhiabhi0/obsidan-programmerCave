### **Overview**

#### **Problem Statement**

A single-node deployment can become a bottleneck due to:

- Increased read workloads during peak times.
- The inability to handle complex analytical queries on OLTP databases.
- Lack of redundancy, risking data loss during failures.

#### **Goals of Master-Slave Replication**

1. **Increased Read Throughput**: Load balancing read queries across replicas.
2. **High Availability**: Redundant nodes for instant failover with no data loss.
3. **Data Distribution**: Backups and distributed read workloads.
4. **Real-time Sync**: Ensures replicas are updated as master data changes.

---

### **How Replication Works in MySQL**

1. **Binary Log (binlog)**: The master records data changes in a binary log.
2. **Relay Log**: Replicas fetch updates from the master’s binlog and store them in a relay log.
3. **IO Thread**: Fetches binlog updates from the master.
4. **SQL Thread**: Processes relay log updates to apply changes on the replica.
5. **Serialization**: Ensures transactions run sequentially on replicas.

---

### **Setup: Master-Slave Configuration**

#### **Blank Server Setup**

**Master Configuration (master.conf)**

```ini
[mysqld]
server-id = 1
log_bin = 1
binlog_format = ROW
binlog_do_db = scalerdb
default_authentication_plugin = mysql_native_password
```

**Slave Configuration (slave.conf)**

```ini
[mysqld]
server-id = 2
log_bin = 1
binlog_do_db = scalerdb
default_authentication_plugin = mysql_native_password
```

**Run MySQL Containers with Docker**

```bash
# Master Node
➜ docker run \
 --name masternode --rm \
 -e MYSQL_DATABASE=scalerdb \
 -e MYSQL_USER=master_user \
 -e MYSQL_PASSWORD=master_pwd \
 -e MYSQL_ROOT_PASSWORD=111 \
 -v $PWD/master.conf:/etc/mysql/conf.d/mysql.conf.cnf \
 mysql:8

# Slave Node
➜ docker run \
 --name slavenode --rm \
 -e MYSQL_DATABASE=scalerdb \
 -e MYSQL_USER=slave_user \
 -e MYSQL_PASSWORD=slave_pwd \
 -e MYSQL_ROOT_PASSWORD=111 \
 -v $PWD/slave.conf:/etc/mysql/conf.d/mysql.conf.cnf \
 mysql:8
```

**Create Replication User on Master**

```sql
CREATE USER 'replication_user'@'%' IDENTIFIED BY 'replication_password';
GRANT REPLICATION SLAVE ON *.* TO 'replication_user'@'%';
FLUSH PRIVILEGES;
SHOW MASTER STATUS;
```

Example Output:

```
+----------+----------+--------------+------------------+-------------------+
| File     | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+----------+----------+--------------+------------------+-------------------+
| 1.000003 |      851 | scalerdb     |                  |                   |
+----------+----------+--------------+------------------+-------------------+
```

**Configure Slave**

```sql
CHANGE MASTER TO \
   MASTER_HOST='172.17.0.2', \
   MASTER_USER='replication_user', \
   MASTER_PASSWORD='replication_password', \
   MASTER_LOG_FILE='1.000003', \
   MASTER_LOG_POS=851;
START SLAVE;
```

---

### **Replication Types**

#### **Synchronous Replication**

- **Process**: Master waits for all replicas to confirm writes before acknowledging the client.
- **Pros**: Ensures consistency and fault tolerance.
- **Cons**: Increased latency; blocked writes if a replica is unavailable.
- **Best Use Case**: Single synchronous replica for high availability.

#### **Asynchronous Replication**

- **Process**: Master acknowledges writes immediately without waiting for replicas.
- **Pros**: Low latency; high throughput.
- **Cons**: Risk of data loss if the master crashes before replication.
- **Best Use Case**: Scenarios prioritizing performance over strict consistency.

---

### **Replication Formats**

#### **Statement-Based Replication**

- Logs SQL queries executed on the master.
- **Pros**: Compact logs with low bandwidth usage.
- **Cons**: Issues with non-deterministic functions (e.g., `NOW()`, `UUID()`).

#### **Row-Based Replication**

- Logs actual data changes at the row level.
- **Pros**: Accurate replication; avoids non-deterministic issues.
- **Cons**: Larger logs for bulk updates.

#### **Mixed Replication**

- Dynamically switches between statement and row-based replication.
- **Best Use Case**: Flexible environments with varying workloads.

---

### **AWS Multi-AZ Deployment**

1. **Enable Multi-AZ**: Ensures high availability with automatic failover.
2. **Read Replicas**: Use read replicas to scale reads.
3. **Failover Testing**: Simulate a failover to verify availability and consistency.

---

### **Advanced Topics**

#### **GTID (Global Transaction ID)**

- Simplifies replication management and failover by uniquely identifying each transaction.

#### **Replication Lag**

- **Causes**: Network latency, heavy master load, or slow SQL thread on the replica.
- **Mitigation**: Optimize queries; use faster storage.

#### **Custom Replication Pipelines**

- Use tools like Debezium and Kafka for real-time change data capture (CDC).
- Example: Sync MySQL (master) to Postgres (slave) for OLAP workloads.

#### **Load Balancing Reads**

- Tools: HAProxy, ProxySQL.
- Strategy: Distribute read queries across replicas.

---
https://docs.google.com/document/d/1w0dCLJgqfnC8ylWCLCj3yCVkLwfBTvx2QuVTIUKPtAI/edit?tab=t.0
### **References**

1. [DigitalOcean: Setting up MySQL Replication](https://www.digitalocean.com/community/tutorials/how-to-set-up-replication-in-mysql)
2. [Shashwot Risal: Load Balancing MySQL](https://shashwotrisal.medium.com/load-balancing-mysql-servers-using-proxysql-ff5ab6d1d4ea)
3. [Debezium Visual Introduction](https://medium.com/event-driven-utopia/a-visual-introduction-to-debezium-32563e23c6b8)
4. Kleppmann’s "Designing Data-Intensive Applications" (Chapter 5).