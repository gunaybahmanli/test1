```mermaid

%%{init: {
  "theme": "dark",
  "flowchart": {
    "curve": "monotone",
    "nodeSpacing": 40,
    "rankSpacing": 40
  }
}}%%

flowchart LR

classDef source fill:#2d2d2d,stroke:#facc15,color:#facc15
classDef processing fill:#1e293b,stroke:#a855f7,color:#e9d5ff
classDef storage fill:#0f172a,stroke:#38bdf8,color:#bae6fd
classDef transform fill:#020617,stroke:#22c55e,color:#bbf7d0
classDef orchestration fill:#18181b,stroke:#fb7185,color:#fecdd3
classDef observability fill:#111827,stroke:#f97316,color:#fed7aa
classDef consumption fill:#020617,stroke:#eab308,color:#fef08a

subgraph Data Sources
    CSV[users.csv<br/>products.csv]
    API[Exchange Rates API]
    KAFKA[Kafka<br/>user_events]
end
class CSV,API,KAFKA source

subgraph Processing
    SPARK_BATCH[Spark Batch]
    SPARK_STREAM[Spark Streaming]
end
class SPARK_BATCH,SPARK_STREAM processing

subgraph Lakehouse
    BRONZE[Iceberg Bronze]
    SILVER[Iceberg Silver]
    GOLD[Iceberg Gold]
end
class BRONZE,SILVER,GOLD storage

subgraph Transformations
    DBT[dbt Models<br/>Tests & Snapshots]
end
class DBT transform

subgraph Orchestration
    AIRFLOW[Airflow DAGs]
end
class AIRFLOW orchestration

subgraph Observability
    GE[Great Expectations]
    OL[OpenLineage]
    MARQUEZ[Marquez UI]
end
class GE,OL,MARQUEZ observability

subgraph Consumption
    STREAMLIT[Streamlit Dashboard]
end
class STREAMLIT consumption

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

%% LINK STYLES
linkStyle 0,1,2 stroke:#facc15,stroke-width:2px
linkStyle 3,4,13,14 stroke:#a855f7,stroke-width:2px
linkStyle 5,7,17 stroke:#38bdf8,stroke-width:2px
linkStyle 6,8,12,15 stroke:#22c55e,stroke-width:2px
linkStyle 9,10,11 stroke:#fb7185,stroke-width:2px
linkStyle 16 stroke:#f97316,stroke-width:2px
