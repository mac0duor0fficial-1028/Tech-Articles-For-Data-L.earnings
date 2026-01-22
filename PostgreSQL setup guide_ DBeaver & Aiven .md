# PostgreSQL Setup Guide: DBeaver & Aiven

## Introduction

This comprehensive guide walks you through setting up PostgreSQL database connections using DBeaver (a free, powerful database management tool) and connecting to a cloud-hosted PostgreSQL database on Aiven.io. Whether you're a beginner or an experienced developer, this guide balances practical steps with technical insights.

**What you'll learn:**
- Installing and configuring DBeaver
- Understanding PostgreSQL connection parameters
- Creating and managing an Aiven PostgreSQL database
- Establishing secure connections between DBeaver and Aiven
- Best practices for database security and management

**Prerequisites:**
- A computer running Windows, macOS, or Linux
- Internet connection
- Basic understanding of databases (helpful but not required)

---

## Part 1: Understanding the Components

### What is PostgreSQL?

PostgreSQL is an open-source relational database management system (RDBMS) known for its reliability, feature robustness, and performance. Think of it as a highly organized filing system for your data, where information is stored in tables with rows and columns, similar to spreadsheets but far more powerful.

### What is DBeaver?

DBeaver is a universal database tool that provides a graphical interface to interact with databases. Instead of typing commands in a terminal, you can view tables, run queries, and manage your data through an intuitive visual interface. It's like having a control panel for your database.

### What is Aiven.io?

Aiven is a cloud platform that hosts and manages databases for you. Instead of setting up your own server, Aiven handles the infrastructure, backups, updates, and scaling. You get a fully managed PostgreSQL database accessible from anywhere.

---

## Part 2: Installing DBeaver

### Step 1: Download DBeaver

1. Visit the official DBeaver website: https://dbeaver.io/download/
2. Choose the appropriate version for your operating system:
   - **Windows**: Download the Windows installer (.exe)
   - **macOS**: Download the macOS installer (.dmg)
   - **Linux**: Download the .deb (Debian/Ubuntu) or .rpm (Fedora/RedHat) package

**Technical Note**: DBeaver Community Edition is free and sufficient for most use cases. The Enterprise Edition offers additional features like NoSQL database support and cloud storage integration.

### Step 2: Install DBeaver

**For Windows:**

1. Run the downloaded .exe file
2. Follow the installation wizard
3. Accept the license agreement
4. Choose installation directory (default is recommended)
5. Click "Install" and wait for completion

**For macOS:**

1. Open the downloaded .dmg file
2. Drag the DBeaver icon to your Applications folder
3. Open DBeaver from Applications (you may need to allow it in Security & Privacy settings)

**For Linux (Ubuntu/Debian):**

```bash
sudo dpkg -i dbeaver-ce_*.deb
sudo apt-get install -f  # Install dependencies if needed
```

### Step 3: Initial DBeaver Configuration

1. Launch DBeaver
2. On first launch, you'll see a welcome screen
3. Click "Create New Connection" or select "Database" → "New Database Connection" from the menu

**Technical Detail**: DBeaver uses JDBC (Java Database Connectivity) drivers to connect to databases. When you first connect to PostgreSQL, DBeaver will automatically download the necessary PostgreSQL JDBC driver.

---

## Part 3: Setting Up Aiven PostgreSQL

### Step 1: Create an Aiven Account

1. Navigate to https://aiven.io
2. Click "Sign Up" or "Get Started Free"
3. Register using your email or OAuth (GitHub, Google)
4. Verify your email address
5. Complete the account setup

**What You Get**: Aiven offers a free trial with credits to test their services. No credit card required initially for the trial.

### Step 2: Create a PostgreSQL Service

1. Log into your Aiven console
2. Click "Create Service" or "Create a new service"
3. Select "PostgreSQL" from the database options
4. Configure your service:

**Cloud Provider Selection:**

- Choose AWS, Google Cloud, or Azure
- Select a region close to your users for lower latency
- Example: "AWS - US East (N. Virginia)" or "Google Cloud - Europe West"

