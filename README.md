# Microservices Architecture with RabbitMQ

A microservices-based architecture implementing user authentication with RabbitMQ message broker for event handling.

## Architecture

- **User Service**: Handles user registration and authentication
- **Message Broker**: Manages message queues and event processing
- **RabbitMQ**: Core message broker infrastructure
- **MongoDB**: Database storage
- **Nginx**: API Gateway and load balancer

## Prerequisites

- Docker and Docker Compose
- Node.js (for development)

## Quick Start

### Projet Structure

project/  
├── gateway/  
│   ├── Dockerfile  
│   ├── package.json  
│   ├── docker-compose.yml  
│   ├── docker-compose.dev.yml  
│   ├── docker-compose.prod.yml  
│   └── src/  
│       └── index.ts  
├── userservice/  
│   ├── Dockerfile  
│   ├── package.json  
│   └── src/  
│       └── server.ts  
├── messagebroker/  
│   ├── Dockerfile  
│   ├── package.json  
│   └── src/  
│       └── server.ts  
├── frontend-showcase-site/  
│   ├── Dockerfile  
│   ├── package.json  
│   └── src/  
│       └── App.vue  

### Development Mode

Start infrastructure services:
```bash
cd gateway
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build
```

### Production Mode

```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml up --build
```

## Service URLs

- **API Gateway**: http://localhost:85
- **MongoDB Express**: http://localhost:8081
- **RabbitMQ Dashboard**: http://localhost:15672 (guest/guest)

## API Endpoints

### User Service
- `POST /api/user/register`: Register new user
- `POST /api/user/login`: User authentication
- `POST /api/user/verify-token`: Validate a JWT Token 

## Environment Variables

Required environment variables per service:

```env
NODE_ENV=production
PORT=8082           # Change per service
MONGO_URI=mongodb://root:example@mongo:27017
MESSAGE_BROKER_URL=amqp://guest:guest@rabbitmq:5672
WAIT_HOSTS=mongo:27017,rabbitmq:5672
```

## Docker Services

- **MongoDB**: Database (Port: 27017)
    - Web interface available via Mongo Express (Port: 8081)
- **User Service**: Authentication service (Port: 8082)
- **Message Broker**: Event processing service (Port: 8083)
- **RabbitMQ**: Message queue service (Ports: 5672, 15672)
- **Nginx**: API Gateway (Port: 85)

## Networks

The services communicate through a bridged network named `rabbitmq_cluster`.

## Volumes

- `mongo-data`: Persistent storage for MongoDB

## Troubleshooting

1. Connection Issues:
    - Check service status: `docker-compose ps`
    - View service logs: `docker-compose logs [service_name]`
    - Verify network connectivity: `docker network inspect rabbitmq_cluster`

2. Common Fixes:
    - Restart services: `docker-compose restart`
    - Rebuild containers: `docker-compose up --build`
    - Reset environment: `docker-compose down -v`
