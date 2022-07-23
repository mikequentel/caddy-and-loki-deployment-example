# caddy-and-loki-deployment-example

## About

Demonstration of how to more securely deploy Grafana Loki by using Caddy as a reverse proxy with support for TLS and Basic Authentication, all via Docker Compose. 

## Why

Loki does not include support for authentication, and enabling TLS is an undocumented feature. Using Caddy solves these short-comings of Loki.

# Steps

1. Generate server key and certificate for your machine.
2. Run the following in a terminal: `docker run -it caddy sh`
3. A command prompt (shell in Docker) will appear. Enter `caddy hash-password` and provide a password for Loki, then copy the resulting hashed password, and exit the Docker shell. You will use this hashed password to populate the environment variable `LOKI_PASSWORD`
4. Create a file `.env` which will contain environment variables that Docker will use...in this example, the hashed password is for the value "admin":
```
SERVER=mycomputername
SERVER_KEY=mycomputername.key.pem
SERVER_CERT=mycomputername.cert.pem
LOKI_USER=admin
LOKI_PASSWORD=JDJhJDE0JFNrLkt5bjAxSGVTL2tUS2FYLldTak9Vc2ZYQ2YwMWd5d0dnWTdnanFFQmliVko0VGZyLjMu
```
5. Launch Docker via Docker Compose: `docker-compose up -d`
6. To verify, navigate to https://mycomputername:8443/loki/api/v1/query ...a dialog should appear asking you for a username and password.
7. To enable Grafana to use this more secured deployment of Loki, you will need to include Basic Authentication in the Loki datasource settings, as well as remember to include `https` in the URL.
