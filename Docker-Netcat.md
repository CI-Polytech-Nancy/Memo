# Réaliser un docker pour un listener Python

D'abord, il faut installer `docker-compose` :
```bash
# Debian
sudo apt install docker-compose

# Fedora
sudo dnf install docker-compose
```

Ensuite, on crée notre script python `listen.py` :
```python
from os import getenv
from threading import Thread
from socket import AF_INET, SOCK_STREAM, socket


def listen(conn):
    while True:
        ans = conn.recv(1024).decode()
        conn.send(ans.encode());


if __name__ == "__main__":
    port = int(getenv("LISTEN_PORT"))
    sock = socket(AF_INET, SOCK_STREAM)

    sock.bind(("", port))
    sock.listen(1)

    while True:
        conn, _ = sock.accept()
        Thread(target=listen, args=[conn]).start()
```

Puis, il faut créer les fichiers docker :
- `Dockerfile`
```
FROM python:latest

COPY listen.py .
CMD python listen.py
```
- `docker-compose.yml`
```
version: "3"
services:
  netcat-listener:
    build: .
    ports:
      - "<PORT>:<PORT>"
    environment:
      - LISTEN_PORT=<PORT>
```

Enfin, on peut démarrer notre docker :
```bash
sudo docker-compose build
sudo docker-compose up -d
```
