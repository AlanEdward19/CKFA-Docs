# TDD Pattern (Test Driven Development)

## 1.0 Beneficios deste padrão de codigo
Feedback rápido: como estamos testando novas funcionalidades logo de cara, ja sabemos como ela se comporta antes mesma de acoplarmos ela no código ja existente. 

Código mais limpo: Como estamos escrevendo códigos simples, com intuito de passar no teste, acabamos por ser direto ao ponto.  

Segurança: Conseguimos ter controle no refactoring e correção de bugs.  
Produtividade: Já que estamos testando a funcionalidade diretamente, conseguimos encontrar menos bugs, e caso encontre, podem ser resolvidos de maneira rapida.  

## 2.0 O que é?
*TDD* é um padrão de desenvolvimento orientado por testes. Onde desenvolvemos nosso software baseado em testes, que são escritos antes mesmo da propria função ser implementada.  

## 3.0 Como funciona?
Este padrão de desenvolvimento se baseia em pequenos ciclos de repetições, que segue basicamente a seguinte sequência:  

-  Criação de Teste para nova funcionalidade.  
-  Teste falha (pois ainda não existe a funcionalidade).  
-  Criação e implementação da funcionalidade.  
-  Teste passa.  
-  Refatoração de código.  

## 4.0 Por quê refatoração?
Basicamente quando fazemos a função baseada em um teste, nosso objetivo é que a nova funcionalidade, seja feita com escopo de passar no teste. Mas nem sempre isso significa que ela esta adequada, ou até mesmo seguindo boas práticas.  
Por conta disto, fazemos uma refatoração, com o intuito de ainda passar no teste, porém ter um código totalmente limpo, coeso e menos acoplado.  