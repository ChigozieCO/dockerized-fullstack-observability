# Dockerized Deployment of a FastAPI and React App with Reverse Proxy, Monitoring and Observability

## Project Overview

This project demonstrates the deployment of a containerized full-stack application (FastAPI + React) with comprehensive monitoring and observability capabilities. It includes reverse proxy configuration and cloud deployment with proper domain setup.

The goal is to provide a robust and scalable application infrastructure while showcasing best practices in containerization, monitoring, and cloud deployment. By the end of this project, users will have a fully functional web application deployed on a cloud platform with proper domain configuration, detailed logging, and real-time performance monitoring.

## Features

####  Containerized Application Stack

- FastAPI backend
- React frontend
- PostgreSQL database
- Adminer for database management
- Traefik reverse proxy


#### Monitoring & Observability

- Prometheus for metrics collection
- Grafana for visualization
- Loki for log aggregation
- Promtail for log collection
- cAdvisor for container monitoring

## Prerequisites

- Docker and Docker Compose
- AWS Account and AWS CLI
- Domain name and access to DNS settings
- Basic knowledge of:

    - Docker and containerization
    - Cloud deployment concepts
    - Monitoring and observability tools

## System Requirements

- Minimum 4GB RAM
- Stable internet connection
- t2.medium or equivalent instance type (for AWS deployment)

## Quick Start

1. Clone this repository:

    ```sh
    git clone https://github.com/ChigozieCO/dockerized-fullstack-observability.git 
    ```


2. Cloud Deployment and DNS Configuration

      - Launch an EC2 instance (t2.medium recommended).
      - Configure security groups for ports 80, 443 and 8080.
      - Set up DNS A records for `your domain`, `db.your domain` and `www.your domain` pointing to EC2 public IP.


3. Create required configuration files:

    ```sh
    # Create and set permissions for SSL certificate storage
    touch acme.json
    chmod 600 ./acme.json
    ```


4. Configure domain settings:

      - Update domain name in `traefik.yml` and in the docker compose files (update the traefik labels).
      - Update environment files in frontend and backend directories.


5. Generate Password hash 

    You need to generate a hashed password for the traefik dashboard authentication. You can use an htpasswd generator online or use the `htpasswd` command below if you have it installed.

    ```sh
    echo $(htpasswd -nbB admin <yourpassword>) | sed -e s/\\$/\\$\\$/g
    ```

    :warning: NOTE

    When used in docker-compose.yml all dollar signs in the hash need to be doubled for escaping.


6. Update the `dashboard-auth` middleware

    In the `traefik` service in the `compose.yaml` file, update the `dashboard-auth` middleware with your own hashed password from the step above, this is necessary so you can have access to your traefik dashboard.


7. Deploy the application:

    ```sh
    docker compose up -d
    ```

## Component Setup

### Application Stack

- Frontend runs on port 5173 (proxied through Traefik) accessible at yourdomain.com.
- Backend runs on port 8000 (proxied through Traefik)accessible at yourdomain.com/api and accessible at yourdomain.com/docs.
- Database runs on port 5432 (internal)
- Adminer available at db.yourdomain.com.
- Traefik dashboard at yourdomain.com/dashboard

### Monitoring Stack

- Prometheus accessible at yourdomain.com/prometheus.
- Grafana accessible at yourdomain.com/grafana
- Loki configured for log aggregation
- cAdvisor monitoring container metrics
- Promtail streams logs from application containers to Loki

### Monitoring Setup

- Access Grafana at `yourdomain.com/grafana`
- Configure data sources:

    - Prometheus: `yourdomain.com/prometheus`
    - Loki: `yourdomain.com/loki`

- Import dashboards:

    - cAdvisor Dashboard (ID: 19792)
    - Loki Dashboard (ID: 13186)
    - Traefik Dashboard (ID: 4475)

## Security Notes

- SSL certificates are automatically managed by Let's Encrypt
- Traefik dashboard is password protected
- Database credentials should be changed in production
- All services are accessible only via HTTPS

## Project Documentation/Walk Through

For a more detailed step by step implementation of this project see the [project documentation](https://dev.to/chigozieco/dockerized-deployment-of-a-full-stack-application-with-reverse-proxy-monitoring-observability-5c04).