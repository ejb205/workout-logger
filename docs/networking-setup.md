# AWS Networking + RDS Setup

## VPC
- CIDR: 10.0.0.0/16
- Subnets:
  - `public-subnet-1`: 10.0.0.0/24
  - `public-subnet-2`: 10.0.1.0/24
  - `private-subnet-1`: 10.0.2.0/24
  - `private-subnet-2`: 10.0.3.0/24

## Security Groups
- `rds-mysql-sg`: Port 3306 open to developer IP and Lambda SG (TBD)

## RDS
- MySQL 8, free-tier `db.t3.micro`
- DB Identifier: `workoutlogger-db`
- Placed in private subnets via `rds-private-subnet-group`
