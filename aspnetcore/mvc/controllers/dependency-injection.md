---
title: Dependency Injection in Controller
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 118f504311b58258b5a0510477280505135dd2d9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-into-controllers"></a>Dependency Injection in Controller

<a name="dependency-injection-controllers"></a>

Von [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC-Controller sollten ihre Abhängigkeiten explizit über ihre Konstruktoren anfordern. In einigen Fällen können einzelne Controlleraktionen einen Dienst erfordern. Dann ist es möglicherweise nicht sinnvoll, eine Anforderung auf Controllerebene durchzuführen. In diesem Fall können Sie einen Dienst auch als Parameter für die Aktionsmethode einfügen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Dependency Injection

Dependency Injection folgt dem [Prinzip der Umkehr von Abhängigkeiten](http://deviq.com/dependency-inversion-principle/), wodurch Anwendungen aus lose gekoppelten Modulen erstellt werden können. ASP.NET Core verfügt über integrierte Unterstützung für [Dependency Injection](../../fundamentals/dependency-injection.md). Dadurch können Anwendungen einfacher getestet und verwaltet werden.

## <a name="constructor-injection"></a>Constructor Injection

Die integrierte Unterstützung von ASP.NET Core für konstruktorbasierte Dependency Injection gilt auch für MVC-Controller. Indem Sie Ihrem Controller einfach einen Diensttyp als Konstruktorparameter hinzufügen, versucht ASP.NET Core, den Typ mithilfe des integrierten Dienstcontainers aufzulösen. Dienste werden meist, wenn auch nicht immer, mithilfe von Schnittstellen definiert. Wenn Ihre Anwendung beispielsweise über Geschäftslogik verfügt, die die aktuelle Uhrzeit benötigt, können Sie einen Dienst einfügen, der die Uhrzeit abruft (anstatt sie vorzudefinieren). Dadurch bestehen Ihre Tests auch in Implementierungen, die eine festgelegte Uhrzeit verwenden.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Das Implementieren einer solchen Schnittstelle, die die Systemuhr zur Laufzeit verwendet, ist einfach:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Nun kann der Dienst im Controller verwendet werden. In diesem Fall wurde der `HomeController` `Index`-Methode Logik hinzugefügt, um dem Benutzer je nach Tageszeit einen entsprechenden Gruß anzuzeigen.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Beim Ausführen der Anwendung wird nun vermutlich die folgende Fehlermeldung angezeigt:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Dieser Fehler tritt auf, wenn in der `ConfigureServices`-Methode der `Startup`-Klasse kein Dienst konfiguriert wurde. Damit Anforderungen für `IDateTime` mithilfe einer Instanz von `SystemDateTime` aufgelöst werden, fügen Sie der `ConfigureServices`-Methode die hervorgehobene Zeile der folgenden Liste hinzu:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Dieser Dienst kann mithilfe von unterschiedlichen Lebensdaueroptionen (`Transient`, `Scoped` oder `Singleton`) implementiert werden. Informationen zu den Auswirkungen der jeweiligen Bereichsoptionen auf das Verhalten des Diensts finden Sie unter [Dependency Injection](../../fundamentals/dependency-injection.md).

Sobald der Dienst konfiguriert wurde, sollte beim Ausführen der Anwendung und beim Navigieren zur Startseite wie erwartet eine zeitbasierte Nachricht angezeigt werden:

![Serverbegrüßung](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Informationen zum einfacheren Testen von Code mithilfe der expliziten Anforderung von Abhängigkeiten im Controller [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) (Prinzip expliziter Abhängigkeiten) finden Sie unter [Testen von Controllerlogik](testing.md).

Die integrierte Dependency Injection von ASP.NET Core unterstützt das Vorhandensein eines einzelnen Konstruktors für Klassen, die Dienste anfordern. Bei mehr als einem Konstruktor wird möglicherweise die folgende Ausnahme angezeigt:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Wie in der Fehlermeldung angegeben kann dieses Problem mit einem einzelnen Konstruktor behoben werden. Sie können [die Standardunterstützung für Dependency Injection auch durch Drittanbieterimplementierungen ersetzen](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), von denen einige mehrere Konstruktoren unterstützen.

## <a name="action-injection-with-fromservices"></a>Action Injection mit FromServices

In einigen Fällen benötigen Sie für mehr als eine Aktion innerhalb des Controllers keinen Dienst. In diesem Fall kann es sinnvoll sein, den Dienst als Parameter in die Aktionsmethode einzufügen. Markieren Sie hierzu wie im folgenden Beispiel gezeigt den Parameter mit dem Attribut `[FromServices]`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Zugreifen auf Einstellungen von einem Controller

Das Zugreifen auf Anwendungs- oder Konfigurationseinstellungen von einem Controller aus ist ein häufiges Szenario. Verwenden Sie für diesen Zugriff das in [Konfiguration](xref:fundamentals/configuration/index) beschriebene Optionsmuster. Einstellungen sollten generell nicht direkt vom Controller mithilfe von Dependency Injection angefordert werden. Fordern Sie stattdessen eine `IOptions<T>`-Instanz an, wobei `T` die benötigte Konfigurationsklasse darstellt.

Damit Sie mit dem Optionsmuster arbeiten können, erstellen Sie wie folgt eine Klasse, die die Optionen darstellt:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Konfigurieren Sie anschließend die Anwendung so, dass das Optionsmodell verwendet wird. Fügen Sie Ihre Konfigurationsklasse der Dienstauflistung in `ConfigureServices` hinzu:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> In der voranstehenden Liste wird die Anwendung so konfiguriert, dass die Einstellungen aus einer JSON-formatierten Datei gelesen werden. Sie können die Einstellungen auch vollständig in Code konfigurieren, wie im kommentierten Code oben veranschaulicht wird. Weitere Konfigurationsoptionen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

Sobald Sie ein stark typisiertes Konfigurationsobjekt (in diesem Fall `SampleWebSettings`) angegeben und der Dienstauflistung hinzugefügt haben, können Sie es von jeder Controller- oder Aktionsmethode mithilfe der Anforderung einer Instanz von `IOptions<T>` (in diesem Fall `IOptions<SampleWebSettings>`) anfordern. Im folgenden Code wird verdeutlicht, wie die Einstellungen von einem Controller angefordert werden können:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Mithilfe des Optionsmusters können Einstellungen und Konfiguration voneinander entkoppelt werden. Es wird sichergestellt, dass der Controller das Prinzip der [Trennung von Belangen](http://deviq.com/separation-of-concerns/) befolgt, da ihm nicht bekannt sein muss, wie und wo die Einstellungsinformationen zu finden sind. Dadurch kann für den Controller auch einfacher der Komponententest [Testen von Controllerlogik](testing.md) durchgeführt werden, da kein [statischer Zusammenhang](http://deviq.com/static-cling/) und keine direkte Instanziierung von Einstellungsklassen innerhalb der Controllerklasse vorhanden sind.
