```mermaid
flowchart LR

%% DATA SOURCES
subgraph Data_Sources
    CSV[Batch CSV<br/>users.csv<br/>products.csv]
    API[External API<br/>Exchange Rates]
    KAFKA[Kafka Topic<br/>user_events]
end

%% PROCESSING
subgraph Processing
    SPARK_BATCH[Spark Batch Jobs]
    SPARK_STREAM[Spark Structured Streaming]
end

%% LAKEHOUSE
subgraph Lakehouse
    BRONZE[Iceberg Bronze<br/>Raw Data]
    SILVER[Iceberg Silver<br/>Cleaned Data]
    GOLD[Iceberg Gold<br/>Business Metrics]
end

%% TRANSFORMATIONS
subgraph Transformations
    DBT[dbt Models<br/>Tests & Snapshots]
end

%% ORCHESTRATION
subgraph Orchestration
    AIRFLOW[Airflow DAGs]
end

%% OBSERVABILITY
subgraph Observability
    GE[Great Expectations]
    OL[OpenLineage]
    MARQUEZ[Marquez UI]
end

%% CONSUMPTION
subgraph Consumption
    STREAMLIT[Streamlit Dashboard]
end

%% FLOWS
CSV --> SPARK_BATCH
API --> SPARK_BATCH
KAFKA --> SPARK_STREAM

SPARK_BATCH --> BRONZE
SPARK_STREAM --> BRONZE

BRONZE --> DBT
DBT --> SILVER
SILVER --> DBT
DBT --> GOLD

AIRFLOW --> SPARK_BATCH
AIRFLOW --> SPARK_STREAM
AIRFLOW --> DBT

DBT --> GE
SPARK_BATCH --> OL
SPARK_STREAM --> OL
DBT --> OL
OL --> MARQUEZ

GOLD --> STREAMLIT
