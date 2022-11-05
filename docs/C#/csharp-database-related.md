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

Este codigo esta disponivel em: [EntityFrameworkKnowleadge](https://github.com/AlanEdward19/EntityFrameworkKnowleadge), e futuramente serão adicionados mais exemplos com entity, em outros tipos de aplicação.  

Caso utilize o codigo acima, lembre-se de alterar a connectionString e criar novas migrations.

OBS: Em casos de retorno de muitos dados, podemos utilizar a função de _no tracking_ do EF Core, para poupar o framework de ter que fazer rastreamento de todas as querys, podemos desabilitar essa função, e com isso aumentar performance
do processo. Ex:

```c#
var no tracking = await _context.TABELA.AsNoTracking().FirstOrDefaultAsync();  //poderia utilizar toListAsync tbm ou qlqr outro metodo de finalizar.  
```

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

#### 1.2.8 Relações

##### 1.2.8.1 Relações 1 para muitos
Por padrão o EF Core, segue uma nomeclatura para relações entre tabelas, por exemplo automaticamente por denominar uma variavel (em uma classe) de Id, ele ja consegue reconhecer que se trata de uma chave primaria, assim como tambem o nome de alguma tabela seguida de Id (para chaves estrangeiras), por boas praticas quando vamos montar uma relação de chave estrangeira, referenciamos a tabela com um modificador virtual, exemplo abaixo.

1 para muitos:
```c#
 public class Team
    {

        public int Id { get; set; }
        public string Name { get; set; }
        public int LeagueId { get; set; }
        public virtual League League { get; set; }

    }
```

Muitos para 1:
```c#
 public class League
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public List<Team> Teams { get; set; }
    }
```

##### 1.2.8.2 Relações muitos para muitos
Em casos onde uma tabela faz referencia de muitos dados, de uma mesma tabela, referenciamos a mesma como uma relação de muitas para muitas, segue o exemplo:

```c#
public class Match : BaseDomainObject
    {
        public int HomeTeamId { get; set; }
        public virtual Team HomeTeam { get; set; }
        public int AwayTeamId { get; set; }
        public virtual Team AwayTeam { get; set; }

        public DateTime Date { get; set; }
    }
```

Nesta classe, ela referencia a varios times, de duas maneiras distintas, sendo eles jogos que o time era o da casa, e jogos que eles eram convidados.

Por conta desta alteração na relação das tabelas, foram modificadas as classes anteriores, e agora estão:

```c#
public class Team : BaseDomainObject
    {

        public string Name { get; set; }
        public int LeagueId { get; set; }
        public virtual League League { get; set; }

        public virtual List<Match> HomeMatches { get; set; }
        public virtual List<Match> AwayMatches { get; set; }

    }
```

```c#
 public class League : BaseDomainObject
    {

        public string Name { get; set; }
        public List<Team> Teams { get; set; }
    
    }
```

Ambas as classes de Times e Ligas, agora herdam de uma classe de objetoBasico, que tem intuito de possuir todos os campos padrão das tabelas, como por exemplo Id

##### 1.2.8.3 Relações 1 para 1
Para exemplificar a relação de um para um, iremos criar um exemplo, onde uma classe de Treinador (Coach), será criada, onde um Time possuí um Treinador, e um Treinador treina somente um Time.

Codigo da classe do Treinador:

```c#
 public class Coach : BaseDomainObject
    {
        public string Name { get; set; }
        public int? TeamId { get; set; }
        public virtual Team Team { get; set; }
    }
```

E por consequencia, devemos referenciar esse treinador na classe de Times:

```c#
public class Team : BaseDomainObject
    {

        public string Name { get; set; }
        public int LeagueId { get; set; }
        public virtual League League { get; set; }
        public virtual Coach  Coach { get; set; }

        public virtual List<Match> HomeMatches { get; set; }
        public virtual List<Match> AwayMatches { get; set; }
    
    }
```

E tambem devemos referenciar essa nova tabela em nosso DbContext, para quando rodarmos a migration, essa tabela e suas relações serem alteradas de maneira correta.

```c#
public DbSet<Coach> Coaches { get; set; }
```

#### 1.2.9 Procedures
A ser add

#### 1.2.10 Raw SQL
Podemos mesmo utilizando _EF Core_, fazer uso de queries passadas manualmente, segue exemplo:

Caso sabemos o tipo que será retornado, e queremos *exatamente*, como está na model:

```c#
public async Task<List<League>> SearchLeagueByNameRawSql(string name)
        {
            var league = await _context.Leagues.FromSqlRaw($"select * from league where name = '{name}'").ToListAsync();
            /* Maneira incorreta, pois desta forma estamos tirando a parametrização, e ficamos suscetíveis a sql injections */

            var league2 = await _context.Leagues.FromSqlRaw("select * from league where name = {0}", name).ToListAsync();
            /* Maneira correta, pois desta forma estamos inserindo a parametrização*/

            var league3 = await _context.Leagues.FromSqlInterpolated($"select * from league where name = {name}").ToListAsync();
            /* Outra forma de utilizarmos query SQL, com entity framework, desta forma ele automaticamente parametriza os valores para nós*/


            return league;
        }
```

No exemplo acima fizemos uso das funções _SqlRaw_ e _SqlInterpolated_, suas diferenças são, que utilizando a primeira forma, devemos passar a parametrização, nos mesmos, afim de evitar possiveis ataques de _Sql injection_, já a segunda opção, ja faz essa trabalho para nós.

Caso sabemos o tipo que será retornado, porém queremos menos colunas, ou até mesmo nem sabemos o que será retornado (sem ter uma model feita para ele):  

```c#

```

#### 1.2.11 Manipulando banco
A ser add

#### 1.2.12 Eager Loading
Quando necessitamos retornar dados, que envolvem multiplas tabelas (joins), precisamos fazer o _Eager Loading_, basicamente _joins_, onde o *_EF core_* consegue fazer automaticamente, somente incluindo o .Include(lambda expression), o Include deve ser posicionado antes de qualquer comando de retorno.

Ex: Retornar informação de qual liga pertence tal time:

Sem Eager Loading
```c#
public async void GeneralTeamSearch()
        {
            var teams = await _context.Teams.ToListAsync();

            foreach (var team in teams)
            {
                var league = await _context.Leagues.Where(league => league.Id == team.LeagueId).FirstOrDefaultAsync();
                team.PrintData(league);
            }
        }
```

Com Eager Loading
```c#
public async void GeneralTeamSearch()
        {
            var teams = await _context.Teams.Include(teamProperty => teamProperty.League).ToListAsync();

            foreach (var team in teams)
            {
                team.PrintData();
            }
        }
```

Eager Loading com multiplas referencias (_Gran Children related record_), é reservado pela palavra  _Then Include_.
```c#
public async void GetTeamsWithMatchesAndOpponents()
        {
            var teams = await _context.Teams.Include(teamProperty => teamProperty.AwayMatches).ThenInclude(teamProperty => teamProperty.HomeTeam).ToListAsync();

            foreach (var team in teams)
            {
                team.PrintData(); // Visto que necessitamos atualizar o metodo PrintData para retornar as novas informações destes joins
            }
        }
```

Tambem podemos utilizar diversos Includes:
```c#
public async void GetTeamsWithMatchesAndOpponents()
        {
            var teams = 
            await _context.Teams.Include(teamProperty => teamProperty.AwayMatches).ThenInclude(teamProperty => teamProperty.HomeTeam)
            .Include(teamProperty => teamProperty.HomeMatches).ThenInclude(teamProperty => teamProperty.AwayTeam).ToListAsync();

            foreach (var team in teams)
            {
                team.PrintData(); // Visto que necessitamos atualizar o metodo PrintData para retornar as novas informações destes joins
            }
        }
```

Exemplo Anterior retornando treinadores:
```c#
public async void GetTeamsWithMatchesAndOpponents()
        {
            var teams = 
            await _context.Teams
            .Include(teamProperty => teamProperty.AwayMatches).ThenInclude(teamProperty => teamProperty.HomeTeam).ThenInclude(teamProperty => teamProperty.Coach)
            .Include(teamProperty => teamProperty.HomeMatches).ThenInclude(teamProperty => teamProperty.AwayTeam).ThenInclude(teamProperty => teamProperty.Coach)
            .ToListAsync();

            foreach (var team in teams)
            {
                team.PrintData(); // Visto que necessitamos atualizar o metodo PrintData para retornar as novas informações destes joins
            }
        }
```

### 1.3 Via Dapper
Nesta seção será demonstrado como fazer CRUD (Acronimo para Create, Read, Update, Delete), em uma Web API, utilizando Dapper.  

#### 1.3.1 Requerimentos
Para utilizar o que vêm a seguir, é necessario:  
  
A instalação dos seguintes pacotes NuGet: Dapper.
