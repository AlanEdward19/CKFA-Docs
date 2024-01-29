# Factory Method Pattern

## O Problema
- Uso demasiado de estruturas condicionais para criação de objetos.
- Seu código precisa utilizar muito estruturas condicionais para decidir qual instância de classe utilizar.
- A cada nova classe a ser utilizada, mais uma condicional será adicionada, e a classe ficará cada vez maior e complexa.

### Exemplos
- Tipos de notificação (e-mail, SMS, etc)
- Meios de pagamento (Cartão, Boleto, Saldo em conta)

## Funcionamento

Disponibiliza uma interface para criação dos objetos em uma superclasse, chamada Factory, deixando a ela a função de decidir qual tipo de objeto a ser criado, e a cada novo objeto inserido na logica, só deve atualizar a superclasse, não impactando quem a usa.

## Codigo de exemplo

[Factory Template Method](https://github.com/AlanEdward19/FactoryMethodTemplate)