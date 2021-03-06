---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: "Erstellen einer Routeneinschränkung (c#) | Microsoft Docs"
author: StephenWalther
description: "In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: ee83a134dcbdd1abfb296f3126a64c7d4ebab7f5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-route-constraint-c"></a>Erstellen einer Routeneinschränkung (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie steuern können, wie Browser Übereinstimmung Routen anfordert, indem routeneinschränkungen mit regulären Ausdrücken erstellen.


Verwenden Sie routeneinschränkungen, um die Browseranforderungen einzuschränken, die mit eine bestimmte Route übereinstimmen. Einen regulären Ausdruck können Sie eine routeneinschränkung angeben.

Angenommen Sie, dass Sie die Route in Codebeispiel 1 in der Datei "Global.asax" definiert haben.

**1 – Global.asax.cs auflisten**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

Auflisten von 1 enthält eine Route mit dem Namen Product. Die Produkt-Route können Sie die ProductController in auflisten 2 enthaltene Browseranforderungen zuordnen.

**Auflisten von 2 – Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Beachten Sie, dass die Details()-Aktion, die von der Produkt-Controller verfügbar gemacht werden, einen einzelnen Parameter mit dem Namen "ProductID" akzeptiert. Dieser Parameter ist ein Integer-Parameter.

Im Codebeispiel 1 definierten Route entspricht eine der folgenden URLs:

- / Produkt/23
- / Produkt/7

Leider wird die Route auch die folgenden URLs übereinstimmen:

- / Produkt/Bla
- / Produkt/apple

Da die Details() Aktion einen Ganzzahlparameter erwartet, verursacht der eine Anforderung, die etwas anderes als einen ganzzahligen Wert enthält einen Fehler aus. Beispielsweise bei der Eingabe der URL-/Product/apple in Ihrem Browser erhalten die Seite "Fehler" in Abbildung 1 Sie.


[![Das Dialogfeld "Neues Projekt"](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Abbildung 01**: Anzeigen einer Seite auflösen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-route-constraint-cs/_static/image2.png))


Was Sie tatsächlich möchten ist nur mit URLs übereinstimmen, die eine richtige ganze Zahl "ProductID" enthalten. Eine Einschränkung können beim Definieren einer Route die URLs einzuschränken, die die Route entsprechen. Die geänderte Produkt Route auflisten 3 enthält eine reguläre-Einschränkung, die nur ganze Zahlen übereinstimmt.

**3 – Global.asax.cs auflisten**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

Der reguläre Ausdruck \d+ entspricht mindestens einem ganzen Zahlen. Diese Einschränkung bewirkt, dass die Route Produkt entsprechend den folgenden URLs:

- / Produkt/3
- / Produkt/8999

Aber nicht die folgenden URLs:

- / Produkt/apple
- / Produkt

- Diese Browseranforderungen von einer anderen Route verarbeitet werden oder, wenn keine übereinstimmenden Routen vorhanden sind eine *die Ressource konnte nicht gefunden werden* Fehler zurückgegeben.

>[!div class="step-by-step"]
[Zurück](creating-custom-routes-cs.md)
[Weiter](creating-a-custom-route-constraint-cs.md)
