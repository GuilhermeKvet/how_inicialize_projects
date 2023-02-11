## Implementando Docker no projeto Fullstack <img align="center" alt="Gui-React" height="20" width="40" src="https://raw.githubusercontent.com/devicons/devicon/master/icons/react/react-original.svg"><img align="center" alt="Gui-Nodejs" height="20" width="20" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/nodejs/nodejs-original.svg" />

---

Crie um `Dockerfile` na raiz do projeto frontend, com o seguinte conteúdo:

```
FROM node:16.14-alpine

WORKDIR /app-frontend

EXPOSE 3000

COPY package.json .

RUN npm install

COPY . .

CMD ["npm", "start"]
```

Crie um `Dockerfile` na raiz do projeto backend, com o seguinte conteúdo:

```
FROM node:16.14-alpine

WORKDIR /app-backend

EXPOSE 3001

COPY package.json .

RUN npm install

COPY . .

CMD ["npm", "start"]
```

No `docker-compose.yaml` coloque o seguinte código:

```
version: '3.9'
services:
  frontend:
    build: ./frontend
    ports:
      - 3000:3000
    platform: linux/x86_64
    working_dir: /app-frontend
    depends_on:
      backend:
        condition: service_healthy
    # Os `healthcheck` devem garantir que a aplicação
    # está operacional, antes de liberar o container
    healthcheck:
      test: ["CMD", "lsof", "-t", "-i:3000"]  # Caso utilize outra porta interna para o front, altere ela aqui também
      timeout: 10s
      retries: 5
  backend:
    container_name: app_backend
    build: ./backend
    ports:
      - 3001:3001
    platform: linux/x86_64
    working_dir: /app-backend
    depends_on:
      db:
        condition: service_healthy
    environment:
      - APP_PORT=3001
      # Caso tenha um db no projeto
      - DB_USER=root
      - DB_PASS=123456
      - DB_HOST=db
      - DB_PORT=3306
    healthcheck:
      test: ["CMD", "lsof", "-t", "-i:3001"] # Caso utilize outra porta interna para o back, altere ela aqui também
      timeout: 10s
      retries: 5
  db:
    image: mysql:8.0.21
    container_name: db
    platform: linux/x86_64
    ports:
      - 3002:3306
    environment:
      - MYSQL_ROOT_PASSWORD=123456
    restart: 'always'
    healthcheck:
      test: ["CMD", "mysqladmin" ,"ping", "-h", "localhost"]
      timeout: 10s
      retries: 5
    cap_add:
      - SYS_NICE # Deve omitir alertas menores
```
