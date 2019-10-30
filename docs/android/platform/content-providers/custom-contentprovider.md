---
title: Criando um ContentProvider personalizado
description: A seção anterior demonstrou como consumir dados de uma implementação de ContentProvider interna. Esta seção explicará como criar um ContentProvider personalizado e, em seguida, consumir seus dados.
ms.prod: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3e57e0cd2fa87db8035fa68995b69f231151fa09
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020534"
---
# <a name="creating-a-custom-contentprovider"></a>Criando um ContentProvider personalizado

_A seção anterior demonstrou como consumir dados de uma implementação de ContentProvider interna. Esta seção explicará como criar um ContentProvider personalizado e, em seguida, consumir seus dados._

## <a name="about-contentproviders"></a>Sobre o ContentProviders

Uma classe de provedor de conteúdo deve herdar de `ContentProvider`. Ele deve consistir em um armazenamento de dados interno que é usado para responder a consultas e deve expor URIs e tipos de MIME como constantes para ajudar a consumir código para fazer solicitações válidas de dados.

### <a name="uri-authority"></a>URI (autoridade)

`ContentProviders` são acessadas no Android usando um URI. Um aplicativo que expõe um `ContentProvider` define os URIs aos quais ele responderá em seu arquivo **AndroidManifest. xml** . Quando o aplicativo é instalado, esses URIs são registrados para que outros aplicativos possam acessá-los.

No mono para Android, a classe de provedor de conteúdo deve ter um atributo `[ContentProvider]` para especificar o URI (ou URIs) que deve ser adicionado ao **AndroidManifest. xml**.

### <a name="mime-type"></a>Tipo MIME

O formato típico para tipos MIME consiste em duas partes. O Android `ContentProviders` geralmente usa essas duas cadeias de caracteres para a primeira parte do tipo MIME:

1. `vnd.android.cursor.item` &ndash; para representar uma única linha, use a constante `ContentResolver.CursorItemBaseType` no código.

1. `vnd.android.cursor.dir` &ndash; para várias linhas, use a constante `ContentResolver.CursorDirBaseType` no código.

A segunda parte do tipo MIME é específica para seu aplicativo e deve usar um padrão DNS reverso com um prefixo `vnd.`. O código de exemplo usa `vnd.com.xamarin.sample.Vegetables`.

### <a name="data-model-metadata"></a>Metadados do modelo de dados

O consumo de aplicativos precisa construir consultas de URI para acessar diferentes tipos de dados. O URI base pode ser expandido para se referir a uma determinada tabela de dados e também pode incluir parâmetros para filtrar os resultados. As colunas e cláusulas usadas com o cursor resultante para exibir dados também devem ser declaradas.

Para garantir que apenas consultas de URI válidas sejam construídas, é personalizado fornecer as cadeias de caracteres válidas como valores constantes. Isso facilita o acesso ao `ContentProvider`, pois torna os valores detectáveis por meio de auto-completar de código e evita erros de digitação nas cadeias de caracteres.

No exemplo anterior, a classe `android.provider.ContactsContract` expôs os metadados dos dados de contatos. Para nosso `ContentProvider` personalizado, vamos expor apenas as constantes na própria classe.

## <a name="implementation"></a>Implementação

Há três etapas para criar e consumir um `ContentProvider`personalizado:

1. **Crie uma classe de banco de dados** &ndash; implementar `SQLiteOpenHelper`.

2. **Crie uma classe de `ContentProvider`** &ndash; implemente `ContentProvider` com uma instância do banco de dados, os metadados expostos como valores constantes e métodos para acessá-los.

3. **Acesse o `ContentProvider` por meio de seu uri** &ndash; popular um `CursorAdapter` usando o `ContentProvider`, acessado por meio de seu URI.

Conforme discutido anteriormente, `ContentProviders` pode ser consumido de aplicativos diferentes de onde estão definidos. Neste exemplo, os dados são consumidos no mesmo aplicativo, mas lembre-se de que outros aplicativos também podem acessá-lo, desde que eles saibam o URI e informações sobre o esquema (que geralmente são expostos como valores constantes).

## <a name="create-a-database"></a>Criar um banco de dados

A maioria das implementações de `ContentProvider` será baseada em um banco de dados `SQLite`. O código do banco de dados de exemplo em **SimpleContentProvider/VegetableDatabase. cs** cria um banco de dados de duas colunas muito simples, como mostrado:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

A implementação do banco de dados em si não precisa de nenhuma consideração especial para ser exposta com um `ContentProvider`, no entanto, se você pretende associar os dados de `ContentProvider's` a um controle de `ListView`, uma coluna de inteiro exclusiva chamada `_id` deve fazer parte do conjunto de resultados. Consulte o documento [ListViews e adaptadores](~/android/user-interface/layouts/list-view/index.md) para obter mais detalhes sobre como usar o controle de `ListView`.

## <a name="create-the-contentprovider"></a>Criar o ContentProvider

O restante desta seção fornece instruções passo a passo sobre como a classe de exemplo **SimpleContentProvider/VegetableProvider. cs** foi criada.

### <a name="initialize-the-database"></a>Inicializar o banco de dados

A primeira etapa é criar uma subclasse `ContentProvider` e adicionar o banco de dados que ela usará.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

