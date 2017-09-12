---
title: Sostituire < machineKey > in ASP.NET
author: rick-anderson
description: Sostituire < machineKey > in ASP.NET
keywords: ASP.NET Core, sicurezza, '< machineKey >'
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 29229b9507ece6aff8278b0ad66169c9e4e7498b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="c25e0-104">Sostituzione di `<machineKey>` in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c25e0-104">Replacing `<machineKey>` in ASP.NET</span></span>

<a name=compatibility-replacing-machinekey></a>

<span data-ttu-id="c25e0-105">L'implementazione del `<machineKey>` elemento ASP.NET [sostituibile](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="c25e0-105">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="c25e0-106">In questo modo la maggior parte delle chiamate alle routine di crittografia di ASP.NET essere instradata attraverso un meccanismo di protezione dei dati di sostituzione, tra cui il nuovo sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="c25e0-106">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="c25e0-107">Installazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c25e0-107">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="c25e0-108">Il nuovo sistema di protezione dati può solo essere installato in un'applicazione ASP.NET esistente come destinazione .NET 4.5.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c25e0-108">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="c25e0-109">L'installazione verrà esito negativo se l'applicazione è destinata a .NET 4.5 o inferiore.</span><span class="sxs-lookup"><span data-stu-id="c25e0-109">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="c25e0-110">Per installare il nuovo sistema di protezione dati in un progetto 4.5.1+ ASP.NET esistente, installare il pacchetto Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="c25e0-110">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="c25e0-111">Questo crea un'istanza di sistema di protezione dati mediante il [configurazione predefinita](../configuration/default-settings.md#data-protection-default-settings) impostazioni.</span><span class="sxs-lookup"><span data-stu-id="c25e0-111">This will instantiate the data protection system using the [default configuration](../configuration/default-settings.md#data-protection-default-settings) settings.</span></span>

<span data-ttu-id="c25e0-112">Quando si installa il pacchetto, inserita una riga in *Web. config* che indica ad ASP.NET di usarlo per [più operazioni di crittografia](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), inclusi autenticazione basata su form, lo stato di visualizzazione e le chiamate a MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="c25e0-112">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="c25e0-113">La riga di inserimento è simile alla seguente.</span><span class="sxs-lookup"><span data-stu-id="c25e0-113">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="c25e0-114">È possibile stabilire se il nuovo sistema di protezione dati è attivo controllando campi quali `__VIEWSTATE`, che deve iniziare con "CfDJ8" come nell'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="c25e0-114">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="c25e0-115">"CfDJ8" è la rappresentazione base64 dell'intestazione magic "09 F0 C9 F0" che identifica un payload protetto dal sistema di protezione dati.</span><span class="sxs-lookup"><span data-stu-id="c25e0-115">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="c25e0-116">Configurazione del pacchetto</span><span class="sxs-lookup"><span data-stu-id="c25e0-116">Package configuration</span></span>

<span data-ttu-id="c25e0-117">Il sistema di protezione dati viene creato con una configurazione predefinita per l'installazione di zero.</span><span class="sxs-lookup"><span data-stu-id="c25e0-117">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="c25e0-118">Tuttavia, poiché per impostazione predefinita le chiavi vengono rese persistenti nel file system locale, non funzionerà per le applicazioni che vengono distribuite in una farm.</span><span class="sxs-lookup"><span data-stu-id="c25e0-118">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="c25e0-119">Per risolvere questo problema, è possibile specificare la configurazione tramite la creazione di un tipo che crea una sottoclasse DataProtectionStartup ed esegue l'override di metodo ConfigureServices.</span><span class="sxs-lookup"><span data-stu-id="c25e0-119">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="c25e0-120">Di seguito è riportato un esempio di un tipo di avvio di protezione dati personalizzati che è configurato sia in cui le chiavi sono persistenti e come essi sta crittografati a riposo.</span><span class="sxs-lookup"><span data-stu-id="c25e0-120">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="c25e0-121">Esegue l'override anche i criteri di isolamento predefinito app fornendo il proprio nome dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c25e0-121">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="c25e0-122">È inoltre possibile utilizzare `<machineKey applicationName="my-app" ... />` al posto di una chiamata esplicita a SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="c25e0-122">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="c25e0-123">Si tratta di un meccanismo utile per evitare di forzare lo sviluppatore di creare un tipo derivato da DataProtectionStartup se il nome dell'applicazione tutti volevano per configurare l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="c25e0-123">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="c25e0-124">Per abilitare questa configurazione personalizzata, tornare al file Web. config e cercare il `<appSettings>` elemento che consentono di installare il pacchetto aggiunto al file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="c25e0-124">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="c25e0-125">Sarà simile al seguente il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="c25e0-125">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="c25e0-126">Inserire il valore vuoto con il nome completo dell'assembly del tipo derivato DataProtectionStartup che appena creato.</span><span class="sxs-lookup"><span data-stu-id="c25e0-126">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="c25e0-127">Se il nome dell'applicazione è DataProtectionDemo, questo sarà simile di seguito.</span><span class="sxs-lookup"><span data-stu-id="c25e0-127">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="c25e0-128">Il sistema di protezione dati appena configurato è ora pronto per l'utilizzo all'interno dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c25e0-128">The newly-configured data protection system is now ready for use inside the application.</span></span>
