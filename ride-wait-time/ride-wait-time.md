# 🎢 Prompt 1: Ride Wait-Time Tracking System — DevOps Cheat Sheet

## 📌 Purpose
Track real-time ride wait times and serve this data reliably to mobile apps and park displays.

---

## ⚙️ Architecture Overview

[Sensor Input / Staff API]
|
[API Gateway / Ingestion]
|
[Kafka or Pub/Sub]
|
[Stream Processor (Flink/Kafka Streams)]
|
[Redis (hot data)] + [Postgres/BigQuery (storage)]
|
[API Service] ←→ [Mobile App / Screens]


---

## ☁️ Cloud Stack Comparison

| Component                     | AWS                                  | GCP                                    |
|------------------------------|---------------------------------------|----------------------------------------|
| API Gateway / Ingress        | API Gateway + ALB                     | Cloud Endpoints + Load Balancer        |
| Messaging Bus                | Amazon MSK (Kafka) or SNS/SQS         | Pub/Sub                                |
| Stream Processing            | Kinesis Data Analytics or Flink on EMR| Dataflow (Apache Beam)                 |
| Real-time Cache              | ElastiCache (Redis)                   | Memorystore (Redis)                    |
| Storage                      | Aurora/PostgreSQL or DynamoDB         | Cloud SQL (Postgres) or Bigtable       |
| Historical Analytics         | Redshift / S3                         | BigQuery / GCS                         |
| Observability (metrics/logs) | CloudWatch + Prometheus + Grafana     | Cloud Monitoring + Prometheus + Grafana|
| CI/CD                        | CodePipeline / CodeBuild              | Cloud Build + Cloud Deploy             |
| IaC                          | Terraform or AWS CDK                  | Terraform or Deployment Manager        |

---

## 🔧 Key Functional Components

| Area           | Details |
|----------------|---------|
| **Ingestion**  | Sensor data via REST/gRPC or message queue. Validates input, deduplicates. |
| **Processing** | Sliding window average wait time calculation, optional anomaly detection. |
| **Storage**    | Redis for fast access. Postgres for persistence. TTLs to expire old data. |
| **Serving**    | REST API with rate-limiting. CDN edge cache for mobile UI content. |
| **Monitoring** | Alerts for stale ride data. Track ingestion lag and Redis expiration. |

---

## ✅ DevOps Coverage

- **High availability**: Stateless services behind ALB; Redis in cluster mode.
- **Scaling**: KEDA or HPA based on Kafka lag, CPU, or custom Prometheus metrics.
- **Observability**: Alert on stale rides, ingestion lag, Redis hit rate.
- **Resilience**: Fallback to Postgres if Redis fails. Circuit breakers for downstream.
- **Cost optimization**: Scale stream processing based on ride activity or park hours.

---

## 🎯 One-Liner Summary
> “This system ingests and processes real-time ride wait data using a scalable, observable, and cloud-native event-driven architecture that balances latency, cost, and reliability.”
