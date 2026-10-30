# Docker Course Projects

Three small projects from a Docker course, covering images, environment variables, container networking and load balancing.

## Projects

- **Go web server:** A small HTTP server packaged in a Debian slim image, with the port set through the `PORT` environment variable.
- **BookBot in a container:** A Python script that analyses a book, built into an image together with its data.
- **Caddy load balancer:** Two Caddy web servers behind a Caddy reverse proxy that splits traffic between them round robin.

## Run them

### Go web server

The Dockerfile copies a prebuilt Linux binary, so build that first (Go 1.25+):

```bash
GOOS=linux GOARCH=amd64 go build -o goserver
docker build -t goserver .
docker run -p 8991:8991 goserver
```

Then open http://localhost:8991.

### BookBot

```bash
docker build -f bookbot.Dockerfile -t bookbot .
docker run bookbot
```

### Caddy load balancer

```bash
docker network create caddytest
docker run -d --network caddytest --name caddy1 -v "$PWD/index1.html:/usr/share/caddy/index.html" caddy
docker run -d --network caddytest --name caddy2 -v "$PWD/index2.html:/usr/share/caddy/index.html" caddy
docker run -d --network caddytest -p 8880:80 -v "$PWD/Caddyfile:/etc/caddy/Caddyfile" caddy
```

Open http://localhost:8880 and refresh. The page switches between server 1 and server 2.
