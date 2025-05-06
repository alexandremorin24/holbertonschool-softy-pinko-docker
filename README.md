## 🧰 Key Docker Commands & Options

Here are the **essential Docker commands and their most common options**, with explanations.

### 🛠️ `docker build`

```bash
docker build -t <image_name>:<tag> <path>
```

| Option         | Meaning                                         |
|----------------|-------------------------------------------------|
| `-t`           | Tags the image with a name and optional tag     |
| `<path>`       | Path to the folder containing the Dockerfile    |

**Example:**

```bash
docker build -t softy-pinko:task1 .
```

---

### ▶️ `docker run`

```bash
docker run [OPTIONS] <image>
```

| Option             | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `--name`           | Assigns a custom name to the container                                      |
| `--rm`             | Automatically removes the container when it exits                           |
| `-it`              | `-i` = interactive, `-t` = allocate terminal (lets you see logs/output)      |
| `-p host:container`| Maps a container port to the host machine                                   |

**Example:**

```bash
docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
```

---

### 📋 `docker images`

List all locally stored Docker images:

```bash
docker images
```

---

### 📋 `docker ps -a`

List all containers (including stopped ones):

```bash
docker ps -a
```

---

### 🧹 Cleanup Commands

Remove all stopped containers:

```bash
docker container prune
```

Remove a specific image:

```bash
docker rmi <image_name>
```

---

## 🔧 Example Tasks

### ✅ Task 0 – Basic Hello World Image

A simple image that echoes a message.

**Dockerfile (task0/Dockerfile):**

```Dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get upgrade -y
CMD echo "Hello, World!"
```

**Build and Run:**

```bash
cd task0
docker build -t softy-pinko:task0 .
docker run -it --rm --name softy-pinko-task0 softy-pinko:task0
```

Expected output:
```
Hello, World!
```

---

### ✅ Task 1 – Flask API in a Container

A basic API using Flask, listening on port 5252.

**api.py (task1/api.py):**

```python
from flask import Flask

app = Flask(__name__)

@app.route('/api/hello')
def hello_world():
    return 'Hello, World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5252)
```

**Dockerfile (task1/Dockerfile):**

```Dockerfile
FROM ubuntu:latest
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3 python3-pip
RUN rm -f /usr/lib/python*/EXTERNALLY-MANAGED
RUN pip3 install flask
WORKDIR /app
COPY ./api.py /app/api.py
CMD ["python3", "api.py"]
```

**Build and Run:**

```bash
cd task1
docker build -t softy-pinko:task1 .
docker run -p 5252:5252 -it --rm --name softy-pinko-task1 softy-pinko:task1
```

**Test in browser or curl:**

```bash
curl http://localhost:5252/api/hello
```

Expected:
```
Hello, World!
```

---

## 🧠 Summary

| Command                      | Purpose                                  |
|-----------------------------|------------------------------------------|
| `docker build`              | Create a Docker image from a Dockerfile  |
| `docker run`                | Start a container from an image          |
| `-p <host>:<container>`     | Expose a container’s port to the host    |
| `--rm`                      | Automatically delete container on exit   |
| `--name <name>`             | Assign a custom name to the container    |
| `-it`                       | Interact with the container via terminal |
| `docker images`             | List all images on the system            |
| `docker ps -a`              | List all containers (even stopped ones)  |
| `docker container prune`    | Remove all stopped containers            |
