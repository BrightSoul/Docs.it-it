---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: La gestione della concorrenza con Entity Framework in un'applicazione ASP.NET MVC (7 di 10) | Documenti Microsoft
author: tdykstra
description: L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 con Code First di Entity Framework 5 e Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b072134043ceda809bfeca98447a132ed407b323
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>La gestione della concorrenza con Entity Framework in un'applicazione ASP.NET MVC (7 di 10)
====================
Da [Tom Dykstra](https://github.com/tdykstra)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione web di Contoso University esempio viene illustrato come creare applicazioni ASP.NET MVC 4 utilizzando Code First di Entity Framework 5 e Visual Studio 2012. Per informazioni sulle serie di esercitazioni, vedere [la prima esercitazione di serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). È possibile avviare la serie di esercitazioni dall'inizio o [scaricare un progetto di avvio per questo capitolo](building-the-ef5-mvc4-chapter-downloads.md) e iniziare da qui.
> 
> > [!NOTE] 
> > 
> > Se si esegue un problema, è possibile risolvere, [scaricare il capitolo completato](building-the-ef5-mvc4-chapter-downloads.md) e provare a riprodurre il problema. In genere, è possibile trovare la soluzione al problema di mediante un confronto tra il codice per il codice completato. Per alcuni errori comuni e come risolverli, vedere [errori e soluzioni alternative.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Nelle due esercitazioni precedente si è lavorato con i dati correlati. In questa esercitazione viene illustrato come gestire la concorrenza. Si creeranno le pagine web che utilizzano il `Department` entità e le pagine modificare ed eliminare `Department` entità gestirà gli errori di concorrenza. Le illustrazioni seguenti mostrano le pagine di indice e Delete, inclusi alcuni messaggi che vengono visualizzati se si verifica un conflitto di concorrenza.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Conflitti di concorrenza

Un conflitto di concorrenza si verifica quando un utente visualizza dati di un'entità per modificarlo, mentre un altro utente aggiorna i dati dell'entità stessa prima che la modifica del primo utente venga scritto nel database. Se non si abilita il rilevamento dei conflitti, ultimo utente che aggiorna il database sovrascrive le modifiche dell'utente. In molte applicazioni, questo rischio è accettabile: se sono presenti alcuni utenti o alcuni aggiornamenti o se non è realmente critico se alcune modifiche vengono sovrascritte, il costo di programmazione per la concorrenza potrebbe annullare il vantaggio. In tal caso, non è necessario configurare l'applicazione per gestire i conflitti di concorrenza.

### <a name="pessimistic-concurrency-locking"></a>Concorrenza pessimistica (blocco)

Se l'applicazione è necessario evitare la perdita accidentale dei dati in scenari di concorrenza, un modo per eseguire questa operazione è necessario utilizzare blocchi di database. Si tratta di *concorrenza pessimistica*. Ad esempio, prima di leggere una riga da un database, si richiedono il blocco di sola lettura o per l'accesso per l'aggiornamento. Se si blocca una riga per l'accesso per l'aggiornamento, altri utenti non sono consentiti per bloccare la riga di sola lettura o aggiornare l'accesso, poiché si potrebbe ottenere una copia dei dati che sono in corso di modifica. Se si blocca una riga per l'accesso in sola lettura, altri utenti anche possibile bloccarla per l'accesso in sola lettura, ma non per l'aggiornamento.

La gestione dei blocchi presenta gli svantaggi. Può risultare difficile al programma. Richiede risorse di gestione di database significativa, e come il numero di utenti di un'applicazione può causare problemi di prestazioni aumenta (vale a dire non è facilmente scalabile). Per questi motivi, non tutti i sistemi di gestione di database supportano la concorrenza pessimistica. Entity Framework è disponibile alcun supporto predefinito per il e, in questa esercitazione non mostra come implementarlo.

### <a name="optimistic-concurrency"></a>Concorrenza ottimistica

L'alternativa alla concorrenza pessimistica è *la concorrenza ottimistica*. Concorrenza ottimistica significa consentire i conflitti di concorrenza consente di eseguire e quindi reazione in modo appropriato in questo caso. Ad esempio, Giorgio esegue la pagina Modifica reparti, le modifiche di **Budget** quantità per il reparto inglese da $350,000.00 a $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Prima di John fa clic su **salvare**, Jane esegue la stessa pagina e le modifiche di **data di inizio** 1/9/2007 campo a 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Seleziona John **salvare** first e vede sua modifica quando il browser torna alla pagina di indice, Jane quindi fa clic su **salvare**. Le operazioni successive è determinata dalla modalità di gestione i conflitti di concorrenza. Alcune opzioni includono:

- È possibile tenere traccia di quali proprietà di un utente ha modificato e aggiornare solo le colonne corrispondenti nel database. Nello scenario di esempio, non dati andrebbero persi, perché sono state aggiornate diverse proprietà per i due utenti. La volta successiva che un utente che accede al reparto in lingua inglese, verranno visualizzate le modifiche di John sia di Jane, ovvero una data di inizio di 8/8/2013 e un budget di Zero dollari.

    Questo metodo di aggiornamento può ridurre il numero di conflitti che potrebbero comportare la perdita di dati, ma è possibile evitare la perdita di dati se vengono apportate modifiche competizione per la stessa proprietà di un'entità. Se Entity Framework funziona allo stesso modo dipende dalla modalità di implementazione del codice di aggiornamento. È spesso poco pratico in un'applicazione web, perché possono essere necessarie gestire grandi quantità di stato per tenere traccia di tutti i valori originali di proprietà per un'entità, nonché i nuovi valori. Gestione di grandi quantità di stato può influire sulle prestazioni di applicazioni perché richiede risorse di server o deve essere incluso nella pagina web stessa (ad esempio, nei campi nascosti).
- È possibile consentire la modifica di Jane sovrascrivere la modifica di John. La volta successiva che un utente che accede al reparto in lingua inglese, verranno visualizzate 8/8/2013 e il valore di $350,000.00 ripristinato. Si tratta di un *prevalenza del Client* o *ultimo in Wins* scenario. (I valori del client hanno la precedenza su ciò che si trova nell'archivio dati). Come accennato nell'introduzione di questa sezione, se non si configura questo codice per la gestione della concorrenza, verrà eseguita automaticamente.
- È possibile impedire la modifica di Jane vengano aggiornati nel database. In genere, verrebbe visualizzato un messaggio di errore, Mostra lo stato corrente dei dati e consente di riapplicare le proprie modifiche se desiderate in modo da renderle. Si tratta di un *archivio Wins* scenario. (I valori dell'archivio dati hanno la precedenza sui valori inviati dal client). Lo scenario Wins archivio viene implementato in questa esercitazione. Questo metodo assicura che modifiche non vengono sovrascritti senza un utente da ricevere un avviso se ciò che accade.

### <a name="detecting-concurrency-conflicts"></a>Il rilevamento dei conflitti di concorrenza

È possibile risolvere i conflitti gestendo [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) eccezioni generate Entity Framework. Per sapere quando generano queste eccezioni, Entity Framework deve essere in grado di rilevare i conflitti. Pertanto, è necessario configurare il database e il modello di dati in modo appropriato. Di seguito sono elencate alcune opzioni per abilitare il rilevamento dei conflitti:

- Nella tabella di database, includere una colonna di rilevamento che può essere utilizzata per determinare quando è stata modificata una riga. È quindi possibile configurare Entity Framework per includere la colonna di `Where` clausola SQL `Update` o `Delete` comandi.

    Il tipo di dati della colonna di rilevamento viene in genere [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). Il [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) valore è un numero sequenziale che sia stato incrementato ogni volta che viene aggiornata la riga. In un `Update` o `Delete` comando, il `Where` clausola include il valore originale della colonna di rilevamento (la versione originale). Se la riga aggiornata è stata modificata da un altro utente, il valore di `rowversion` colonna è diverso dal valore originale, pertanto la `Update` o `Delete` istruzione non è possibile trovare la riga da aggiornare perché il `Where` clausola. Quando Entity Framework rileva che nessuna riga è stata aggiornata mediante la `Update` o `Delete` comando (ovvero, quando il numero di righe interessate è pari a zero), che interpreta come un conflitto di concorrenza.
- Configurare Entity Framework per includere i valori originali di ogni colonna nella tabella di `Where` clausola di `Update` e `Delete` comandi.

    Come prima opzione, qualsiasi elemento nella riga è stato modificato dopo la riga prima di tutto è stato letto, il `Where` clausola non restituisce una riga da aggiornare, che Entity Framework viene interpretato come un conflitto di concorrenza. Per le tabelle di database con molte colonne, questo approccio può comportare notevoli `Where` clausole e possono richiedere mantenere grandi quantità di stato. Come notato in precedenza, la gestione di grandi quantità di stato può sulle prestazioni dell'applicazione perché richiede risorse di server o deve essere incluso nella stessa pagina web. Pertanto questo approccio in genere sconsigliato, e non è il metodo utilizzato in questa esercitazione.

    Se si desidera implementare questo approccio alla concorrenza, è necessario contrassegnare tutte le proprietà chiave non primaria dell'entità a cui si desidera tenere traccia di concorrenza per aggiungendo il [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) attributo. Che modifica consente a Entity Framework includere tutte le colonne in SQL `WHERE` clausola di `UPDATE` istruzioni.

Nella parte restante di questa esercitazione si aggiungerà un [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) rilevamento proprietà per il `Department` entità, creazione di un controller e visualizzazioni e il test per verificare che tutto funzioni correttamente.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Aggiungere una proprietà di concorrenza ottimistica all'entità reparto

In *Models\Department.cs*, aggiungere una proprietà di rilevamento denominata `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

Il [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) attributo specifica che questa colonna includerà il `Where` clausola di `Update` e `Delete` comandi inviati al database. L'attributo viene chiamato [Timestamp](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) perché le versioni precedenti di SQL Server utilizzato un database SQL [timestamp](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) il tipo di dati prima di SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) sostituito. Il tipo .net per `rowversion` è una matrice di byte. Se si preferisce utilizzare l'API fluent, è possibile utilizzare il [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) per specificare la proprietà di rilevamento, come illustrato nell'esempio seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Tramite l'aggiunta di una proprietà è stato modificato il modello di database, pertanto è necessario eseguire la migrazione di un altro. In Package Manager Console (PMC), immettere i comandi seguenti:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Creare un Controller di reparto

Creare un `Department` controller e visualizzazioni allo stesso modo è stato eseguito gli altri controller, usando le impostazioni seguenti:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

In *Controllers\DepartmentController.cs*, aggiungere un `using` istruzione:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Modifica "Cognome" in "FullName" ovunque in questo file (quattro occorrenze) in modo che il reparto amministratore elenco a discesa sono elencati conterrà il nome completo del istruttore anziché solo l'ultimo nome.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Sostituire il codice esistente per il `HttpPost` `Edit` (metodo) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

La vista archivierà originale `RowVersion` valore in un campo nascosto. Quando il gestore di associazione del modello crea il `department` istanza, tale oggetto avrà originale `RowVersion` valore della proprietà e i nuovi valori per le altre proprietà, così come immesse dall'utente nella pagina di modifica. Quindi quando Entity Framework consente di creare un database SQL `UPDATE` comando comando includerà un `WHERE` clausola che esegue la ricerca di una riga con originale `RowVersion` valore.

Se nessuna riga è interessata dal `UPDATE` comando (righe non avere originale `RowVersion` valore), Entity Framework genera un `DbUpdateConcurrencyException` eccezione e il codice di `catch` blocco Ottiene l'oggetto interessato `Department` entità dell'eccezione oggetto. Questa entità contiene sia i valori letti dal database e i nuovi valori immessi dall'utente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Successivamente, il codice aggiunge un messaggio di errore personalizzato per ogni colonna con valori di database diversi da ciò che l'utente ha immesso nella pagina Modifica:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Un messaggio di errore più spiega cosa è successo e quali operazioni eseguire su di esso:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Infine, il codice imposta il `RowVersion` valore di `Department` oggetto per il nuovo valore recuperato dal database. Questa nuova `RowVersion` valore verrà archiviato nel campo nascosto quando la modifica di pagina viene visualizzata e la successiva ora l'utente fa clic **salvare**, solo gli errori di concorrenza che si verificano dopo la visualizzazione della pagina di modifica verrà rilevata.

In *Views\Department\Edit.cshtml*, aggiungere un campo nascosto per salvare il `RowVersion` valore della proprietà, subito dopo il campo nascosto per il `DepartmentID` proprietà:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

In *Views\Department\Index.cshtml*, sostituire il codice esistente con il codice seguente per spostare i collegamenti di riga a sinistra e modificare le intestazioni di titolo e la colonna di pagina per visualizzare `FullName` anziché `LastName` nel **Amministratore** colonna:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Test di gestione della concorrenza ottimistica

Esecuzione del sito e fare clic su **reparti**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Fare clic il **modifica** collegamento ipertestuale per Kim Abercrombie e selezionare **aperto in una nuova scheda,** quindi fare clic su di **modifica** collegamento ipertestuale per Kim Abercrombie. Nelle due finestre visualizzate le stesse informazioni.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Modificare un campo nella prima finestra del browser e fare clic su **salvare**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Il browser viene mostrata la pagina di indice con il valore modificato.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Modificare qualsiasi campo nella seconda finestra del browser e fare clic su **salvare**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Fare clic su **salvare** nella seconda finestra del browser. Viene visualizzato un messaggio di errore:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Fare clic su **salvare** nuovamente. Il valore che immesso nel browser secondo viene salvato insieme al valore originale di dati modificati nel browser prima. Vengono visualizzati i valori salvati quando viene visualizzata la pagina di indice.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aggiornare la pagina di eliminazione

Per la pagina di eliminazione, Entity Framework rileva i conflitti di concorrenza causati da un utente in caso contrario la modifica del reparto in modo simile. Quando il `HttpGet` `Delete` metodo visualizza la conferma, la visualizzazione include originale `RowVersion` valore in un campo nascosto. Valore ottenuto viene quindi disponibili per il `HttpPost` `Delete` metodo che viene chiamato quando l'utente conferma l'eliminazione. Quando Entity Framework crea SQL `DELETE` comando include un `WHERE` clausola con l'originale `RowVersion` valore. Se i risultati del comando in zero righe interessato (che indica la riga è stata modificata dopo che è stata visualizzata la pagina di conferma eliminazione), viene generata un'eccezione di concorrenza e `HttpGet Delete` metodo viene chiamato con un flag di errore impostato su `true` per visualizzare nuovamente il pagina di conferma con un messaggio di errore. È anche possibile che siano stati interessati zero righe, perché la riga è stata eliminata da un altro utente, pertanto in questo caso viene visualizzato un messaggio di errore diverso.

In *DepartmentController.cs*, sostituire il `HttpGet` `Delete` (metodo) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Il metodo accetta un parametro facoltativo che indica se la pagina viene nuovamente visualizzata dopo un errore di concorrenza. Se questo flag è `true`, viene inviato un messaggio di errore per la visualizzazione con un `ViewBag` proprietà.

Sostituire il codice di `HttpPost` `Delete` metodo (denominato `DeleteConfirmed`) con il codice seguente:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Nel codice scaffolding che è stato sostituito solo, questo metodo è accettato solo un ID di record:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

È stato modificato questo parametro per un `Department` istanza entità creata dal gestore di associazione del modello. Ciò consente di accedere al `RowVersion` valore della proprietà oltre alla chiave del record.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

È stato modificato il nome del metodo di azione da `DeleteConfirmed` a `Delete`. Il codice di supporto temporaneo denominato il `HttpPost` `Delete` metodo `DeleteConfirmed` per fornire il `HttpPost` metodo una firma univoca. (Common Language Runtime richiede metodi di overload per disporre di parametri di metodo diverso). Ora che le firme sono univoche, è possibile utilizzare la convenzione MVC e utilizzare lo stesso nome per il `HttpPost` e `HttpGet` eliminare i metodi.

Se viene rilevato un errore di concorrenza, il codice viene visualizzata nuovamente la pagina di conferma eliminazione e fornisce un flag che indica che si visualizza un messaggio di errore di concorrenza.

In *Views\Department\Delete.cshtml*, sostituire il codice di supporto temporaneo con il codice seguente che effettua la formattazione cambia e aggiunge un campo di messaggio di errore. Le modifiche sono evidenziate.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Questo codice aggiunge un messaggio di errore tra il `h2` e `h3` intestazioni:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Sostituisce `LastName` con `FullName` nel `Administrator` campo:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Infine, aggiunge i campi nascosti per i `DepartmentID` e `RowVersion` proprietà dopo che il `Html.BeginForm` istruzione:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Eseguire la pagina di indice reparti. Fare clic il **eliminare** collegamento ipertestuale per il reparto in lingua inglese e selezionare **Apri in nuova finestra,** quindi nella prima finestra fare clic su di **modifica** collegamento ipertestuale per la lingua inglese reparto.

Nella prima finestra, modificare uno dei valori e fare clic su **salvare** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

La pagina di indice viene confermata la modifica.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Nella seconda finestra, fare clic su **eliminare**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Viene visualizzato il messaggio di errore di concorrenza e i valori di reparto vengono aggiornati con i dati attualmente il database.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Se si fa clic **eliminare** nuovamente, si viene reindirizzati alla pagina di indice, che indica che il reparto è stato eliminato.

## <a name="summary"></a>Riepilogo

Introduzione alla gestione di conflitti di concorrenza è stata completata. Per informazioni su altri modi per gestire vari scenari di concorrenza, vedere [modelli di concorrenza ottimistica](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) e [si lavora con valori di proprietà](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) nel blog del team di Entity Framework. L'esercitazione successiva viene illustrato come implementare l'ereditarietà tabella per gerarchia per il `Instructor` e `Student` entità.

Collegamenti ad altre risorse di Entity Framework, vedere il [mappa del contenuto ASP.NET dati accesso](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Precedente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Successivo](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
