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
    F[Amazon Kinesis or MSK (Kafka)]
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