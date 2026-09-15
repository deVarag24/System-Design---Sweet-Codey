# Distributed Logging System Design

## Functional Requirements
1. Unified logging Collection
2. Efficient search and retrieval
3. distributed storage
4. Centralized Visualization and Monitoring
5. Log retention and archival

## Non-Functional Requirements
1. low latency
    - Log ingestion latency `p95 < 25ms`
    - Log query latency `p95 < 100ms`
2. High Scalability
    - Log ingestion rate `30k logs/sec`
    - Search query rate `200 queries/sec`
    - Hot Storage `5 TB/day`
3. High Availability & Reliability `99.999%`
4. Security & Compliance

## High-Level Design
1. Client and Correlation ID
    - Each request coming from client to system should attach a unique correlation ID and this ID should be propagated through all the services handling the request.
    - This helps in tracing and debugging requests across distributed services.
2. Collecting Logs from all Services
    - All service store there logs locally and forward them to a centralized logging system using `DaemonSet log agent`.
    - `DaemonSet log agent` runs on each node and collects logs from all services running on that node, ensuring that no logs are missed.
    - `DaemonSet log agent` eg `Fluentd`, `Logstash`, `Filebeat`
3. Kafka as a Message Broker
    - Collected logs are sent to Kafka, which acts as a durable message broker.
    - Kafka ensures that logs are reliably stored and can be consumed by multiple downstream systems for processing and storage.
    - Kafka topics can be partitioned to handle high throughput and provide scalability.
4. Processing Kafka Messages using `Apache Flink`
    - Kafka messages are consumed by `Apache Flink` for real-time processing and transformation.
    - `Apache Flink` can perform operations like filtering, aggregation, and enrichment on the log data before storing it in the backend storage.
    - This ensures that only relevant and processed logs are stored, reducing storage costs and improving query performance.
5. Backend Storage
    - Processed logs from `Apache Flink` are stored in
        1. `Elasticsearch` or `Cassandra` for efficient search and querying
        2. `HDFS` or `S3` for long-term storage and archival
        3. `Time-series database` like `InfluxDB` or `TimescaleDB` for storing time-series log data and metrics
6. Visualization and Monitoring
    - Logs stored in the backend storage can be visualized using tools like `Kibana` or `Grafana`.
    - Dashboards and alerts can be set up to monitor the health and performance of the system.
    - This enables proactive detection of issues and helps in troubleshooting and debugging.