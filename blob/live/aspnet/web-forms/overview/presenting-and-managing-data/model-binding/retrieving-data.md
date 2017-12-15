---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: e750250285fcb0088da284588d721ac21e73820c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="096aa-104">Abrufen und Anzeigen von Daten mit modellbindung und WebForms</span><span class="sxs-lookup"><span data-stu-id="096aa-104">Retrieving and displaying data with model binding and web forms</span></span>
====================
<span data-ttu-id="096aa-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="096aa-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="096aa-106">Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt.</span><span class="sxs-lookup"><span data-stu-id="096aa-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="096aa-107">Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="096aa-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="096aa-108">Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.</span><span class="sxs-lookup"><span data-stu-id="096aa-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="096aa-109">Das Modell bindungsmuster funktioniert mit jeder datenzugriffstechnologie.</span><span class="sxs-lookup"><span data-stu-id="096aa-109">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="096aa-110">In diesem Lernprogramm verwenden Sie Entity Framework, jedoch können Sie die datenzugriffstechnologie, die für Sie am besten vertraut ist.</span><span class="sxs-lookup"><span data-stu-id="096aa-110">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="096aa-111">Von einem datengebundenen Serversteuerelement, z. B. eine GridView, ListView, DetailsView oder FormView-Steuerelement geben Sie die Namen der Methoden zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="096aa-111">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="096aa-112">In diesem Lernprogramm werden Sie einen Wert für die SelectMethod angeben.</span><span class="sxs-lookup"><span data-stu-id="096aa-112">In this tutorial, you will specify a value for the SelectMethod.</span></span>
> 
> <span data-ttu-id="096aa-113">Innerhalb dieser Methode können bereitstellen Sie die Logik zum Abrufen der Daten.</span><span class="sxs-lookup"><span data-stu-id="096aa-113">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="096aa-114">In den nächsten Lernprogrammen werden Werte für UpdateMethod, DeleteMethod und InsertMethod festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-114">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
> 
> <span data-ttu-id="096aa-115">Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar.</span><span class="sxs-lookup"><span data-stu-id="096aa-115">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="096aa-116">Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="096aa-116">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="096aa-117">Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="096aa-117">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="096aa-118">In diesem Lernprogramm führen Sie die Anwendung in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="096aa-118">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="096aa-119">Sie können auch die Anwendung verfügbar über das Internet, indem sie in einem Hostinganbieter bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="096aa-119">You can also make the application available over the Internet by deploying it to a hosting provider.</span></span> <span data-ttu-id="096aa-120">Microsoft bietet kostenlose Webhosting für bis zu 10 Websites in einem</span><span class="sxs-lookup"><span data-stu-id="096aa-120">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="096aa-121">[Freigeben von Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="096aa-121">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="096aa-122">Informationen zum Bereitstellen einer Visual Studio-Webprojekt zu Azure App Service-Web-Apps finden Sie unter der [ASP.NET Web-Bereitstellung mit Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Reihe.</span><span class="sxs-lookup"><span data-stu-id="096aa-122">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="096aa-123">Dieses Lernprogramm wird gezeigt, wie Entity Framework Code First-Migrationen zu verwenden, um die SQL Server-Datenbank auf Azure SQL-Datenbank bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="096aa-123">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="096aa-124">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="096aa-124">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="096aa-125">Microsoft Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web</span><span class="sxs-lookup"><span data-stu-id="096aa-125">Microsoft Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web</span></span>
>   
> 
> <span data-ttu-id="096aa-126">Dieses Lernprogramm funktioniert auch mit Visual Studio 2012, jedoch werden einige Unterschiede in der Vorlage "Benutzer" Schnittstelle "und"-Projekt.</span><span class="sxs-lookup"><span data-stu-id="096aa-126">This tutorial also works with Visual Studio 2012 but there will be some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="096aa-127">Was müssen Sie erstellen</span><span class="sxs-lookup"><span data-stu-id="096aa-127">What you'll build</span></span>

<span data-ttu-id="096aa-128">In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:</span><span class="sxs-lookup"><span data-stu-id="096aa-128">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="096aa-129">Data-Objekte, die eine Universität mit Studenten in Kursen registriert wiedergeben</span><span class="sxs-lookup"><span data-stu-id="096aa-129">Build data objects that reflect a university with students enrolled in courses</span></span>
2. <span data-ttu-id="096aa-130">Erstellen von Tabellen aus den Objekten</span><span class="sxs-lookup"><span data-stu-id="096aa-130">Build database tables from the objects</span></span>
3. <span data-ttu-id="096aa-131">Füllen Sie die Datenbank mit Testdaten</span><span class="sxs-lookup"><span data-stu-id="096aa-131">Populate the database with test data</span></span>
4. <span data-ttu-id="096aa-132">Anzeigen von Daten in einem Web form</span><span class="sxs-lookup"><span data-stu-id="096aa-132">Display data in a web form</span></span>

## <a name="set-up-project"></a><span data-ttu-id="096aa-133">Einrichten des Projekts</span><span class="sxs-lookup"><span data-stu-id="096aa-133">Set up project</span></span>

<span data-ttu-id="096aa-134">In Visual Studio 2013, erstellen Sie ein neues **ASP.NET-Webanwendung** aufgerufen **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="096aa-134">In Visual Studio 2013, create a new **ASP.NET Web Application** called **ContosoUniversityModelBinding**.</span></span>

![Projekt erstellen](retrieving-data/_static/image2.png)

<span data-ttu-id="096aa-136">Wählen Sie die Web Forms-Vorlage, und lassen Sie die andere Standardoptionen.</span><span class="sxs-lookup"><span data-stu-id="096aa-136">Select the Web Forms template, and leave the other default options.</span></span> <span data-ttu-id="096aa-137">Klicken Sie auf OK, um das Projekt einrichten.</span><span class="sxs-lookup"><span data-stu-id="096aa-137">Click OK to setup the project.</span></span>

![Wählen Sie WebForms](retrieving-data/_static/image3.png)

<span data-ttu-id="096aa-139">Zunächst wird eine Reihe von kleinen Änderungen zum Anpassen der Darstellung des Standorts erstellt.</span><span class="sxs-lookup"><span data-stu-id="096aa-139">First, you will make a couple of small changes to customize the appearance of the site.</span></span> <span data-ttu-id="096aa-140">Öffnen der **Site.Master** Datei, und ändern Sie den Titel Einbeziehung von Contoso University statt meine ASP.NET-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="096aa-140">Open the **Site.Master** file, and change the title to include Contoso University instead of My ASP.NET Application.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

<span data-ttu-id="096aa-141">Ändern Sie dann den Headertext aus **Anwendungsname** auf **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="096aa-141">Then, change the header text from **Application name** to **Contoso University**.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

<span data-ttu-id="096aa-142">Ändern Sie auch in Site.Master, die Navigationslinks, die angezeigt, in der Kopfzeile werden entsprechend der Seiten, die diesem Standort relevant sind.</span><span class="sxs-lookup"><span data-stu-id="096aa-142">Also in Site.Master, change the navigation links that appear in the header to reflect the pages that are relevant to this site.</span></span> <span data-ttu-id="096aa-143">Sie benötigen nicht entweder die **zu** Seite oder die **wenden Sie sich an** Seite, damit diese Links entfernt werden kann.</span><span class="sxs-lookup"><span data-stu-id="096aa-143">You will not need either the **About** page or the **Contact** page so those links can be removed.</span></span> <span data-ttu-id="096aa-144">Stattdessen benötigen Sie einen Link zu einer Seite aufgerufen **Studenten**.</span><span class="sxs-lookup"><span data-stu-id="096aa-144">Instead, you will need a link to a page called **Students**.</span></span> <span data-ttu-id="096aa-145">Diese Seite wurde noch nicht erstellt.</span><span class="sxs-lookup"><span data-stu-id="096aa-145">This page has not been created yet.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

<span data-ttu-id="096aa-146">Speichern und Schließen von Site.Master.</span><span class="sxs-lookup"><span data-stu-id="096aa-146">Save and close Site.Master.</span></span>

<span data-ttu-id="096aa-147">Nun erstellen Sie das Web Form zur Anzeige von Student-Daten.</span><span class="sxs-lookup"><span data-stu-id="096aa-147">Now, you'll create the web form for displaying student data.</span></span> <span data-ttu-id="096aa-148">Mit der rechten Maustaste des Projekts und **hinzufügen** eine **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="096aa-148">Right-click your project, and **Add** a **New Item**.</span></span> <span data-ttu-id="096aa-149">Wählen Sie die **Webformular mit Gestaltungsvorlage** Vorlage, und nennen Sie sie **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="096aa-149">Select the **Web Form with Master Page** template, and name it **Students.aspx**.</span></span>

![Seite "erstellen"](retrieving-data/_static/image5.png)

<span data-ttu-id="096aa-151">Wählen Sie **Site.Master** als die Gestaltungsvorlage für das neue WebForm.</span><span class="sxs-lookup"><span data-stu-id="096aa-151">Select **Site.Master** as the master page for the new web form.</span></span>

## <a name="create-the-data-models-and-database"></a><span data-ttu-id="096aa-152">Erstellen Sie die Datenmodelle und Datenbank</span><span class="sxs-lookup"><span data-stu-id="096aa-152">Create the data models and database</span></span>

<span data-ttu-id="096aa-153">Sie werden Code First-Migrationen verwenden, um Objekte und den entsprechenden Datenbanktabellen erstellen.</span><span class="sxs-lookup"><span data-stu-id="096aa-153">You will use Code First Migrations to create objects and the corresponding database tables.</span></span> <span data-ttu-id="096aa-154">Diese Tabellen werden Informationen zu den Studenten und ihre Kurse gespeichert.</span><span class="sxs-lookup"><span data-stu-id="096aa-154">These tables will store information about the students and their courses.</span></span>

<span data-ttu-id="096aa-155">Fügen Sie eine neue Klasse mit dem Namen im Ordner Models **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="096aa-155">In the Models folder, add a new class named **UniversityModels.cs**.</span></span>

![Erstellen der Skriptobjektmodell-Klasse](retrieving-data/_static/image7.png)

<span data-ttu-id="096aa-157">Definieren Sie die Klassen SchoolContext, Studenten, Registrierung und Kurs in dieser Datei wie folgt:</span><span class="sxs-lookup"><span data-stu-id="096aa-157">In this file, define the SchoolContext, Student, Enrollment, and Course classes as follows:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

<span data-ttu-id="096aa-158">Die SchoolContext-Klasse abgeleitet von ' DbContext ' abgeleitet, die die datenbankverbindung und die Änderungen in den Daten verwaltet wird.</span><span class="sxs-lookup"><span data-stu-id="096aa-158">The SchoolContext class derives from DbContext, which manages the database connection and changes in the data.</span></span>

<span data-ttu-id="096aa-159">Beachten Sie die Attribute, die auf angewendet wurden, in der Klasse Student die **FirstName**, **LastName**, und **Jahr** Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="096aa-159">In the Student class, notice the attributes that have been applied to the **FirstName**, **LastName**, and **Year** properties.</span></span> <span data-ttu-id="096aa-160">Diese Attribute werden für die datenüberprüfung in diesem Lernprogramm verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-160">These attributes will be used for data validation in this tutorial.</span></span> <span data-ttu-id="096aa-161">Um den Code für diese Demo-Projekt zu vereinfachen, wurden nur diese Eigenschaften mit Data-Validierungsattributen gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="096aa-161">To simplify the code for this demonstration project, only these properties were marked with data-validation attributes.</span></span> <span data-ttu-id="096aa-162">In einem echten Projekt würde Sie Validierungsattribute für alle Eigenschaften gelten, die überprüfte Daten, z. B. Eigenschaften in der Registrierung und Kurs Klassen benötigen.</span><span class="sxs-lookup"><span data-stu-id="096aa-162">In a real project, you would apply validation attributes to all properties that need validated data, such as properties in the Enrollment and Course classes.</span></span>

<span data-ttu-id="096aa-163">UniversityModels.cs zu speichern.</span><span class="sxs-lookup"><span data-stu-id="096aa-163">Save UniversityModels.cs.</span></span>

<span data-ttu-id="096aa-164">Die Tools für die Code First-Migrationen verwendet zum Einrichten einer Datenbank, basierend auf diesen Klassen.</span><span class="sxs-lookup"><span data-stu-id="096aa-164">You will use the tools for Code First Migrations to set up a database based on these classes.</span></span>

<span data-ttu-id="096aa-165">In **Package Manager Console**, führen Sie den Befehl:</span><span class="sxs-lookup"><span data-stu-id="096aa-165">In **Package Manager Console**, run the command:</span></span>  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

<span data-ttu-id="096aa-166">Wenn der Befehl erfolgreich ausgeführt wird, erhalten Sie eine Meldung, die besagt, dass die Migrationen aktiviert wurden,</span><span class="sxs-lookup"><span data-stu-id="096aa-166">If the command completes successfully you will receive a message stating migrations have been enabled,</span></span>

![Aktivieren von Migrationen](retrieving-data/_static/image8.png)

<span data-ttu-id="096aa-168">Beachten Sie, dass eine neue Datei mit dem Namen **"Configuration.cs"** erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="096aa-168">Notice that a new file named **Configuration.cs** has been created.</span></span> <span data-ttu-id="096aa-169">In Visual Studio wird diese Datei automatisch geöffnet, nachdem er erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="096aa-169">In Visual Studio, this file is automatically opened after it is created.</span></span> <span data-ttu-id="096aa-170">Enthält die Konfigurationsklasse eine **Ausgangswert** Methode, die Ihnen ermöglicht, die die Datenbanktabellen mit Testdaten vorab aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="096aa-170">The Configuration class contains a **Seed** method which enables you to pre-populate the database tables with test data.</span></span>

<span data-ttu-id="096aa-171">Fügen Sie den folgenden Code, um die Seed-Methode.</span><span class="sxs-lookup"><span data-stu-id="096aa-171">Add the following code to the Seed method.</span></span> <span data-ttu-id="096aa-172">Sie müssen Hinzufügen einer **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.</span><span class="sxs-lookup"><span data-stu-id="096aa-172">You'll need to add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

<span data-ttu-id="096aa-173">"Configuration.cs" zu speichern.</span><span class="sxs-lookup"><span data-stu-id="096aa-173">Save Configuration.cs.</span></span>

<span data-ttu-id="096aa-174">In der Paket-Manager-Konsole führen Sie den Befehl `add-migration initial`.</span><span class="sxs-lookup"><span data-stu-id="096aa-174">In the Package Manager Console, run the command `add-migration initial`.</span></span>

<span data-ttu-id="096aa-175">Führen Sie dann den Befehl `update-database`.</span><span class="sxs-lookup"><span data-stu-id="096aa-175">Then, run the command `update-database`.</span></span>

<span data-ttu-id="096aa-176">Wenn Sie eine Ausnahme beim Ausführen dieses Befehls erhalten, ist es möglich, dass die Werte für StudentID und CourseID aus den Werten der Seed-Methode geändert haben.</span><span class="sxs-lookup"><span data-stu-id="096aa-176">If you receive an exception when running this command, it is possible that the values for StudentID and CourseID have varied from the values in the Seed method.</span></span> <span data-ttu-id="096aa-177">Öffnen Sie die Tabellen in der Datenbank, und suchen Sie vorhandene Werte für StudentID und CourseID.</span><span class="sxs-lookup"><span data-stu-id="096aa-177">Open those tables in the database and find existing values for StudentID and CourseID.</span></span> <span data-ttu-id="096aa-178">Fügen Sie diese Werte in den Code für das seeding der Registrierung für die Tabelle hinzu.</span><span class="sxs-lookup"><span data-stu-id="096aa-178">Add those values in the code for seeding the Enrollments table.</span></span>

<span data-ttu-id="096aa-179">Die Datenbankdatei wurde hinzugefügt, aber derzeit im Projekt ausgeblendet ist.</span><span class="sxs-lookup"><span data-stu-id="096aa-179">The database file has been added, but it is currently hidden in the project.</span></span> <span data-ttu-id="096aa-180">Klicken Sie auf **alle Dateien anzeigen** die Datei anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="096aa-180">Click **Show All Files** to display the file.</span></span>

![Alle Dateien anzeigen](retrieving-data/_static/image10.png)

<span data-ttu-id="096aa-182">Beachten Sie die MDF-Datei wird jetzt in der App\_Datenordner.</span><span class="sxs-lookup"><span data-stu-id="096aa-182">Notice the .mdf file now appears in the App\_Data folder.</span></span>

![Datenbankdatei](retrieving-data/_static/image12.png)

<span data-ttu-id="096aa-184">Doppelklicken Sie auf die MDF-Datei, und öffnen Sie den Server-Explorer.</span><span class="sxs-lookup"><span data-stu-id="096aa-184">Double-click the .mdf file and open the Server Explorer.</span></span> <span data-ttu-id="096aa-185">Die Tabellen jetzt vorhanden und werden mit Daten aufgefüllt.</span><span class="sxs-lookup"><span data-stu-id="096aa-185">The tables now exist and are populated with data.</span></span>

![Datenbanktabellen](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a><span data-ttu-id="096aa-187">Anzeigen von Daten aus verknüpften Tabellen und Studenten</span><span class="sxs-lookup"><span data-stu-id="096aa-187">Display data from Students and related tables</span></span>

<span data-ttu-id="096aa-188">Die Daten in der Datenbank sind Sie nun bereit zum Abrufen der Daten und zeigt sie auf einer Webseite.</span><span class="sxs-lookup"><span data-stu-id="096aa-188">With data in the database, you are now ready to retrieve that data and display it in a web page.</span></span> <span data-ttu-id="096aa-189">Verwenden Sie eine **GridView** Steuerelement zum Anzeigen der Daten in Spalten und Zeilen.</span><span class="sxs-lookup"><span data-stu-id="096aa-189">You will use a **GridView** control to display the data in columns and rows.</span></span>

<span data-ttu-id="096aa-190">Students.aspx öffnen, und suchen Sie die **MainContent** Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="096aa-190">Open Students.aspx, and locate the **MainContent** placeholder.</span></span> <span data-ttu-id="096aa-191">In dieser Platzhalter Hinzufügen einer **GridView** Steuerelement, das den folgenden Code enthält.</span><span class="sxs-lookup"><span data-stu-id="096aa-191">Within that placeholder, add a **GridView** control that includes the following code.</span></span>

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

<span data-ttu-id="096aa-192">Es gibt eine Reihe von wichtiger Konzepte, die in diesem Markupcode für Sie zu beachten.</span><span class="sxs-lookup"><span data-stu-id="096aa-192">There are a couple of important concepts in this markup code for you to notice.</span></span> <span data-ttu-id="096aa-193">Zuerst, beachten Sie, dass ein Wert, für festgelegt wird die **SelectMethod** Eigenschaft im GridView-Element.</span><span class="sxs-lookup"><span data-stu-id="096aa-193">First, notice that a value is set for the **SelectMethod** property in the GridView element.</span></span> <span data-ttu-id="096aa-194">Dieser Wert gibt den Namen der Methode, die zum Abrufen von Daten für die GridView verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="096aa-194">This value specifies the name of the method that is used for retrieving data for the GridView.</span></span> <span data-ttu-id="096aa-195">Diese Methode wird im nächsten Schritt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-195">You will create this method in the next step.</span></span> <span data-ttu-id="096aa-196">Zweitens, beachten Sie, dass die **ItemType** Eigenschaftensatz Student-Klasse, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="096aa-196">Second, notice that the **ItemType** property is set to the Student class that you created earlier.</span></span> <span data-ttu-id="096aa-197">Durch Festlegen dieses Werts finden Sie in den Eigenschaften dieser Klasse im Markupcode.</span><span class="sxs-lookup"><span data-stu-id="096aa-197">By setting this value, you can refer to properties of that class in the markup code.</span></span> <span data-ttu-id="096aa-198">Beispielsweise enthält die Student-Klasse eine Auflistung mit dem Namen Bereitstellungen.</span><span class="sxs-lookup"><span data-stu-id="096aa-198">For example, the Student class contains a collection named Enrollments.</span></span> <span data-ttu-id="096aa-199">Sie können **Item.Enrollments** auf diese Auflistung abzurufen und dann LINQ-Syntax verwenden, um die Summe der registrierten Gutschriften für Studenten zu abzurufen.</span><span class="sxs-lookup"><span data-stu-id="096aa-199">You can use **Item.Enrollments** to retrieve that collection and then use LINQ syntax to retrieve the sum of enrolled credits for each student.</span></span>

<span data-ttu-id="096aa-200">In der Code-Behind-Datei müssen Sie die Methode hinzufügen, die für angegeben wird die **SelectMethod** Wert.</span><span class="sxs-lookup"><span data-stu-id="096aa-200">In the code-behind file, you need to add the method that is specified for the **SelectMethod** value.</span></span> <span data-ttu-id="096aa-201">Open **Students.aspx.cs**, und fügen **mit** -Anweisungen für die **ContosoUniversityModelBinding.Models** und **System.Data.Entity**Namespaces.</span><span class="sxs-lookup"><span data-stu-id="096aa-201">Open **Students.aspx.cs**, and add **using** statements for the **ContosoUniversityModelBinding.Models** and **System.Data.Entity** namespaces.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

<span data-ttu-id="096aa-202">Anschließend fügen Sie die folgende Methode hinzu.</span><span class="sxs-lookup"><span data-stu-id="096aa-202">Then, add the following method.</span></span> <span data-ttu-id="096aa-203">Beachten Sie, dass die Namen dieser Methode mit dem Namen übereinstimmt, den Sie für die SelectMethod bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="096aa-203">Notice that the name of this method matches the name you provided for SelectMethod.</span></span>

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

<span data-ttu-id="096aa-204">Die **Include** Klausel verbessert die Leistung dieser Abfrage jedoch nicht unbedingt erforderlich für die Abfrage funktioniert.</span><span class="sxs-lookup"><span data-stu-id="096aa-204">The **Include** clause improves the performance of this query but is not essential for the query to work.</span></span> <span data-ttu-id="096aa-205">Ohne die Include-Klausel würde die Daten abgerufen werden mithilfe des verzögerten Ladens umfasst das Senden, dass eine separate Abfrage an die Datenbank jedes Mal-bezogene Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-205">Without the Include clause, the data would be retrieved using lazy loading, which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="096aa-206">Durch die Bereitstellung der Include-Klausel, werden die Daten abgerufen, mit unverzüglichem Laden, d. h., alle verwandten Daten über eine einzelne Abfrage der Datenbank abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="096aa-206">By providing the Include clause, the data is retrieved using eager loading, which means all of the related data is retrieved through a single query of the database.</span></span> <span data-ttu-id="096aa-207">In Fällen, in denen meisten der zugehörigen Daten werden nicht verwendet wird, sind unverzüglichem Laden kann weniger effizient sein, da mehr Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-207">In cases where most of the related data is not be used, eager loading can be less efficient because more data is retrieved.</span></span> <span data-ttu-id="096aa-208">Allerdings bietet die beste Leistung in diesem Fall unverzüglichem Laden, da die verknüpften Daten für jeden Datensatz angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-208">However, in this case, eager loading provides the best performance because the related data is displayed for each record.</span></span>

<span data-ttu-id="096aa-209">Weitere Informationen zu Überlegungen zur Leistung beim Laden von Daten beziehen, finden Sie im Abschnitt **Lazy Eager und explizite Laden von verknüpften Daten** in die [verwandte Lesen von Daten mit dem Entity Framework in einer ASP .NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Thema.</span><span class="sxs-lookup"><span data-stu-id="096aa-209">For more information about performance consideration when loading related data, see the section titled **Lazy, Eager, and Explicit Loading of Related Data** in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) topic.</span></span>

<span data-ttu-id="096aa-210">Standardmäßig ist die Daten durch die Werte der Eigenschaft markiert als Schlüssel sortiert.</span><span class="sxs-lookup"><span data-stu-id="096aa-210">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="096aa-211">Sie können eine OrderBy-Klausel geben Sie einen anderen Wert für die Sortierung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="096aa-211">You can add an OrderBy clause to specify a different value for sorting.</span></span> <span data-ttu-id="096aa-212">In diesem Beispiel wird die StudentID-Standardeigenschaft für die Sortierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="096aa-212">In this example, the default StudentID property is used for sorting.</span></span> <span data-ttu-id="096aa-213">In der [sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) Thema, damit Sie der Benutzer für eine Spalte für die Sortierung auswählen.</span><span class="sxs-lookup"><span data-stu-id="096aa-213">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) topic, you will enable the user to select a column for sorting.</span></span>

<span data-ttu-id="096aa-214">Führen Sie die Webanwendung, und navigieren Sie zu der Seite "Students".</span><span class="sxs-lookup"><span data-stu-id="096aa-214">Run your web application, and navigate to the Students page.</span></span> <span data-ttu-id="096aa-215">Die Seite "Students" zeigt die folgenden Student-Informationen.</span><span class="sxs-lookup"><span data-stu-id="096aa-215">The Students page displays the following student information.</span></span>

![Anzeigen von Daten](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="096aa-217">Automatische Generierung von Modell Bindung Methoden</span><span class="sxs-lookup"><span data-stu-id="096aa-217">Automatic generation of model binding methods</span></span>

<span data-ttu-id="096aa-218">Wenn Sie über diese Reihe von Lernprogrammen zu arbeiten, können Sie einfach den Code aus dem Lernprogramm zu Ihrem Projekt kopieren.</span><span class="sxs-lookup"><span data-stu-id="096aa-218">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="096aa-219">Ein Nachteil dieses Ansatzes ist jedoch, dass Sie nicht über die Funktion von Visual Studio zum automatischen Generieren von Code für die Bindung Modellmethoden bereitgestellten informiert werden können.</span><span class="sxs-lookup"><span data-stu-id="096aa-219">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="096aa-220">Wenn Sie auf Ihren eigenen Projekten arbeiten, können Sie automatische codegenerierung speichern, Zeit und Hilfe erhalten Sie einen Eindruck davon, wie einen Vorgang implementiert.</span><span class="sxs-lookup"><span data-stu-id="096aa-220">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="096aa-221">Dieser Abschnitt beschreibt die Funktion "Automatische Code generieren".</span><span class="sxs-lookup"><span data-stu-id="096aa-221">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="096aa-222">Dieser Abschnitt dient nur zu Informationszwecken und enthält keiner Code, den Sie in Ihrem Projekt implementieren müssen.</span><span class="sxs-lookup"><span data-stu-id="096aa-222">This section is only informational and does not contain any code you need to implement in your project.</span></span>

<span data-ttu-id="096aa-223">Beim Festlegen eines Werts für die **SelectMethod**, **UpdateMethod**, **InsertMethod**, oder **DeleteMethod** Eigenschaften im Markupcode Sie können auswählen, die **erstellen neue Methode** Option.</span><span class="sxs-lookup"><span data-stu-id="096aa-223">When setting a value for the **SelectMethod**, **UpdateMethod**, **InsertMethod**, or **DeleteMethod** properties in the markup code, you can select the **Create New Method** option.</span></span>

![Erstellen Sie neue Methode](retrieving-data/_static/image18.png)

<span data-ttu-id="096aa-225">Visual Studio nicht nur eine Methode im Code-Behind mit der richtigen Signatur erstellt, sondern generiert auch Implementierungscode um Unterstützung bei der Ausführung des Vorgangs.</span><span class="sxs-lookup"><span data-stu-id="096aa-225">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to assist you with performing the operation.</span></span> <span data-ttu-id="096aa-226">Wenn Sie zuerst festlegen, dass die **ItemType** Eigenschaft vor der Verwendung der automatischen Code Generation-Funktion den generierten Code verwendet dieses Typs für die Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="096aa-226">If you first set the **ItemType** property before using the automatic code generation feature, the generated code will use that type for the operations.</span></span> <span data-ttu-id="096aa-227">Beispielsweise wird beim Festlegen der Eigenschaft UpdateMethod automatisch der folgende Code generiert:</span><span class="sxs-lookup"><span data-stu-id="096aa-227">For example, when setting the UpdateMethod property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="096aa-228">Der obige Code müssen erneut, nicht dem Projekt hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="096aa-228">Again, the above code does not need to be added to your project.</span></span> <span data-ttu-id="096aa-229">In den nächsten Lernprogrammen implementieren Sie Methoden zum Aktualisieren, löschen und neue Daten hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="096aa-229">In the next tutorial, you will implement methods for updating, deleting and adding new data.</span></span>

## <a name="conclusion"></a><span data-ttu-id="096aa-230">Schlussfolgerung</span><span class="sxs-lookup"><span data-stu-id="096aa-230">Conclusion</span></span>

<span data-ttu-id="096aa-231">In diesem Lernprogramm erstellte Modell Datenklassen und eine Datenbank aus dieser Klassen generiert.</span><span class="sxs-lookup"><span data-stu-id="096aa-231">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="096aa-232">Sie können die Datenbanktabellen mit Testdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="096aa-232">You filled the database tables with test data.</span></span> <span data-ttu-id="096aa-233">Sie wurden die modellbindung zum Abrufen von Daten aus der Datenbank verwendet, und klicken Sie dann die Daten in einem GridView angezeigt.</span><span class="sxs-lookup"><span data-stu-id="096aa-233">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="096aa-234">In der nächsten [Lernprogramm](updating-deleting-and-creating-data.md) in dieser Serie, aktivieren Sie aktualisieren, löschen und Erstellen von Daten.</span><span class="sxs-lookup"><span data-stu-id="096aa-234">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you will enable updating, deleting, and creating data.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="096aa-235">Nächste</span><span class="sxs-lookup"><span data-stu-id="096aa-235">Next</span></span>](updating-deleting-and-creating-data.md)