---
title: Configurazione di account di accesso esterno Facebook in ASP.NET Core
author: rick-anderson
description: Configurazione di account di accesso esterno Facebook in ASP.NET Core
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 8/1/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 9554d66712f93df6d2c50503b60162757986e707
ms.sourcegitcommit: 74e22e08e3b08cb576e5184d16f4af5656c13c0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/25/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="d822b-104">Configurazione dell'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="d822b-104">Configuring Facebook authentication</span></span>

<a name=security-authentication-facebook-logins></a>

<span data-ttu-id="d822b-105">Da [Valeriy Novytskyy](https://github.com/01binary) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d822b-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d822b-106">In questa esercitazione viene illustrato come consentire agli utenti di accedere con il proprio account Facebook tramite un progetto ASP.NET Core 2.0 di esempio creato nel [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="d822b-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="d822b-107">Viene innanzitutto creato un ID App Facebook seguendo il [passaggi ufficiali](https://developers.facebook.com/docs/apps/register).</span><span class="sxs-lookup"><span data-stu-id="d822b-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com/docs/apps/register).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="d822b-108">Creare l'app in Facebook</span><span class="sxs-lookup"><span data-stu-id="d822b-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="d822b-109">Passare il [Facebook per gli sviluppatori](https://developers.facebook.com/apps) pagina ed eseguire l'accesso.</span><span class="sxs-lookup"><span data-stu-id="d822b-109">Navigate to the [Facebook for Developers](https://developers.facebook.com/apps) page and sign in.</span></span> <span data-ttu-id="d822b-110">Se si dispone già di un account Facebook, utilizzare il **iscriversi a Facebook** collegamento nella pagina di accesso per crearne uno.</span><span class="sxs-lookup"><span data-stu-id="d822b-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="d822b-111">Toccare il **creare App** pulsante nell'angolo superiore destro per creare un nuovo ID di App.</span><span class="sxs-lookup"><span data-stu-id="d822b-111">Tap the **Create App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook per portale sviluppatori aprire in Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="d822b-113">Compilare il modulo e toccare il **Crea ID App** pulsante.</span><span class="sxs-lookup"><span data-stu-id="d822b-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Creare un nuovo ID di App](index/_static/FBNewAppId.png)

* <span data-ttu-id="d822b-115">Quando verrà visualizzata **selezionare un prodotto** prompt dei comandi, fare clic su **Set Up** sul **account Facebook** scheda.</span><span class="sxs-lookup"><span data-stu-id="d822b-115">When presented with **Select a product** prompt, Click **Set Up** on the **Facebook Login** card.</span></span>

   ![Pagina di installazione del prodotto](index/_static/FBProductSetup.png)

* <span data-ttu-id="d822b-117">Il **delle Guide rapide** guidata consentirà di avviare con **scegliere una piattaforma** come la prima pagina.</span><span class="sxs-lookup"><span data-stu-id="d822b-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="d822b-118">Fare clic su ignorare la procedura guidata per il momento di **impostazioni** collegamento nel menu a sinistra:</span><span class="sxs-lookup"><span data-stu-id="d822b-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Guida introduttiva a Skip](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="d822b-120">Viene visualizzata la **impostazioni OAuth Client** pagina:</span><span class="sxs-lookup"><span data-stu-id="d822b-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Pagina Impostazioni OAuth client](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="d822b-122">Immettere l'URI di sviluppo con */signin-facebook* aggiunti al **URI di reindirizzamento di OAuth validi** campo (ad esempio: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="d822b-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="d822b-123">L'autenticazione di Facebook configurato più avanti in questa esercitazione consente di gestire automaticamente le richieste nel */signin-facebook* route per implementare il flusso di OAuth.</span><span class="sxs-lookup"><span data-stu-id="d822b-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="d822b-124">Fare clic su **salvare modifiche**.</span><span class="sxs-lookup"><span data-stu-id="d822b-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="d822b-125">Fare clic su di **Dashboard** collegamento nel riquadro di spostamento a sinistra.</span><span class="sxs-lookup"><span data-stu-id="d822b-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="d822b-126">In questa pagina, annotare il `App ID` e `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="d822b-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="d822b-127">Nella sezione successiva verranno aggiunti entrambi nell'applicazione ASP.NET di base:</span><span class="sxs-lookup"><span data-stu-id="d822b-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Dashboard dello sviluppatore di Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="d822b-129">Quando si distribuisce il sito è necessario rivedere il **account Facebook** pagina di installazione e registrare un nuovo URI pubblico.</span><span class="sxs-lookup"><span data-stu-id="d822b-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="d822b-130">Archiviare ID App Facebook e segreto dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="d822b-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="d822b-131">Collegare le impostazioni sensibili, ad esempio Facebook `App ID` e `App Secret` all'applicazione mediante configurazione di [Manager segreto](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d822b-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="d822b-132">Ai fini di questa esercitazione, denominare il token `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="d822b-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

## <a name="configure-facebook-authentication"></a><span data-ttu-id="d822b-133">Configurare l'autenticazione di Facebook</span><span class="sxs-lookup"><span data-stu-id="d822b-133">Configure Facebook Authentication</span></span>

<span data-ttu-id="d822b-134">Il modello di progetto utilizzato in questa esercitazione assicura che [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) pacchetto è già installato.</span><span class="sxs-lookup"><span data-stu-id="d822b-134">The project template used in this tutorial ensures that [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package is already installed.</span></span>

* <span data-ttu-id="d822b-135">Per installare il pacchetto con Visual Studio 2017, fare clic sul progetto e scegliere **Gestisci pacchetti NuGet**.</span><span class="sxs-lookup"><span data-stu-id="d822b-135">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="d822b-136">Per installare con .NET Core CLI, eseguire le operazioni seguenti nella directory del progetto:</span><span class="sxs-lookup"><span data-stu-id="d822b-136">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d822b-137">ASP.NET Core 2. x</span><span class="sxs-lookup"><span data-stu-id="d822b-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d822b-138">Aggiungere il servizio di Facebook nel `ConfigureServices` metodo il *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="d822b-138">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

<span data-ttu-id="d822b-139">Il `AddAuthentication` metodo deve essere chiamato solo una volta quando si aggiunge più provider di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="d822b-139">The `AddAuthentication` method should only be called once when adding multiple authentication providers.</span></span> <span data-ttu-id="d822b-140">Le chiamate successive a esso hanno la possibilità di cui si esegue l'override di qualsiasi configurati in precedenza [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) proprietà.</span><span class="sxs-lookup"><span data-stu-id="d822b-140">Subsequent calls to it have the potential of overriding any previously configured [AuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) properties.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d822b-141">ASP.NET Core 1. x</span><span class="sxs-lookup"><span data-stu-id="d822b-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d822b-142">Aggiungere il middleware di Facebook nel `Configure` metodo *Startup.cs* file:</span><span class="sxs-lookup"><span data-stu-id="d822b-142">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="d822b-143">Vedere il [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) riferimento API per altre informazioni sulle opzioni di configurazione supportati dall'autenticazione di Facebook.</span><span class="sxs-lookup"><span data-stu-id="d822b-143">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="d822b-144">Opzioni di configurazione possono essere utilizzate per:</span><span class="sxs-lookup"><span data-stu-id="d822b-144">Configuration options can be used to:</span></span>

* <span data-ttu-id="d822b-145">Richiedere informazioni diverse relative all'utente.</span><span class="sxs-lookup"><span data-stu-id="d822b-145">Request different information about the user.</span></span>
* <span data-ttu-id="d822b-146">Aggiungere gli argomenti di stringa di query per personalizzare l'esperienza di accesso.</span><span class="sxs-lookup"><span data-stu-id="d822b-146">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="d822b-147">Accedere con Facebook</span><span class="sxs-lookup"><span data-stu-id="d822b-147">Sign in with Facebook</span></span>

<span data-ttu-id="d822b-148">Eseguire l'applicazione e fare clic su **Accedi**.</span><span class="sxs-lookup"><span data-stu-id="d822b-148">Run your application and click **Log in**.</span></span> <span data-ttu-id="d822b-149">Viene visualizzata un'opzione per accedere con Facebook.</span><span class="sxs-lookup"><span data-stu-id="d822b-149">You see an option to sign in with Facebook.</span></span>

![Applicazione Web: utente non autenticato](index/_static/DoneFacebook.png)

<span data-ttu-id="d822b-151">Facendo clic sul **Facebook**, si verrà reindirizzati a Facebook per l'autenticazione:</span><span class="sxs-lookup"><span data-stu-id="d822b-151">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLogin.png)

<span data-ttu-id="d822b-153">Indirizzo di posta elettronica e profilo pubblico le richieste di autenticazione di Facebook per impostazione predefinita:</span><span class="sxs-lookup"><span data-stu-id="d822b-153">Facebook authentication requests public profile and email address by default:</span></span>

![Pagina di autenticazione di Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="d822b-155">Dopo aver immesso le credenziali di Facebook, che si verrà reindirizzati al sito in cui è possibile impostare la posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="d822b-155">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="d822b-156">A questo punto si è connessi utilizzando le credenziali di Facebook:</span><span class="sxs-lookup"><span data-stu-id="d822b-156">You are now logged in using your Facebook credentials:</span></span>

![Applicazione Web: utente autenticato](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="d822b-158">Risoluzione dei problemi</span><span class="sxs-lookup"><span data-stu-id="d822b-158">Troubleshooting</span></span>

* <span data-ttu-id="d822b-159">**ASP.NET Core solo 2. x:** identità se non è configurato per la chiamata `services.AddIdentity` in `ConfigureServices`, il tentativo di autenticazione comporterà *ArgumentException: è necessario specificare l'opzione 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="d822b-159">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="d822b-160">Il modello di progetto utilizzato in questa esercitazione garantisce che questa operazione viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="d822b-160">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="d822b-161">Se il database del sito non è stato creato applicando la migrazione iniziale, è possibile ottenere *un'operazione di database non riuscita durante l'elaborazione della richiesta* errore.</span><span class="sxs-lookup"><span data-stu-id="d822b-161">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="d822b-162">Toccare **applicare le migrazioni** per creare il database e dell'aggiornamento per ignorare l'errore.</span><span class="sxs-lookup"><span data-stu-id="d822b-162">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d822b-163">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="d822b-163">Next steps</span></span>

* <span data-ttu-id="d822b-164">In questo articolo è stato illustrato come è possibile eseguire l'autenticazione con Facebook.</span><span class="sxs-lookup"><span data-stu-id="d822b-164">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="d822b-165">È possibile seguire un approccio simile per l'autenticazione con altri provider elencati nella [pagina precedente](index.md).</span><span class="sxs-lookup"><span data-stu-id="d822b-165">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="d822b-166">Quando si pubblica il sito web all'app web di Azure, è consigliabile reimpostare il `AppSecret` nel portale per sviluppatori di Facebook.</span><span class="sxs-lookup"><span data-stu-id="d822b-166">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="d822b-167">Impostare il `Authentication:Facebook:AppId` e `Authentication:Facebook:AppSecret` come impostazioni dell'applicazione nel portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="d822b-167">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="d822b-168">Il sistema di configurazione è impostare lettura delle chiavi dalle variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="d822b-168">The configuration system is set up to read keys from environment variables.</span></span>