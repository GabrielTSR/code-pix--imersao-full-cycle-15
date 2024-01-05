# Full Stack & Full Cycle Immersion - CodePix Repository
![Imersão Full Stack && Full Cycle](https://events-fullcycle.s3.amazonaws.com/events-fullcycle/static/site/img/grupo_4417.png)

Imersão Full Stack & Full Cycle, um evento educacional que explora o desenvolvimento full stack com foco em microsserviços, utilizando tecnologias modernas como Go, Nest.js, Next.js, Apache Kafka, Docker e muito mais.

## Sobre a Imersão
A Imersão Full Stack & Full Cycle - CodePix é uma oportunidade de aprendizado prático, onde os participantes podem construir um projeto completo de microsserviços simulando transações bancárias entre diferentes bancos. Este repositório contém uma série de READMEs detalhados que orientam os participantes através de diferentes aspectos do projeto, desde a configuração inicial até a execução e possíveis soluções para erros comuns.

---
## Principais Componentes
Frontend (Next.js): Código fonte e instruções para executar o frontend do projeto.
API (Nest.js): Instruções para configurar e executar a API desenvolvida com Nest.js.
Apache Kafka: Configurações e informações para rodar o Apache Kafka, essencial para a mensageria entre os microsserviços.
Microsserviço CodePix (Golang): Detalhes sobre o microsserviço central, incluindo como executar e possíveis erros com soluções.

---

### Alterações no `Dockerfile - Go`:

Vamos atualizar a versão do `Go` para a `golang:1.19`, desta forma os pacotes instalados serão compatíveis entre eles.

Durante a aula 01 o Wesley configura o `Dockerfile` com a instalação do `cobra` que vamos utilizar mais a frente no curso, mas há uma alteração a ser feita na instalação desta ferramenta como abaixo:

Precisamos substituir e incluir o comando de instalação do `cobra-cli`: `go get github.com/spf13/cobra-cli@v1.3.0 && \`

Após realizar as alterações acima o `Dockerfile` ficará da seguinte forma:

```docker
FROM golang:1.19

WORKDIR /go/src
ENV PATH="/go/bin:${PATH}"
ENV GO111MODULE=on
ENV CGO_ENABLED=1

RUN apt-get update && \
    apt-get install build-essential protobuf-compiler librdkafka-dev -y && \
    go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.3.0 && \
    go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.31.0 && \
    go install github.com/spf13/cobra-cli@v1.3.0 && \
    wget https://github.com/ktr0731/evans/releases/download/0.9.1/evans_linux_amd64.tar.gz && \
    tar -xzvf evans_linux_amd64.tar.gz && \
    mv evans ../bin && rm -f evans_linux_amd64.tar.gz

CMD ["tail", "-f", "/dev/null"]
```

Dentro do arquivo `go.mod` apague todo o conteúdo de `require` e no terminal rode o comando abaixo para instalar todas as depêndencias:

`go mod init`

Com isso a aplicação deve funcionar corretamente.

---

### Alteração de comando - Aula 02 - Imersão - gRPC e o abismo entre devs e empresas

Como explicado acima, precisamos incluir o `cobra-cli` nas instalações internas do `container`, por isso o comando para iniciar o `cobra` será: `cobra-cli init`
