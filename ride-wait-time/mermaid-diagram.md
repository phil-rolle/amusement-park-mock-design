```mermaid
graph TD

subgraph Client
    A[Mobile App UI]
end

subgraph API Layer
    B[API Gateway / Load Balancer]
    C[Ride API Service - REST/gRPC on K8s]
end

subgraph Cache & Storage
    D[Redis `(hot cache`)]
    E[Postgres / Cloud SQL / Spanner]
end

subgraph Stream Ingestion
    F[Kafka / PubSub `(Sensor Input`)]
    G[Stream Processor - Kafka Streams / Dataflow]
end

subgraph Observability
    M[Prometheus / Grafana]
    N[Logs `(ELK / Loki`)]
    O[Alerting `(PagerDuty / Alertmanager`)]
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