O restante do código irá formar a implementação do provedor de conteúdo real que permite que os dados sejam descobertos e consultados.

## <a name="add-metadata-for-consumers"></a>Adicionar metadados para consumidores

Há quatro tipos diferentes de metadados que vamos expor na classe `ContentProvider`. Somente a autoridade é necessária, o restante é feito por convenção.

- A **autoridade** &ndash; atributo de `ContentProvider` *deve* ser adicionada à classe para que seja registrada com o Android quando o aplicativo for instalado.

- O **Uri** &ndash; o `CONTENT_URI` é exposto como uma constante para que seja fácil de usar no código. Ele deve corresponder à autoridade, mas incluir o esquema e o caminho base.

- **Tipos de MIME** &ndash; listas de resultados e resultados únicos são tratados como tipos de conteúdo diferentes, portanto, definimos dois tipos de MIME para representá-los.

- **InterfaceConsts** &ndash; fornece um valor constante para cada nome de coluna de dados, de modo que o consumo de código possa facilmente descobrir e se referir a eles sem arriscar os erros tipográficos.

Esse código mostra como cada um desses itens é implementado, adicionando à definição de banco de dados da etapa anterior:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

## <a name="implement-the-uri-parsing-helper"></a>Implementar o auxiliar de análise de URI

Como o consumo de código usa URIs para fazer solicitações de um `ContentProvider`, precisamos ser capaz de analisar essas solicitações para determinar quais dados retornar. A classe `UriMatcher` pode ajudar a analisar URIs, depois que ela tiver sido inicializada com os padrões de URI aos quais o `ContentProvider` dá suporte.

O `UriMatcher` no exemplo será inicializado com dois URIs:

1. solicitação de *"com. xamarin. Sample. VegetableProvider/SA"* &ndash; para retornar a lista completa de os meus.

2. *"com. xamarin. Sample. VegetableProvider/sameter/\#"* &ndash; em que o \# é um espaço reservado para um parâmetro numérico (a `_id` da linha no banco de dados). Um espaço reservado para asterisco ("\*") também pode ser usado para corresponder a um parâmetro de texto.

No código, usamos as constantes para se referir a valores de metadados como a autoridade e o caminho de\_BASE. Os códigos de retorno serão usados em métodos que fazem a análise de URI, para determinar quais dados serão retornados.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

Esse código é privado para a classe `ContentProvider`. Consulte a [documentação do UriMatcher do Google](xref:Android.Content.UriMatcher) para obter mais informações.

## <a name="implement-the-querymethod"></a>Implementar o QueryMethod

O método de `ContentProvider` mais simples para implementar é o método `Query`. A implementação a seguir usa o `UriMatcher` para analisar o parâmetro `uri` e chamar o método de banco de dados correto. Se o `uri` contiver um parâmetro de ID, o inteiro será analisado (usando `LastPathSegment`) e usado na consulta de banco de dados.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

O método `GetType` também deve ser substituído. Esse método pode ser chamado para determinar o tipo de conteúdo que será retornado para um determinado URI.
Isso pode dizer ao aplicativo de consumo como lidar com esses dados.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

## <a name="implement-the-other-overrides"></a>Implementar as outras substituições

Nosso exemplo simples não permite a edição ou exclusão de dados, mas os métodos INSERT, Update e Delete devem ser implementados para que sejam adicionados sem uma implementação:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

Isso conclui a implementação básica de `ContentProvider`. Depois que o aplicativo tiver sido instalado, os dados que ele expõe estarão disponíveis dentro do aplicativo, mas também para qualquer outro aplicativo que conheça o URI para fazer referência a ele.

## <a name="access-the-contentprovider"></a>Acessar o ContentProvider

Depois que o `VegetableProvider` tiver sido implementado, acessá-lo é feito da mesma maneira que o provedor de contatos no início deste documento: Obtenha um cursor usando o URI especificado e, em seguida, use um adaptador para acessar os dados.

## <a name="bind-a-listview-to-a-contentprovider"></a>Associar um ListView a um ContentProvider

Para popular um `ListView` com os dados, usamos o URI que corresponde à lista não filtrada de descontinuados. No código, usamos o valor constante `VegetableProvider.CONTENT_URI`, que sabemos que resolve para `com.xamarin.sample.vegetableprovider/vegetables`. Nossa implementação de `VegetableProvider.Query` retornará um cursor que pode ser associado ao `ListView`.

O código no `SimpleContentProvider/HomeScreen.cs` mostra como é simples exibir dados de um `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

O aplicativo resultante é semelhante a este:

[Captura de tela de![de listagem de aplicativos, frutas, flor Buds, legumes, lâmpadas, tubers](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## <a name="retrieve-a-single-item-from-a-contentprovider"></a>Recuperar um único item de um ContentProvider

Um aplicativo de consumo também pode querer acessar linhas únicas de dados, o que pode ser feito por meio da construção de um URI diferente que se refere a uma linha específica (por exemplo).

Use `ContentResolver` diretamente para acessar um único item, criando um URI com o `Id`necessário.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

O método Complete é semelhante a este:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```

## <a name="related-links"></a>Links relacionados

- [SimpleContentProvider (exemplo)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
