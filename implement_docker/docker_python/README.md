## Implementando Docker no projeto Python <img align="center" alt="Gui-Docker" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-plain.svg" /><img align="center" alt="Gui-Python" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" />

---

Crie um `Dockerfile` na raiz do projeto, com o seguinte conteúdo:

```
FROM python:3.9-bullseye #ou alpine

ADD . /app
WORKDIR /app
COPY ./ /app

RUN python3 -m pip install --upgrade pip
RUN pip install --no-cache-dir -r dev-requirements.txt # ou requirements se estiver em produção

CMD ["python", "main.py"]  # comando para ser executado quando o container inciar
```

No `docker-compose.yaml`, insira o seguinte código:

```
version: '3.6'

services:
  python:
    build:
      context: .  #caso o Dockerfile esteja na mesma hierarquia que o compose
    volumes:
      - .:/app
    environment:
      - TEMPLATES_AUTO_RELOAD=True
      - PYTHONUNBUFFERED=1
      - APPLY_TO_ALL=True

```
