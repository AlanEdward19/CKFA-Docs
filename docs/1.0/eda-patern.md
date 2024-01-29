#  _EDA (*Event Driven Architecture*)_

## O que é?
- Padrão de comunicação entre sistemas é request-response
- Padrão para orientada a eventos é reação após uma ação.

## Exemplos de funcionalidades que são orientadas a eventos
- Compra em um E-Commerce
- Cadastro de um usuário
- Pagamento de uma assinatura
- Publicação de um texto

Que por sua vez disparam um evento, no caso ações a serem realizadas após a execução destas funcionalidades, tais como:
- Notificação por E-mail ou SMS
- Sincronização de dados com outro sistema
- Notificação de inscritos em uma newsletter ou rede social após alguém se inscrever ou te seguir
- Próxima tarefa de um fluxo, como separar para estoque após um pagamento
- Uma esteira de dados

Em uma arquitetura orientada a eventos é utilizado sistemas de mensageria para a reação gerada por determinadas ações ou neste caso eventos, que são ocasionados por algo em algum dos componentes que aquele aplicação está escutando.

Ótima para arquitetura distribuída, pois reduz acoplamento e melhora resiliência do sistema.

## Em resumo
A arquitetura orientada a eventos (EDA) é um padrão de comunicação entre sistemas em que a reação ocorre após uma ação. 

Exemplos de funcionalidades orientadas a eventos incluem compras em um e-commerce, cadastro de usuários, pagamentos de assinaturas e publicação de textos. 

Essas funcionalidades disparam eventos que podem desencadear ações como notificações por e-mail ou SMS, sincronização de dados com outros sistemas e tarefas subsequentes em um fluxo. A arquitetura orientada a eventos utiliza sistemas de mensageria para lidar com esses eventos, reduzindo o acoplamento e melhorando a resiliência do sistema.