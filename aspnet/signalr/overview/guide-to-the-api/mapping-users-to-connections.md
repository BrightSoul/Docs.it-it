---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Il mapping degli utenti di SignalR per le connessioni | Documenti Microsoft
author: tfitzmac
description: In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni. Patrick Fletcher hanno contribuito alla scrittura di questo argomento. Versioni di software utilizzate in questo argomento...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: 9b50d8805beabbc48467e20331c7593de9bc4254
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="mapping-signalr-users-to-connections"></a>Il mapping degli utenti di SignalR per le connessioni
====================
da [Tom FitzMacken](https://github.com/tfitzmac)

> In questo argomento viene illustrato come mantenere le informazioni relative agli utenti e le relative connessioni.
> 
> Patrick Fletcher hanno contribuito alla scrittura di questo argomento.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software utilizzate in questo argomento
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versione 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
> 
> Per informazioni sulle versioni precedenti di SignalR, vedere [le versioni precedenti di SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Domande e commenti
> 
> Lasciare commenti e suggerimenti su come è stato apprezzato questa esercitazione e cosa migliorare nei commenti nella parte inferiore della pagina. In caso di domande che non sono direttamente correlate all'esercitazione, è possibile registrarli per il [forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Introduzione

Ogni client che si connette a un hub passa un id connessione univoco. È possibile recuperare il valore di `Context.ConnectionId` proprietà del contesto di hub. Se l'applicazione deve eseguire il mapping di un utente per l'id di connessione e mantenere tale mapping, è possibile utilizzare uno dei valori seguenti:

- [Il Provider di ID utente (SignalR 2)](#IUserIdProvider)
- [Archiviazione in memoria](#inmemory), ad esempio un dizionario
- [Gruppo di SignalR per ogni utente](#groups)
- [Archiviazione permanente, esterna](#database), ad esempio una tabella di database o l'archiviazione tabelle di Azure

Ognuna di queste implementazioni è illustrato in questo argomento. Utilizzare il `OnConnected`, `OnDisconnected`, e `OnReconnected` metodi il `Hub` classe per tenere traccia dello stato di connessione utente.

L'approccio migliore per l'applicazione dipende da:

- Il numero di server web che ospita l'applicazione.
- Se è necessario ottenere un elenco di utenti attualmente connessi.
- Se è necessario rendere persistenti le informazioni utente e gruppo quando l'applicazione o il server viene riavviato.
- Indica se la latenza della chiamata a un server esterno è un problema.

Nella tabella seguente quale approccio funziona per queste considerazioni.

|  | Più di un server | Ottenere l'elenco degli utenti attualmente connessi. | Rendere persistenti le informazioni dopo il riavvio | Prestazioni ottimali |
| --- | --- | --- | --- | --- |
| Provider di ID utente | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| In memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Gruppi utente singolo | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Rese permanenti, esterno | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Provider IUserID

Questa funzionalità consente agli utenti di specificare l'ID utente in base a un IRequest tramite una nuova interfaccia IUserIdProvider.

**Il IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Per impostazione predefinita, vi sarà un'implementazione che l'utente utilizza `IPrincipal.Identity.Name` come nome utente. Per modificare questa impostazione, registrare l'implementazione di `IUserIdProvider` con l'host globale all'avvio dell'applicazione:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

All'interno di un hub, sarà in grado di inviare messaggi agli utenti tramite l'API seguente:

**Invia un messaggio a un utente specifico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Archiviazione in memoria

Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un dizionario che verrà archiviato in memoria. Il dizionario utilizza un `HashSet` per archiviare l'id di connessione. In qualsiasi momento un utente potrebbe avere più di una connessione all'applicazione di SignalR. Ad esempio, un utente connesso tramite più dispositivi o più di una scheda del browser avrebbe più di un id di connessione.

Se l'applicazione si arresta, tutte le informazioni vengono persi, ma verrà nuovamente popolato come gli utenti ristabilire le connessioni. Archiviazione in memoria non funziona se l'ambiente include più di un server web, perché ogni server ha una raccolta separata di connessioni.

Nel primo esempio viene illustrata una classe che gestisce il mapping degli utenti per le connessioni. La chiave per il HashSet sarà il nome dell'utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

Nell'esempio seguente viene illustrato come utilizzare la classe di mapping di connessione da un hub. L'istanza della classe viene archiviata in un nome di variabile `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Gruppi utente singolo

È possibile creare un gruppo per ogni utente e quindi inviare un messaggio a tale gruppo quando si desidera raggiungere solo tale utente. Il nome di ogni gruppo è il nome dell'utente. Se un utente dispone di più di una connessione, viene aggiunto ogni id di connessione per il gruppo dell'utente.

È consigliabile non rimuovere manualmente l'utente dal gruppo di quando l'utente si disconnette. Questa azione viene eseguita automaticamente dal framework di SignalR.

Nell'esempio seguente viene illustrato come implementare i gruppi utente singolo.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Archiviazione permanente, esterna

In questo argomento viene illustrato come utilizzare un database o un archivio tabelle di Azure per archiviare le informazioni di connessione. Questo approccio funziona quando si dispone di più server web, poiché ogni server web può interagire con lo stesso repository di dati. Se i server web arresta lavoro o il riavvio dell'applicazione, il `OnDisconnected` non viene chiamato. Pertanto, è possibile che il repository di dati disporrà di record per ID di connessione che non sono più validi. Per eliminare questi record orfani, si desidera invalidare qualsiasi connessione che è stato creato all'esterno di un intervallo di tempo è rilevante per l'applicazione. Gli esempi in questa sezione includono un valore per il rilevamento quando la connessione è stata creata, ma non mostrano come pulire i vecchi record perché è consigliabile eseguire questa operazione come processo in background.

### <a name="database"></a>Database

Nell'esempio seguente viene illustrato come mantenere le informazioni di connessione e l'utente in un database. È possibile usare qualsiasi tecnologia di accesso ai dati; Tuttavia, nell'esempio seguente viene illustrato come definire i modelli tramite Entity Framework. Questi modelli di entità corrispondono ai campi e tabelle di database. La struttura dei dati può variare notevolmente a seconda dei requisiti dell'applicazione.

Nel primo esempio viene illustrato come definire un'entità utente che può essere associata a tutte le entità di connessione.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Dall'hub, quindi, è possibile rilevare lo stato di ogni connessione con il codice riportato di seguito.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Archiviazione tabelle di Azure

Nell'esempio seguente viene archiviazione tabelle di Azure è simile all'esempio di database. Non include tutte le informazioni necessarie per iniziare a utilizzare il servizio di archiviazione tabelle di Azure. Per informazioni, vedere [come usare l'archiviazione di tabelle da .NET](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-how-to-use-tables/).

L'esempio seguente mostra un'entità di tabella per archiviare le informazioni di connessione. I dati vengono partizionati in base al nome utente e identifica ogni entità dall'id di connessione, in modo che un utente può avere più connessioni in qualsiasi momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

Nell'hub, registrare lo stato di ogni connessione utente.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
