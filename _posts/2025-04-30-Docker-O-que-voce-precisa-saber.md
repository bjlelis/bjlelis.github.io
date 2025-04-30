
# O que você 'precisa' saber sobre Docker

## 1. Fundamentos do Docker e Containerização

Longe de mim querer ditar o que você deve ou não aprender. Humildemente me coloco como alguem disposto a compartilhar o que venho aprendendo. E containerização é um dos temas. Então vamos lá. O docker é uma plataforma que permite empacotar aplicações e suas dependências em **containers** — unidades leves, portáveis e consistentes que podem ser executadas em qualquer ambiente que tenha o Docker instalado.

### O que é containerização?
- É o processo de empacotar código e suas dependências para que a aplicação possa rodar rapidamente e de forma confiável em qualquer ambiente. Elimina aquele papo de 'na minha máquina funciona'.
- Os containers compartilham o mesmo kernel do sistema operacional, tornando-os mais leves que VMs.

## 2. Vantagens do Docker
- **Portabilidade**: Roda em qualquer lugar (dev, test, prod).
- **Rapidez**: Inicialização quase instantânea.
- **Isolamento**: Aplicações independentes e isoladas umas das outras.
- **Escalabilidade**: Fácil de replicar containers.
- **Consistência entre ambientes**: Lembra do 'na minha máquina funciona?', então, você pode migrar seus containers para outros ambientes e eles irão funcionar muito bem.

## 3. Principais Comandos Docker
```
# Verificar versão do docker instalada:
$ docker --version

# Listar apenas os containers ativos:
$ docker ps

# Listar TODOS os containers: 
$ docker ps -a

# Iniciar um container com nginx:
$ docker run -d -p 8080:80 nginx

# Acessar o terminal de um container
$ docker exec -it <container_id> /bin/bash

# Parar, iniciar e remover containers
$ docker stop <id> | docker start <id> | docker rm <id>

# Listar imagens baixadas:
$ docker images

# Remover imagem baixada 
$ docker rmi <image_id>
```

## 4. Dockerfile: Construindo Imagens Personalizadas

```dockerfile
# Dockerfile básico para app Node.js
FROM node:18               # imagem base que será usada noo container
WORKDIR /app               # Diretporio de trabalho. Todos os comandos serão executados dentro dessa pasta
COPY package*.json ./      # Copia do host para o container
RUN npm install            # Instala as dependências listadas no json usando npm
COPY . .                   # Copia tudo do diretório atual do host para o container
EXPOSE 3000                # Mostra que o container vai escutar na porta 3000
CMD ["node", "index.js"]   # Comando padrão que será executado quando o container iniciar
```

## 5. Multi-Staging e Layer Caching

### Multi-Staging:
Evita que dependências de build fiquem na imagem final. Torna a imagem mais leve e segura, diminuindo a superfície de ataque.
```dockerfile
# Etapa de build
FROM node:18 as builder              # nomeia a imagem que será usada na etapa de build e dá um alias a ela de 'as'
WORKDIR /app                         # Define o diretório de trabalho
COPY . .                             # Copia do host para o container
RUN npm install && npm run build     # Instala as dependências e compila a aplicação

# Etapa final
FROM nginx:alpine                                        # Define a imagem alpine, leve.
COPY --from=builder /app/dist /usr/share/nginx/html      # Copia o conteúdo gerado pela etapa anterior para o diretório dentro do container
```

### Layer Caching:
Cada instrução no Dockerfile cria uma camada. Se nada mudar, a camada é reutilizada para acelerar o build.

## 6. Docker Compose

Permite orquestrar múltiplos containers com um único arquivo YAML. Útil para cenários de testes, desenvolvimento e laboratórios
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

```
# Subir containers com Compose
$ docker-compose up -d
# Parar
$ docker-compose down
```

## 7. Volumes

Os containers são efêmeros, os dados se perdem quando ele é destruído. O volueme permite persistência de dados entre execuções de containers.
```
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

Montagem direta de um diretório do host no container. Útil para testes, não recomendado para produção, ja que acessa um espaço no host e mapeia no container.
```
$ docker run -v $(pwd):/app meu-container
```

## 10. Segurança e Boas Práticas

- Use imagens oficiais, verificadas ou customizadas por sua equipe.
- Mantenha imagens atualizadas.
- Minimize o uso de root no container. Crie e use um usuário.
- Use Docker secrets para senhas e chaves.
- Reduza o tamanho da imagem com `alpine` ou multi-staging.
- Faça scan de vulnerabilidades com ferramentas como o `Trivy`.

## 11. Docker Registry (Hub e Alternativas)

- **Docker Hub**: Principal registry público.
- **Harbor**: Registry privado.
- **Amazon ECR / Azure ACR / Google GCR**: Registries gerenciados em nuvem.

```
# Login no Docker Hub
$ docker login
# Taggear imagem
$ docker tag minha-imagem user/minha-imagem:latest
# Enviar imagem
$ docker push user/minha-imagem:latest
```

Com esse material, tenho documentado tudo o que venho aprendendo sobre Docker, com exemplos práticos para cada etapa. Ainda há muito o que explorar, como Kubernetes e ferramentas como Helm e Skaffold, mas esse é um ótimo ponto de partida.
