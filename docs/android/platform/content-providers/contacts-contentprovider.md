---
title: Usando o ContentProvider de contatos
ms.prod: xamarin
ms.assetid: 21C5D1B4-3783-6090-33AB-78A484E65925
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/22/2018
ms.openlocfilehash: fca57b7af34ae2b28dda9bf20a95183138cbc641
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020538"
---
# <a name="using-the-contacts-contentprovider"></a>Usando o ContentProvider de contatos

O código que usa dados de acesso expostos por um `ContentProvider` não requer uma referência à classe `ContentProvider`. Em vez disso, um URI é usado para criar um cursor sobre os dados expostos pelo `ContentProvider`. O Android usa o URI para pesquisar no sistema o aplicativo que expõe um `ContentProvider` com esse identificador. O URI é uma cadeia de caracteres, normalmente em um formato DNS reverso, como `com.android.contacts/data`.

Em vez de fazer com que os desenvolvedores se comentem com essa cadeia de caracteres, o provedor de *contatos* do Android expõe seus metadados na classe `android.provider.ContactsContract`. Essa classe é usada para determinar o URI do `ContentProvider`, bem como os nomes das tabelas e colunas que podem ser consultadas.

Alguns tipos de dados também exigem permissão especial para acesso. A lista de contatos internos requer a permissão `android.permission.READ_CONTACTS` no arquivo **AndroidManifest. xml** .

Há três maneiras de criar um cursor a partir do URI:

1. **ManagedQuery ()** &ndash; a abordagem preferida no Android 2,3 (API nível 10) e anterior, uma `ManagedQuery` retorna um cursor e também gerencia automaticamente a atualização dos dados e o fechamento do cursor. Esse método é preterido no Android 3,0 (nível de API 11).

1. **ContentResolver retornem. Query ()** &ndash; retorna um cursor não gerenciado, o que significa que ele deve ser atualizado e fechado explicitamente no código.

1. **CursorLoader (). LoadInBackground ()** &ndash; introduzido no Android 3,0 (API nível 11), `CursorLoader` agora é a maneira preferida de consumir uma `ContentProvider`. `CursorLoader` consulta uma `ContentResolver` em um thread em segundo plano para que a interface do usuário não seja bloqueada.
   Essa classe pode ser acessada em versões mais antigas do Android usando a biblioteca de compatibilidade v4.

Cada um desses métodos tem o mesmo conjunto básico de entradas:

- **Uri** &ndash; o nome totalmente qualificado do `ContentProvider`.
- **Projeção** &ndash; especificação de quais colunas selecionar para o cursor.
- A **seleção** &ndash; semelhante a uma cláusula SQL `WHERE`.
- **SelectionArgs** &ndash; parâmetros a serem substituídos na seleção.
- **SortOrder** &ndash; colunas para classificar.

## <a name="creating-inputs-for-a-query"></a>Criando entradas para uma consulta

O código de exemplo `ContactsProvider` executa uma consulta muito simples no provedor de contatos interno do Android. Você não precisa saber o URI ou os nomes de coluna reais – todas as informações necessárias para consultar o `ContentProvider` de contatos está disponível como constantes expostas pela classe `ContactsContract`.

