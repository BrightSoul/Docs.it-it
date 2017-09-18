---
title: Metodi e viste del controller
author: rick-anderson
description: Utilizzo di metodi, viste e DataAnnotations del controller
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 87516295c6e8c49b492312afda80c92c968e778b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a><span data-ttu-id="8806d-104">Metodi e viste del controller</span><span class="sxs-lookup"><span data-stu-id="8806d-104">Controller methods and views</span></span>

<span data-ttu-id="8806d-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8806d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8806d-106">Le operazioni iniziali con l'app per i film sono state efficaci, ma la presentazione non è ottimale.</span><span class="sxs-lookup"><span data-stu-id="8806d-106">We have a good start to the movie app, but the presentation is not ideal.</span></span> <span data-ttu-id="8806d-107">Deve essere corretto il valore dell'ora (12:00:00 AM nell'immagine seguente) e **ReleaseDate** dovrebbe essere composto da due parole.</span><span class="sxs-lookup"><span data-stu-id="8806d-107">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![Vista Index: Release Date è un'unica parola (senza spazi) e la data di rilascio di ogni film indica l'ora nel formato 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

<span data-ttu-id="8806d-109">Aprire il file *Models/Movie.cs* e aggiungere le righe evidenziate illustrate di seguito:</span><span class="sxs-lookup"><span data-stu-id="8806d-109">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

<span data-ttu-id="8806d-110">[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span><span class="sxs-lookup"><span data-stu-id="8806d-110">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]</span></span>

<span data-ttu-id="8806d-111">Compilare ed eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="8806d-111">Build and run the app.</span></span>

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
<span data-ttu-id="8806d-112">[Precedente - Utilizzo di SQLite](working-with-sql.md)
[Successivo - Aggiungere la funzionalità di ricerca](search.md)</span><span class="sxs-lookup"><span data-stu-id="8806d-112">[Previous - Working with SQLite](working-with-sql.md)
[Next - Add search](search.md)</span></span>  