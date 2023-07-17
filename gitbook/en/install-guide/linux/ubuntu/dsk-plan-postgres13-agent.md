# dsk-plan-postgres13-agent

The 'Postgres13 agent' collects the state of the database and slow queries in real time.
This allows you to collect a variety of information, including performance metrics, resource usage, and slow queries from your database.
Based on the collected data, you can identify and respond to database performance bottlenecks.
It can also detect slow queries to improve database performance by creating indexes, optimizing queries, and more.
We tailor agent settings to your needs to deliver optimal results.

# Did you run the DataSaker predecessor?
In the current Ubuntu environment, if the preceding task of `DataSaker` has not been carried out, please proceed with the preceding task of `DataSaker` first. [DataSaker predecessor](${PREPARATION_MANUAL_KR})

# Install Plan Postgres13 Agent
## 1. agent-config settings

```yaml
agent:
  metadata:
    agent_name: "dsk-plan-postgres13-agent" # replace you want
  data_source_name:
    user: # <user_name>
    password: # <user_password>
    address: # <database_address>
    port: # <database_port>
    DBName: # <database_name>
  explain:
    scrape_interval: 30s # <activity_session_scrape_time>
    scrape_timeout: 5s # <activity_session_scrape_query_timeout>
    slow_query_standard: 5s # <slow_query_standard>
    executor_number: 10 # <explain executor number>
    sender_number: 10 # <explain sender number>
    activity_query_buffer: 50 # <activity query buffer>
    plan_sender_buffer: 50 # <explain result buffer>
```

## 2. Install the package
Requires sudo privileges.
```bash
curl -fsSL -o installer.sh https://dsk-agent-s3.s3.ap-northeast-2.amazonaws.com/dsk-agent-s3/public/install.sh
chmod 700 installer.sh
sudo ./installer.sh dsk-postgres13-agent
```

## 3. Run the package
```bash
sudo dsk-plan-postgres13-agent start
```

## 4. Check Package Execution Status
### Running
```bash
sudo dsk-plan-postgres13-agent status
```
### Not Running
```bash
sudo dsk-plan-postgres13-agent status
```

# Remove Plan Postgres13 Agent
## 1. Package abort
```bash
sudo dsk-plan-postgres13-agent stop
```

## 2. Remove packages
```bash
sudo apt remove dsk-plan-postgres13-agent
```