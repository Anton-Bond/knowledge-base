Razor Pages — это способ организации кода в ASP.NET Core, который облегчает создание веб-страниц с использованием шаблонов Razor. Razor Pages подходят для приложений, в которых основной задачей является обработка страниц, а не API.

---

#### 1. **Что такое Razor Pages?**

Razor Pages — это парадигма, основанная на страницах, где каждая страница является обработчиком запросов. Каждая Razor Page состоит из двух частей:

- **.cshtml**: Представление (шаблон Razor).
- **.cshtml.cs**: Страница модели (код C# для обработки логики).

**Пример Razor Page:**

**Index.cshtml** (представление):

```html
@page
@model IndexModel

<h1>@Model.Message</h1>
```

**Index.cshtml.cs** (модель страницы):

```csharp
using Microsoft.AspNetCore.Mvc.RazorPages;

public class IndexModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = "Hello, Razor Pages!";
    }
}
```

---

#### 2. **Как работает Razor Pages?**

Razor Pages обрабатывает запросы на основе страницы, а не контроллера. Каждая Razor Page имеет свой собственный обработчик для различных HTTP-методов.

1. **Методы обработки**:  
    Razor Pages использует методы с префиксом **On** для обработки HTTP-запросов:
    
    - `OnGet()`: Обрабатывает GET-запросы.
    - `OnPost()`: Обрабатывает POST-запросы.
    - `OnPut()`, `OnDelete()`, и другие методы поддерживаются для соответствующих HTTP-методов.
2. **Шаблонный файл**:  
    Каждая Razor Page имеет свой файл **.cshtml**, который использует синтаксис Razor для встраивания C# кода в HTML.
    
3. **Страница модели**:  
    Модель страницы (файл **.cshtml.cs**) содержит обработчики и логику для страницы.
    

---

#### 3. **Создание и настройка Razor Pages**

Для использования Razor Pages необходимо включить их в конфигурации приложения. В файле **Program.cs** или **Startup.cs** добавляем вызов `AddRazorPages`:

```csharp
builder.Services.AddRazorPages();  // Добавляем поддержку Razor Pages
```

Затем необходимо настроить маршруты в конвейере обработки запросов:

```csharp
app.MapRazorPages();  // Регистрируем маршруты Razor Pages
```

---

#### 4. **Основные особенности Razor Pages**

1. **Простота**:  
    Каждая страница представляет собой файл **.cshtml** и модель **.cshtml.cs**, что облегчает организацию и поддержку кода.
    
2. **Изоляция логики и представления**:  
    Логика и представление находятся рядом друг с другом, что упрощает работу с простыми страницами и улучшает читаемость кода.
    
3. **Поддержка моделей**:  
    Каждая Razor Page может содержать собственную модель данных, которая инкапсулирует бизнес-логику и взаимодействие с данными.
    
4. **Интеграция с контроллерами**:  
    Razor Pages могут быть использованы вместе с контроллерами в одном проекте. Они позволяют легко переключаться между MVC и страницами, когда это необходимо.
    

---

#### 5. **Пример использования Razor Pages**

**Контроллер:**

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }
}
```

**Razor Page (Index.cshtml)**:

```html
@page
@model IndexModel

<h1>Welcome to Razor Pages!</h1>
```

**Страница модели (Index.cshtml.cs):**

```csharp
public class IndexModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = "Welcome to the page!";
    }
}
```

---

#### 6. **Передача данных между страницами**

1. **Использование параметров в URL**:  
    В Razor Pages можно передавать параметры через URL, например:
    
    ```csharp
    @page "{id}"
    public IActionResult OnGet(int id)
    {
        // Логика обработки параметра id
    }
    ```
    
2. **Использование модели данных**:  
    Модель страницы может хранить данные, которые используются в представлении.
    

---

#### 7. **Обработка форм и отправка данных**

Razor Pages идеально подходят для обработки форм. Пример обработки формы:

**Page:**

```html
@page
@model FormModel

<form method="post">
    <input type="text" asp-for="UserName" />
    <button type="submit">Submit</button>
</form>

<p>@Model.Message</p>
```

**Страница модели (FormModel.cs):**

```csharp
public class FormModel : PageModel
{
    [BindProperty]
    public string UserName { get; set; }

    public string Message { get; set; }

    public void OnPost()
    {
        Message = $"Hello, {UserName}!";
    }
}
```
