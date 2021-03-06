---
title: Aggiunta della convalida
author: rick-anderson
description: Illustra come aggiungere la convalida alla pagina Razor.
keywords: ASP.NET Core, convalida, DataAnnotations, Razor, pagine Razor
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/validation
ms.openlocfilehash: bd794ae2217c2a56f36cd46c2f12f1c80f6b4f2b
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2017
---
# <a name="adding-validation-to-a-razor-page"></a>Aggiunta della convalida alla pagina Razor

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione la logica della convalida viene aggiunta al modello `Movie`. Le regole di convalida vengono applicate ogni volta che un utente crea o modifica un film.

## <a name="validation"></a>Convalida

Un concetto di base dello sviluppo del software si chiama [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself", Non ripeterti). Le pagine Razor promuovono lo sviluppo in cui la funzionalità è stata specificata una volta e questa modifica è riflessa sull'intera app. DRY consente di ridurre la quantità di codice in un'app. DRY rende il codice meno soggetto ad errori ed è più facile da testare e gestire.

Il supporto della convalida fornito dalle pagine di Razor e da Entity Framework è un buon esempio del principio di DRY. Le regole di convalida vengono specificate in modo dichiarativo in un'unica posizione (nella classe del modello) e le regole vengono applicate ovunque nell'app.

### <a name="adding-validation-rules-to-the-movie-model"></a>Aggiunta di regole di convalida al modello movie

Aprire il file *Movie.cs*. [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) fornisce un set incorporato di attributi di convalida che si applicano in modo dichiarativo a una classe o proprietà. DataAnnotations contiene anche gli attributi di formattazione come ad esempio `DataType` che guidano nella formattazione e non forniscono la convalida.

Aggiornamento della classe `Movie` per poter sfruttare gli attributi di convalida `Required`, `StringLength`, `RegularExpression`, e `Range`.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

Gli attributi di convalida specificano il comportamento che viene applicato alle proprietà del modello:

* Gli attributi `Required` e `MinimumLength` indicano che una proprietà deve avere un valore. Tuttavia, niente impedisce a un utente di immettere spazi vuoti per soddisfare il vincolo di convalida per un tipo nullable. I [tipi valore](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) non nullable (ad esempio `decimal`, `int`, `float` e `DateTime`) sono intrinsecamente necessari e non richiedono l'attributo `Required`.
* L'attributo `RegularExpression` limita i caratteri che può immettere l'utente. Nel codice precedente `Genre` e `Rating` devono usare solo lettere (spazi, numeri e caratteri speciali non sono consentiti).
* L'attributo `Range` vincola un valore a un intervallo specificato.
* L'attributo `StringLength` imposta la lunghezza massima di una stringa e, facoltativamente, la lunghezza minima. 

Il possesso di regole di convalida applicate automaticamente da ASP.NET Core consente di rendere un'app più solida. La convalida automatica sui modelli consente di proteggere l'app perché non è necessario ricordarsi di applicarli quando viene aggiunto nuovo codice.

### <a name="validation-error-ui-in-razor-pages"></a>Errore di convalida dell'interfaccia utente nelle pagine Razor

Eseguire l'app e passare a pagine/film.

Selezionare il collegamento **Crea nuovo**. Completare il modulo con alcuni valori non validi. Quando la convalida del lato client jQuery rileva l'errore, viene visualizzato un messaggio di errore.

![Il modulo di vista del film con più errori di convalida del lato client jQuery](validation/_static/val.png)

> [!NOTE]
> Potrebbe non essere possibile immettere separatori decimali o virgole nel campo `Price`. Per supportare la [convalida jQuery](https://jqueryvalidation.org/) in impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese degli Stati Uniti, è necessario eseguire alcuni passaggi per globalizzare l'app. Per altre informazioni, vedere [Risorse aggiuntive](#additional-resources). Per il momento, immettere solo numeri interi come 10.

Si noti come il modulo ha eseguito automaticamente il rendering di un messaggio di errore di convalida in ogni campo che contiene un valore non valido. Gli errori vengono applicati sia sul lato client (uso di JavaScript e jQuery) sia sul lato server (quando un utente ha JavaScript disabilitato).

Un vantaggio significativo è che non c'era **nessuna** modifica del codice necessaria nelle pagine Crea o Modifica. Una volta che le DataAnnotations sono state applicate al modello, è stata abilitata la convalida dell'interfaccia utente. Le pagine Razor create in questa esercitazione hanno selezionato automaticamente le regole di convalida (usando gli attributi di convalida delle proprietà della classe `Movie` del modello). Convalida del test tramite la pagina Modifica, viene applicata la stessa convalida.

I dati del modulo non vengono registrati nel server fino a quando non sono presenti errori di convalida nel lato client. Verificare che i dati del modulo non siano stati registrati da uno o più degli approcci seguenti:

* Inserire un punto di interruzione nel metodo `OnPostAsync`. Inviare il modulo (selezionare **Crea** o **Salva**). Il punto di interruzione non viene mai raggiunto.
* Usare lo [Strumento Fiddler](http://www.telerik.com/fiddler).
* Usare gli strumenti di sviluppo del browser per monitorare il traffico di rete.

### <a name="server-side-validation"></a>Convalida sul lato server

Quando JavaScript è disabilitato nel browser, l'invio del modulo con errori verrà inoltrato al server.

Facoltativo, convalida sul lato server del test:

* Disabilitare JavaScript nel browser. Se non è possibile disabilitare JavaScript nel browser, provare un altro browser.
* Impostare un punto di interruzione nel metodo `OnPostAsync` della pagina Crea o Modifica.
* Invio di un form con errori di convalida.
* Verificare che lo stato del modello non sia valido:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

Il codice seguente mostra una porzione della pagina *Create.cshtml* di cui è stato eseguito lo scaffolding in precedenza nell'esercitazione. Viene usato dalle pagine Creazione e Modifica per visualizzare il modulo iniziale e per visualizzare nuovamente il modulo in caso di errore.

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

L'[helper tag di input](xref:mvc/views/working-with-forms) usa gli attributi [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) e produce gli attributi HTML necessari per la convalida jQuery sul lato client. L'[helper tag di convalida](xref:mvc/views/working-with-forms#the-validation-tag-helpers) visualizza gli errori di convalida. Per altre informazioni, vedere [Convalida](xref:mvc/models/validation).

Le pagine Crea e Modifica non dispongono di nessuna regola di convalida. Le regole di convalida e le stringhe di errore vengono specificate solo nella classe `Movie`. Queste regole di convalida vengono applicate automaticamente alle pagine Razor che modificano il modello `Movie`.

Quando la logica di convalida deve cambiare, avviene solo nel modello. La convalida viene applicata in modo coerente in tutta l'applicazione (la logica di convalida è definita in un'unica posizione). La convalida in un'unica posizione consente di mantenere il codice pulito e rende più semplice la gestione e l'aggiornamento.

## <a name="using-datatype-attributes"></a>Utilizzo degli attributi DataType

Esaminare la classe `Movie`. Lo spazio dei nomi `System.ComponentModel.DataAnnotations` fornisce gli attributi di formattazione oltre al set predefinito di attributi di convalida. L'attributo `DataType` viene applicato alle proprietà `ReleaseDate` e `Price`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Gli attributi `DataType` forniscono solo gli hint per far sì che il motore di vista formatti i dati (e fornisca gli attributi, ad esempio `<a>` per gli URL e `<a href="mailto:EmailAddress.com">` per la posta elettronica). Usare l'attributo `RegularExpression` per convalidare il formato dei dati. L'attributo `DataType` viene usato per specificare un tipo di dati che è più specifico del tipo intrinseco del database. Gli attributi `DataType` non sono gli attributi di convalida. Nell'applicazione di esempio, viene visualizzata solo la data, senza l'ora.

L'enumerazione `DataType` fornisce per molti tipi di dati, ad esempio Data, Ora, Numero di telefono, Valuta, Indirizzo di posta elettronica e altro ancora. L'attributo `DataType` può anche consentire all'applicazione di fornire automaticamente le funzionalità specifiche del tipo. Ad esempio, è possibile creare un collegamento `mailto:` per `DataType.EmailAddress`. È possibile fornire un selettore di dati per `DataType.Date` nei browser che supportano HTML5. Gli attributi `DataType` generano attributi di HTML 5 `data-` (dash di dati pronunciati) che usano browser di HTML 5. Gli attributi `DataType` **non** forniscono alcuna convalida.

`DataType.Date` non specifica il formato della data visualizzata. Per impostazione predefinita, il campo dei dati viene visualizzato in base ai formati predefiniti per il valore `CultureInfo` del server.

L'attributo `DisplayFormat` viene usato per specificare in modo esplicito il formato della data:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

L'impostazione `ApplyFormatInEditMode` specifica che deve essere applicata la formattazione quando il valore viene visualizzato per la modifica. Questo comportamento potrebbe non essere desiderato per alcuni campi. Ad esempio, in valori di valuta, è probabile che non si desideri il simbolo di valuta in modalità di modifica dell'interfaccia utente.

L'attributo `DisplayFormat` può essere usato da solo, ma in genere è consigliabile usare l'attributo `DataType`. L'attributo `DataType` fornisce la semantica dei dati anziché la modalità di esecuzione del rendering in una schermata e fornisce i seguenti vantaggi che non si ottengono con DisplayFormat:

* Il browser può abilitare le funzionalità HTML5, ad esempio visualizzare un controllo di calendario, il simbolo della valuta appropriato per le impostazioni locali, i collegamenti alla posta elettronica e così via.
* Per impostazione predefinita, il browser eseguirà il rendering dei dati usando il formato corretto in base alle impostazioni locali del sistema.
* L'attributo `DataType` può abilitare il framework di ASP.NET Core a scegliere il modello di campo corretto per il rendering dei dati. Il `DisplayFormat` se usato da solo usa il modello di stringa.

Nota: la convalida jQuery non funziona con l'attributo `Range` e con `DateTime`. Ad esempio, il codice seguente visualizzerà sempre un errore di convalida sul lato client, anche quando la data è compreso nell'intervallo specificato:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

In genere non è consigliabile compilare date reali nei modelli, quindi l'uso dell'attributo `Range` e di `DateTime` è sconsigliato.

Il codice seguente illustra la combinazione di attributi in una sola riga:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[Guida introduttiva a Razor Pages ed Entity Framework Core](xref:data/ef-rp/intro) Mostra operazioni più avanzate di Entity Framework Core con Razor Pages.

### <a name="publish-to-azure"></a>Pubblicare in Azure

Per istruzioni su come pubblicare l'app in Azure, vedere [Pubblicare un'app Web ASP.NET Core in Servizio app di Azure con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

## <a name="additional-resources"></a>Risorse aggiuntive

* [Utilizzo dei moduli](xref:mvc/views/working-with-forms)
* [Globalizzazione e localizzazione](xref:fundamentals/localization)
* [Introduzione agli helper tag](xref:mvc/views/tag-helpers/intro)
* [Creazione e modifica di helper tag](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
[Articolo precedente: Aggiunta di un nuovo campo](xref:tutorials/razor-pages/new-field)
[Articolo successivo: Caricamento di file](xref:tutorials/razor-pages/uploading-files)
