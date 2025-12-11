#### 1. **Что такое Middleware?**

Middleware — это компонент, обрабатывающий HTTP-запросы и ответы в конвейере ASP.NET Core. Каждый middleware выполняет следующие задачи:

- Может выполнить действия до передачи запроса следующему компоненту в конвейере.
- Может обработать ответ до его отправки клиенту.

#### 2. **Принципы работы Middleware**

- Middleware работают как цепочка: каждый компонент вызывает следующий, или завершает обработку запроса самостоятельно.
- Компоненты middleware подключаются в порядке, указанном в коде.

Пример базового конвейера:

1. Приходит запрос от клиента.
2. Первое middleware проверяет запрос (например, проверка токена).
3. Передает запрос следующему middleware.
4. Контроллер формирует ответ.
5. Middleware обрабатывают ответ в обратном порядке перед возвратом клиенту.

#### 3. **Порядок выполнения Middleware**

- Порядок добавления middleware в `Program.cs` или `Startup.cs` определяет их последовательность выполнения.
- Если middleware не вызывает следующий компонент, запрос останавливается.

Пример:

```csharp
app.Use(async (context, next) =>
{
    Console.WriteLine("Первое middleware: до вызова next()");
    await next.Invoke(); // Передача запроса следующему компоненту
    Console.WriteLine("Первое middleware: после вызова next()");
});

app.Use(async (context, next) =>
{
    Console.WriteLine("Второе middleware: до вызова next()");
    await next.Invoke();
    Console.WriteLine("Второе middleware: после вызова next()");
});
```

#### 4. **Создание собственного Middleware**

Создание пользовательского middleware:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        // Действия до вызова следующего middleware
        Console.WriteLine("Обработка запроса в CustomMiddleware");
        await _next(context); // Передача запроса следующему компоненту
        // Действия после обработки
        Console.WriteLine("Обработка ответа в CustomMiddleware");
    }
}

// Регистрация в Program.cs
app.UseMiddleware<CustomMiddleware>();
```

#### 5. **Встроенные Middleware**

ASP.NET Core предоставляет встроенные middleware для различных задач:

- **UseHttpsRedirection**: Перенаправляет HTTP-запросы на HTTPS.
- **UseStaticFiles**: Обрабатывает запросы к статическим файлам из папки `wwwroot`.
- **UseRouting**: Активирует систему маршрутизации.
- **UseAuthorization**: Проверяет права пользователя на выполнение запроса.
- **UseExceptionHandler**: Обрабатывает ошибки.

#### 6. **Преимущества Middleware**

- Легкость добавления и настройки.
- Возможность модульного и последовательного управления запросами и ответами.
- Возможность повторного использования кода.

---

### Встроенные Middleware в ASP.NET Core

Middleware — это ключевой элемент конвейера обработки запросов. В ASP.NET Core существует множество встроенных middleware, предназначенных для выполнения типичных задач. Рассмотрим наиболее популярные из них.

#### 1. **UseHttpsRedirection**

Перенаправляет запросы HTTP на HTTPS.

- Используется для обеспечения безопасности.
- Обычно применяется вместе с **UseHsts** (HTTP Strict Transport Security).

**Пример:**

```csharp
app.UseHttpsRedirection();
app.UseHsts();
```

Если приложение работает в режиме разработки, **UseHsts** часто отключают, чтобы избежать проблем с сертификатами.

---

#### 2. **UseStaticFiles**

Обрабатывает запросы к статическим файлам (CSS, JS, изображения и т.д.) из папки `wwwroot` по умолчанию.

- Подключается после **UseHttpsRedirection**.

**Пример:**

```csharp
app.UseStaticFiles();
```

Вы можете настроить кастомные пути для файлов:

```csharp
app.UseStaticFiles(new StaticFileOptions
{
    FileProvider = new PhysicalFileProvider(
        Path.Combine(Directory.GetCurrentDirectory(), "CustomStaticFiles")),
    RequestPath = "/CustomFiles"
});
```

---

#### 3. **UseRouting**

Активирует маршрутизацию, которая определяет, какой контроллер или обработчик будет обрабатывать запрос.

- Обычно идет до middleware авторизации.

**Пример:**

```csharp
app.UseRouting();
```

---

#### 4. **UseAuthorization**

Проверяет права доступа пользователя для выполнения определенных действий.

- Работает только после **UseRouting**, так как авторизация зависит от определенного маршрута.

**Пример:**

```csharp
app.UseAuthorization();
```

Для использования требуется настроить службы авторизации:

```csharp
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdminPolicy", policy =>
        policy.RequireRole("Admin"));
});
```

---

#### 5. **UseAuthentication**

Проверяет, является ли пользователь аутентифицированным.

- Обязательно добавляется перед **UseAuthorization**.

**Пример:**

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

Для работы требуется настройка аутентификации, например, через JWT:

```csharp
builder.Services.AddAuthentication("Bearer")
    .AddJwtBearer("Bearer", options =>
    {
        options.Authority = "https://authserver.com";
        options.TokenValidationParameters = new TokenValidationParameters
        {
            ValidateAudience = false
        };
    });
