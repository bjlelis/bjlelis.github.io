
# Docker: O que você 'precisa' saber

## 1. Fundamentos do Docker e Containerização

Docker é uma plataforma que permite empacotar aplicações e suas dependências em **containers** — unidades leves, portáveis e consistentes que podem ser executadas em qualquer ambiente que tenha o Docker instalado.

### O que é containerização?
- É o processo de empacotar código e suas dependências para que a aplicação possa rodar rapidamente e de forma confiável em qualquer ambiente.
- Os containers compartilham o mesmo kernel do sistema operacional, tornando-os mais leves que VMs.

## 2. Vantagens do Docker
- **Portabilidade**: Roda em qualquer lugar (dev, test, prod).
- **Rapidez**: Inicialização quase instantânea.
- **Isolamento**: Aplicações independentes e isoladas umas das outras.
- **Escalabilidade**: Fácil de replicar containers.
- **Consistência entre ambientes**.

## 3. Principais Comandos Docker
```bash
# Verificar versão
$ docker --version

# Listar containers ativos
$ docker ps

# Listar todos containers (inclusive parados)
$ docker ps -a

# Iniciar um container
$ docker run -d -p 8080:80 nginx

# Acessar o terminal de um container
$ docker exec -it <container_id> /bin/bash

# Parar, iniciar e remover containers
$ docker stop <id> | docker start <id> | docker rm <id>

# Listar imagens
$ docker images

# Remover imagem
$ docker rmi <image_id>
```

## 4. Dockerfile: Construindo Imagens Personalizadas

```dockerfile
# Dockerfile básico para app Node.js
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "index.js"]
```

## 5. Multi-Staging e Layer Caching

### Multi-Staging:
Evita que dependências de build fiquem na imagem final.
```dockerfile
# Etapa de build
FROM node:18 as builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Etapa final
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
```

### Layer Caching:
Cada instrução no Dockerfile cria uma camada. Se nada mudar, a camada é reutilizada para acelerar o build.

## 6. Docker Compose

Permite orquestrar múltiplos containers com um único arquivo YAML.
```yaml
version: '3.9'
services:
  web:
    build: .
    ports:
      - "3000:3000"
  db:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
```

```bash
# Subir containers com Compose
$ docker-compose up -d
# Parar
$ docker-compose down
```

## 7. Volumes

Permitem persistência de dados entre execuções de containers.
```bash
# Criar volume
$ docker volume create meu-volume
# Usar volume
$ docker run -v meu-volume:/dados busybox
```

## 8. Redes Docker

Facilitam a comunicação entre containers.
```bash
# Criar rede
$ docker network create minha-rede
# Rodar container na rede
$ docker run --network minha-rede nome-imagem
```

## 9. Bind Mounts

Montagem direta de um diretório do host no container.
```bash
$ docker run -v $(pwd):/app meu-container
```

## 10. Segurança e Boas Práticas

- Use imagens oficiais ou verificadas.
- Mantenha imagens atualizadas.
- Minimize o uso de root no container.
- Use Docker secrets para senhas e chaves.
- Reduza o tamanho da imagem com `alpine` ou multi-staging.
- Faça scan de vulnerabilidades com ferramentas como `docker scan` ou `Trivy`.

## 11. Docker Registry (Hub e Alternativas)

- **Docker Hub**: Principal registry público.
- **GitHub Container Registry (GHCR)**: Alternativa gratuita.
- **Harbor**: Registry privado.
- **Amazon ECR / Azure ACR / Google GCR**: Registries gerenciados em nuvem.

```bash
# Login no Docker Hub
$ docker login
# Taggear imagem
$ docker tag minha-imagem user/minha-imagem:latest
# Enviar imagem
$ docker push user/minha-imagem:latest
```

## 12. CI/CD com Docker

Pipeline típico (ex: GitHub Actions):
```yaml
name: CI/CD Docker
on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Login no Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u ${{ secrets.DOCKER_USERNAME }} --password-stdin
      - name: Build imagem
        run: docker build -t user/app:latest .
      - name: Push imagem
        run: docker push user/app:latest
```

---

Com esse material, tenho documentado tudo o que venho aprendendo sobre Docker, com exemplos práticos para cada etapa. Ainda há muito o que explorar, como Kubernetes e ferramentas como Helm e Skaffold, mas esse é um ótimo ponto de partida.
