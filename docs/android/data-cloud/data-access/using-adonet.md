---
title: Usando o ADO.NET com o Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: 6592bd6d5cf7b78918fa2d020be723d662625e06
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023764"
---
# <a name="using-adonet-with-android"></a>Usando o ADO.NET com o Android

O Xamarin tem suporte interno para o banco de dados SQLite que está disponível no Android e pode ser exposto usando uma sintaxe familiar semelhante a ADO.NET. Usar essas APIs exige que você escreva instruções SQL que são processadas pelo SQLite, como `CREATE TABLE`, `INSERT` e instruções de `SELECT`.

## <a name="assembly-references"></a>Referências de Assembly

Para usar o SQLite de acesso via ADO.NET, você deve adicionar `System.Data` e `Mono.Data.Sqlite` referências ao seu projeto do Android, conforme mostrado aqui:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Referências do Android no Visual Studio](using-adonet-images/image7.png "Referências do Android no Visual Studio") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos) 

![Referências do Android no Visual Studio para Mac](using-adonet-images/image5.png "Referências do Android no Visual Studio para Mac") 

-----

Clique com o botão direito do mouse em **referências > editar referências...** em seguida, clique para selecionar os assemblies necessários.

## <a name="about-monodatasqlite"></a>Sobre o mono. Data. sqlite

Usaremos a classe `Mono.Data.Sqlite.SqliteConnection` para criar um arquivo de banco de dados em branco e, em seguida, criar uma instância `SqliteCommand` objetos que podemos usar para executar instruções SQL no banco de dados.

**Criar um banco de dados em branco** &ndash; chamar o método `CreateFile` com um caminho de arquivo (ou seja, gravável) válido. Você deve verificar se o arquivo já existe antes de chamar esse método, caso contrário, um novo banco de dados (em branco) será criado na parte superior do antigo, e o arquivo antigo será perdido.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` a variável `dbPath` deve ser determinada de acordo com as regras discutidas anteriormente neste documento.

**Criar uma conexão de banco de dados** &ndash; depois que o arquivo de banco de dados do SQLite for criado, você poderá criar um objeto de conexão para acessar os dados. A conexão é construída com uma cadeia de conexão que assume a forma de `Data Source=file_path`, como mostrado aqui:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Como mencionado anteriormente, uma conexão nunca deve ser reutilizada em threads diferentes. Em caso de dúvida, crie a conexão conforme necessário e feche-a quando terminar; Mas lembre-se de fazer isso com mais frequência do que o necessário.

**Criar e executar um comando de banco de dados** &ndash; assim que tivermos uma conexão, podemos executar comandos SQL arbitrários nele. O código a seguir mostra uma instrução `CREATE TABLE` que está sendo executada.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Ao executar o SQL diretamente no banco de dados, você deve tomar as precauções normais para não fazer solicitações inválidas, como tentar criar uma tabela que já existe. Acompanhe a estrutura do seu banco de dados para que você não cause uma `SqliteException` como a **tabela de erro do SQLite [Items] já exista**.

## <a name="basic-data-access"></a>Acesso a dados básico

O código de exemplo *DataAccess_Basic* para este documento é semelhante ao executado no Android:

![Exemplo de ADO.NET do Android](using-adonet-images/image8.png "Exemplo de ADO.NET do Android")

O código a seguir ilustra como executar operações simples do SQLite e mostra os resultados em como texto na janela principal do aplicativo.

Você precisará incluir esses namespaces:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

O exemplo de código a seguir mostra uma interação de banco de dados inteira:

1. Criando o arquivo de banco de dados
2. Inserindo alguns dados
3. Consultando os dados

Essas operações normalmente apareceriam em vários lugares em todo o seu código, por exemplo, você pode criar o arquivo de banco de dados e as tabelas quando seu aplicativo for iniciado pela primeira vez e executar leituras e gravações de dados em telas individuais em seu aplicativo. No exemplo abaixo, foram agrupados em um único método para este exemplo:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Consultas mais complexas

Como o SQLite permite que comandos SQL arbitrários sejam executados em relação aos dados, você pode executar qualquer uma das instruções `CREATE`, `INSERT`, `UPDATE`, `DELETE`ou `SELECT` que desejar. Você pode ler sobre os comandos SQL com suporte pelo SQLite no site do SQLite. As instruções SQL são executadas usando um dos três métodos em um objeto `SqliteCommand`:

- O **ExecuteNonQuery** &ndash; normalmente usado para criação de tabela ou inserção de dados. O valor de retorno para algumas operações é o número de linhas afetadas, caso contrário, é-1.

- **ExecuteReader** &ndash; usado quando uma coleção de linhas deve ser retornada como um `SqlDataReader`.

- **ExecuteScalar** &ndash; recupera um único valor (por exemplo, uma agregação).

### <a name="executenonquery"></a>EXECUTENONQUERY

as instruções `INSERT`, `UPDATE`e `DELETE` retornarão o número de linhas afetadas. Todas as outras instruções SQL retornarão-1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

O método a seguir mostra uma cláusula `WHERE` na instrução `SELECT`.
Como o código está criando uma instrução SQL completa, é necessário tomar cuidado para escapar caracteres reservados, como a aspa (') em vez de cadeias.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

O método `ExecuteReader` retorna um objeto `SqliteDataReader`. Além do método de `Read` mostrado no exemplo, outras propriedades úteis incluem:

- **RowsAffected** &ndash; contagem das linhas afetadas pela consulta.

- **HasRows** &ndash; se alguma linha foi retornada.

### <a name="executescalar"></a>EXECUTESCALAR

Use isso para `SELECT` instruções que retornam um único valor (como uma agregação).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

O tipo de retorno do método de `ExecuteScalar` é `object` &ndash; você deve converter o resultado dependendo da consulta de banco de dados. O resultado pode ser um inteiro de uma consulta `COUNT` ou uma cadeia de caracteres de uma única coluna `SELECT` consulta. Observe que isso é diferente para outros métodos de `Execute` que retornam um objeto leitor ou uma contagem do número de linhas afetadas.

## <a name="related-links"></a>Links relacionados

- [DataAccess básico (exemplo)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess avançado (exemplo)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Receitas de dados do Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Acesso a dados do Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
