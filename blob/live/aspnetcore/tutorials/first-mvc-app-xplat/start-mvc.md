---
title: Introduzione a ASP.NET Core MVC in Mac, Linux o Windows
author: rick-anderson
description: Introduzione ad ASP.NET Core MVC e Visual Studio Code in Mac, Linux e Windows
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 87ce5dca011a7bc37d34799db159d933c158cba1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a><span data-ttu-id="ed97f-104">Introduzione a ASP.NET Core MVC in Mac, Linux o Windows</span><span class="sxs-lookup"><span data-stu-id="ed97f-104">Getting started with ASP.NET Core MVC  on Mac, Linux, or Windows</span></span>

<span data-ttu-id="ed97f-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ed97f-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ed97f-106">Questa esercitazione mostra i concetti fondamentali di compiplazione di un'app Web ASP.NET Core MVC tramite [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span><span class="sxs-lookup"><span data-stu-id="ed97f-106">This tutorial will teach you the basics of building an ASP.NET Core MVC web app using [Visual Studio Code](https://code.visualstudio.com) (VS Code).</span></span> <span data-ttu-id="ed97f-107">Nell'esercitazione si presuppone una familarità con Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ed97f-107">The tutorial assumes familarity with VS Code.</span></span> <span data-ttu-id="ed97f-108">Vedere [Introduzione a VS Code](https://code.visualstudio.com/docs) e [Guida a Visual Studio Code](#visual-studio-code-help) per altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="ed97f-108">See [Getting started with VS Code](https://code.visualstudio.com/docs) and [Visual Studio Code help](#visual-studio-code-help) for more information.</span></span> [!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="ed97f-109">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="ed97f-109">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="ed97f-110">macOS: [Creare un'app ASP.NET Core MVC con Visual Studio per Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed97f-110">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="ed97f-111">Windows: [Creare un'app ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed97f-111">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="ed97f-112">macOS, Linux e Windows: [Creare un'app ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="ed97f-112">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span> 

## <a name="install-vs-code-and-net-core"></a><span data-ttu-id="ed97f-113">Installare VS Code e .NET Core</span><span class="sxs-lookup"><span data-stu-id="ed97f-113">Install VS Code and .NET Core</span></span>

<span data-ttu-id="ed97f-114">Questa esercitazione richiede il [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ed97f-114">This tutorial requires the [.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span> <span data-ttu-id="ed97f-115">Vedere [il pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) per la versione ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="ed97f-115">See [the pdf](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) for the ASP.NET Core 1.1 version.</span></span>

<span data-ttu-id="ed97f-116">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed97f-116">Install the following:</span></span>

* <span data-ttu-id="ed97f-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="ed97f-117">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
* [<span data-ttu-id="ed97f-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ed97f-118">Visual Studio Code</span></span>](https://code.visualstudio.com)
* <span data-ttu-id="ed97f-119">VS Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="ed97f-119">VS Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span> 

## <a name="create-a-web-app-with-dotnet"></a><span data-ttu-id="ed97f-120">Creare un'app web con dotnet</span><span class="sxs-lookup"><span data-stu-id="ed97f-120">Create a web app with dotnet</span></span>

<span data-ttu-id="ed97f-121">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ed97f-121">From a terminal, run the following commands:</span></span>

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

<span data-ttu-id="ed97f-122">Aprire la cartella *MvcMovie* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ed97f-122">Open the *MvcMovie* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="ed97f-123">Selezionare **Yes** (Sì) nel messaggio di **Avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".</span><span class="sxs-lookup"><span data-stu-id="ed97f-123">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'MvcMovie'.</span></span> <span data-ttu-id="ed97f-124">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="ed97f-124">Add them?"</span></span>
- <span data-ttu-id="ed97f-125">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="ed97f-125">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "MvcMovie".](../web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="ed97f-129">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="ed97f-129">Press **Debug** (F5) to build and run the program.</span></span>

![App in esecuzione](../first-mvc-app/start-mvc/_static/1.png)

<span data-ttu-id="ed97f-131">VS Code avvia il server web [Kestrel](xref:fundamentals/servers/kestrel) ed esegue l'app.</span><span class="sxs-lookup"><span data-stu-id="ed97f-131">VS Code starts the [Kestrel](xref:fundamentals/servers/kestrel) web server and runs your app.</span></span> <span data-ttu-id="ed97f-132">Si noti che la barra degli indirizzi visualizza `localhost:5000` e non `example.com` o simili.</span><span class="sxs-lookup"><span data-stu-id="ed97f-132">Notice that the address bar shows `localhost:5000` and not something like `example.com`.</span></span> <span data-ttu-id="ed97f-133">Ciò accade perché `localhost` è il nome host standard per il computer locale.</span><span class="sxs-lookup"><span data-stu-id="ed97f-133">That's because `localhost` is the standard hostname for your local computer.</span></span>

<span data-ttu-id="ed97f-134">Il modello predefinito include collegamenti **Home page, Informazioni su** e **Contatto**.</span><span class="sxs-lookup"><span data-stu-id="ed97f-134">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="ed97f-135">L'immagine del browser visualizzata sopra non mostra questi collegamenti.</span><span class="sxs-lookup"><span data-stu-id="ed97f-135">The browser image above doesn't show these links.</span></span> <span data-ttu-id="ed97f-136">A seconda delle dimensioni del browser, può essere necessario fare clic sull'icona di spostamento per visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="ed97f-136">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![Icona di spostamento in alto a destra](../first-mvc-app/start-mvc/_static/2.png)

<span data-ttu-id="ed97f-138">Nella parte seguente di questa esercitazione vengono fornite informazioni su MVC e istruzioni per iniziare a creare codice.</span><span class="sxs-lookup"><span data-stu-id="ed97f-138">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

## <a name="visual-studio-code-help"></a><span data-ttu-id="ed97f-139">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ed97f-139">Visual Studio Code help</span></span>

- [<span data-ttu-id="ed97f-140">Introduzione</span><span class="sxs-lookup"><span data-stu-id="ed97f-140">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="ed97f-141">Debug</span><span class="sxs-lookup"><span data-stu-id="ed97f-141">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="ed97f-142">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="ed97f-142">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="ed97f-143">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="ed97f-143">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="ed97f-144">Tasti di scelta rapida di Mac</span><span class="sxs-lookup"><span data-stu-id="ed97f-144">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="ed97f-145">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="ed97f-145">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="ed97f-146">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="ed97f-146">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

>[!div class="step-by-step"]
[<span data-ttu-id="ed97f-147">Successivo - Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="ed97f-147">Next - Add a controller</span></span>](adding-controller.md)