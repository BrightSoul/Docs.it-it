---
title: Helper di Tag di immagine | Documenti Microsoft
author: pkellner
description: Di seguito viene illustrato l'utilizzo di Helper di Tag di immagine
keywords: Helper per tag di ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="imagetaghelper"></a>ImageTagHelper

Di [Peter Kellner](http://peterkellner.net) 

L'Helper di Tag di immagine migliora la `img` (`<img>`) tag. Richiede un `src` tag, nonché `boolean` attributo `asp-append-version`.

Se l'origine dell'immagine (`src`) è un file statico nel server web host, una cache univoca busting stringa viene aggiunto come un parametro di query per l'origine dell'immagine. In questo modo si garantisce che se il file nel server web host cambia, richiesta univoco viene generato un URL che include il parametro di richiesta di aggiornamento. La cache busting stringa è un valore univoco che rappresenta l'hash del file di immagine statica.

Se l'origine dell'immagine (`src`) non è un file statico (ad esempio un URL remoto o il file non esiste nel server), il `<img>` tag `src` attributo viene generato senza cache busting parametro stringa di query.

## <a name="image-tag-helper-attributes"></a>Attributi Helper di Tag di immagine


### <a name="asp-append-version"></a>versione aggiungere ASP

Quando specificato insieme a un `src` viene richiamato l'attributo, l'Helper di Tag di immagine.

Un esempio di un oggetto valido `img` helper di tag è:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Se il file statico esiste nella directory *... Wwwroot/Images/asplogo.PNG* il codice html generato è simile al seguente (il valore hash saranno diversi):

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Il valore assegnato al parametro `v` è il valore hash dei file su disco. Se il server web è in grado di ottenere l'accesso in lettura al file statico a cui fa riferimento, no `v` i parametri vengono aggiunti i `src` attributo.

- - -

### <a name="src"></a>src

Per attivare l'Helper di Tag di immagine, è necessario l'attributo src di `<img>` elemento. 

> [!NOTE]
> Utilizza l'Helper di Tag di immagine di `Cache` provider nel server web locale per archiviare calcolata `Sha512` di un file specificato. Se il file viene richiesto di nuovo il `Sha512` non deve essere ricalcolata. La Cache viene invalidata da un controllo di file che è associato al file quando il file `Sha512` viene calcolata.

## <a name="additional-resources"></a>Risorse aggiuntive

* <xref:performance/caching/memory>