**Service Plan:**

- **Startup plans**: 1-2 GB RAM, good for development/testing
- **Business plans**: 4+ GB RAM, production workloads
- **Premium plans**: High availability with standby nodes

**Technical Consideration**: Starting with a smaller plan for development is cost-effective. You can scale up later without data loss.

5. Name your service (e.g., "my-postgres-db")
6. Click "Create Service"

### Step 3: Wait for Service Initialization

The service creation takes 3-10 minutes. You'll see status indicators:

- **Rebuilding**: Service is being created
- **Running**: Service is ready to use

During this time, Aiven is:

- Provisioning virtual machines
- Installing PostgreSQL
- Configuring security settings
- Setting up automated backups

### Step 4: Gather Connection Information

Once your service is running:

1. Click on your PostgreSQL service name
2. Navigate to the "Overview" tab
3. Locate the "Connection Information" section

You'll see several important pieces of information:

**Host (Hostname):**

```
example-project-db.aivencloud.com
```

This is the server address where your database lives.

**Port:**

```
12345
```

The port number (typically a custom port like 12345 instead of the default 5432).

**User:**

```
avnadmin
```

The default superuser account created by Aiven.

**Password:**

```
RANDOM_GENERATED_PASSWORD
```

A secure, auto-generated password. Click the eye icon to reveal it.

**Database Name:**

```
defaultdb
```

The default database that's pre-created for you.

**SSL Mode:**

```
require
```

Aiven enforces SSL/TLS encryption for all connections.

**Technical Note**: Keep these credentials secure. Never commit them to version control or share them publicly.

---

## Part 4: Connecting DBeaver to Aiven PostgreSQL

### Step 1: Create New Database Connection in DBeaver

1. In DBeaver, click "Database" → "New Database Connection" (or click the plug icon with a plus sign)
2. In the connection wizard, select "PostgreSQL"
3. Click "Next"

### Step 2: Configure Connection Settings

You'll see a connection configuration window with several tabs. Focus on the "Main" tab first.

**Main Tab Configuration:**

1. **Host**: Paste your Aiven hostname
   ```
   example-project-db.aivencloud.com
   ```

2. **Port**: Enter your Aiven port number
   ```
   12345
   ```

3. **Database**: Enter the database name
   ```
   defaultdb
   ```

4. **Username**: Enter your Aiven username
   ```
   avnadmin
   ```

5. **Password**: Enter your Aiven password
   - Check "Save password locally" if you want DBeaver to remember it
   - Unchecked means you'll enter the password each time you connect

**Show all databases checkbox**: Leave unchecked initially to connect to only the specified database.

### Step 3: Configure SSL/TLS Settings

This is crucial for Aiven connections, as they require encrypted connections.

1. Click on the "SSL" tab in the connection window
2. Configure the following:

**SSL Mode**: Select "require" or "verify-full"

- **require**: Encrypts the connection but doesn't verify the server certificate
- **verify-full**: Encrypts and verifies the server's identity (most secure)

**Technical Explanation**: 

- SSL/TLS creates an encrypted tunnel between DBeaver and Aiven
- This prevents anyone from intercepting your database credentials or data
- Aiven provides SSL certificates automatically

**For Enhanced Security (Optional but Recommended):**

Download the Aiven CA certificate:

1. In Aiven console, go to your service
2. Click "Download CA cert" button
3. Save the file (usually named `ca.pem`)

In DBeaver SSL tab:

1. Check "Use SSL"
2. **SSL Mode**: verify-full
3. **Root certificate**: Browse and select your downloaded `ca.pem` file

### Step 4: Test the Connection

1. Click the "Test Connection" button at the bottom of the connection window
2. DBeaver will attempt to connect to your Aiven database

**Possible Outcomes:**

**Success Message:**

```
Connected
PostgreSQL 15.x
Driver: PostgreSQL JDBC Driver
```

If you see this, congratulations! Your connection is properly configured.

**First-Time Connection:**

