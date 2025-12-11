### **`INotifyCompletion` в C#**

`INotifyCompletion` — это интерфейс, определяющий механизм уведомления для объектов, которые поддерживают продолжение выполнения после завершения асинхронной операции. Он играет ключевую роль в реализации ключевого слова `await` и асинхронного программирования в C#.

---

## **1. Где используется `INotifyCompletion`?**

`INotifyCompletion` реализуется структурами, которые могут быть использованы с оператором `await`. Это включает:

- **`Task`** и **`Task<T>`** (асинхронные задачи).
- **`ValueTask`**.
- Любые пользовательские типы, которые хотят поддерживать оператор `await`.

---

## **2. Определение интерфейса**

```csharp
public interface INotifyCompletion
{
    void OnCompleted(Action continuation);
}
```

### **Метод `OnCompleted`**

- **`Action continuation`** — делегат, представляющий метод, который должен быть вызван, когда асинхронная операция завершена.
- Этот метод используется для регистрации действия (continuation), которое выполнится после завершения операции.

---

## **3. Связь с асинхронным программированием**

Когда оператор `await` используется с объектом, компилятор:

1. Проверяет, реализует ли объект интерфейс **`INotifyCompletion`**.
2. Если да, то вызывает метод `OnCompleted`, чтобы зарегистрировать продолжение выполнения.

Это позволяет асинхронному коду приостанавливаться и возобновляться без блокировки потоков.

---

## **4. Пример пользовательской реализации**

Рассмотрим пример пользовательского типа, который поддерживает `await` через реализацию `INotifyCompletion`.

### **Пользовательский `Awaiter`**

```csharp
using System;
using System.Runtime.CompilerServices;

public class CustomAwaiter : INotifyCompletion
{
    private readonly int _delay;

    public CustomAwaiter(int delay)
    {
        _delay = delay;
    }

    public bool IsCompleted => false; // Указывает, завершена ли операция

    public void OnCompleted(Action continuation)
    {
        // Регистрация действия продолжения
        Console.WriteLine("Operation completed, resuming...");
        continuation?.Invoke();
    }

    public void GetResult()
    {
        // Метод вызывается после завершения операции
        Console.WriteLine($"Finished waiting for {_delay}ms.");
    }
}

public class CustomAwaitable
{
    private readonly int _delay;

    public CustomAwaitable(int delay)
    {
        _delay = delay;
    }

    public CustomAwaiter GetAwaiter()
    {
        return new CustomAwaiter(_delay);
    }
}

class Program
{
    static async System.Threading.Tasks.Task Main(string[] args)
    {
        var customAwaitable = new CustomAwaitable(1000);
        await customAwaitable;
        Console.WriteLine("Resumed!");
    }
}
```

### **Объяснение кода:**

1. **`CustomAwaitable`** — пользовательский тип, который поддерживает `await` через метод `GetAwaiter`.
2. **`CustomAwaiter`**:
    - Реализует `INotifyCompletion`.
    - Определяет метод `OnCompleted` для регистрации продолжения.
    - Определяет метод `GetResult`, вызываемый после завершения операции.

Когда вызывается `await customAwaitable`, выполняется:

- `CustomAwaitable.GetAwaiter()`, который возвращает объект `CustomAwaiter`.
- `CustomAwaiter.OnCompleted`, чтобы зарегистрировать продолжение.
- После завершения выполняется `CustomAwaiter.GetResult`.

---

## **5. Связь с `TaskAwaiter`**

В стандартных задачах `Task` используется структура **`TaskAwaiter`**, которая реализует `INotifyCompletion`. Именно через `TaskAwaiter` задачи интегрируются с ключевым словом `await`.

```csharp
public struct TaskAwaiter : ICriticalNotifyCompletion
{
    public void OnCompleted(Action continuation) { ... }
    public void UnsafeOnCompleted(Action continuation) { ... }
    public void GetResult() { ... }
}
```

---

## **6. Расширение через `ICriticalNotifyCompletion`**

`ICriticalNotifyCompletion` расширяет `INotifyCompletion` и добавляет метод:

- **`UnsafeOnCompleted(Action continuation)`** — используется для оптимизации, позволяя обходить некоторые проверки безопасности контекста.

Обычно используется в высокопроизводительных сценариях.

---

## **7. Применение и важность**

- `INotifyCompletion` предоставляет основу для реализации асинхронного кода.
- Это интерфейс, который делает возможным использование пользовательских типов с оператором `await`.
- Реализация позволяет контролировать, как и когда будет выполнено продолжение (continuation).