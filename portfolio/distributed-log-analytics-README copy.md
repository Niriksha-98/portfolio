# distributed-log-analytics

scalable telemetry pipeline for processing large volumes of semi-structured log data. built this to get hands-on with distributed data processing — specifically how you handle data at a scale where single-node tools stop working.

## what it does

ingests 2TB+ of semi-structured log data, processes it with PySpark, stores it in a Hive-partitioned data lake on HDFS, and orchestrates the whole thing with Airflow.

## results

- **60% reduction** in query latency via PySpark Broadcast Joins and custom partitioning
- **40% storage reduction** by switching to Parquet columnar format with Hive multi-tier partitioning
- predicate push-down enabled through strategic partition design

## architecture

```
raw logs (JSON/CSV)
      ↓
  ingestion layer (Python)
      ↓
  HDFS raw storage
      ↓
  PySpark processing
  ├── broadcast joins for dimension tables
  ├── custom partitioning by date/source/severity
  └── Parquet conversion
      ↓
  Hive metastore
  └── multi-tier partitioning (year/month/day/source)
      ↓
  query layer
  └── predicate push-down enabled
```

## stack

- **processing**: PySpark, Hadoop HDFS
- **storage**: Parquet (columnar), Hive metastore
- **orchestration**: Apache Airflow
- **language**: Python

## setup

```bash
git clone https://github.com/niriksha-narendra/distributed-log-analytics
cd distributed-log-analytics
pip install -r requirements.txt
```

requires a Hadoop/HDFS cluster and Spark installation. see `docs/setup.md` for local dev setup using Docker.

## things i learned

- broadcast joins are critical when you have a large fact table joining to small dimension tables — distributing the small table to every executor avoids the shuffle
- partition column choice matters a lot: partitioning by date + source meant most queries only touched a fraction of the data
- Parquet's column pruning meant queries selecting 3 columns out of 50 only read those 3 — huge for latency

## structure

```
├── dags/                  # Airflow DAG definitions
├── src/
│   ├── ingestion/         # raw log ingestion scripts
│   ├── processing/        # PySpark transformation jobs
│   └── utils/             # shared helpers
├── config/                # environment configs
├── docs/
│   └── setup.md           # local dev setup
└── requirements.txt
```
