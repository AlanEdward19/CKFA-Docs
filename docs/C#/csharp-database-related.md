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

#### 1.2.2 Abrir conexão
Por padrão toda conexão feita com Entity Framework Core, se baseia em um arquivo context, onde la teremos variaveis
_DbSet_ seguidas de um tipo (classe), que seriam sua referencia a alguma classe criada que representa uma tabela do banco, uma ja existente ou a ser criada.

```c#
namespace EntityFrameworkKnowleadge.Data
{
    public class FootballLeagueDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .UseSqlServer(
                    @"Data Source=LAPTOP-J71CJ04F\SQLEXPRESS;Initial Catalog=EntityTesteBD;Integrated Security=True") //Ta assim para testes, pessima pratica
                /*.LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Command.Name}, LogLevel.Information)
                .EnableSensitiveDataLogging()*/;
        }
        public DbSet<Team> Teams { get; set; }
        public DbSet<League> Leagues { get; set; }
    }
}
```

Aqui, como o exemplo de conexão está sendo via _Console Application_, passamos a _connectionString_ diretamente no código, porem em um exemplo real, ela não deveria estar ali, por medidas de segurança.  
  
Em uma API, ela estaria localizada no userSecrets, ou até mesmo appsettings.json.  

Para darmos continuidade e podermos criar nossos _DbSet_, devemos criar as classes referentes ao mesmos.

```c#
namespace EntityFrameworkKnowleadge.Domain
{
    public class Team
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public int LeagueId { get; set; }
        public virtual League League { get; set; }


        public void PrintData(League league)
        {
            Console.WriteLine($"\nTime com Indentificação: {Id}" +
                              $"\n\t Nome: {Name}" +
                              $"\n\t Pertence a liga: {league.Name}");
        }

    }
```

Veja que aqui, estamos passando o nome das colunas com conotação de sintaxe para C#, porem caso o banco não esteja dessa forma, podemos manter a sintaxe de C#, porem passar uma _Data Annotation_ para mostrarmos ao Entity qual o nome real da coluna no SQL.  

O Entity Framework por padrão, consegue identificar chaves primarias e estrangeiras, através do nome da variavel conter o nome Id, em alguma parte de seu nome, porém quando este não é o caso, podemos passar _Data Annotation_ para especificarmos qual a chave primaria da tabela.

```c#
public class Team
    {
        [Key]
        [Column("Nome Real da coluna")]
        public int Id { get; set; }

        [Column("Nome Real da coluna")]
        public string Name { get; set; }

        [Column("Nome Real da coluna")]
        public int LeagueId { get; set; }
        public virtual League League { get; set; } // <---- Esta variavel é utilizada para dizermos ao entity que a variavel acima é uma referencia a uma chave estrangeira.
    
    }
```

#### 1.2.3 SelectAll
No código a seguir, implementaremos um selectAll (trazer todos os dados, sem especificação)

```c#
 public async void GeneralTeamSearch()
        {
            var teams = await _context.Teams.ToListAsync();

            foreach (var team in teams)
            {
                var league = await _context.Leagues.FirstOrDefaultAsync(league => league.Id == team.Id);

                team.PrintData(league);
            }
        }
```

#### 1.2.4 Select
No código a seguir, implementaremos um select comum, utilizando uma fonte de pesquisa.

```c#
public async Task<Team> NameTeamSearch(string name)
        {
            var team = await _context.Teams.FirstOrDefaultAsync(team => team.Name == name);
            var league = await _context.Leagues.FirstOrDefaultAsync(league => league.Id == team.LeagueId);

            team.PrintData(league);

            return team;
        }
```

#### 1.2.5 Insert
No código a seguir, implementaremos um insert dentro da tabela de times.  

```c#
public async void insertNewTeam(Team team)
        {
            await _context.Teams.AddAsync(team);

            Console.WriteLine($"Inserindo novo time de nome: {team.Name}\nId do Time: {team.LeagueId}");
        }
```

#### 1.2.6 Update
No código a seguir, implementaremos um update dentro da tabela de times. 

```c#
public async void UpdateTeam(Team teamReference)
        {
            Console.WriteLine("Insira o novo nome deste time");
            var newName = Console.ReadLine();

            Console.WriteLine("Insira o Id da liga que este time pertencerá");
            var leagueID = int.Parse(Console.ReadLine());

            var league = await _context.Leagues.FirstOrDefaultAsync(league => league.Id == teamReference.LeagueId);

            Console.WriteLine($"Valores antigos desse time:");
            teamReference.PrintData(league);

            teamReference.Name = newName;
            teamReference.LeagueId = leagueID;

            league = await _context.Leagues.FirstOrDefaultAsync(league => league.Id == teamReference.LeagueId);

            Console.WriteLine($"Novos valores para esse time:");
            teamReference.PrintData(league);

            _context.Teams.Update(teamReference);
        }
```

#### 1.2.7 Delete
No código a seguir, implementaremos um delete dentro da tabela de times.

```c#
public async void DeleteTeam(Team teamReference)
        {
            var league = await _context.Leagues.FirstOrDefaultAsync(league => league.Id == teamReference.LeagueId);

            Console.WriteLine($"Valores antigos desse time:");
            teamReference.PrintData(league);

            Console.WriteLine("Deletando do banco");

            _context.Teams.Remove(teamReference);
        }
```

### 1.3 Via Dapper
Nesta seção será demonstrado como fazer CRUD (Acronimo para Create, Read, Update, Delete), em uma Web API, utilizando Dapper.  

#### 1.3.1 Requerimentos
Para utilizar o que vêm a seguir, é necessario:  
  
A instalação dos seguintes pacotes NuGet: Dapper.
