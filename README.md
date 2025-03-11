# OrangeHRM Docker Setup

Simple instructions to get OrangeHRM running on your local machine using Docker.

## Prerequisites

- Docker installed on your machine
- Docker Compose installed on your machine

## Quick Setup

1. Create a new folder for your OrangeHRM setup
2. Create a file named `docker-compose.yml` with the following content:

```yaml
version: '3'

services:
  db:
    image: mysql:5.7
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=root_password
      - MYSQL_DATABASE=orangehrm
      - MYSQL_USER=orangehrm
      - MYSQL_PASSWORD=orangehrm_password
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - orangehrm-network

  orangehrm:
    image: orangehrm/orangehrm:latest
    restart: always
    ports:
      - '8080:80'
    environment:
      - MYSQL_HOST=db
      - MYSQL_PORT=3306
      - MYSQL_DATABASE=orangehrm
      - MYSQL_USERNAME=orangehrm
      - MYSQL_PASSWORD=orangehrm_password
    volumes:
      - orangehrm_data:/var/www/html
    depends_on:
      - db
    networks:
      - orangehrm-network

networks:
  orangehrm-network:
    driver: bridge

volumes:
  db_data:
  orangehrm_data:
```

3. Open a terminal in the folder and run:
```
docker-compose up -d
```

4. Wait about 1-2 minutes for everything to initialize

5. Access OrangeHRM at: http://localhost:8080

## Login Credentials

After installation, use these default login credentials:
- Username: `Admin`
- Password: `admin123`

## Troubleshooting

If you encounter any errors, try resetting everything:
```
docker-compose down -v
docker-compose up -d
```
