## Implementando Docker no projeto Nodejs <img align="center" alt="Gui-Docker" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-plain.svg" /><img align="center" alt="Gui-Nodejs" height="30" width="40" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" />

---

Crie um `Dockerfile` na raiz do projeto, com o seguinte conteúdo:

```
FROM node:16.14-alpine

WORKDIR /app

EXPOSE 3001

COPY package.json .

RUN npm install

COPY . .

CMD ["npm", "start"]
```

No `docker-compose.yaml`, insira o seguinte código:

```
version: '3.9'
services:
  node:
    container_name: app
    build: .  # caso o Dockerfile esteja na mesma hierarquia que o compose
    tty: true
    stdin_open: true
    command: bash
    restart: always
    volumes:
      - ./:/app
    ports:
      - 3001:3001
    platform: linux/x86_64
    working_dir: /app
    environment:
      - APP_PORT=3001
      # Caso tenha um db, adicione aqui as chaves de conexão
      - DB_USER=root
      - DB_PASS=123456
      - DB_HOST=db
      - DB_PORT=3306
    healthcheck:
      test: ["CMD", "lsof", "-t", "-i:3001"]
      timeout: 10s
      retries: 5

networks:
  default:
    name: networks_node
```