If this is your first PostgreSQL connection in DBeaver, you may see a driver download prompt:

1. Click "Download" to get the PostgreSQL JDBC driver
2. Wait for the download to complete
3. Click "Test Connection" again

**Common Issues and Solutions:**

1. **"Connection refused" error:**
   - Double-check your hostname and port
   - Verify your service is running in Aiven console
   - Check if your IP is whitelisted (if IP filtering is enabled)

2. **"SSL connection required" error:**
   - Go to SSL tab and set SSL mode to "require"
   - Ensure "Use SSL" is checked

3. **"Password authentication failed" error:**
   - Verify you copied the password correctly
   - Check for extra spaces at the beginning or end
   - Regenerate password in Aiven if needed

4. **Timeout errors:**
   - Check your internet connection
   - Verify firewall isn't blocking the port
   - Try from a different network to rule out corporate firewall issues

### Step 5: Finalize Connection

1. Once the test succeeds, click "Finish"
2. Your new connection will appear in the "Database Navigator" panel (usually on the left side)
3. The connection name will default to your hostname, but you can rename it:
   - Right-click the connection
   - Select "Rename"
   - Choose a friendly name like "Aiven Production DB"

---

## Part 5: Working with Your Connected Database

### Exploring Your Database

1. **Expand the connection** in Database Navigator by clicking the arrow next to it
2. You'll see a tree structure:
   ```
   └── PostgreSQL - Aiven Production DB
       └── Databases
           └── defaultdb
               ├── Schemas
               │   └── public
               │       ├── Tables
               │       ├── Views
               │       ├── Sequences
               │       └── Functions
               ├── Security
               └── System Info
   ```

3. **Double-click on your connection** to connect if it's not already connected (you'll see a plug icon turn green)

### Creating Your First Table

Let's create a simple table to test everything is working:

1. **Right-click** on "Tables" under the public schema
2. Select "Create New Table"
3. Configure the table:
   - **Table name**: users
   - Click "Columns" tab
   - Add columns:

| Column Name | Data Type    | Not Null | Primary Key |
|-------------|--------------|----------|-------------|
| id          | serial       | ✓        | ✓           |
| username    | varchar(50)  | ✓        |             |
| email       | varchar(100) | ✓        |             |
| created_at  | timestamp    | ✓        |             |

4. Click "Save" or "Persist"

**Technical Explanation**:

- **serial**: Auto-incrementing integer, perfect for IDs
- **varchar(n)**: Variable-length text with maximum length
- **timestamp**: Stores date and time
- **Not Null**: Field must have a value
- **Primary Key**: Unique identifier for each row

### Running SQL Queries

1. **Open SQL Editor**:
   - Right-click your connection → "SQL Editor" → "New SQL Script"
   - Or press Ctrl+] (Windows/Linux) or Cmd+] (macOS)

2. **Try a simple query**:

```sql
-- Insert some test data
INSERT INTO users (username, email, created_at)
VALUES 
    ('john_doe', 'john@example.com', NOW()),
    ('jane_smith', 'jane@example.com', NOW());

-- View the data
SELECT * FROM users;
```

3. **Execute the query**:
   - Highlight the SQL you want to run
   - Press Ctrl+Enter (Windows/Linux) or Cmd+Enter (macOS)
   - Or click the orange "Execute SQL Statement" button

4. **View results** in the results panel below the editor

### Understanding the DBeaver Interface

**Database Navigator (Left Panel)**:

- Tree view of all your connections and database objects
- Browse tables, views, and other database elements

**SQL Editor (Center)**:

- Write and execute SQL queries
- Syntax highlighting and auto-completion
- Multiple tabs for different scripts

**Results Panel (Bottom)**:

- Query results displayed in a grid
- Can edit data directly in this grid
- Export results to CSV, JSON, XML, etc.

**Properties Panel (Right)**:

- Detailed information about selected objects
- Connection properties and settings

---

## Part 6: Advanced Connection Features

### Connection Management Best Practices

