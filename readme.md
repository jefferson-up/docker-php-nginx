# Sobre

Neste repositório vamos fazer a criação de uma imagem Docker que embora possa ser utilizada em produção, ainda merece ser aperfeiçoada para permitir realmente o escalonamento da aplicação.

# Conteúdo da Imagem Docker

- <b>PHP</b>, e diversas extensões e Libs do PHP, incluindo php-redis, pgsql, mysql, entre outras.

- <b>Nginx</b>, como proxy reverso/servidor. Por fim de testes é que o Nginx está presente nesta imagem, em um momento de otimização está imagem deixará de ter o Nginx.

- <b>Supervisor</b>, indispensal para executarmos a aplicação PHP e permitir por exemplo a execução de filas e jobs.

- <b>Composer</b>, afinal de contas é preciso baixar as dependências mais atuais toda vez que fomos crontruir uma imagem Docker.

## Certifique-se de estar com o Docker em execução.

```sh
docker ps
```

## Certifique-se de ter o Docker Compose instalado.

```sh
docker compose version
```

## Clone sua aplicação Laravel para a pasta 'app'. Caso a pasta app não existe, crie a pasta.

A listagem de pastas do projeto deve ficar:

```
    app/
    docker/
    .gitignore
    docker-compose.yml
    readme.md
```

## Contruir a imagem Docker, execute:

```sh
docker compose build
```

## Caso não queira utilizar o cache da imagem presente no seu ambiente Docker, então execute:

```sh
docker compose build --no-cache
```

## Para subir a aplicação, execute:

```sh
docker compose up
```

- Para rodar o ambiente sem precisar manter o terminar aberto, execute:

```sh
docker compose up -d
```

## Para derrubar a aplicação, execute:

```sh
docker compose down
```

## Para entrar dentro do Container da Aplicação, execute:

```sh
docker exec -it web bash
```