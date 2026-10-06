# Prática 1 — Flask, Redis e Docker

Este projeto mostra uma página Flask com um contador de acessos. O número é armazenado no Redis, permanecendo disponível mesmo quando os containers são recriados, desde que o volume seja mantido.

```text
Navegador → Flask → Redis → Volume
```

## O que há no projeto

- `app.py`: aplicação Flask e página do contador.
- `Dockerfile`: instruções para criar a imagem da aplicação.
- `docker-compose.yml`: configuração da aplicação, do Redis, da rede e do volume.
- `RESPOSTAS.md`: respostas às perguntas da atividade.
- `Vídeo_Funcionamento_Sistema.mkv`: demonstração do funcionamento.

## Vídeo de demonstração

O vídeo com a demonstração do funcionamento da aplicação também está disponível pelo Google Drive:

[Assistir ao vídeo no Google Drive](https://drive.google.com/file/d/1VfmrJVEIVA_TelDdEZRXVuvFLrzkTlz4/view?usp=sharing)

## Repositório

Repositório da atividade no GitHub:

[Prática 1 — Sistemas Distribuídos](https://github.com/lucasmt01/Pratica-1-Sistemas-Distribuidos)

## Pré-requisito

Instale e inicie o Docker Desktop. Abra o terminal na pasta do projeto, onde estão `app.py` e `docker-compose.yml`.

## Executar com Docker Compose

Construa a imagem e inicie a aplicação e o Redis:

```powershell
docker compose up -d --build
```

Abra [http://localhost:5050](http://localhost:5050). Atualize a página para ver o contador aumentar.

Para conferir os containers, a rede e o volume:

```powershell
docker compose ps
docker network ls
docker volume ls
```

## Testar se o contador é mantido

1. Acesse a página algumas vezes e observe o número.
2. Pare e remova os containers:

```powershell
docker compose down
```

3. Inicie-os novamente:

```powershell
docker compose up -d
```

4. Acesse a página novamente. O contador continua de onde parou, pois o Redis utiliza um volume para persistência dos dados.

Não use `docker compose down -v` nesse teste, pois a opção `-v` remove também o volume e os dados armazenados.

## Alterar a mensagem da página

Edite o título em `app.py`, salve o arquivo e reconstrua a aplicação:

```powershell
docker compose up -d --build
```

Atualize a página no navegador para visualizar a alteração. O `--build` é necessário para reconstruir a imagem incluindo as alterações realizadas no código.

## Execução manual dos containers

Também é possível iniciar cada container sem utilizar o Docker Compose. Execute os comandos abaixo na pasta do projeto:

```powershell
docker build -t app-flask .
docker network create app-rede
docker volume create redis-dados
docker run -d --name redis --network app-rede -v redis-dados:/data redis:7-alpine redis-server --appendonly yes
docker run -d --name app --network app-rede -p 5050:5050 -e REDIS_HOST=redis app-flask
```

Abra [http://localhost:5050](http://localhost:5050). O nome `redis` permite que a aplicação encontre o serviço Redis através da rede Docker.

Antes de iniciar a aplicação utilizando o Docker Compose, remova os containers e a rede utilizados na execução manual:

```powershell
docker rm -f app redis
docker network rm app-rede
```

O volume `redis-dados` é mantido.

Para encerrar a execução com Docker Compose mantendo os dados:

```powershell
docker compose down
```