**1. Connection Pooling:**

In DBeaver, you can configure connection pooling for better performance:

1. Right-click your connection → "Edit Connection"
2. Go to "Connection Settings" tab
3. Configure:
   - **Close idle connections**: After 30 minutes
   - **Ping connection on open**: Verify connection is alive
   - **Auto-commit**: Enable for safety (each statement commits immediately)

**Technical Context**: Connection pooling maintains a cache of database connections that can be reused, reducing overhead of creating new connections for each query.

**2. SSH Tunneling (For Additional Security):**

If you need extra security layers, you can connect through an SSH tunnel:

1. Edit Connection → "SSH" tab
2. Check "Use SSH Tunnel"
3. Configure:
   - **Host/IP**: Your SSH server
   - **Port**: 22 (default SSH port)
   - **Username**: SSH username
   - **Authentication**: Password or Private Key

**When to Use**: If you have a bastion host or jump server setup for database access.

**3. Multiple Databases on Same Service:**

Your Aiven service can host multiple databases:

Create a new database:

```sql
CREATE DATABASE my_new_db;
```

Add it to DBeaver:

1. Edit your existing connection
2. Check "Show all databases"
3. Or create a new connection pointing to `my_new_db`

### Performance Optimization Tips

**1. Query Execution Plans:**

Understand how PostgreSQL executes your queries:

```sql
EXPLAIN ANALYZE
SELECT * FROM users WHERE email LIKE '%@example.com';
```

In DBeaver:

- Write your query
- Right-click → "Execute" → "Explain Execution Plan"
- Visual diagram shows query performance

**2. Enable Query History:**

DBeaver keeps a history of executed queries:

- View → "Query Manager"
- Review past queries and their execution times
- Re-run successful queries easily

**3. Auto-Completion Configuration:**

Improve coding speed:

- Window → Preferences → Database → SQL Editor → Code Completion
- Enable "Use global search for column/table names"
- Set completion delay to your preference (500ms recommended)

---

## Part 7: Security Best Practices

### Credential Management

**1. Never Hard-Code Credentials:**

Bad practice:

```sql
-- Don't do this in shared scripts
\connect host=example.aivencloud.com user=avnadmin password=secret123
```

Good practice:

- Store credentials in DBeaver's secure credential storage
- Use environment variables for application connections
- Utilize connection profiles in DBeaver

**2. Create Limited-Privilege Users:**

Don't use the admin account for everything:

```sql
-- Create a read-only user for reporting
CREATE USER reporter WITH PASSWORD 'secure_password';
GRANT CONNECT ON DATABASE defaultdb TO reporter;
GRANT USAGE ON SCHEMA public TO reporter;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO reporter;

-- Create an application user with specific permissions
CREATE USER app_user WITH PASSWORD 'another_secure_password';
GRANT CONNECT ON DATABASE defaultdb TO app_user;
GRANT USAGE ON SCHEMA public TO app_user;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user;
```

**3. Regular Password Rotation:**

In Aiven console:

1. Navigate to your service
2. Click "Users" tab
3. Click "Reset Password" next to a user
4. Update the password in DBeaver connection

### Network Security

**1. IP Whitelisting in Aiven:**

Restrict database access to specific IP addresses:

1. In Aiven console, go to your service
2. Click "VPC" or "Networking" tab (depending on plan)
3. Add allowed IP ranges:
   ```
   203.0.113.10/32    # Single IP
   198.51.100.0/24    # IP range
   ```

**2. VPC Peering (For Production):**

For production environments, consider VPC peering:

- Connects your application's private network directly to Aiven
- Traffic never touches public internet
- Lower latency and higher security
- Requires Aiven Business or Premium plan

### Monitoring and Alerts

**In Aiven Console:**

1. Navigate to "Metrics" tab
2. Monitor:
   - CPU usage
   - Memory usage
   - Disk space
   - Connection count
   - Query performance

3. Set up integrations:
   - Email alerts for high resource usage
   - Slack notifications
   - Webhook integrations for custom monitoring

