# Architecture Overview

## Purpose
A secure and modular backend for logging and retrieving user workouts, designed to support a future SwiftUI-based iOS frontend.

## Diagram (to be added)
### Logical Flow:

+------------+         +-------------+        +-----------------+        +---------------------+
|            | HTTP    |             |        |                 |        |                     |
|   Client   +-------->+ API Gateway +------->+   Lambda (Py)   +------->+  RDS MySQL (Private)|
| (iOS/Postman)        | (HTTP API)  |        |                 |        |                     |
|            |         |             |        |                 |        |                     |
+------------+         +-------------+        +-----------------+        +---------------------+
                                              | (in VPC behind NAT)     |
                                              +-------------------------+

## Key AWS Components
- **RDS (MySQL)**: Stores workout logs
- **Lambda**: Validates and inserts log data
- **API Gateway (HTTP)**: Exposes REST endpoint
- **VPC**: Contains RDS (private) and Lambda (private)
- **NAT Gateway**: Enables Lambda to resolve DNS for RDS endpoint

## Network Architecture

### VPC Configuration

We use a dedicated VPC named `workoutlogger-vpc` (CIDR: 10.0.0.0/16) to isolate our infrastructure and support a scalable, secure deployment architecture.

#### Subnet Design

| Name                               | AZ           | CIDR Block   | Type     |
|------------------------------------|--------------|--------------|----------|
| workoutlogger-public-us-east-2a   | us-east-2a   | 10.0.0.0/24  | Public   |
| workoutlogger-public-us-east-2b   | us-east-2b   | 10.0.1.0/24  | Public   |
| workoutlogger-private-us-east-2a  | us-east-2a   | 10.0.2.0/24  | Private  |
| workoutlogger-private-us-east-2b  | us-east-2b   | 10.0.3.0/24  | Private  |

### Routing

- Public route table (`workoutlogger-public-rt`) routes `0.0.0.0/0` to the Internet Gateway
- Private route table (`workoutlogger-private-rt`) routes `0.0.0.0/0` to the NAT Gateway for secure outbound access
