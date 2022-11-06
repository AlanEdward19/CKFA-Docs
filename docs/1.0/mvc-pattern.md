# _MVC (Model View Controller)_

![EstruturaMVC](../assets/img/EstruturaMVC.png)

## 1.0 Beneficios deste padrão de codigo
Por utilizar este padrão de arquitetura, você se submete aos seguintes benefícios:  
  - Segurança: A controller funciona como uma forma de filtro que consegue impedir que qualquer dado incorreto chegue até a *Model*.  
  - Organização: Este padrão de arquitetura, permite que qualquer um que vá ler o código tenha muito mais facilidade em entender o que foi construído.  
  - Eficiência: Por conta da aplicação ser repartida em 3 camadas, fica mais leve e facil, permitindo que varios programadores trabalhem no mesmo projeto de forma independente.  

## 2.0 Estrutura
É repartida em 3 partes sendo elas:

Model -> Logica de negocio, entidades.

View -> Aparência.

Controller -> Intermedia informações entre as outras camadas.

### 2.1 Model
Também conhecida como _Business Object Model_. Tem como responsabilidade gerenciar e controlar a forma como os dados se comportam, por funções, logicas e regras de negócio estabalecidas.

### 2.2 View
Responsável por apresentar as informações de maneira visual para o usuário, neste camada devem ser aplicadas somente recursos ligados a aparencia. *_(Frontend)_*

### 2.3 Controller
Reponsável por intermediar requisições enviadas pela *View*, com as repostas fornecidas pela *Model*, processando as informações enviadas pelo usuário e compartilhando para as outras camadas.