---

## Part 8: Backup and Recovery

### Understanding Aiven Backups

Aiven automatically backs up your database:

- **Frequency**: Continuous backups using PostgreSQL WAL (Write-Ahead Logging)
- **Retention**: Depends on your plan (typically 2-14 days)
- **Storage**: Backups stored in multiple availability zones

### Viewing Backup Status

In Aiven Console:

1. Go to your service
2. Click "Backups" tab
3. View:
   - Latest backup timestamp
   - Backup size
   - Available recovery points

### Performing a Point-in-Time Recovery

If you need to restore your database:

1. In Aiven console, click "Create fork" or "Restore"
2. Choose recovery point:
   - Specific timestamp
   - Latest backup
3. This creates a new service with restored data
4. Test the restored data
5. If correct, update your application connection strings

**Important**: This creates a NEW service; your original remains unchanged.

### Manual Backups with DBeaver

For additional safety, create manual exports:

**Export a Single Table:**

1. Right-click table → "Export Data"
2. Choose format: SQL, CSV, JSON, XML
3. Configure export settings
4. Click "Start" to save

**Export Entire Database:**

1. Right-click database → "Tools" → "Backup"
2. Or use pg_dump command:

```bash
pg_dump -h example-project-db.aivencloud.com -p 12345 -U avnadmin -d defaultdb -F c -f backup.dump
```

**Restore from Backup:**

```bash
pg_restore -h example-project-db.aivencloud.com -p 12345 -U avnadmin -d defaultdb backup.dump
```

---

## Part 9: Troubleshooting Common Issues

### Connection Problems

**Issue: "Could not connect to server"**

Solutions:

1. Verify service is running in Aiven console
2. Check hostname and port are correct
3. Test internet connectivity: `ping example-project-db.aivencloud.com`
4. Try from different network (to rule out firewall)
5. Check Aiven service status page for outages

**Issue: "SSL connection required"**

Solutions:

1. In DBeaver connection settings, go to SSL tab
2. Set SSL mode to "require"
3. Ensure "Use SSL" is checked
4. If using verify-full, confirm CA certificate path is correct

**Issue: "Too many connections"**

This means you've hit the connection limit.

Solutions:

1. Close idle connections in DBeaver (Database → Connection → Close)
2. Increase `max_connections` in Aiven (Advanced Configuration)
3. Implement connection pooling in your applications
4. Upgrade your Aiven plan for more connections

### Performance Issues

**Issue: Slow Queries**

Diagnosis steps:

1. Run EXPLAIN ANALYZE:

```sql
EXPLAIN ANALYZE
SELECT * FROM large_table WHERE some_column = 'value';
```

2. Check for missing indexes:

```sql
-- Find tables without indexes
SELECT schemaname, tablename, indexname
FROM pg_indexes
WHERE schemaname = 'public';
```

3. Create appropriate indexes:

```sql
CREATE INDEX idx_users_email ON users(email);
```

**Issue: High Memory Usage**

Check in Aiven metrics:

1. If consistently high (>80%), consider upgrading plan
2. Optimize queries to use less memory
3. Review and close unused connections

### Data Issues

**Issue: Unexpected Data Changes**

Troubleshooting:

1. Check query history in DBeaver (View → Query Manager)
2. Review PostgreSQL logs in Aiven console
3. Implement audit logging:

```sql
-- Create audit trigger
CREATE OR REPLACE FUNCTION audit_trigger_func()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (table_name, operation, old_data, new_data, changed_at)
    VALUES (TG_TABLE_NAME, TG_OP, row_to_json(OLD), row_to_json(NEW), NOW());
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

**Issue: Disk Space Running Low**

Solutions:

1. Delete old/unnecessary data
2. Run VACUUM to reclaim space:

```sql
VACUUM FULL;
```

3. Upgrade your Aiven service plan
4. Implement data archiving strategy

---

## Part 10: Migration and Scaling

### Migrating Existing Data to Aiven

**From Another PostgreSQL Server:**

Using pg_dump and pg_restore:

```bash
# 1. Export from source database
pg_dump -h source-host -U source-user -d source_db -F c -f migration.dump

