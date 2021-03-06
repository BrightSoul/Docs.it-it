---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Creazione di helper HTML personalizzati (VB) | Documenti Microsoft
author: microsoft
description: "L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. L'utilizzo degli HTML Helper..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: e389a03228995ce0a6926a53af38f26ad51372d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-vb"></a>Creazione di helper HTML personalizzati (VB)
====================
da [Microsoft](https://github.com/microsoft)

[Scarica il PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.


L'obiettivo di questa esercitazione è dimostrare come è possibile creare l'helper HTML personalizzati che è possibile utilizzare all'interno di visualizzazioni MVC. Sfruttando degli helper HTML, è possibile ridurre la quantità di noiosa digitazione dei tag HTML che è necessario eseguire per creare una pagina HTML standard.

Nella prima parte di questa esercitazione, descrivo alcuni degli helper HTML esistente incluso con il framework di MVC ASP.NET. Successivamente, descrivo due metodi di creazione di helper HTML personalizzati: illustrano come creare l'helper HTML personalizzati creando un metodo condiviso e la creazione di un metodo di estensione.

## <a name="understanding-html-helpers"></a>Comprendere l'helper HTML

Un HTML Helper è semplicemente un metodo che restituisce una stringa. La stringa può rappresentare qualsiasi tipo di contenuto che si desidera. Ad esempio, è possibile utilizzare l'helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e `<img>` tag. È inoltre possibile utilizzare helper HTML per il rendering del contenuto più complesso, ad esempio un elenco di schede o una tabella HTML di dati del database.

Il framework di MVC ASP.NET include il seguente set di standard helper HTML (ciò non è un elenco completo):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Si consideri ad esempio il modulo nel listato 1. Questo modulo viene eseguito il rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1). Questo modulo Usa la `Html.BeginForm()` e `Html.TextBox()` metodi di supporto.


[![Pagina di cui è stato eseguito il rendering con helper HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Figura 01**: pagina di cui è stato eseguito il rendering con helper HTML ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-vb/_static/image3.png))


**Elenco 1:`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

Il `Html.BeginForm()` metodo Helper viene utilizzato per creare il codice HTML di apertura e chiusura `<form>` tag. Si noti che il `Html.BeginForm()` metodo viene chiamato all'interno di un utilizzo istruzione. L'istruzione using garantisce che il `<form>` tag viene chiuso alla fine della mediante blocco.

Se si preferisce, invece di creare un utilizzando blocco, è possibile chiamare il metodo di supporto Html.EndForm() per chiudere la `<form>` tag. Utilizzare qualsiasi approccio alla creazione di apertura e chiusura `<form>` tag apparentemente più intuitiva.

Il `Html.TextBox()` vengono utilizzati metodi Helper nel listato 1 per il rendering HTML `<input>` tag. Se si seleziona HTML nel browser viene visualizzato l'origine HTML nel listato 2. Si noti che l'origine contiene tag HTML standard.

> [!IMPORTANT]
> Si noti che il `Html.TextBox()`-HTML Helper viene eseguito il rendering con `<%= %>` tag anziché `<% %>` tag. Se non si include il segno di uguale, non viene eseguito nel browser.

Il framework ASP.NET MVC contiene un piccolo set di supporti. Molto probabilmente, è necessario estendere il framework MVC con helper HTML personalizzati. Nel resto di questa esercitazione, si apprenderà due metodi di creazione di helper HTML personalizzati.

**Elenco di 2:`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Creazione di helper HTML con i metodi condivisi

Il modo più semplice per creare un nuovo HTML Helper consiste nel creare un metodo condiviso che restituisce una stringa. Si supponga, ad esempio, che si decide di creare un nuovo HTML Helper che esegue il rendering HTML `<label>` tag. È possibile utilizzare la classe nel listato 2 per il rendering di un `<label>`.

**Elenco di 2:`Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Non c'è niente di speciale sulla classe listato 2. Il `Label()` metodo restituisce semplicemente una stringa.

La visualizzazione dell'indice modificata nel listato 3 Usa il `LabelHelper` per il rendering HTML `<label>` tag. Si noti che la visualizzazione include un `<%@ imports %>` direttiva che importa lo spazio dei nomi Application1.Helpers.

**Elenco di 2:`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creazione di helper HTML con metodi di estensione

Se si desidera creare helper HTML che funzionano solo come gli helper HTML standard incluso nel framework di MVC ASP.NET è necessario creare metodi di estensione. Metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente. Quando si crea un metodo HTML Helper, si aggiungono nuovi metodi per la `HtmlHelper` classe rappresentato dalla proprietà di una visualizzazione Html.

Il modulo di Visual Basic nel listato 3 aggiunge un metodo di estensione denominato `Label()` per la `HtmlHelper` classe. Esistono un paio di aspetti che è opportuno notare su questo modulo. In primo luogo, si noti che il modulo è decorato con il `<Extension()>` attributo. Per usare questo attributo, è necessario importare il `System.Runtime.CompilerServices` dello spazio dei nomi

In secondo luogo, si noti che il primo parametro del `Label()` metodo rappresenta la `HtmlHelper` classe. Il primo parametro di un metodo di estensione indica la classe che estende il metodo di estensione.

**Elenco di 3:`Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Dopo aver creare un metodo di estensione e compilare correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio Intellisense, come tutti gli altri metodi di una classe (vedere la figura 2). L'unica differenza è che estensione metodi vengono visualizzati con un simbolo speciale accanto agli (un'icona di una freccia verso il basso).


[![Utilizzando il metodo di estensione Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Figura 02**: utilizzando il metodo di estensione Html.Label() ([fare clic per visualizzare l'immagine ingrandita](creating-custom-html-helpers-vb/_static/image6.png))


La visualizzazione dell'indice modificata listato 4 utilizza il metodo di estensione Html.Label() per eseguire il rendering di tutti i relativi &lt;etichetta&gt; tag.

**Elenco di 4:`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato due metodi di creazione di helper HTML personalizzati. In primo luogo, si è appreso come creare una classe personalizzata `Label()` HTML Helper mediante la creazione di un metodo condiviso che restituisce una stringa. Successivamente, si è appreso come creare una classe personalizzata `Label()` metodo HTML Helper mediante la creazione di un metodo di estensione sulla `HtmlHelper` classe.

In questa esercitazione è incentrata sulla creazione di un metodo HTML Helper estremamente semplice. Tenere presente che un HTML Helper può essere complesso desiderato. È possibile compilare l'helper HTML che eseguono il rendering di contenuto complesso, ad esempio le visualizzazioni albero, menu o tabelle di dati di database.

>[!div class="step-by-step"]
[Precedente](asp-net-mvc-views-overview-vb.md)
[Successivo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
