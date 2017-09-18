## <a name="register-the-database-context"></a><span data-ttu-id="06cdd-101">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="06cdd-101">Register the database context</span></span>

<span data-ttu-id="06cdd-102">Per inserire il contesto del database nel controller, è necessario registrarlo nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="06cdd-102">In order to inject the database context into the controller, we need to register it with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="06cdd-103">Registrare il contesto del database nel contenitore dei servizi usando il supporto incorporato per [inserimento dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="06cdd-103">Register the database context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="06cdd-104">Sostituire il contenuto del file *Startup.cs* con il codice riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="06cdd-104">Replace the contents of the *Startup.cs* file with the following:</span></span>

<span data-ttu-id="06cdd-105">[!code-csharp[Principale](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span><span class="sxs-lookup"><span data-stu-id="06cdd-105">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]</span></span>

<span data-ttu-id="06cdd-106">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="06cdd-106">The preceding code:</span></span>

* <span data-ttu-id="06cdd-107">Rimuove il codice non in uso.</span><span class="sxs-lookup"><span data-stu-id="06cdd-107">Removes the code we're not using.</span></span>
* <span data-ttu-id="06cdd-108">Specifica che un database in memoria viene inserito nel contenitore dei servizi.</span><span class="sxs-lookup"><span data-stu-id="06cdd-108">Specifies an in-memory database is injected into the service container.</span></span>