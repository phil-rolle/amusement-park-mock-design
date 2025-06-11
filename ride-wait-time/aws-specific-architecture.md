## 📐 Design Properties (AWS-Specific Architecture)

### 📈 Diagram
```mermaid
graph TD

subgraph Client
    A[Mobile App UI]
end

subgraph API_Layer
    B[Amazon API Gateway + ALB]
    C[Ride API Service - ECS Fargate or EKS]
end

subgraph Cache_and_Storage
    D[Amazon ElastiCache - Redis]
    E[Amazon RDS - Aurora PostgreSQL]
end

subgraph Stream_Ingestion
    F[Amazon Kinesis or Kafka]
    G[Stream Processor - AWS Lambda or Kinesis Analytics]
end

subgraph Observability
    M[Amazon CloudWatch - Metrics & Dashboards]
    N[CloudWatch Logs]
    O[AWS SNS or PagerDuty - Alerts]
end

A --> B --> C
C --> D
C --> E
F --> G --> D
G --> E

C --> M
C --> N
M --> O
```

### ✅ High Availability

- **Multi-AZ Deployments**:
  - API Gateway, ALB, ECS (Fargate), EKS, Aurora PostgreSQL, and ElastiCache all support Multi-AZ configurations to ensure fault tolerance.
  
- **Auto Scaling**:
  - ECS/EKS can auto-scale based on CPU, memory, or custom metrics (via CloudWatch).
  - Aurora supports read replicas and failover to standby nodes.
  
- **Stateless Services**:
  - API and microservices are stateless, allowing easy horizontal scaling and resiliency.

---

### ⚖️ Scalability

- **Event-Driven Ingestion**:
  - Amazon Kinesis or MSK (Kafka) handles high-throughput sensor data with partition-based scaling.
  
- **Compute**:
  - ECS Fargate scales containers dynamically per load.
  - EKS supports advanced scaling (e.g., HPA, Cluster Autoscaler).

- **Storage**:
  - ElastiCache (Redis) handles hot-path data (e.g., current wait times).
  - Aurora scales horizontally for reads, vertically for writes.

- **Stream Processing**:
  - AWS Lambda or Kinesis Data Analytics enables decoupled and scalable stream transformations.

---

### 💰 Cost Optimization

- **Pay-per-Use Services**:
  - Lambda, Kinesis, and Fargate are billed per usage, helping minimize idle resource costs.

- **TTL & Tiering**:
  - Redis cache entries expire after a short TTL (e.g., 5–10 mins), avoiding overconsumption.
  - Logs can be offloaded to Amazon S3 from CloudWatch to reduce long-term retention costs.

- **Serverless & Spot Options**:
  - Aurora Serverless v2 enables scale-to-zero for dev/test workloads.
  - Spot instances or savings plans for ECS/EKS nodes where applicable.

- **Observability Cost Control**:
  - Use CloudWatch metric filters selectively.
  - Consider OpenTelemetry with Amazon Managed Prometheus/Grafana to centralize observability with cost control.
