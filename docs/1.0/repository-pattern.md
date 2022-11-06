# Repository
Um dos padrões de projeto mais utilizado e conhecido no desenvolvimento de software. Esse padrão contribui no isolamento da camada de acesso a dados (DAL) com a camada de negócio, conhecida como camada de domínio.  
  
Ele permite encapsulamento da lógica de acesso a dados, utilizando a injeção de dependencia e proporcionando uma visão mais orientada a objetos.  
  
Com esse _pattern_, aplicamos o principio da *persistencia ignorante*, ou seja nossas entidades (classes), não devem sofrer impactos pela forma em que são persistidas no banco de dados.

## 1.0 Beneficios deste padrão de codigo
Permitir a troca do banco de dados utilizado sem afetar o sistema como um todo.  
  
Código centralizado em um único ponto, evitando duplicidade.  
  
Facilita a implementação de testes unitários.  
  
Diminui o acoplamento entre classes.  
  
Padronização de códigos e serviços.  

## 1.1 Como utilizar esse Pattern
Devemos como primeiro caso, definir nossa entidade (classe)

## 1.2 Criar entidade

```c#
namespace RepositoryPatternExemplo
{
    public class Cliente
    {
        public string Nome { get; private set; }
        public string Sobrenome { get; private set; }
        public string Cargo { get; private set; }
    }
}
```

## 1.3 Criar Interface do repositorio

```c#
namespace RepositoryPatternExemplo.Interfaces
{
    public interface IClienteRepository
    {
        Cliente Get(int IdCliente);
        Cliente GetAll(int IdCliente);
        bool Save(Cliente cliente);
        bool Update(Cliente cliente);
        bool Delete(Cliente cliente);
    }
}
```
Podendo ainda ser substituido por sua versão generica.  

```c#
public interface IEntidadeGenerica<TEntity>
    {
        TEntity Get(int IdCliente);
        TEntity GetAll(int IdCliente);
        bool Save(TEntity cliente);
        bool Update(TEntity cliente);
        bool Delete(TEntity cliente);
    }
```

## 1.3 Criar repositorio

```c#
using RepositoryPatternExemplo;
using RepositoryPatternExemplo.Interfaces;
using System;

namespace RepositoryPatternExemplo.Repository
{
    public class ClienteRepository : IClienteRepository
    {
        public bool Delete(Cliente cliente)
        {
            // Código para deletar um cliente
        }

        public Cliente Get(int IdCliente)
        {
            // Código para obter um cliente pelo Id
        }

        public Cliente GetAll(int IdCliente)
        {
            // Código para obter todos os clientes
        }

        public bool Save(Cliente cliente)
        {
            // Código para salvar um novo cliente
        }

        public bool Update(Cliente cliente)
        {
            // Código para editar um cliente
        }
    }
}
```

## 1.4 Após isso
Agora é so chamar a interface no seu codigo e chamar seus metodos. Para exemplos de codigos com banco em c#, pode acessar a aba de database related, nas linguagens disponiveis.