---
title: Applicazione di SSL in un'applicazione ASP.NET di base
author: rick-anderson
description: Viene illustrato come richiedere SSL in un ASP.NET Core app web
keywords: ASP.NET Core, SSL, HTTPS, RequireHttpsAttribute, IIS Express
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 35554939bd574b2826053004ed437bee46154c2b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Applicazione di SSL in un'applicazione ASP.NET di base

Da [Rick Anderson](https://twitter.com/RickAndMSFT)

Questo documento illustra come:

- Richiedere SSL per tutte le richieste (solo richieste HTTPS).
- Reindirizzare tutte le richieste HTTP a HTTPS.

## <a name="require-ssl"></a>Richiedere SSL

Il [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) viene utilizzato per richiedere SSL. È possibile decorare controller o i metodi con questo attributo o è possibile applicare a livello globale, come illustrato di seguito:

Aggiungere il seguente codice al `ConfigureServices` in `Startup`:

[!code-csharp[Principale](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

Il codice evidenziato sopra è necessario utilizzano tutte le richieste `HTTPS`, pertanto vengono ignorate le richieste HTTP. Il seguente codice evidenziato reindirizza tutte le richieste HTTP a HTTPS:

[!code-csharp[Principale](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Vedere [Middleware di riscrittura URL](xref:fundamentals/url-rewriting) per ulteriori informazioni.

Che richiedono HTTPS a livello globale (`options.Filters.Add(new RequireHttpsAttribute());`) è una procedura consigliata. L'applicazione di `[RequireHttps]` attributo per tutti i controller non è considerata sicura come che richiedono HTTPS a livello globale. Non è possibile garantire nuovi controller aggiunto all'app verranno memorizzate applicare il `[RequireHttps]` attributo.

## <a name="set-up-iis-express-for-sslhttps"></a>Configurare IIS Express per SSL e HTTPS

Vedere [impostazione HTTPS per lo sviluppo di ASP.NET Core](xref:security/https#iisxpress).