### ASP.NET Core – Основные аспекты

#### 1. **Что такое ASP.NET Core?**

ASP.NET Core — это кроссплатформенный, высокопроизводительный фреймворк для создания современных веб-приложений.  
Основные особенности:

- Поддержка Windows, macOS и Linux.
- Поддержка облачных приложений, REST API, MVC, Razor Pages и Blazor.
- Открытый исходный код (Open Source).

#### 2. **Чем отличается от ASP.NET?**

- **Кроссплатформенность**: ASP.NET Core работает на разных операционных системах, тогда как ASP.NET работал только на Windows.
- **Высокая производительность**: Использует Kestrel в качестве веб-сервера.
- **Модульность**: Подключение только нужных компонентов.
- **Единый фреймворк**: Включает все современные технологии для веб-разработки.

#### 3. **Основные преимущества ASP.NET Core**

- **Высокая производительность**: Поддерживает миллионы запросов в секунду.
- **Легковесная архитектура**: Меньший объем памяти и быстрая загрузка.
- **Интеграция с современными инструментами**: Контейнеризация (Docker), облачные сервисы, CI/CD.
- **Поддержка DI (Dependency Injection)**: Встроенная поддержка внедрения зависимостей.

#### 4. **Структура проекта ASP.NET Core**

Основные файлы и папки:

- **Program.cs**: Точка входа в приложение.
- **appsettings.json**: Настройки конфигурации приложения.
- **Startup.cs** (в ASP.NET Core до версии 6.0): Определяет middleware и сервисы.
- **wwwroot**: Статические файлы (CSS, JS, изображения).
- **Controllers**: Контроллеры MVC.
- **Views**: Представления для MVC.

#### 5. **Web Host и Generic Host**

- **Web Host**: Используется в ASP.NET Core для настройки веб-приложений.
- **Generic Host**: Более универсальный подход, применимый для любых типов приложений, включая фоновые службы.

#### Пример кода (Program.cs в ASP.NET Core 6.0):

```csharp
var builder = WebApplication.CreateBuilder(args);

// Добавление сервисов
builder.Services.AddControllersWithViews();

var app = builder.Build();

// Конфигурация middleware
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}

app.UseHttpsRedirection();
app.UseStaticFiles();

app.UseRouting();
app.UseAuthorization();

app.MapControllerRoute(
    name: "default",
    pattern: "{controller=Home}/{action=Index}/{id?}");

app.Run();
```

#### 6. **Типы приложений на ASP.NET Core**

- **MVC (Model-View-Controller)**: Для разделения логики, представлений и данных.
- **Razor Pages**: Упрощенный подход для страниц, похожий на Web Forms.
- **Blazor**: Интерактивные приложения на C# с использованием WebAssembly.
- **Web API**: Для создания RESTful сервисов.
