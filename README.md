# Docker Compose Setup

This Docker Compose configuration sets up a multi-service environment including databases, messaging systems, and utilities for development.

## Services

### PostgreSQL
- **Image**: postgres:15
- **Container Name**: postgres
- **Port**: 5432
- **Environment**:
  - User: siveing
  - Database: siveing_db
- **Volume**: postgres_data
- **Description**: A PostgreSQL database instance for relational data storage.

### MySQL
- **Image**: mysql:8.0
- **Container Name**: mysql
- **Port**: 3306
- **Environment**:
  - Root Password: [Set in docker-compose.yml]
  - Database: siveing_db
  - User: siveing
- **Volume**: mysql_data
- **Description**: A MySQL database instance for relational data storage.

### phpMyAdmin
- **Image**: phpmyadmin/phpmyadmin
- **Container Name**: phpmyadmin
- **Port**: 6801 (maps to 80 internally)
- **Environment**:
  - Host: mysql
  - Port: 3306
  - User: root
- **Description**: Web-based interface for managing MySQL databases.

### MongoDB
- **Image**: mongo:6.0
- **Container Name**: mongodb
- **Port**: 27017
- **Environment**:
  - Root Username: siveing
- **Volume**: mongodb_data
- **Description**: A NoSQL MongoDB instance for document-based data storage.

### Redis
- **Image**: redis:7.0
- **Container Name**: redis
- **Port**: 6379
- **Description**: In-memory data store used as a cache or message broker.

### Kafka
- **Image**: confluentinc/cp-kafka:7.2.2
- **Container Name**: kafka
- **Ports**: 9092, 29092
- **Depends On**: zookeeper
- **Environment**:
  - Broker ID: 1
  - Zookeeper Connect: zookeeper:2181
  - Advertised Listeners: PLAINTEXT://kafka:29092,PLAINTEXT_HOST://localhost:9092
- **Description**: Distributed streaming platform for handling large-scale data streams.

### Zookeeper
- **Image**: confluentinc/cp-zookeeper:7.2.2
- **Container Name**: zookeeper
- **Port**: 2181
- **Environment**:
  - Client Port: 2181
- **Description**: Centralized service for maintaining configuration information for Kafka.

### Kafka UI
- **Image**: provectuslabs/kafka-ui:latest
- **Container Name**: kafka-ui
- **Port**: 8080
- **Environment**:
  - Cluster Name: local
  - Bootstrap Servers: kafka:29092
  - Zookeeper: zookeeper:2181
- **Description**: Web-based UI for managing and monitoring Kafka clusters.

### Mailhog
- **Image**: mailhog/mailhog
- **Container Name**: mailhog
- **Ports**:
  - SMTP: 1025
  - Web UI: 6802 (maps to 8025 internally)
- **Description**: Email testing tool with a web interface for capturing and viewing emails.

### RabbitMQ
- **Image**: rabbitmq:3-management
- **Container Name**: rabbitmq
- **Ports**:
  - AMQP: 5672
  - Management UI: 15672
- **Environment**:
  - Default User: admin
- **Volume**: rabbitmq_data
- **Description**: Message broker for handling asynchronous messaging.

## Volumes
- **postgres_data**: Persistent storage for PostgreSQL data.
- **mysql_data**: Persistent storage for MySQL data.
- **mongodb_data**: Persistent storage for MongoDB data.
- **rabbitmq_data**: Persistent storage for RabbitMQ data.

## Usage
1. Ensure Docker and Docker Compose are installed.
2. Create a \`.env\` file or directly set environment variables for sensitive data (e.g., passwords).
3. Run \`docker-compose up -d\` to start all services in detached mode.
4. Access services via their respective ports (e.g., http://localhost:6801 for phpMyAdmin, http://localhost:8080 for Kafka UI).

## Notes
- Replace asterisks (\`*******\`) in the docker-compose.yml with secure passwords.
- All services are configured to restart automatically (\`restart: always\`).
- Ensure ports do not conflict with other applications running on the host.
