# C# - Database related content

## 1.0 Topicos a serem abordados nesta pagina
Aqui encontrará diversos metodos e maneiras de se usar para conectar-se ao banco de dados na linguagem C# 

### 1.1 Via Console Application
Nesta seção será demonstrado como fazer CRUD (Acronimo para Create, Read, Update, Delete), em uma Console Application


#### 1.1.1 Requerimentos
Para utilizar o que vêm a seguir, é necessario:  
 
A instalação dos seguintes pacotes NuGet: Microsoft.Data.SqlClient ou System.Data.SqlClient  
  
Referencia a uma SqlConnection, que nos exemplos a seguir, usaremos o nome de _connection.  

Estaremos acessando uma tabela fictica chamada funcionarios.


#### 1.1.2 Abrir conexão
Primeira coisa que devemos fazer antes de processeguir é abrir a conexão, para assim podermos acessar e manipular nosso banco de dados.  

Estamos alocando o código a seguir dentro de um _try catch_ para podermos isolar possiveis erros.

```c#
try
    {
        Console.WriteLine("Tentando Abrir conexão com banco..");
        
        if (_connection.State == System.Data.ConnectionState.Closed)
            {
                _connection.Open();
            }
                
            Console.WriteLine("Conexão Aberta!");
            Console.Clear();
    }
    
catch (SqlException e)
    {
        Console.WriteLine($"Erro: {e.Message}");
        Console.WriteLine("Pressione qualquer tecla para continuar!");
        Console.ReadKey();
    }
```


#### 1.1.3 SelectAll
No código a seguir, implementaremos um selectAll (trazer todos os dados, sem especificação)

```c#
var strBuilder = new StringBuilder();

strBuilder.Append("select * from Funcionario");

using (SqlCommand command = new SqlCommand(strBuilder.ToString(), _connection))
    {
        reader = command.ExecuteReader();

        while (reader.Read())
            {
                Console.WriteLine($"\nNome: {reader["nome"]}\nCPF: {reader["cpf"]}\nEndereço: {reader["endereco"]}\nTelefone: {reader["telefone"]}\nCargo: {reader["cargo"]}");
            }

            reader.Close();
    }
```


#### 1.1.4 Select
No código a seguir, implementaremos um select comum, utilizando uma fonte de pesquisa.

```c#
var strBuilder = new StringBuilder();

strBuilder.Append($"select * from Funcionario where nome like N'{nome}%';");

using (SqlCommand command = new SqlCommand(strBuilder.ToString(), _connection))
    {
        reader = command.ExecuteReader();

        while (reader.Read())
            {
                Console.WriteLine($"\nNome: {reader["nome"]}\nCPF: {reader["cpf"]}\nEndereço: {reader["endereco"]}\nTelefone: {reader["telefone"]}\nCargo: {reader["cargo"]}");
            }
            
            reader.Close();
    }
```

#### 1.1.5 Insert
No código a seguir, implementaremos um insert dentro da tabela de funcionarios.  

```c#
var strBuilder = new StringBuilder();

strBuilder.Append("insert into Funcionario (nome, cpf, endereco, telefone, cargo) values ");
strBuilder.Append($"(N'{funcionario.nome}',N'{funcionario.cpf}',N'{funcionario.endereco}',N'{funcionario.telefone}',N'{funcionario.cargo}')");

using (SqlCommand command = new SqlCommand(strBuilder.ToString(), _connection))
    {
        command.ExecuteNonQuery();
        Console.WriteLine($"Funcionario {funcionario.nome} Inserido!!");
    }
```

#### 1.1.6 Update
No código a seguir, implementaremos um update dentro da tabela de funcionarios.  

```c#
var strBuilder = new StringBuilder();

strBuilder.Append($"update Funcionario set nome = N'{funcionario.nome}' , cpf = N'{funcionario.cpf}', endereco = N'{funcionario.endereco}', telefone = N'{funcionario.telefone}', cargo = N'{funcionario.cargo}' where nome= N'{nome}';");

using (SqlCommand command = new SqlCommand(strBuilder.ToString(), _connection))
    {
        command.ExecuteNonQuery();
        Console.WriteLine($"Funcionario Antigo nome: {nome}\nFuncionario Novo nome: {funcionario.nome}");
    }
```

#### 1.1.7 Delete
No código a seguir, implementaremos um delete dentro da tabela de funcionarios.  

```c#
var strBuilder = new StringBuilder();

strBuilder.Append($"delete from Funcionario where nome= N'{nome}';");

using (SqlCommand command = new SqlCommand(strBuilder.ToString(), _connection))
    {
        Console.WriteLine($"Funcionario: {nome} foi deletado!!");

        Console.WriteLine("Deseja reverter?");
        var resposta = Console.ReadLine();

        if (resposta.ToLower() == "sim" || resposta.ToLower() == "s")
            {
                Console.WriteLine($"Funcionario: {nome} foi recuperado!!");
            }
        
        else
            {
                command.ExecuteNonQuery();
            }
    }
```

### 1.2 Via Entity Framework
Nesta seção será demonstrado como fazer CRUD (Acronimo para Create, Read, Update, Delete), em uma Web API, utilizando Entity Framework Core.  

#### 1.2.1 Requerimentos
Para utilizar o que vêm a seguir, é necessario:  
  
A instalação dos seguintes pacotes NuGet: Entity Framework Core for SQL Server.

### 1.3 Via Dapper
Nesta seção será demonstrado como fazer CRUD (Acronimo para Create, Read, Update, Delete), em uma Web API, utilizando Dapper.  

#### 1.3.1 Requerimentos
Para utilizar o que vêm a seguir, é necessario:  
  
A instalação dos seguintes pacotes NuGet: Dapper.