# 2. Import to Aiven
pg_restore -h aiven-host.aivencloud.com -p 12345 -U avnadmin -d defaultdb migration.dump
```

**Using DBeaver:**

1. Connect to both source and destination databases in DBeaver
2. Right-click source database → "Export Data"
3. Choose "Database" as export source
4. Select SQL format
5. Execute the exported SQL on destination database

**For Large Databases:**

Consider using Aiven's migration tools:

1. In Aiven console → Service → "Service Integrations"
2. Set up database replication from source
3. Perform cutover during low-traffic period

### Scaling Your Database

**Vertical Scaling (Upgrading Plan):**

When you need more resources:

1. Aiven console → Your service → "Service Plan"
2. Select larger plan
3. Click "Change Plan"
4. Upgrade happens with minimal downtime (typically seconds)

**When to scale:**

- CPU consistently >70%
- Memory usage >80%
- Disk usage >75%
- Slow query performance
- Connection limit reached frequently

**Horizontal Scaling (Read Replicas):**

For read-heavy workloads:

1. Available on Business and Premium plans
2. Aiven console → "Service Integrations"
3. Create read-replica service
4. Configure applications to send reads to replica
5. Writes still go to primary

In DBeaver:

- Create separate connections for primary (writes) and replica (reads)
- Use connection naming to distinguish: "Aiven Primary" vs "Aiven Replica"

---

## Part 11: Monitoring and Maintenance

### Essential Metrics to Monitor

**In Aiven Console - Metrics Tab:**

1. **CPU Usage**: Should stay below 70% for comfortable headroom
2. **Memory Usage**: Monitor for memory leaks or inefficient queries
3. **Disk Usage**: Set alerts at 75% to prevent out-of-space issues
4. **Network Traffic**: Unusual spikes may indicate issues
5. **Active Connections**: Track against your plan's limit
6. **Query Performance**: Identify slow queries

### Setting Up Alerts

In Aiven console:

1. Navigate to "Integrations"
2. Add notification integrations:
   - **Email**: Immediate notifications
   - **Slack**: Team visibility
   - **PagerDuty**: Critical production alerts
   - **Webhooks**: Custom integrations

Configure alert thresholds:

- CPU > 80% for 5 minutes
- Disk > 85%
- Memory > 90%
- Connection count > 90% of limit

### Regular Maintenance Tasks

**Weekly:**

- Review slow query logs
- Check disk space trends
- Verify backup success

**Monthly:**

- Analyze table bloat and run VACUUM if needed
- Review and optimize indexes
- Check for unused tables/data
- Update statistics for query planner:

```sql
ANALYZE;
```

**Quarterly:**

- Review security settings
- Audit user permissions
- Test disaster recovery procedures
- Review and optimize service plan

---

## Part 12: Best Practices Summary

### Development Best Practices

1. **Use Separate Environments:**
   - Development: For coding and testing
   - Staging: Mirror of production for final testing
   - Production: Live user data
   
   Create separate Aiven services for each environment.

2. **Version Control Your Schema:**
   - Store all DDL (CREATE, ALTER) statements in Git
   - Use migration tools like Flyway or Liquibase
   - Track schema changes over time

3. **Write Defensive SQL:**

```sql
-- Always use transactions for related changes
BEGIN;
    UPDATE accounts SET balance = balance - 100 WHERE id = 1;
    UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- Use WHERE clauses carefully
