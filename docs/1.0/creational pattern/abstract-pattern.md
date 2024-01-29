# Abstract Pattern

## O Problema
- Uso excessivo de estruturas condicionais para criação de conjutos de objetos, especificos para cada condição.

### Exemplos
- Cálculo de diferentes impostos (FGTS, IR) dependendo do salário de funcionário podem virar diferentes classes, uma por imposto, mas com implementações diferentes para cada faixa salarial.

## Funcionamento

Disponibiliza uma interface, para criação de um conjutno de objetos em uma superclasse, chamado AbstractFactory, deixando a ela a função de decidir qual familia de objetos deve ser criada.  Para cada nova “familia” a ser inserida na logica, só deve atualizar a superclasse, não impactando quem a usa.

## Codigo de exemplo

[Abstract Template Method](https://github.com/AlanEdward19/AbtractFactory)