```

---

#### 6. **UseExceptionHandler**

Перехватывает необработанные исключения и перенаправляет на пользовательскую страницу ошибок.

**Пример:**

```csharp
app.UseExceptionHandler("/Home/Error");
```

Настройка обработчика ошибок:

```csharp
app.UseExceptionHandler(errorApp =>
{
    errorApp.Run(async context =>
    {
        context.Response.StatusCode = 500;
        await context.Response.WriteAsync("Произошла ошибка на сервере.");
    });
});
```

---

#### 7. **UseDeveloperExceptionPage**

Отображает подробную информацию об исключении в режиме разработки.

**Пример:**

```csharp
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
```

---

#### 8. **UseCors**

Активирует политику CORS (Cross-Origin Resource Sharing), позволяющую или запрещающую запросы с других доменов.

**Пример:**

```csharp
app.UseCors(builder =>
    builder.WithOrigins("https://example.com")
           .AllowAnyHeader()
           .AllowAnyMethod());
```

---

#### 9. **UseEndpoints**

Регистрирует конечные точки (endpoints) для маршрутов, например контроллеров или Razor Pages.

- Работает после **UseRouting**.

**Пример:**

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```

---

#### 10. **UseWebSockets**

Добавляет поддержку WebSocket-протокола.

**Пример:**

```csharp
app.UseWebSockets();
```

---

### Общий порядок подключения Middleware

1. **UseExceptionHandler** или **UseDeveloperExceptionPage** (в зависимости от среды).
2. **UseHttpsRedirection**.
3. **UseStaticFiles**.
4. **UseRouting**.
5. **UseAuthentication**.
6. **UseAuthorization**.
7. **UseEndpoints**.

---

### 3. **Конвейер обработки запросов (Request Pipeline)**

Конвейер обработки запросов — это последовательность middleware-компонентов, которые обрабатывают HTTP-запросы и формируют HTTP-ответы.

#### 1. **Что представляет собой конвейер обработки запросов?**

Конвейер состоит из нескольких этапов, где каждый middleware может:

- Обработать запрос (например, проверить аутентификацию).
- Передать управление следующему компоненту в цепочке.
- Вернуть результат (HTTP-ответ) обратно.

**Поток запроса и ответа**:

1. Клиент отправляет HTTP-запрос.
2. Запрос проходит через все middleware-компоненты по порядку.
3. Контроллер или обработчик обрабатывает запрос и возвращает ответ.
4. Ответ проходит через middleware-компоненты в обратном порядке.
5. Клиент получает HTTP-ответ.

---

#### 2. **Пример работы конвейера**

Пример `Program.cs`:

```csharp
var builder = WebApplication.CreateBuilder(args);

var app = builder.Build();

// Первое middleware
app.Use(async (context, next) =>
{
    Console.WriteLine("До выполнения первого middleware");
    await next(); // Передача управления следующему компоненту
    Console.WriteLine("После выполнения первого middleware");
});

// Второе middleware
app.Use(async (context, next) =>
{
    Console.WriteLine("До выполнения второго middleware");
    await next(); 
    Console.WriteLine("После выполнения второго middleware");
});

// Конечный обработчик запроса
app.Run(async context =>
{
    Console.WriteLine("Обработка запроса в конечном обработчике");
    await context.Response.WriteAsync("Ответ сформирован!");
});

app.Run();
```

**Вывод в консоль при запросе:**

```
До выполнения первого middleware  
До выполнения второго middleware  
Обработка запроса в конечном обработчике  
После выполнения второго middleware  
После выполнения первого middleware  
```

---

#### 3. **Основные задачи конвейера обработки запросов**

- **Маршрутизация**: Определяет, какой обработчик будет работать с запросом.
- **Аутентификация и авторизация**: Проверяет пользователя.
- **Обработка ошибок**: Предоставляет обработчики исключений.
- **Кэширование**: Сокращает время обработки повторных запросов.
- **Сжатие**: Уменьшает размер ответов для экономии пропускной способности.

---

#### 4. **Порядок выполнения middleware**

- Middleware подключаются в `Program.cs` (или `Startup.cs`) в указанном порядке.
- Неправильный порядок подключения может привести к ошибкам. Например:
    - **UseRouting** должен быть до **UseAuthorization**.
    - **UseStaticFiles** должен быть до маршрутизации.

