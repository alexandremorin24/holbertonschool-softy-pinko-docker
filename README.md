# Softy Pinko Docker Project

## Overview

This project showcases a basic microservice web architecture using Docker and Docker Compose. The setup includes a front-end static server, an API back-end server using Flask, a reverse proxy with load balancing using Nginx, and finally scaling the back-end servers.

### Technologies Used

* Docker
* Docker Compose
* Nginx
* Flask (Python)

---

## Project Structure

```
holbertonschool-softy-pinko-docker/
├── task0/                # Basic Ubuntu container echoing Hello, World!
├── task1/                # Flask API container
├── task2/                # Front-end and back-end split
├── task3/                # Front-end fetches API data via JavaScript
├── task4/                # Docker Compose integration
├── task5/                # Add proxy container
├── task6/                # Scaled API (horizontal scaling)
```

---

## Commands Cheat Sheet

### General Docker Commands

```sh
# Build Docker image
docker build -t <image-name> .

# Run a Docker container
# -p maps host:container port
# --rm removes the container after exit
# -it for interactive mode
docker run -p 5252:5252 -it --rm --name <container-name> <image-name>

# Stop all running containers
docker stop $(docker ps -q)

# Remove all stopped containers
docker container prune
```

### Docker Compose

```sh
# Build all services
docker-compose build

# Run all services (specified in docker-compose.yml)
docker-compose up

# Run with scaling
docker-compose up --scale back-end=2

# Shut down all services
docker-compose down
```

---

## Tasks Summary

### Task 0 - Basic Dockerfile

* Base image: `ubuntu:latest`
* Commands:

```Dockerfile
RUN apt-get update && apt-get upgrade -y
CMD echo "Hello, World!"
```

### Task 1 - Back-End API

* Install Python3, pip3, Flask
* Flask app at `/api/hello`, runs on port 5252
* Add CORS support if needed:

```Dockerfile
RUN pip3 install flask flask-cors
```

* `api.py`:

```python
from flask import Flask
from flask_cors import CORS

app = Flask(__name__)
CORS(app)

@app.route('/api/hello')
def hello():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5252)
```

### Task 2 - Front-End

* Use `nginx:latest` as base image
* Clone front-end repo inside `/var/www/html/softy-pinko-front-end`
* Nginx config example (`softy-pinko-front-end.conf`):

```nginx
server {
    listen 9000;
    server_name softy-pinko-front-end;
    location / {
        root /var/www/html/softy-pinko-front-end;
        index index.html;
    }
}
```

### Task 3 - Connecting Front-End and Back-End

* Add to `index.html`:

```html
<h1 id="dynamic-content"></h1>
<script>
$(function() {
    $.ajax({
        type: "GET",
        url: "http://localhost:5252/api/hello",
        success: function(data) {
            $('#dynamic-content').text(data);
        }
    });
});
</script>
```

### Task 4 - Docker Compose

* Example `docker-compose.yml`:

```yaml
version: '3'
services:
  back-end:
    build:
      context: ./back-end
    ports:
      - "5252:5252"

  front-end:
    build:
      context: ./front-end
    ports:
      - "9000:9000"
```

### Task 5 - Proxy Server

* Use Nginx as proxy container
* `proxy.conf`:

```nginx
upstream backend_servers {
    server back-end:5252;
}

server {
    listen 80;

    location / {
        proxy_pass http://front-end:9000;
    }

    location /api {
        proxy_pass http://backend_servers;
    }
}
```

* Add to `docker-compose.yml`:

```yaml
  proxy:
    build:
      context: ./proxy
    ports:
      - "80:80"
    depends_on:
      - front-end
      - back-end
```

* Update JavaScript in `index.html`:

```js
<script>
$(function() {
    $.ajax({
        type: "GET",
        url: "/api/hello",
        success: function (data) {
            $('#dynamic-content').text(data);
        }
    });
});
</script>
```

### Task 6 - Scale API Servers

* In `task6`, run:

```sh
docker-compose up --scale back-end=2 --build
```

* Create a file `2-api-servers.txt` with:

```
docker-compose up --scale back-end=2
```
