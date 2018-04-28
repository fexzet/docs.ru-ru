### <a name="change-in-behavior-in-data-definition-language-ddl-apis"></a><span data-ttu-id="0d636-101">Изменение поведения API-интерфейсов языка описания данных (DDL)</span><span class="sxs-lookup"><span data-stu-id="0d636-101">Change in behavior in Data Definition Language (DDL) APIs</span></span>

|   |   |
|---|---|
|<span data-ttu-id="0d636-102">Подробные сведения</span><span class="sxs-lookup"><span data-stu-id="0d636-102">Details</span></span>|<span data-ttu-id="0d636-103">Поведение API-интерфейсов языка DDL при заданном значении AttachDBFilename изменилось следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0d636-103">The behavior of DDL APIs when AttachDBFilename is specified has changed as follows:</span></span><ul><li><span data-ttu-id="0d636-104">В строках подключения не требуется указывать значение Initial Catalog.</span><span class="sxs-lookup"><span data-stu-id="0d636-104">Connection strings need not specify an Initial Catalog value.</span></span> <span data-ttu-id="0d636-105">Ранее требовались оба значения — AttachDBFilename и Initial Catalog.</span><span class="sxs-lookup"><span data-stu-id="0d636-105">Previously, both AttachDBFilename and Initial Catalog were required.</span></span></li><li><span data-ttu-id="0d636-106">Если указаны AttachDBFilename и Initial Catalog и данный MDF-файл существует, метод ObjectContext.DatabaseExists возвращает значение true.</span><span class="sxs-lookup"><span data-stu-id="0d636-106">If both AttachDBFilename and Initial Catalog are specified and the given MDF file exists, the ObjectContext.DatabaseExists method returns true.</span></span> <span data-ttu-id="0d636-107">Раньше он возвращал значение false.</span><span class="sxs-lookup"><span data-stu-id="0d636-107">Previously, it returned false.</span></span></li><li><span data-ttu-id="0d636-108">Если указаны AttachDBFilename и Initial Catalog и данный MDF-файл существует, вызов метода ObjectContext.DeleteDatabase приведет к удалению файлов.</span><span class="sxs-lookup"><span data-stu-id="0d636-108">If both AttachDBFilename and Initial Catalog are specified and the given MDF file exists, calling the ObjectContext.DeleteDatabase method deletes the files.</span></span></li><li><span data-ttu-id="0d636-109">Если метод ObjectContext.DeleteDatabase вызывается в случае, когда в строке подключения указывается значение AttachDBFilename с несуществующим MDF-файлом и несуществующим Initial Catalog, метод вызывает исключение InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="0d636-109">If ObjectContext.DeleteDatabase is called when the connection string specifies an AttachDBFilename value with an MDF that doesn't exist and an Initial Catalog that doesn't exist, the method throws an InvalidOperationException exception.</span></span> <span data-ttu-id="0d636-110">Раньше он вызывал исключение SqlException.</span><span class="sxs-lookup"><span data-stu-id="0d636-110">Previously, it threw a SqlException exception.</span></span></li></ul>|
|<span data-ttu-id="0d636-111">Предложение</span><span class="sxs-lookup"><span data-stu-id="0d636-111">Suggestion</span></span>|<span data-ttu-id="0d636-112">Эти изменения облегчают создание средств и приложений, в которых используются API-интерфейсы DDL.</span><span class="sxs-lookup"><span data-stu-id="0d636-112">These changes make it easier to build tools and applications that use the DDL APIs.</span></span> <span data-ttu-id="0d636-113">Эти изменения могут повлиять на совместимость приложений в следующих сценариях:</span><span class="sxs-lookup"><span data-stu-id="0d636-113">These changes can affect application compatibility in the following scenarios:</span></span><ul><li><span data-ttu-id="0d636-114">Пользователь пишет код, который выполняет команду DROP DATABASE напрямую, а не вызывает ObjectContext.DeleteDatabase, если ObjectContext.DatabaseExists возвращает значение true.</span><span class="sxs-lookup"><span data-stu-id="0d636-114">The user writes code that executes a DROP DATABASE command directly instead of calling ObjectContext.DeleteDatabase if ObjectContext.DatabaseExists returns true.</span></span> <span data-ttu-id="0d636-115">Это нарушает логику существующего кода, если база данных не присоединена, но MDF-файл существует.</span><span class="sxs-lookup"><span data-stu-id="0d636-115">This breaks existing code If the database is not attached but the MDF file exists.</span></span></li><li><span data-ttu-id="0d636-116">Пользователь пишет код, который ожидает, что метод ObjectContext.DeleteDatabase вызовет исключение SqlException, а не исключение InvalidOperationException, когда Initial Catalog и MDF-файл не существуют.</span><span class="sxs-lookup"><span data-stu-id="0d636-116">The user writes code that expects the ObjectContext.DeleteDatabase method to throw a SqlException exception rather than an InvalidOperationException exception when the Initial Catalog and MDF file don't exist.</span></span></li></ul>|
|<span data-ttu-id="0d636-117">Область</span><span class="sxs-lookup"><span data-stu-id="0d636-117">Scope</span></span>|<span data-ttu-id="0d636-118">Дополнительный номер</span><span class="sxs-lookup"><span data-stu-id="0d636-118">Minor</span></span>|
|<span data-ttu-id="0d636-119">Версия</span><span class="sxs-lookup"><span data-stu-id="0d636-119">Version</span></span>|<span data-ttu-id="0d636-120">4.5</span><span class="sxs-lookup"><span data-stu-id="0d636-120">4.5</span></span>|
|<span data-ttu-id="0d636-121">Тип</span><span class="sxs-lookup"><span data-stu-id="0d636-121">Type</span></span>|<span data-ttu-id="0d636-122">Среда выполнения</span><span class="sxs-lookup"><span data-stu-id="0d636-122">Runtime</span></span>|
