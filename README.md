# Criação de Imagem para Aplicação em Container

Este guia mostra como containerizar um website estático (HTML, CSS e
JavaScript) utilizando **Docker** e um **Dockerfile**.

## Por que utilizar containerização?

-   **Portabilidade**: O site funcionará de forma idêntica em qualquer
    ambiente.\
-   **Isolamento**: Evita problemas de compatibilidade entre máquinas.\
-   **Escalabilidade**: Permite evoluir para cenários mais complexos.\
-   **Padrão de mercado**: Docker é amplamente adotado na indústria.

## Ferramentas necessárias

### 1. Docker

-   **Windows/Mac**: Instale via [Docker
    Desktop](https://www.docker.com/products/docker-desktop)\
-   **Linux**: Instale com:\

``` bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Verifique a instalação:

``` bash
docker --version
```

### 2. Editor de Código

-   Recomendado: [Visual Studio Code](https://code.visualstudio.com/)\
-   Extensões úteis: *Docker* e *AWS Toolkit*

------------------------------------------------------------------------

## Fase 1: Preparação do ambiente

### Passo 1.1: Estrutura do projeto

Acesse o diretório do projeto:

``` bash
cd caminho/para/seu/projeto
ls -la
```

Certifique-se de que a pasta `website/` existe com os arquivos do site.

### Passo 1.2: Testar localmente (opcional)

Abra o `index.html` no navegador:

``` bash
# Mac
open website/index.html

# Linux
xdg-open website/index.html

# Windows PowerShell
start website/index.html
```

------------------------------------------------------------------------

## Fase 2: Containerização com Docker

### Passo 2.1: Criar o Dockerfile

Na raiz do projeto:

``` bash
touch Dockerfile
```

### Passo 2.2: Conteúdo do Dockerfile

``` dockerfile
FROM nginx:alpine
COPY website/ /usr/share/nginx/html/
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

**Explicação rápida:** - `FROM`: Define a imagem base (Nginx em Alpine
Linux).\
- `COPY`: Copia arquivos do site para dentro do container.\
- `EXPOSE`: Documenta a porta exposta.\
- `CMD`: Comando executado ao iniciar o container.

### Passo 2.3: Construir a imagem

``` bash
docker build -t meu-website:v1.0 .
```

-   `-t`: Define nome e versão da imagem.\
-   `.`: Usa o diretório atual como contexto.

### Passo 2.4: Listar imagens criadas

``` bash
docker images
```

------------------------------------------------------------------------

## Fase 3: Teste do container

### Passo 3.1: Executar

``` bash
docker run -d -p 8080:80 --name meu-website-container meu-website:v1.0
```

### Passo 3.2: Verificar execução

``` bash
docker ps
```

### Passo 3.3: Acessar no navegador

    http://localhost:8080

### Passo 3.4: Logs (opcional)

``` bash
docker logs meu-website-container
```

### Passo 3.5: Encerrar e remover container

``` bash
docker stop meu-website-container
docker rm meu-website-container
```

------------------------------------------------------------------------
