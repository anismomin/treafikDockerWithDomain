# Run Docker Containers On Local Domain Using Treafik Reverse Proxy

# Set Up DNS Resolution
```
version: '3.8'

services:
  traefik:
    image: traefik:v2.5
    command:
      - "--api.insecure=true" # Traefik dashboard, can be removed in production
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
    ports:
      - "80:80"
      - "8080:8080" # Traefik dashboard
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"

  my_service:
    image: your-service-image
    labels:
      - "traefik.http.routers.my_service.rule=Host(`my.recrut.test`)"
      - "traefik.http.services.my_service.loadbalancer.server.port=80"

  admin_service:
    image: your-service-image
    labels:
      - "traefik.http.routers.admin_service.rule=Host(`admin.recrut.test`)"
      - "traefik.http.services.admin_service.loadbalancer.server.port=80"

networks:
  web:
    driver: bridge

```
## Install Docker Desktop
Ensure Docker Desktop is installed on both Mac and Windows.

## Set Up Traefik as a Reverse Proxy

## On Mac:
You can continue using dnsmasq or edit your /etc/hosts file to map .test domains to localhost.

## On Windows:
You’ll need to edit the C:\Windows\System32\drivers\etc\hosts file:

## Copy code
```
127.0.0.1 my.recrut.test
127.0.0.1 admin.recrut.test
```
This allows both systems to resolve .test domains to localhost.


##  Run Docker Compose
```
docker-compose up -d
```

##  Access Your Services
You should now be able to access your services on both Mac and Windows via:

http://my.recrut.test

http://admin.recrut.test


## Summary
This setup with Docker, Traefik, and host file modifications allows you to run microservices locally on both Mac and Windows, using a common .test domain and subdomains. Traefik handles the routing internally, making the setup consistent across different operating systems.