Independentemente de qual método é usado para recuperar o cursor, esses mesmos objetos são usados como parâmetros, conforme mostrado no arquivo *contactprovider/ContactsAdapter. cs* :

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName,
   ContactsContract.Contacts.InterfaceConsts.PhotoId,
};
```

Para este exemplo, o `selection`, `selectionArgs` e `sortOrder` serão ignorados definindo-os como `null`.

## <a name="creating-a-cursor-from-a-content-provider-uri"></a>Criando um cursor a partir de um URI do provedor de conteúdo

Depois que os objetos de parâmetro tiverem sido criados, eles poderão ser usados de uma das três maneiras a seguir:

### <a name="using-a-managed-query"></a>Usando uma consulta gerenciada

Os aplicativos destinados ao Android 2,3 (API nível 10) ou anterior devem usar este método:

```csharp
var cursor = activity.ManagedQuery(uri, projection, null, null, null);
```

Esse cursor será gerenciado pelo Android para que você não precise fechá-lo.

### <a name="using-contentresolver"></a>Usando ContentResolver retornem

Acessar `ContentResolver` diretamente para obter um cursor em relação a um `ContentProvider` pode ser feito da seguinte maneira:

```csharp
var cursor = activity.ContentResolver(uri, projection, null, null, null);
```

Esse cursor não é gerenciado, portanto, ele deve ser fechado quando não for mais necessário.
Certifique-se de que o código Feche um cursor que esteja aberto, caso contrário, ocorrerá um erro.

```csharp
cursor.Close();
```

Como alternativa, você pode chamar `StartManagingCursor()` e `StopManagingCursor()` para ' Gerenciar ' o cursor. Cursores gerenciados são automaticamente desativados e consultados novamente quando as atividades são interrompidas e reiniciadas.

### <a name="using-cursorloader"></a>Usando CursorLoader

Os aplicativos criados para Android 3,0 (API nível 11) ou mais recente devem usar este método:

```csharp
var loader = new CursorLoader (activity, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
```

O `CursorLoader` garante que todas as operações de cursor sejam feitas em um thread em segundo plano e possa reutilizar de forma inteligente um cursor existente nas instâncias de atividade quando uma atividade for reiniciada (por exemplo, devido a uma alteração de configuração), em vez de recarregar os dados novamente.

Versões anteriores do Android também podem usar a classe `CursorLoader` usando as [bibliotecas de suporte v4](https://developer.android.com/tools/support-library/index.html).

## <a name="displaying-the-cursor-data-with-a-custom-adapter"></a>Exibindo os dados do cursor com um adaptador personalizado

Para exibir a imagem de contato, usaremos um adaptador personalizado, para que possamos resolver manualmente a referência de `PhotoId` para um caminho de arquivo de imagem.

Para exibir dados com um adaptador personalizado, o exemplo usa uma `CursorLoader` para recuperar todos os dados de contato em uma coleção local no método **FillContacts** de **contactprovider/ContactsAdapter. cs**:

```csharp
void FillContacts ()
{
   var uri = ContactsContract.Contacts.ContentUri;
   string[] projection = {
       ContactsContract.Contacts.InterfaceConsts.Id,
       ContactsContract.Contacts.InterfaceConsts.DisplayName,
       ContactsContract.Contacts.InterfaceConsts.PhotoId
  };
   // CursorLoader introduced in Honeycomb (3.0, API11)
   var loader = new CursorLoader(activity, uri, projection, null, null, null);
   var cursor = (ICursor)loader.LoadInBackground();
   contactList = new List<Contact> ();
   if (cursor.MoveToFirst ()) {
      do {
          contactList.Add (new Contact{
              Id = cursor.GetLong (cursor.GetColumnIndex (projection [0])),
              DisplayName = cursor.GetString (cursor.GetColumnIndex (projection [1])),
              PhotoId = cursor.GetString (cursor.GetColumnIndex (projection [2]))
          });
       } while (cursor.MoveToNext());
   }
}
```

Em seguida, implemente os métodos do BaseAdapter usando a coleção de `contactList`. O adaptador é implementado da mesma forma que seria com qualquer outra coleção &ndash; não há nenhum tratamento especial aqui porque os dados são originados de um `ContentProvider`:

```csharp
Activity activity;
public ContactsAdapter (Activity activity)
{
   this.activity = activity;
   FillContacts ();
}
public override int Count {
   get { return contactList.Count; }
}
public override Java.Lang.Object GetItem (int position)
{
  return null; // could wrap a Contact in a Java.Lang.Object to return it here if needed
}
public override long GetItemId (int position)
{
   return contactList [position].Id;
}
public override View GetView (int position, View convertView, ViewGroup parent)
{
   var view = convertView ?? activity.LayoutInflater.Inflate (Resource.Layout.ContactListItem, parent, false);
   var contactName = view.FindViewById<TextView> (Resource.Id.ContactName);
   var contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
   contactName.Text = contactList [position].DisplayName;
   if (contactList [position].PhotoId == null) {
       contactImage = view.FindViewById<ImageView> (Resource.Id.ContactImage);
       contactImage.SetImageResource (Resource.Drawable.ContactImage);
   } else {
       var contactUri = ContentUris.WithAppendedId (ContactsContract.Contacts.ContentUri, contactList [position].Id);
       var contactPhotoUri = Android.Net.Uri.WithAppendedPath (contactUri, Contacts.Photos.ContentDirectory);
       contactImage.SetImageURI (contactPhotoUri);
   }
   return view;
}
```

A imagem é exibida (se existir) usando o URI para o arquivo de imagem no dispositivo. O aplicativo tem esta aparência:

[![captura de tela do aplicativo exibindo contatos em um ListView; uma imagem é exibida à esquerda de uma entrada](contacts-contentprovider-images/contactsprovider.png)](contacts-contentprovider-images/contactsprovider.png#lightbox)

Usando um padrão de código semelhante, seu aplicativo pode acessar uma ampla variedade de dados do sistema, incluindo fotos, vídeos e música do usuário.
Alguns tipos de dados exigem permissões especiais para serem solicitadas no **AndroidManifest. xml**do projeto.

## <a name="displaying-the-cursor-data-with-a-simplecursoradapter"></a>Exibindo os dados do cursor com um SimpleCursorAdapter

O cursor também pode ser exibido com um `SimpleCursorAdapter` (embora apenas o nome seja exibido, não a foto). Este código demonstra como usar um `ContentProvider` com `SimpleCursorAdapter` (esse código não aparece no exemplo):

```csharp
var uri = ContactsContract.Contacts.ContentUri;
string[] projection = {
   ContactsContract.Contacts.InterfaceConsts.Id,
   ContactsContract.Contacts.InterfaceConsts.DisplayName
};
var loader = new CursorLoader (this, uri, projection, null, null, null);
var cursor = (ICursor)loader.LoadInBackground();
var fromColumns = new string[] {ContactsContract.Contacts.InterfaceConsts.DisplayName};
var toControlIds = new int[] {Android.Resource.Id.Text1};
adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlsIds);
listView.Adapter = adapter;
```

Consulte os [ListViews e adaptadores](~/android/user-interface/layouts/list-view/index.md) para obter mais informações sobre como implementar `SimpleCursorAdapter`.

## <a name="related-links"></a>Links relacionados

- [Demonstração do ContactsAdapter (exemplo)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
