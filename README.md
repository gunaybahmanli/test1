```mermaid

%% ===== DATA SOURCES =====
subgraph Data_Sources
    CSV[Batch CSV\nusers.csv\nproducts.csv]
    API[External API\nExchange Rates]
    KAFKA[Kafka Topic\nuser_events]
end

%% ===== INGESTION & PROCESSING =====
subgraph Processing
    SPARK_BATCH[Spark Batch Jobs]
    SPARK_STREAM[Spark Structured Streaming]
end

%% ===== STORAGE (LAKEHOUSE) =====
subgraph Lakehouse
    BRONZE[Iceberg Bronze\nRaw Data]
    SILVER[Iceberg Silver\nCleaned & Conformed]
    GOLD[Iceberg Gold\nBusiness Metrics]
end

%% ===== TRANSFORMATION =====
subgraph Transformations
    DBT[dbt Models\nTests & Snapshots]
end

%% ===== ORCHESTRATION =====
subgraph Orchestration
    AIRFLOW[Airflow DAGs]
end

%% ===== OBSERVABILITY =====
subgraph Observability
    GE[Great Expectations\nData Quality]
    OL[OpenLineage]
    MARQUEZ[Marquez UI]
end

%% ===== CONSUMPTION =====
subgraph Consumption
    STREAMLIT[Streamlit Dashboard]
end

%% ===== FLOWS =====
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
