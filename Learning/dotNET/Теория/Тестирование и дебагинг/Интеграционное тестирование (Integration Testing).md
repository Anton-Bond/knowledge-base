### 📌 **Интеграционное тестирование в .NET**

Интеграционное тестирование проверяет, как различные части приложения взаимодействуют между собой, например:

- Контроллеры и сервисы
- Сервисы и базы данных
- Взаимодействие с внешними API
- Middleware и ASP.NET Core pipeline

---

## **1️⃣ Подходы к интеграционному тестированию**

🔹 **Тестирование реального взаимодействия** – запускаем приложение и тестируем его работу с реальными зависимостями.  
🔹 **Тестирование с подмененными зависимостями** – вместо реальных сервисов и баз данных используем моки (Mock, Fake, In-Memory).

---

## **2️⃣ Инструменты и библиотеки**

|Инструмент|Описание|
|---|---|
|**TestServer**|Встроенный сервер для тестирования ASP.NET Core API.|
|**WebApplicationFactory**|Позволяет запускать приложение в тестовом окружении.|
|**Moq / FakeItEasy**|Имитация зависимостей (баз данных, сервисов).|
|**Respawn**|Очистка базы перед каждым тестом.|
|**SQLite In-Memory**|Используется вместо реальной БД для тестов.|

---

## **3️⃣ Пример тестирования API с WebApplicationFactory**

Допустим, у нас есть контроллер:

```csharp
[ApiController]
[Route("api/products")]
public class ProductsController : ControllerBase
{
    private readonly IProductService _productService;

    public ProductsController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet("{id}")]
    public async Task<IActionResult> GetProduct(int id)
    {
        var product = await _productService.GetProductByIdAsync(id);
        if (product == null) return NotFound();
        return Ok(product);
    }
}
```

Теперь напишем интеграционный тест с **WebApplicationFactory**:

```csharp
public class ProductsControllerTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly HttpClient _client;

    public ProductsControllerTests(WebApplicationFactory<Program> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetProduct_ReturnsProduct_WhenExists()
    {
        // Act
        var response = await _client.GetAsync("/api/products/1");

        // Assert
        response.EnsureSuccessStatusCode();
        var product = await response.Content.ReadAsStringAsync();
        Assert.NotNull(product);
    }
}
```

💡 **Что здесь происходит?**  
✅ `WebApplicationFactory<Program>` запускает тестовый сервер без реального запуска приложения.  
✅ `CreateClient()` создаёт HTTP-клиент для отправки запросов к API.  
✅ `GetAsync("/api/products/1")` отправляет HTTP-запрос.  
✅ `EnsureSuccessStatusCode()` проверяет, что сервер вернул код `200 OK`.  
✅ `Assert.NotNull(product)` убеждается, что ответ содержит данные.

---

## **4️⃣ Тестирование с TestServer**

`TestServer` позволяет тестировать API без развертывания приложения.

### **Пример**:

```csharp
public class ProductsApiTests
{
    private readonly TestServer _server;
    private readonly HttpClient _client;

    public ProductsApiTests()
    {
        _server = new TestServer(new WebHostBuilder()
            .UseStartup<Startup>());
        _client = _server.CreateClient();
    }

    [Fact]
    public async Task GetProduct_ReturnsNotFound_WhenProductDoesNotExist()
    {
        var response = await _client.GetAsync("/api/products/999");
        Assert.Equal(HttpStatusCode.NotFound, response.StatusCode);
    }
}
```

🔹 **Разница между TestServer и WebApplicationFactory**:

- `WebApplicationFactory` удобнее, если тестируем `ASP.NET Core API`.
- `TestServer` более универсальный, но требует явного запуска `WebHostBuilder`.

---

## **5️⃣ Тестирование БД в интеграционных тестах**

**Подходы:**  
1️⃣ **Использовать SQLite In-Memory** – легковесная БД для тестов.  
2️⃣ **Создавать тестовую БД перед каждым тестом** – например, с помощью `Respawn`.  
3️⃣ **Очищать и заполнять базу перед тестами** – `EnsureDeleted().EnsureCreated()`.

### **Пример использования In-Memory SQLite**:

```csharp
public class TestDbContextFactory
{
    public static ApplicationDbContext Create()
    {
        var options = new DbContextOptionsBuilder<ApplicationDbContext>()
            .UseInMemoryDatabase("TestDb")
            .Options;

        return new ApplicationDbContext(options);
    }
}
```

---

## **6️⃣ Очистка данных между тестами (Respawn)**

`Respawn` помогает сбрасывать состояние БД.

### **Установка**:

```shell
dotnet add package Respawn
```

### **Применение**:

```csharp
private readonly Respawner _respawner;

public async Task InitializeAsync()
{
    _respawner = await Respawner.CreateAsync(connectionString, new RespawnerOptions
    {
        TablesToIgnore = new[] { "Migrations" }
    });
}

public async Task ResetDatabase()
{
    await _respawner.ResetAsync(connectionString);
}
```

🔹 **Преимущество**: позволяет быстро очищать БД без пересоздания.

---

## **7️⃣ Заключение**

✅ Интеграционные тесты **важны**, так как проверяют взаимодействие компонентов.  
✅ **WebApplicationFactory** и **TestServer** – мощные инструменты для тестирования API.  
✅ **In-Memory SQLite** и **Respawn** – полезны для тестирования базы данных.  
✅ Использование **Mock** сервисов снижает зависимость от реальных данных.

🔗 **Полезные ресурсы:**

- [Документация по WebApplicationFactory](https://learn.microsoft.com/en-us/aspnet/core/test/integration-tests)
- [GitHub Respawn](https://github.com/jbogard/Respawn)
