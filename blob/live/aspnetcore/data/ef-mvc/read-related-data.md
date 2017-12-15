---
title: "ASP.NET Core MVC mit EF-Core - lesen verknüpften Daten - 6 von 10"
author: tdykstra
description: "In diesem Lernprogramm Sie lesen und Anzeigen von verknüpften Daten – d. h. die Daten, die das Entity Framework in Navigationseigenschaften lädt."
keywords: "ASP.NET Core, Entity Framework Core, verknüpfte Daten verknüpft."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="1a402-104">Lesen-bezogene Daten – EF-Core mit ASP.NET Core MVC-Lernprogramm (6 von 10)</span><span class="sxs-lookup"><span data-stu-id="1a402-104">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="1a402-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1a402-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a402-106">Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1a402-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="1a402-107">Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="1a402-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="1a402-108">Im vorherigen Lernprogramm abgeschlossen Sie das Datenmodell "School".</span><span class="sxs-lookup"><span data-stu-id="1a402-108">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="1a402-109">In diesem Lernprogramm Sie lesen und Anzeigen von verknüpften Daten – d. h. die Daten, die das Entity Framework in Navigationseigenschaften lädt.</span><span class="sxs-lookup"><span data-stu-id="1a402-109">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="1a402-110">Die folgenden Abbildungen zeigen den Seiten, arbeiten Sie mit.</span><span class="sxs-lookup"><span data-stu-id="1a402-110">The following illustrations show the pages that you'll work with.</span></span>

![Kurse Indexseite](read-related-data/_static/courses-index.png)

