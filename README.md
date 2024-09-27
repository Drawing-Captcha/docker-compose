
# 🎨 Drawing Captcha App Deployment Guide with Docker 🐳

This guide describes how to install and run the Drawing Captcha App using Docker Compose on your server.

## Prerequisites 📋

- Docker installed: [Docker Installation Guide](https://docs.docker.com/get-docker/)
- Docker Compose installed: [Docker Compose Installation Guide](https://docs.docker.com/compose/install/)

## Docker Compose Setup 🐳

Create a file named `docker-compose.yml` with the following content:

```yaml
version: "3.8"

services:
  dc_node:
    container_name: dc_node
    image: williamspesic/drawing-captcha-app:latest
    ports:
      - "9091:9091"
    networks:
      - dc_network
    depends_on:
      - dc_mongo
    restart: always
    environment:
      - MONGO_URI=${MONGO_URI}
      - PORT=${PORT}
      - SERVER_DOMAIN=${SERVER_DOMAIN}
      - REGISTER_KEY=${REGISTER_KEY}
      - DC_ADMIN_EMAIL=${DC_ADMIN_EMAIL}
      - DC_ADMIN_PASSWORD=${DC_ADMIN_PASSWORD}
  dc_mongo:
    container_name: dc_mongo
    image: mongo:latest
    expose:
      - "27017"
    volumes:
      - drawing-captcha:/data/db
    networks:
      - dc_network
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGO_INITDB_ROOT_USERNAME}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_INITDB_ROOT_PASSWORD}
      MONGO_INITDB_DATABASE: ${MONGO_INITDB_DATABASE}

networks:
  dc_network:

volumes:
  drawing-captcha:

```

## Environment Variables 🌍

Create a  `.env`  file in the same directory and add the following content:
```env
MONGO_INITDB_ROOT_USERNAME=root
MONGO_INITDB_ROOT_PASSWORD=zTU?e5ko798u0$s?3iY4^eUZvv.
MONGO_INITDB_DATABASE=drawing-captcha
```

# For local development
```emv
MONGO_URI="mongodb://localhost:7500/drawing-captcha"
```
# For deployment
```env
MONGO_URI="mongodb://${MONGO_INITDB_ROOT_USERNAME}:${MONGO_INITDB_ROOT_PASSWORD}@dc_mongo:27017/${MONGO_INITDB_DATABASE}?authSource=admin"
```
# Enter your domain where you want to host your Drawing Captcha. Important: include http/https
```env
SERVER_DOMAIN="https://yourdomain.com"
# Port of your server
PORT=9091
```
# This will automatically be reset:
```env
REGISTER_KEY="&&+%&%ajkhdjhWIIWNw7>dajh2gg"
```
# Change this email!!
```env
DC_ADMIN_EMAIL="your@mail.com"
```
# Change this password!!!
```env
DC_ADMIN_PASSWORD="admin"` 
```
## Starting the Application 🚀

1.  Navigate to the directory containing the  `docker-compose.yml`  file.
    
2.  Run the following command to start the containers:
    
    `docker-compose up -d` 
    
3.  The application should now be accessible at  `http://localhost:9091`  (or your specified domain).
    

## Notes 📝

-   Ensure you change the email and password in the  `.env`  file before deploying the app in a production environment.
    
-   Check Docker logs if issues arise:
    
    `docker-compose logs` 
    

## Troubleshooting 🛠️

-   **MongoDB Connection Issues**: Check the  `MONGO_URI`  in the  `.env`  file.
-   **Network Issues**: Ensure the  `dc_network`  is correctly configured.

For further questions or issues, please visit the  [GitHub Repository](https://github.com/Drawing-Captcha/Drawing-Captcha-APP)  for more information.