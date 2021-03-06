---
title: Rimozione della protezione payload le cui chiavi sono stati revocati.
author: rick-anderson
description: "Questo documento viene illustrato come rimuovere la protezione dati, protetti con chiavi che poiché revocate in un'applicazione ASP.NET Core."
keywords: ASP.NET Core, protezione dei dati, revocare le chiavi, IPersistedDataProtector
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d431f0bbe7152525c9a360a6e90bccbd26be93d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Rimozione della protezione payload le cui chiavi sono stati revocati.

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Le API di protezione dati ASP.NET Core non sono principalmente destinati indefinita persistenza del payload riservato. Altre tecnologie come [DPAPI CNG di Windows](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) e [Azure Rights Management](https://docs.microsoft.com/rights-management/) sono più adatti per lo scenario di memorizzazione indefinito, e hanno funzionalità di gestione delle chiavi conseguentemente sicuro. Ciò premesso, non c'è niente che uno sviluppatore di utilizzare le API di protezione dati ASP.NET Core per la protezione a lungo termine dei dati riservati. Le chiavi non vengono mai rimosse dalla sequenza di chiave, pertanto `IDataProtector.Unprotect` può sempre il ripristino payload esistenti, purché le chiavi sono disponibili e validi.

Tuttavia, si verifica un problema quando lo sviluppatore tenta di rimuovere la protezione dati che sono stato protetto con una chiave revocata, come `IDataProtector.Unprotect` genererà un'eccezione in questo caso. Potrebbe essere appropriato per payload di breve durato o temporanee (ad esempio, il token di autenticazione), questi tipi di payload possono facilmente essere ricreati mediante il sistema e nel peggiore dei casi i visitatori del sito potrebbe essere necessario eseguire nuovamente l'accesso. Ma per il payload persistente, con `Unprotect` throw può causare la perdita di dati accettabile.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Per supportare lo scenario di consentire il payload essere protetto anche in caso di chiavi revocate, il sistema di protezione dati contiene un `IPersistedDataProtector` tipo. Per ottenere un'istanza di `IPersistedDataProtector`, semplicemente ottenere un'istanza di `IDataProtector` in modo normale e provare a eseguire il cast di `IDataProtector` a `IPersistedDataProtector`.

> [!NOTE]
> Non tutti i `IDataProtector` possono eseguire il cast di istanze per `IPersistedDataProtector`. Gli sviluppatori devono usare c# come operatore o simili per evitare eccezioni di runtime dovuto a cast non valido e deve essere preparato a gestire il caso di errore in modo appropriato.

`IPersistedDataProtector`espone la superficie dell'API seguente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Questa API richiede il payload protetto (come una matrice di byte) e restituisce il payload non protetto. Non vi è alcun overload basato su stringa. Di seguito sono riportati i due parametri out.

* `requiresMigration`: verrà impostato su true se la chiave utilizzata per proteggere il payload non è più la chiave predefinita attiva, ad esempio, la chiave utilizzata per proteggere il payload è precedente ed è una chiave di sequenza di operazione eseguita sul posto. Il chiamante potrebbe essere necessario prendere in considerazione proteggere di nuovo il payload a seconda delle esigenze aziendali.

* `wasRevoked`: verrà impostato su true se la chiave utilizzata per proteggere il payload è stata revocata.

>[!WARNING]
> Prestare molta attenzione quando si passa `ignoreRevocationErrors: true` per il `DangerousUnprotect` metodo. Se dopo aver chiamato questo metodo il `wasRevoked` valore è true, quindi la chiave utilizzata per proteggere il payload è stata revocata e l'autenticità del payload devono essere considerate come sospetto. In questo caso, solo smettere di funzionare nel payload di non protetti, se si dispone di una discreta garanzia di separata che è autentico, ad esempio, che non del provenienti da un database protetto anziché inviati da un client web non attendibili.

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
