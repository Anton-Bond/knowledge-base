#### 1. **Что такое SignalR?**

**Ответ:**  
SignalR — это библиотека от Microsoft, предназначенная для **реализации двусторонней (real-time) связи** между клиентом и сервером в .NET. Она упрощает работу с WebSockets и автоматически использует другие транспортные механизмы (Server-Sent Events, Long Polling), если WebSockets недоступен.

---

#### 2. **Как работает SignalR?**

**Ответ:**  
SignalR устанавливает **постоянное соединение** между клиентом и сервером. Когда клиент отправляет данные, сервер передает их **всем подключенным клиентам** (или определённым группам/пользователям). SignalR автоматически выбирает **наилучший транспортный механизм** (WebSockets, SSE, Long Polling) в зависимости от возможностей сервера и клиента.

---

#### 3. **Какие транспортные протоколы использует SignalR?**

**Ответ:**  
SignalR поддерживает 3 транспортных механизма:

1. **WebSockets** – предпочтительный вариант, если поддерживается клиентом и сервером.
2. **Server-Sent Events (SSE)** – однонаправленный канал от сервера к клиенту (поддерживается только в браузерах).
3. **Long Polling** – резервный механизм, имитирующий постоянное соединение путем периодических HTTP-запросов.

SignalR **автоматически выбирает** лучший транспорт в зависимости от окружения.

---

#### 4. **Как создать серверный хаб (Hub) в SignalR?**

**Ответ:**  
В ASP.NET Core SignalR создается класс **Hub**, унаследованный от `Hub`:

```csharp
public class ChatHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

Этот хаб отправляет сообщение всем клиентам, вызывая `Clients.All.SendAsync`.

---

#### 5. **Как клиент подключается к SignalR-хабу?**

**Ответ:**  
В браузере можно использовать JavaScript-клиент:

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub") // URL вашего хаба
    .build();

connection.on("ReceiveMessage", (user, message) => {
    console.log(`${user}: ${message}`);
});

connection.start().catch(err => console.error(err));
```

После вызова `connection.start()` клиент подключается к серверу.

---

#### 6. **Как отправить сообщение от клиента на сервер?**

**Ответ:**  
В JavaScript-клиенте:

```javascript
connection.invoke("SendMessage", "User1", "Hello, world!")
    .catch(err => console.error(err));
```

Метод `SendMessage` вызывается на сервере.

---

#### 7. **Как отправить сообщение определённому клиенту?**

**Ответ:**  
Можно использовать `Clients.Client(connectionId).SendAsync()`:

```csharp
public async Task SendToClient(string connectionId, string message)
{
    await Clients.Client(connectionId).SendAsync("ReceiveMessage", message);
}
```

Клиентский **connectionId** можно получить через `Context.ConnectionId`.

---

#### 8. **Как отправить сообщение группе клиентов?**

**Ответ:**  
Добавляем клиента в группу:

```csharp
public async Task AddToGroup(string groupName)
{
    await Groups.AddToGroupAsync(Context.ConnectionId, groupName);
}
```

Отправляем сообщение группе:

```csharp
public async Task SendMessageToGroup(string groupName, string message)
{
    await Clients.Group(groupName).SendAsync("ReceiveMessage", message);
}
```

---

#### 9. **Как управлять пользователями в SignalR?**

**Ответ:**  
SignalR позволяет отправлять сообщения конкретному пользователю:

```csharp
await Clients.User(userId).SendAsync("ReceiveMessage", "Hello!");
```

SignalR использует **пользовательские идентификаторы**, полученные через `IUserIdProvider`.

---

#### 10. **Какие способы масштабирования SignalR существуют?**

**Ответ:**  
SignalR поддерживает **масштабирование** при помощи **бекплана (Backplane)**. Возможные варианты:

- **Redis Backplane** (для масштабирования на несколько серверов)
- **Azure SignalR Service** (облачное решение)
- **SQL Server Backplane** (используется редко)

Пример конфигурации Redis Backplane:

```csharp
services.AddSignalR().AddStackExchangeRedis("localhost:6379");
```

---

#### 11. **Что такое SignalR Streaming?**

**Ответ:**  
SignalR поддерживает **потоковую передачу данных**. Например, сервер может передавать клиенту поток данных в реальном времени:

```csharp
public async IAsyncEnumerable<int> StreamingData(CancellationToken cancellationToken)
{
    for (int i = 0; i < 10; i++)
    {
        yield return i;
        await Task.Delay(1000, cancellationToken);
    }
}
```

Клиент принимает поток данных:

```javascript
const stream = connection.stream("StreamingData");
stream.subscribe({
    next: (item) => console.log(item),
    complete: () => console.log("Done"),
    error: (err) => console.error(err)
});
```

---

#### 12. **Какие отличия между SignalR и WebSockets?**

|Характеристика|SignalR|WebSockets|
|---|---|---|
|Протокол|HTTP + WebSockets (или др.)|Только WebSockets|
|Выбор транспорта|Автоматический|Только WebSockets|
|Масштабируемость|Поддержка Redis, Azure|Нужно реализовывать вручную|
|Простой API|Да|Нет, требует настройки|

SignalR **абстрагирует WebSockets**, выбирая лучший доступный транспорт.

---

#### 13. **Как защитить SignalR?**

**Ответ:**  
SignalR поддерживает **JWT-аутентификацию**:

```csharp
services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options =>
    {
        options.Events = new JwtBearerEvents
        {
            OnMessageReceived = context =>
            {
                var accessToken = context.Request.Query["access_token"];
                var path = context.HttpContext.Request.Path;
                if (!string.IsNullOrEmpty(accessToken) && path.StartsWithSegments("/chathub"))
                {
                    context.Token = accessToken;
                }
                return Task.CompletedTask;
            }
        };
    });
```

Клиент должен передавать `access_token` в URL:

```javascript
const connection = new signalR.HubConnectionBuilder()
    .withUrl("/chathub?access_token=your_jwt_token")
    .build();
```

Это позволяет использовать **авторизацию и аутентификацию** в SignalR.

---
