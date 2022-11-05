# Padrão de estruturas

## 1.0 Repository
Reponsavel por logica de banco de dados, sejam CRUD, ou algo relacionado.

## 2.0 Classe de Serviço
Logica de negocio, chama repository caso necessario.

## 3.0 Camadas
Domain -> Coração do programa, classes, variaveis, repository é declarado aqui (sua interface tbm), services
Infra -> Conexão com banco, contexts, e integra a domain para acessar a repository
Presentation -> Interação do usuario.