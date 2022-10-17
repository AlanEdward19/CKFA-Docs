# Principais Comandos do Docker

## O que é Docker?
Docker é um sistema de virtualização não convencional, diferente de uma VM (_virtual machine_), docker utiliza o conceito de Conteiners para isolar um ambiente de desenvolvimento e suas dependencias.  

[O que é container?]("https://www.alura.com.br/artigos/comecando-com-docker?gclid=CjwKCAjw-rOaBhA9EiwAUkLV4n74X9KVsgiQ3_oKylhYbcoFbw_jjs0DP1ybj6L6BlFnmgA7X8-5dhoC-XgQAvD_BwE")

![LogoDocker](../assets/img/DockerLogo.png)

## 1.0 Adquirir uma imagem
Para podermos acessar uma imagem, utilizaremos os seguinte comando:  

Por padrão o docker faz _pull_ de suas imagens através do docker hub

```docker
docker pull "Image name"

ex: docker pull redis
```

## 1.1 Rodar uma imagem
Para podermos rodar uma imagem, utilizaremos os seguinte comando:  

```docker
docker run "Image name"

ex: docker run redis
```

Um comando secundario a esse, seria especificar a porta antes de executar o comando. utilizando o *_-p_*

```docker
docker run -p 6379:6379 redis
```

## 1.2 Enviar uma imagem
Para podermos enviar uma imagem, utilizaremos os seguinte comando:  

```docker
docker pull "Image name"

ex: docker pull redis
```

Este comando é utilizado para enviar uma imagem para o repositorio docker hub.