![Indexseite für Dozenten](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="1a402-113">Eager, explizite und lazy Loading-verknüpfter Daten</span><span class="sxs-lookup"><span data-stu-id="1a402-113">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="1a402-114">Es gibt mehrere Möglichkeiten, objektrelationales Mapping (ORM)-Software, z. B. Entity Framework in die Navigationseigenschaften einer Entität verknüpfte Daten geladen werden können:</span><span class="sxs-lookup"><span data-stu-id="1a402-114">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="1a402-115">Unverzüglichem Laden.</span><span class="sxs-lookup"><span data-stu-id="1a402-115">Eager loading.</span></span> <span data-ttu-id="1a402-116">Wenn die Entität gelesen wird, werden darin verknüpfte Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-116">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="1a402-117">Dies führt normalerweise zu einer einzelnen Join-Abfrage, die alle Daten abruft, die erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="1a402-118">Sie geben unverzüglichem Laden in Entity Framework Core mit der `Include` und `ThenInclude` Methoden.</span><span class="sxs-lookup"><span data-stu-id="1a402-118">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Beispiel mit unverzüglichem Laden](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="1a402-120">Sie können einige der Daten in eine eigene Abfrage abrufen und EF "behoben von" die Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="1a402-120">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="1a402-121">D. h. fügt EF automatisch separat abgerufenen Entitäten, auf dem sie gehören, in den Navigationseigenschaften der zuvor abgerufenen Entitäten hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a402-121">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="1a402-122">Für die Abfrage, die verknüpfte Daten abruft, können Sie die `Load` Methode anstelle einer Methode, die eine Liste oder ein Objekt, z. B. zurückgibt `ToList` oder `Single`.</span><span class="sxs-lookup"><span data-stu-id="1a402-122">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Beispiel für eine eigene Abfrage](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="1a402-124">Explizites Laden.</span><span class="sxs-lookup"><span data-stu-id="1a402-124">Explicit loading.</span></span> <span data-ttu-id="1a402-125">Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-125">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1a402-126">Schreiben Sie Code, der die verwandten Daten abruft, wenn es benötigt wird.</span><span class="sxs-lookup"><span data-stu-id="1a402-126">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="1a402-127">Wie im Fall von Eager loading mit separaten Abfragen an die Datenbank gesendet explizite das Laden der Ergebnisse in mehreren Abfragen.</span><span class="sxs-lookup"><span data-stu-id="1a402-127">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="1a402-128">Der Unterschied besteht darin, dass der Code mit expliziten Laden die Navigationseigenschaften zu ladenden gibt an.</span><span class="sxs-lookup"><span data-stu-id="1a402-128">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="1a402-129">In Entity Framework Core 1.1 können Sie die `Load` Methode, um das explizite laden auszuführen.</span><span class="sxs-lookup"><span data-stu-id="1a402-129">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="1a402-130">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1a402-130">For example:</span></span>

  ![Explizites Laden-Beispiel](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="1a402-132">Verzögertes Laden.</span><span class="sxs-lookup"><span data-stu-id="1a402-132">Lazy loading.</span></span> <span data-ttu-id="1a402-133">Wenn die Entität zuerst gelesen wird, ist nicht verbundene Daten abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-133">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="1a402-134">Allerdings werden beim ersten Versuch, auf eine Navigationseigenschaft, für diese Navigationseigenschaft erforderlichen Daten automatisch abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-134">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="1a402-135">Eine Abfrage wird jedes Mal an die Datenbank gesendet Sie zum Abrufen von Daten von einer Navigationseigenschaft zum ersten Mal versuchen.</span><span class="sxs-lookup"><span data-stu-id="1a402-135">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="1a402-136">Verzögertes Laden von Entity Framework Core 1.0 nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1a402-136">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="1a402-137">Überlegungen zur Leistung</span><span class="sxs-lookup"><span data-stu-id="1a402-137">Performance considerations</span></span>

<span data-ttu-id="1a402-138">Wenn Sie, die Sie für jede Entität abgerufen verknüpfte Daten benötigen wissen, bietet eager loading, häufig die beste Leistung, da eine einzelne Abfrage, die an die Datenbank gesendet, in der Regel effizienter als separate Abfragen für jede Entität abgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="1a402-138">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="1a402-139">Nehmen wir beispielsweise an, dass jede Abteilung zehn Verwandte Kurse gelten.</span><span class="sxs-lookup"><span data-stu-id="1a402-139">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="1a402-140">Unverzüglichem Laden von alle verknüpften Daten führt in einer einzelnen (Join)-Abfrage und einem einzelnen Roundtrip-mit der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1a402-140">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="1a402-141">Für Kurse für jede Abteilung eine eigene Abfrage führt zu elf Roundtrips zur Datenbank.</span><span class="sxs-lookup"><span data-stu-id="1a402-141">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="1a402-142">Zusätzliche Roundtrips zur Datenbank sind vor allem auf die Leistung beeinträchtigen, wenn Latenz hoch ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-142">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="1a402-143">Andererseits, in einigen Szenarien eine eigene Abfrage ist jedoch effizienter.</span><span class="sxs-lookup"><span data-stu-id="1a402-143">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="1a402-144">Unverzüglichem Laden von alle verknüpften Daten in einer Abfrage könnte eine sehr komplexe Verknüpfung generiert werden, verursachen, die SQL Server nicht effizient verarbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="1a402-144">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="1a402-145">Oder Sie müssen eine Entität Navigationseigenschaften nur für einen Teil einer Reihe von Entitäten zugreifen, die Sie verarbeiten, kann eine eigene Abfrage möglicherweise besser ausgeführt werden, da unverzüglichem Laden aller Elemente im Vorfeld mehr Daten als benötigt abrufen würde.</span><span class="sxs-lookup"><span data-stu-id="1a402-145">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="1a402-146">Wenn die Leistung kritisch ist, empfiehlt es sich zum Testen von Leistung beides Möglichkeiten, um die beste Wahl vornehmen zu können.</span><span class="sxs-lookup"><span data-stu-id="1a402-146">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="1a402-147">Erstellen Sie eine Kurse-Seite, die Name der Abteilung anzeigt</span><span class="sxs-lookup"><span data-stu-id="1a402-147">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="1a402-148">Die Kurs-Entität enthält, eine Navigationseigenschaft, die die Entität "Department" der Abteilung enthält, die der Kurs zugewiesen ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-148">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="1a402-149">Um den Namen der zugewiesenen Abteilung in einer Liste von Kurse anzuzeigen, müssen Sie die Name-Eigenschaft die Entität Department nicht entnommen werden, die in der `Course.Department` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1a402-149">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="1a402-150">Erstellen Sie einen Controller mit dem Namen CoursesController für den Kurs Entitätstyp, die mit denselben Optionen für die **MVC-Controller mit Ansichten unter Verwendung von Entity Framework** Scaffolder, die Sie zuvor für den Controller Studenten wie gezeigt in der folgende Abbildung:</span><span class="sxs-lookup"><span data-stu-id="1a402-150">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Hinzufügen eines Controllers Kurse](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="1a402-152">Open *CoursesController.cs* und untersuchen Sie die `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="1a402-152">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="1a402-153">Der automatische Gerüstbau angegeben unverzüglichem Laden für die `Department` Navigationseigenschaft mithilfe der `Include` Methode.</span><span class="sxs-lookup"><span data-stu-id="1a402-153">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="1a402-154">Ersetzen Sie die `Index` Methode durch den folgenden Code, der einen geeigneteren Namen für die `IQueryable` Kurs Entitäten zurückgibt (`courses` anstelle von `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="1a402-154">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="1a402-155">Open *Views/Courses/Index.cshtml* , und Ersetzen Sie den Code durch den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="1a402-155">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="1a402-156">Die Änderungen werden hervorgehoben:</span><span class="sxs-lookup"><span data-stu-id="1a402-156">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="1a402-157">Sie haben die folgenden Änderungen an der scaffolded Code vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="1a402-157">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="1a402-158">Die Überschrift von Index in Courses geändert.</span><span class="sxs-lookup"><span data-stu-id="1a402-158">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="1a402-159">Hinzugefügt eine **Anzahl** Spalte, die zeigt die `CourseID` Eigenschaftswert.</span><span class="sxs-lookup"><span data-stu-id="1a402-159">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="1a402-160">Primärschlüssel werden nicht standardmäßig Gerüstbau, da normalerweise sie Endbenutzern bedeutungslos sind.</span><span class="sxs-lookup"><span data-stu-id="1a402-160">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="1a402-161">Allerdings in diesem Fall der Primärschlüssel sinnvoll ist und Sie sie anzeigen möchten.</span><span class="sxs-lookup"><span data-stu-id="1a402-161">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="1a402-162">Geändert die **Abteilung** Spalte der Abteilungsname angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a402-162">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="1a402-163">Der Code zeigt die `Name` -Eigenschaft der Abteilung-Entität, die in geladen ist die `Department` Navigationseigenschaft:</span><span class="sxs-lookup"><span data-stu-id="1a402-163">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="1a402-164">Führen Sie die app, und wählen Sie die **Kurse** Registerkarte ", um die Liste mit den Abteilungsnamen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-164">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Kurse Indexseite](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="1a402-166">Erstellen Sie eine Lehrkräfte-Seite, in der Kurse und Registrierung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a402-166">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="1a402-167">In diesem Abschnitt müssen Sie einem Controller und Ansicht für die Instructor-Entität erstellen, um die Seite Lehrkräfte anzuzeigen:</span><span class="sxs-lookup"><span data-stu-id="1a402-167">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Indexseite für Dozenten](read-related-data/_static/instructors-index.png)

<span data-ttu-id="1a402-169">Auf dieser Seite liest und zeigt verknüpfte Daten auf folgende Weise:</span><span class="sxs-lookup"><span data-stu-id="1a402-169">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="1a402-170">Die Liste der Lehrkräfte zeigt aufeinander bezogene Daten in die OfficeAssignment-Entität.</span><span class="sxs-lookup"><span data-stu-id="1a402-170">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="1a402-171">Die Instructor und OfficeAssignment Entitäten sind in einer 1: 0 (null)-oder-1-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="1a402-171">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="1a402-172">Verwenden Sie für die Entitäten OfficeAssignment unverzüglichem Laden.</span><span class="sxs-lookup"><span data-stu-id="1a402-172">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="1a402-173">Wie zuvor erläutert, ist die unverzüglichem Laden in der Regel effizienter, wenn Sie die verknüpften Daten für alle abgerufenen Zeilen von der primären Tabelle benötigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-173">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="1a402-174">In diesem Fall möchten Sie Office-Zuweisungen für alle angezeigten Lehrkräfte anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-174">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="1a402-175">Wenn der Benutzer einen Kursleiter auswählt, werden verknüpfte Kurs Entitäten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a402-175">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="1a402-176">Die Entitäten Kursleiter und Kurs befinden sich in einer m: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="1a402-176">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="1a402-177">Verwenden Sie für die Kurs-Entitäten und ihre zugehörigen Entitäten der Abteilung unverzüglichem Laden.</span><span class="sxs-lookup"><span data-stu-id="1a402-177">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="1a402-178">In diesem Fall können eine eigene Abfrage effizienter sein, da Sie nur für den ausgewählten Dozenten Kurse benötigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-178">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="1a402-179">Dieses Beispiel zeigt jedoch mit unverzüglichem Laden für Navigationseigenschaften innerhalb von Entitäten, die selbst in den Navigationseigenschaften werden.</span><span class="sxs-lookup"><span data-stu-id="1a402-179">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="1a402-180">Wenn der Benutzer einen Kurs auswählt, werden verknüpfte Daten aus der Entitätssammlung Registrierung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a402-180">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="1a402-181">Die Entitäten Kurs und Registrierung sind in einer 1: n-Beziehung.</span><span class="sxs-lookup"><span data-stu-id="1a402-181">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="1a402-182">Sie müssen eine eigene Abfrage für Registrierung Entitäten und ihre verknüpften Student-Entitäten verwenden.</span><span class="sxs-lookup"><span data-stu-id="1a402-182">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="1a402-183">Erstellen Sie ein Ansichtsmodell für die Indexansicht Dozenten</span><span class="sxs-lookup"><span data-stu-id="1a402-183">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="1a402-184">Die Seite "Lehrkräfte" zeigt Daten aus drei verschiedenen Tabellen.</span><span class="sxs-lookup"><span data-stu-id="1a402-184">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="1a402-185">Aus diesem Grund erstellen Sie ein Ansichtsmodell, das drei Eigenschaften enthält jede mit den Daten für eine der Tabellen.</span><span class="sxs-lookup"><span data-stu-id="1a402-185">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="1a402-186">In der *SchoolViewModels* Ordner erstellen *InstructorIndexData.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="1a402-186">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="1a402-187">Erstellen Sie die Instructor-Controller und Ansichten</span><span class="sxs-lookup"><span data-stu-id="1a402-187">Create the Instructor controller and views</span></span>

<span data-ttu-id="1a402-188">Erstellen Sie einen Kursleiter-Controller mit EF Lese-/schreibaktionen wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="1a402-188">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Hinzufügen eines Controllers Dozenten](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="1a402-190">Open *InstructorsController.cs* und fügen Sie eine using-Anweisung für den Namespace ViewModels:</span><span class="sxs-lookup"><span data-stu-id="1a402-190">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="1a402-191">Ersetzen Sie die Index-Methode, durch den folgenden Code zum unverzüglichem Laden von verknüpften Daten und fügen Sie ihn in das Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-191">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="1a402-192">Die Methode akzeptiert optionale Routendaten (`id`) und einen Abfragezeichenfolgenparameter (`courseID`), die die ID-Werte der ausgewählten Kursleiter und ausgewählten Kurs bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1a402-192">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="1a402-193">Die Parameter bereitgestellt werden, indem die **wählen** links auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="1a402-193">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="1a402-194">Der Code wird zuerst Erstellen einer Instanz des Modells anzeigen und darin die Liste der Lehrkräfte einfügen.</span><span class="sxs-lookup"><span data-stu-id="1a402-194">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="1a402-195">Der Code gibt unverzüglichem Laden für die `Instructor.OfficeAssignment` und `Instructor.CourseAssignments` Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="1a402-195">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="1a402-196">Innerhalb der `CourseAssignments` -Eigenschaft, die `Course` -Eigenschaft ist geladen, und innerhalb der, die `Enrollments` und `Department` Eigenschaften werden geladen, und in jedem `Enrollment` Entität die `Student` Eigenschaft geladen wird.</span><span class="sxs-lookup"><span data-stu-id="1a402-196">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="1a402-197">Da die Sicht immer die OfficeAssignment Entität erfordert, ist es effizienter, die in derselben Abfrage abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-197">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="1a402-198">Kurs Entitäten sind erforderlich, wenn auf der Webseite ein Kursleiter ausgewählt ist, damit eine einzelne Abfrage ist besser als mehrere Abfragen nur, wenn die Seite mit einem Kurs ausgewählt als ohne häufiger angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1a402-198">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="1a402-199">Der Code wird wiederholt `CourseAssignments` und `Course` da Sie zwei Eigenschaften von benötigen `Course`.</span><span class="sxs-lookup"><span data-stu-id="1a402-199">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="1a402-200">Die erste Zeichenfolge von `ThenInclude` ruft ruft `CourseAssignment.Course`, `Course.Enrollments`, und `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="1a402-200">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="1a402-201">An diesem Punkt im Code eine andere `ThenInclude` wäre für Navigationseigenschaften des `Student`, die Sie nicht benötigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-201">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="1a402-202">Aber aufrufenden `Include` beginnt über mit `Instructor` Eigenschaften, daher Sie die Kette erneut, diesen Zeitangaben durchlaufen müssen `Course.Department` anstelle von `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="1a402-202">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="1a402-203">Der folgende Code ausgeführt wird, wenn ein Kursleiter ausgewählt wurde.</span><span class="sxs-lookup"><span data-stu-id="1a402-203">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="1a402-204">Die ausgewählte Kursleiter wird aus der Liste der Kursleiter das Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-204">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="1a402-205">Des Ansichtsmodells `Courses` Eigenschaft wird dann mit den Kurs Entitäten aus dieser Dozenten geladen `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1a402-205">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="1a402-206">Die `Where` -Methode gibt eine Auflistung, aber in diesem Fall die Kriterien, nur einen einzigen Instructor-Entität zurückgegeben wird die Methode zu übergeben.</span><span class="sxs-lookup"><span data-stu-id="1a402-206">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="1a402-207">Die `Single` -Methode konvertiert die Auflistung in eine einzelne Instructor-Entität, die Sie Zugriff auf diese Entität gewährt `CourseAssignments` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1a402-207">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="1a402-208">Die `CourseAssignments` Eigenschaft enthält `CourseAssignment` , nur die verknüpften sollen Entitäten `Course` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="1a402-208">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="1a402-209">Verwenden Sie die `Single` Methode auf eine Auflistung, wenn Sie wissen, dass die Auflistung wird nur ein Element verfügen.</span><span class="sxs-lookup"><span data-stu-id="1a402-209">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="1a402-210">Die einzige Methode löst eine Ausnahme aus, wenn die übergebene Auflistung leer ist oder wenn mehr als ein Element vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-210">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="1a402-211">Ist eine Alternative `SingleOrDefault`, womit einen Default-Wert (in diesem Fall null), wenn die Auflistung leer ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-211">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="1a402-212">Jedoch in diesem Fall, da immer noch ansonsten eine Ausnahme (aus beim Suchen nach einem `Courses` Eigenschaft auf einen null-Verweis), und die Ausnahmemeldung würde weniger deutlich die Ursache des Problems angeben.</span><span class="sxs-lookup"><span data-stu-id="1a402-212">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="1a402-213">Beim Aufrufen der `Single` -Methode, Sie können auch übergeben in der Where Bedingung statt der `Where` Methode getrennt:</span><span class="sxs-lookup"><span data-stu-id="1a402-213">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="1a402-214">anstelle von:</span><span class="sxs-lookup"><span data-stu-id="1a402-214">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="1a402-215">Als Nächstes wird ein Kurs ausgewählt wurde, der ausgewählte Kurs aus der Liste der Kurse in das Ansichtsmodell abgerufen.</span><span class="sxs-lookup"><span data-stu-id="1a402-215">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="1a402-216">Klicken Sie dann die des Ansichtsmodells `Enrollments` Eigenschaft wird geladen, mit der Registrierung Entitäten aus dieser Kurs `Enrollments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1a402-216">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="1a402-217">Ändern Sie die Sicht Instructor-Index</span><span class="sxs-lookup"><span data-stu-id="1a402-217">Modify the Instructor Index view</span></span>

<span data-ttu-id="1a402-218">In *Views/Instructors/Index.cshtml*, den Code durch den folgenden Code ersetzen.</span><span class="sxs-lookup"><span data-stu-id="1a402-218">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="1a402-219">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="1a402-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="1a402-220">Sie haben die folgenden Änderungen an den vorhandenen Code vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="1a402-220">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="1a402-221">Die Modellklasse, um geändert `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="1a402-221">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="1a402-222">Der Seitenname von geändert **Index** auf **Lehrkräfte**.</span><span class="sxs-lookup"><span data-stu-id="1a402-222">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="1a402-223">Hinzugefügt ein **Office** Spalte `item.OfficeAssignment.Location` nur, wenn `item.OfficeAssignment` ist ungleich null.</span><span class="sxs-lookup"><span data-stu-id="1a402-223">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="1a402-224">(Da dies eine 1: 0 (null)-oder-1-Beziehung ist, gibt es möglicherweise nicht verknüpfte Entität OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="1a402-224">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="1a402-225">Hinzugefügt eine **Kurse** Spalte Kurse vermittelten jedes Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="1a402-225">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="1a402-226">Finden Sie unter [explizite Zeile Übergang mit `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) Weitere Informationen über diese Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="1a402-226">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="1a402-227">Code hinzugefügt, der dynamisch hinzugefügt `class="success"` auf die `tr` Element des ausgewählten Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="1a402-227">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="1a402-228">Hiermit wird eine Hintergrundfarbe für die ausgewählte Zeile mit einem Bootstrap-Klasse.</span><span class="sxs-lookup"><span data-stu-id="1a402-228">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="1a402-229">Einen neuen Link mit der Bezeichnung hinzugefügt **wählen** unmittelbar vor der Links von anderen in jeder Zeile, die der ausgewählten Instructor-ID zu sendende verursacht die `Index` Methode.</span><span class="sxs-lookup"><span data-stu-id="1a402-229">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="1a402-230">Führen Sie die app, und wählen Sie die **Lehrkräfte** Registerkarte. Die Seite zeigt die Location-Eigenschaft der verknüpften OfficeAssignment Entitäten und eine leere Zelle, wenn keine verknüpften OfficeAssignment Entität vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-230">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Lehrkräfte Indexseite, der keine Auswahl](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="1a402-232">In der *Views/Instructors/Index.cshtml* Datei, nach die schließenden Tabelle Element (am Ende der Datei), fügen Sie folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="1a402-232">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="1a402-233">Dieser Code zeigt eine Liste der Kurse, die im Zusammenhang mit einen Kursleiter beim ein Kursleiter ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-233">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="1a402-234">Dieser Code liest die `Courses` Eigenschaft des Modells anzeigen, das eine Liste der Kurse anzeigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-234">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="1a402-235">Sie bietet außerdem eine **wählen** Link, der die ID der ausgewählten Kurs, sendet der `Index` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="1a402-235">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="1a402-236">Aktualisieren Sie die Seite, und wählen Sie einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="1a402-236">Refresh the page and select an instructor.</span></span> <span data-ttu-id="1a402-237">Jetzt sehen Sie ein Raster mit den für den ausgewählten Kursleiter zugewiesene Kurse zeigt an, und jeder Kurs Sie finden Sie unter den Namen der zugewiesenen Abteilung.</span><span class="sxs-lookup"><span data-stu-id="1a402-237">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Lehrkräfte Index Seite Dozenten ausgewählt](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="1a402-239">Fügen Sie nachdem der Codeblock, den Sie gerade hinzugefügt haben den folgenden Code ein.</span><span class="sxs-lookup"><span data-stu-id="1a402-239">After the code block you just added, add the following code.</span></span> <span data-ttu-id="1a402-240">Dadurch wird eine Liste der Studenten, die registriert werden in einen Kurs, wenn dieser Kurs ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="1a402-240">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="1a402-241">Dieser Code liest die Registrierung-Eigenschaft des Modells anzeigen, um eine Liste der Schüler in diesem Kurs registriert anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="1a402-241">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="1a402-242">Aktualisieren Sie die Seite erneut, und wählen Sie einen Kursleiter.</span><span class="sxs-lookup"><span data-stu-id="1a402-242">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="1a402-243">Wählen Sie dann einen Kurs, finden in der Liste der registrierten Studenten und deren Qualitäten.</span><span class="sxs-lookup"><span data-stu-id="1a402-243">Then select a course to see the list of enrolled students and their grades.</span></span>

![Lehrkräfte Index Seite Kursleiter und Kurs ausgewählt](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="1a402-245">Explizites Laden</span><span class="sxs-lookup"><span data-stu-id="1a402-245">Explicit loading</span></span>

<span data-ttu-id="1a402-246">Wenn Sie die Liste der Kursleiter abgerufen *InstructorsController.cs*, unverzüglichem Laden für die Angabe der `CourseAssignments` Navigationseigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1a402-246">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="1a402-247">Angenommen Sie, Sie erwartet, dass Benutzer nur selten Bereitstellungen in einem ausgewählten Kursleiter und Kurs angezeigt werden soll.</span><span class="sxs-lookup"><span data-stu-id="1a402-247">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="1a402-248">In diesem Fall möchten Sie die Registrierung Daten zu laden, nur, wenn diese angefordert wird.</span><span class="sxs-lookup"><span data-stu-id="1a402-248">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="1a402-249">Um ein Beispiel dazu explizites Laden anzuzeigen, ersetzen die `Index` -Methode durch folgenden Code wird die unverzüglichem Laden für Bereitstellungen entfernt und lädt diese Eigenschaft explizit.</span><span class="sxs-lookup"><span data-stu-id="1a402-249">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="1a402-250">Die codeänderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="1a402-250">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="1a402-251">Der neue Code löscht die *ThenInclude* Methodenaufrufe für Enrollment Daten aus dem Code, die Instructor-Entitäten abruft.</span><span class="sxs-lookup"><span data-stu-id="1a402-251">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="1a402-252">Wenn eine Lehrkraft- und Kurs ausgewählt sind, ruft der hervorgehobene Code Registrierung Entitäten für den ausgewählten Kurs und Student-Entitäten für jede Anmeldung ab.</span><span class="sxs-lookup"><span data-stu-id="1a402-252">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="1a402-253">Ausführen die app, wechseln Sie zu den Dozenten Indexseite jetzt und Sie keinen Unterschied in der Anzeige auf der Seite angezeigt werden, obwohl Sie geändert haben, wie die Daten abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="1a402-253">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="1a402-254">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="1a402-254">Summary</span></span>

<span data-ttu-id="1a402-255">Sie haben jetzt unverzüglichem Laden mit einer Abfrage und mehrere Abfragen verwendet, um verwandte Daten in den Navigationseigenschaften gelesen.</span><span class="sxs-lookup"><span data-stu-id="1a402-255">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="1a402-256">In den nächsten Lernprogrammen erfahren Sie, wie verknüpfte Daten aktualisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1a402-256">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="1a402-257">[Zurück](complex-data-model.md)
>[Weiter](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="1a402-257">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  