---

#### 5. **Примеры стандартных middleware в конвейере**

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllersWithViews();

var app = builder.Build();

// Подключение middleware
app.UseExceptionHandler("/Home/Error"); // Обработка ошибок
app.UseHttpsRedirection();             // Перенаправление на HTTPS
app.UseStaticFiles();                  // Статические файлы
app.UseRouting();                      // Маршрутизация
app.UseAuthentication();               // Аутентификация
app.UseAuthorization();                // Авторизация

// Конечные точки
app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

---

#### 6. **Создание пользовательского middleware**

Пример: логирование запросов.

```csharp
public class LoggingMiddleware
{
    private readonly RequestDelegate _next;

    public LoggingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        Console.WriteLine($"Запрос: {context.Request.Method} {context.Request.Path}");
        await _next(context);
        Console.WriteLine($"Ответ: {context.Response.StatusCode}");
    }
}

// Регистрация middleware
app.UseMiddleware<LoggingMiddleware>();
```

---
### 4. **Встроенные Middleware**

#### 1. **Обзор встроенных Middleware**

Middleware в ASP.NET Core выполняют ключевые задачи, такие как:

- Безопасность (HTTPS, авторизация, аутентификация).
- Обработка ошибок и логирование.
- Управление статическими файлами.
- Настройка CORS.
- Обработка маршрутизации.

---

#### 2. **Middleware авторизации и аутентификации**

##### **Аутентификация**

- Отвечает за проверку, кто отправляет запрос (пользователь).
- Использует схемы аутентификации, такие как JWT, куки, OAuth.

**Пример настройки аутентификации:**

```csharp
builder.Services.AddAuthentication("CookieAuth")
    .AddCookie("CookieAuth", options =>
    {
        options.LoginPath = "/Account/Login";
    });

app.UseAuthentication(); // Добавляется перед UseAuthorization
```

##### **Авторизация**

- Проверяет, есть ли у пользователя права на выполнение действия.
- Используются роли и политики.

**Пример настройки авторизации:**

```csharp
builder.Services.AddAuthorization(options =>
{
    options.AddPolicy("AdminOnly", policy => policy.RequireRole("Admin"));
});

app.UseAuthorization(); // После UseRouting
```

---

#### 3. **Middleware для обработки ошибок**

##### **UseExceptionHandler**

- Перехватывает необработанные исключения.
- Позволяет перенаправлять на страницу ошибок.

**Пример:**

```csharp
app.UseExceptionHandler("/Error");
```

##### **UseDeveloperExceptionPage**

- Показывает подробную информацию об ошибках.
- Используется только в режиме разработки.

**Пример:**

```csharp
if (app.Environment.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
}
```

---

#### 4. **Middleware для работы со статическими файлами**

##### **UseStaticFiles**

- Обрабатывает запросы к файлам в папке `wwwroot`.

**Пример:**

```csharp
app.UseStaticFiles();
```

##### Настройка статических файлов:

```csharp
app.UseStaticFiles(new StaticFileOptions
{
    FileProvider = new PhysicalFileProvider(
        Path.Combine(Directory.GetCurrentDirectory(), "CustomFiles")),
    RequestPath = "/Files"
});
```

---

#### 5. **Middleware для маршрутизации**

##### **UseRouting**

- Активирует систему маршрутизации в приложении.
- Обязательно используется перед **UseAuthorization**.

##### **UseEndpoints**

- Настраивает конечные точки маршрутизации (endpoints).

**Пример:**

```csharp
app.UseRouting();

app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
    endpoints.MapRazorPages();
});
```

---

#### 6. **CORS Middleware**

##### **UseCors**

- Управляет доступом к приложению с других доменов.

**Пример настройки:**

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowSpecificOrigin", builder =>
        builder.WithOrigins("https://example.com")
               .AllowAnyMethod()
               .AllowAnyHeader());
});

app.UseCors("AllowSpecificOrigin");
```

---

#### 7. **Middleware для сжатия (Compression)**

##### **UseResponseCompression**

- Уменьшает размер ответа, отправляемого клиенту.

**Пример:**

```csharp
builder.Services.AddResponseCompression();

app.UseResponseCompression();
```

---

#### 8. **WebSocket Middleware**

##### **UseWebSockets**

- Обеспечивает поддержку WebSocket-протокола.

**Пример:**

```csharp
app.UseWebSockets();
app.Use(async (context, next) =>
{
    if (context.WebSockets.IsWebSocketRequest)
    {
        var webSocket = await context.WebSockets.AcceptWebSocketAsync();
        // Обработка WebSocket соединения
    }
    else
    {
        await next();
    }
});
```

