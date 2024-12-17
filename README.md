# Recipe - AI-First Migration Recipe Specification v0.1

A standardized format for defining component migrations between platforms/services, designed for AI agent execution with human oversight.

## Table of Contents
- [Overview](#overview)
- [Metadata](#metadata)
- [Source Definition](#source-definition)
- [Target Definition](#target-definition)
- [Analysis Requirements](#analysis-requirements)
- [Validation](#validation)
- [Observability](#observability)
- [Rollback](#rollback)
- [AI Agent Instructions](#ai-agent-instructions)

## Overview

This specification provides a standardized way to define migrations that can be executed by AI agents while maintaining human oversight. It covers all aspects of migration, from source discovery to rollback procedures.

## Metadata

Define basic information about the migration:

```yaml
metadata:
  name: "payment-service-migration"
  description: "Migration recipe for payment processing service"
  owner: "team-payments"
  contact:
    email: "team-payments@example.com"
    slack: "#team-payments"
  tags: ["payment", "api", "critical-service"]
  priority: "high"
  estimated-downtime: "none"  # or duration like "5m"
```

## Source Definition

### Component Type
```yaml
source:
  type: "rest-api"  # Possible types: rest-api, grpc, graphql, batch-job
```

### Discovery
```yaml
discovery:
  locations:
    - type: "git"
      url: "github.com/org/service"
      branch: "main"
      paths: ["src/**"]
    - type: "artifact"
      repository: "artifacts.org/payment-service"
      version: "1.2.3"
    - type: "docker"
      image: "org/payment-service:latest"
```

### Interfaces
```yaml
interfaces:
  - type: "openapi"
    spec: "api/openapi.yaml"
  - type: "proto"
    files: ["proto/*.proto"]
  - type: "graphql"
    schema: "schema.graphql"
```

### Dependencies
```yaml
dependencies:
  services:
    - name: "user-service"
      type: "rest"
      criticality: "required"
    - name: "audit-log"
      type: "kafka"
      criticality: "optional"
  
  databases:
    - name: "payments-db"
      type: "postgresql"
      version: "13"
      schemas: ["schema.sql"]
```

### Behavioral Requirements
```yaml
behavioral:
  scaling:
    min_replicas: 3
    max_replicas: 10
    metrics:
      - type: "cpu"
        target: 80
      - type: "requests_per_second"
        target: 1000
  
  reliability:
    availability: "99.99"
    max_latency: "200ms"
    error_budget: "0.1%"
  
  security:
    auth_methods: ["oauth2", "api-key"]
    data_classification: "pii"
    compliance: ["pci-dss", "gdpr"]
```

## Target Definition

### Platform
```yaml
target:
  platform: "gcp"  # aws, azure, etc.
  region: "us-west1"
```

### Services
```yaml
services:
  primary:
    type: "cloud-run"
    configuration:
      memory: "1Gi"
      cpu: "1"
      concurrency: 80
  
  auxiliary:
    databases:
      - type: "cloud-sql"
        tier: "db-custom-2-4096"
        high-availability: true
    caching:
      - type: "memorystore"
        tier: "basic"
        size: "5Gi"
```

### Networking
```yaml
networking:
  ingress:
    type: "internal"  # or "public"
    domains: ["payments.internal.org"]
  vpc:
    name: "prod-vpc"
    subnet: "services-subnet"
```

### Security
```yaml
security:
  iam:
    service-account: "payment-service@project.iam"
    roles:
      - "roles/run.invoker"
      - "roles/cloudsql.client"
  secrets:
    manager: "secret-manager"
    prefix: "/payment-service"
```

## Analysis Requirements

### Code Analysis
```yaml
analysis:
  code:
    - type: "dependencies"
      scan: ["vulnerabilities", "licenses"]
    - type: "complexity"
      metrics: ["cyclomatic", "cognitive"]
```

### Security Analysis
```yaml
security:
  - type: "authentication"
    verify: ["methods", "flows"]
  - type: "data-handling"
    scan: ["pii", "encryption"]
```

## Validation

### Stages
```yaml
validation:
  stages:
    development:
      - type: "unit-tests"
        coverage: 80
    
    staging:
      - type: "e2e-tests"
        scenarios: ["critical-paths"]
      - type: "performance-tests"
        duration: "30m"
        criteria:
          latency-p95: "200ms"
          throughput: "1000rps"
    
    production:
      - type: "canary"
        duration: "1h"
        traffic: "5%"
```

## Observability

### Metrics and Logging
```yaml
observability:
  metrics:
    - name: "request_latency"
      type: "histogram"
      buckets: [10, 50, 100, 200, 500]
    - name: "error_rate"
      type: "counter"
      labels: ["error_type"]
  
  logs:
    level: "info"
    format: "json"
    retention: "30d"
```

## Rollback

### Triggers and Strategy
```yaml
rollback:
  triggers:
    - metric: "error_rate"
      threshold: "1%"
      duration: "5m"
  
  strategy:
    type: "automated"
    steps:
      - "revert-traffic"
      - "restore-data"
      - "notify-stakeholders"
```

## AI Agent Instructions

### Priorities and Constraints
```yaml
ai_agent_instructions:
  priorities:
    - "maintain-data-integrity"
    - "minimize-downtime"
    - "preserve-security"
  
  constraints:
    - "no-breaking-changes"
    - "maintain-backwards-compatibility"
  
  decision_points:
    - step: "database-migration"
      options:
        - "direct-migration"
        - "dual-write"
      factors:
        - "data-volume"
        - "downtime-tolerance"
        - "risk-level"
```

## Usage

1. Create a migration recipe following this specification
2. Validate the recipe format
3. Review and adjust any organization-specific parameters
4. Execute the migration using an AI agent that supports this specification
5. Monitor the migration progress
6. Verify the results using the specified validation criteria

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## License

This specification is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Support

For questions and discussions, please use the GitHub Discussions feature in this repository.