-- Bad: UPDATE users SET status = 'inactive';
-- Good: UPDATE users SET status = 'inactive' WHERE last_login < NOW() - INTERVAL '1 year';
```

4. **Document Your Database:**
   - Add comments to tables and columns:

```sql
COMMENT ON TABLE users IS 'Application user accounts';
COMMENT ON COLUMN users.email IS 'User email address, must be unique';
```

### Production Best Practices

1. **Enable High Availability:**
   - Use Aiven Business or Premium plans
   - Automatic failover to standby nodes
   - Zero downtime during infrastructure issues

2. **Implement Connection Pooling:**
   - Use PgBouncer (available in Aiven)
   - Reduces connection overhead
   - Prevents connection exhaustion

3. **Regular Backups Beyond Aiven:**
   - Aiven provides excellent backups
   - Consider additional off-site backups for critical data
   - Test restore procedures regularly

4. **Security Hardening:**
   - Use least-privilege principle for user accounts
   - Enable IP whitelisting
   - Rotate credentials regularly
   - Use VPC peering for production
   - Enable audit logging

5. **Performance Monitoring:**
   - Set up comprehensive monitoring
   - Configure alerts for critical metrics
   - Review slow queries weekly
   - Keep statistics updated

---

## Conclusion

You now have a complete understanding of how to:

- Install and configure DBeaver for PostgreSQL management
- Create and configure a managed PostgreSQL database on Aiven
- Establish secure connections between DBeaver and Aiven
- Perform essential database operations
- Implement security best practices
- Monitor and maintain your database
- Troubleshoot common issues

### Next Steps

1. **Experiment**: Create test tables, run queries, explore DBeaver features
2. **Learn SQL**: Deepen your SQL knowledge with PostgreSQL-specific features
3. **Explore Aiven**: Try other Aiven services (Redis, Kafka, OpenSearch)
4. **Automate**: Look into infrastructure-as-code tools like Terraform for managing Aiven services
5. **Monitor**: Set up comprehensive monitoring and alerting

### Additional Resources

**DBeaver Documentation:**

- Official Docs: https://dbeaver.io/docs/
- GitHub: https://github.com/dbeaver/dbeaver

**Aiven Documentation:**

- Main Docs: https://docs.aiven.io/
- PostgreSQL Guide: https://docs.aiven.io/docs/products/postgresql

**PostgreSQL Learning:**

- Official Documentation: https://www.postgresql.org/docs/
- PostgreSQL Tutorial: https://www.postgresqltutorial.com/

**Community Support:**

- DBeaver GitHub Issues: For tool-specific questions
- Aiven Support: Available through console for service issues
- PostgreSQL Mailing Lists: For database-specific questions
- Stack Overflow: Tag questions with [postgresql], [dbeaver], or [aiven]

---

## Appendix: Quick Reference

### Common DBeaver Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| New SQL Script | Ctrl + ] | Cmd + ] |
| Execute Statement | Ctrl + Enter | Cmd + Enter |
| Execute Script | Ctrl + Alt + X | Cmd + Option + X |
| Format SQL | Ctrl + Shift + F | Cmd + Shift + F |
| Auto-complete | Ctrl + Space | Ctrl + Space |
| Find | Ctrl + F | Cmd + F |

### Essential PostgreSQL Commands

```sql
-- List all databases
\l

-- List all tables in current database
\dt

-- Describe table structure
\d table_name

-- Show current user
SELECT current_user;

-- Show database version
SELECT version();

-- Check database size
SELECT pg_size_pretty(pg_database_size('defaultdb'));

-- Find largest tables
SELECT
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC
LIMIT 10;
```

### Aiven Service Plans Quick Comparison

| Plan | RAM | Storage | Connections | HA | Backup Retention |
|------|-----|---------|-------------|-----|------------------|
| Hobbyist | 1 GB | 10 GB | 25 | No | 2 days |
| Startup | 2 GB | 80 GB | 100 | No | 2 days |
| Business | 4 GB | 175 GB | 200 | Yes | 14 days |
| Premium | 8+ GB | 350+ GB | 400+ | Yes | 30 days |

*(Plans and features may vary; check Aiven website for current offerings)*

---

**Document Version**: 1.0  
**Last Updated**: January 2026  
**Author**: Technical Documentation  
**License**: Free to use and distribute

---

*This guide is provided as-is for educational purposes. Always refer to official documentation for the most current information. Database management involves handling sensitive data - always follow security best practices and your organization's policies.*