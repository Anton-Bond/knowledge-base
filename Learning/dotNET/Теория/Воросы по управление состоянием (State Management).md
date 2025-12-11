#### **1. Что такое управление состоянием (State Management) в .NET?**

**Ответ:**  
Управление состоянием — это процесс сохранения и восстановления данных о состоянии приложения между запросами, особенно в веб-приложениях ASP.NET Core, где HTTP является статическим (stateless) протоколом. Это помогает отслеживать информацию о пользователе, сессиях и кэшировании данных.

---

#### **2. Какие бывают типы управления состоянием в .NET?**

**Ответ:**  
Есть **два основных типа** управления состоянием:

1. **Client-Side (на стороне клиента)**:
    
    - Cookies
    - Local Storage / Session Storage
    - Query Strings
    - Hidden Fields
    - Web Storage (например, IndexedDB)
2. **Server-Side (на стороне сервера)**:
    
    - Session State
    - Application State
    - Cache (MemoryCache, Distributed Cache)
    - Database Storage
    - TempData (в ASP.NET Core MVC)

---

#### **3. Чем отличается Session State от Application State?**

**Ответ:**

- **Session State** хранит данные для конкретного пользователя в течение одного сеанса (session), и данные доступны только для данного пользователя.
- **Application State** глобален для всего приложения, данные хранятся в памяти сервера и доступны для всех пользователей.

---

#### **4. Как включить и использовать Session в ASP.NET Core?**

**Ответ:**  
В **`Program.cs`** добавить middleware для сессий:

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSession();
var app = builder.Build();
app.UseSession();
```

В контроллере можно использовать:

```csharp
HttpContext.Session.SetString("User", "JohnDoe");
string user = HttpContext.Session.GetString("User");
```

---

#### **5. Какие проблемы могут возникнуть при использовании Session?**

**Ответ:**

1. **Скалируемость** – в случае хранения данных в памяти сервера при большом количестве пользователей нагрузка возрастает.
2. **Сетевая архитектура** – если приложение развернуто в нескольких экземплярах, сессия должна быть общей (используется Distributed Cache).
3. **Безопасность** – данные сессии могут быть уязвимы, если их не шифровать.

---

#### **6. Какие есть способы кэширования в .NET?**

**Ответ:**

4. **MemoryCache** (локальное кэширование в памяти процесса):
    
    ```csharp
    var cache = new MemoryCache(new MemoryCacheOptions());
    cache.Set("key", "value", TimeSpan.FromMinutes(10));
    ```
    
5. **Distributed Cache** (например, Redis, SQL Server):
    
    ```csharp
    services.AddStackExchangeRedisCache(options =>
    {
        options.Configuration = "localhost";
        options.InstanceName = "SampleApp";
    });
    ```
    
6. **Output Caching** (в ASP.NET Core 7+):
    
    ```csharp
    app.UseOutputCache();
    ```
    

---

#### **7. В чем разница между In-Memory Cache и Distributed Cache?**

**Ответ:**

- **In-Memory Cache** (MemoryCache) – кэш хранится в памяти сервера, быстро работает, но теряется при перезапуске приложения.
- **Distributed Cache** (Redis, SQL Server) – кэш хранится в отдельном хранилище, доступен для нескольких серверов, устойчив к сбоям.

---

#### **8. Что такое TempData и как оно работает в ASP.NET Core?**

**Ответ:**  
**TempData** используется для временного хранения данных между запросами, обычно между редиректами.

Пример использования:

```csharp
public IActionResult SetTempData()
{
    TempData["Message"] = "Hello, TempData!";
    return RedirectToAction("GetTempData");
}

public IActionResult GetTempData()
{
    var message = TempData["Message"]; // Получим и удалим значение
    return Content(message?.ToString() ?? "No data");
}
```

**Важно:** **TempData** использует `Session` или `Cookies` для хранения данных.

---

#### **9. Как обеспечить безопасность данных при управлении состоянием?**

**Ответ:**

7. **Шифрование и подпись cookies** – использовать `CookieOptions` с Secure и HttpOnly.
8. **Защита от XSS и CSRF** – включить `AntiForgeryToken` в MVC.
9. **Ограничение объема хранимых данных** – не хранить большие объемы в Session и Cache.
10. **Использование HTTPS** – для безопасной передачи данных.

---

#### **10. Когда стоит использовать кэширование, а когда – Session?**

**Ответ:**

- **Кэширование** (MemoryCache, Redis) – для хранения общих данных, которые часто используются и редко изменяются (например, настройки, справочники).
- **Session** – когда нужно сохранить данные **только для конкретного пользователя** (например, корзина покупок, аутентификация).

---

#### **11. Как правильно настроить Redis Cache в .NET?**

**Ответ:**

11. Установить пакет:
    
    ```
    dotnet add package Microsoft.Extensions.Caching.StackExchangeRedis
    ```
    
12. Настроить сервис в `Program.cs`:
    
    ```csharp
    builder.Services.AddStackExchangeRedisCache(options =>
    {
        options.Configuration = "localhost:6379";
        options.InstanceName = "MyApp_";
    });
    ```
    
13. Использовать в коде:
    
    ```csharp
    var cache = serviceProvider.GetRequiredService<IDistributedCache>();
    await cache.SetStringAsync("key", "value");
    string value = await cache.GetStringAsync("key");
    ```
    

---

#### **12. Как избежать проблем с памятью при использовании кэша?**

**Ответ:**

- Устанавливать **время жизни (Expiration Time)** кэшируемых данных.
- Использовать **Sliding Expiration** для продления жизни данных, если они запрашиваются.
- Ограничивать размер хранилища (`MemoryCacheOptions.Limit`).
- Использовать **Distributed Cache** при необходимости.

---
