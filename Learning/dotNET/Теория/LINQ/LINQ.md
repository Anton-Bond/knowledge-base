
**Синтаксис:**
- **Query Syntax:** Подобен SQL.
- **Method Syntax:** Использует методы расширения.

Пример:
```C#
int[] numbers = { 1, 2, 3, 4, 5 };
var evenNumbers = from n in numbers
                  where n % 2 == 0
                  select n;
// Или методами:
var evenNumbersMethod = numbers.Where(n => n % 2 == 0);
```

#### **LINQ to Objects**

Позволяет выполнять запросы к объектам в памяти, таким как массивы, списки и словари.

- **Пример:** Фильтрация
```C#
List<string> names = new List<string> { "Alice", "Bob", "Charlie" };
var filtered = names.Where(name => name.StartsWith("A"));
```

**Основные методы LINQ:**

- `Where`: Фильтрация элементов.
- `Select`: Преобразование элементов.
- `OrderBy`, `OrderByDescending`: Сортировка.
- `GroupBy`: Группировка.
- `Join`: Соединение коллекций.

#### **LINQ to SQL** (Entities)

- Предоставляет возможность выполнять запросы к базам данных через LINQ.
- **Пример:**
```C#
using (var context = new DataContext(connectionString))
{
    var customers = from c in context.GetTable<Customer>()
                    where c.City == "London"
                    select c;
}
```


#### **LINQ to XML**

- Позволяет выполнять запросы и манипулировать XML-документами.  
    **Пример:**
```C#
XDocument doc = XDocument.Load("data.xml");
var elements = from e in doc.Descendants("Element")
               where e.Attribute("Id").Value == "1"
               select e;
```

#### **LINQ в асинхронном программировании**: Parallel LINQ (PLINQ)

Методы `AsParallel` и `AsEnumerable` позволяют обрабатывать запросы асинхронно.  
**Пример:**
```C#
var parallelQuery = numbers.AsParallel().Where(n => n % 2 == 0);
```


#### **Оптимизация LINQ**

1. **Минимизируйте количество вызовов LINQ:**
    - Не вызывайте `ToList()` до завершения обработки.
2. **Используйте методы с задержкой (Deferred Execution):**  
    Методы, такие как `Where` и `Select`, не выполняются сразу.
3. **Не используйте LINQ для обработки больших данных:**  
    В некоторых случаях оптимальнее использовать цикл.
4. **Композитные фильтры:**  
    Объединяйте фильтры для уменьшения количества проходов по коллекции.


Есть два способа выполнения запроса LINQ: **отложенное (deferred)** и **немедленное (immediate)** выполнение.

**При отложенном выполнении LINQ**-выражение не выполняется, пока не будет произведена итерация или перебор по выборке, например, в цикле foreach. Обычно подобные операции возвращают объект `IEnumerable<T>` или `IOrderedEnumerable<T>`. 
**Полный список отложенных операций LINQ:**
- AsEnumerable
- Cast
- Concat
- DefaultIfEmpty
- Distinct
- Except
- GroupBy
- GroupJoin
- Intersect
- Join
- OfType
- OrderBy
- OrderByDescending
- Range
- Repeat
- Reverse
- Select
- SelectMany
- Skip
- SkipWhile
- Take
- TakeWhile
- ThenBy
- ThenByDescending
- Union
- Where
```C#
string[] people = { "Tom", "Sam", "Bob" };
 
var selectedPeople = people.Where(s=>s.Length == 3).OrderBy(s=>s);
 
// выполнение LINQ-запроса
foreach (string s in selectedPeople)
    Console.WriteLine(s);
```

### Немедленное выполнение запроса

С помощью ряда методов мы можем применить немедленное выполнение запроса. Это методы, которые возвращают одно атомарное значение или один элемент или данные типов Array, List и Dictionary. Полный список подобных операций в LINQ:
- Aggregate    
- All    
- Any    
- Average    
- Contains    
- Count    
- ElementAt    
- ElementAtOrDefault    
- Empty    
- First    
- FirstOrDefault    
- Last    
- LastOrDefault    
- LongCount    
- Max    
- Min    
- SequenceEqual    
- Single    
- SingleOrDefault    
- Sum    
- ToArray    
- ToDictionary    
- ToList    
- ToLookup

---

## [[Метод расширения в LINQ]]

---
### **10 Популярных вопросов на собеседовании по LINQ с ответами**

#### **1. Что такое LINQ и для чего он используется?**

**Ответ:** LINQ — это способ написания запросов к данным внутри C#. Он используется для работы с коллекциями, базами данных, XML и другими источниками данных, предоставляя удобный синтаксис для фильтрации, сортировки и преобразования данных.

---

#### **2. Чем отличаются Query Syntax и Method Syntax?**

**Ответ:**

- **Query Syntax:** Похож на SQL, используется для написания запросов в стиле языка запросов.
- **Method Syntax:** Использует методы расширения, более гибок и читаем для разработчиков.  
    **Пример:**
`// Query Syntax`
`var result = from n in numbers where n > 2 select n; `

` // Method Syntax `
`var result = numbers.Where(n => n > 2);`

---

#### **3. Что такое Deferred Execution (отложенное выполнение)?**

**Ответ:** Это особенность LINQ, при которой запрос не выполняется до тех пор, пока не потребуется результат (например, при вызове `ToList` или `First`).

---

#### **4. Как работает метод `SelectMany`?**

**Ответ:** Он раскрывает вложенные коллекции, объединяя их в одну плоскую коллекцию.

**Пример:**

`var data = new List<string[]> { new string[] { "A", "B" }, new string[] { "C", "D" } }; `
`var flat = data.SelectMany(x => x); // ["A", "B", "C", "D"]`

---

#### **5. Как работает метод `GroupBy`?**

**Ответ:** Группирует элементы коллекции по ключу.  
**Пример:**
`var grouped = numbers.GroupBy(n => n % 2); `
`foreach (var group in grouped) `
`{`
    `Console.WriteLine($"Key: {group.Key}, Values: {string.Join(",", group)}");`
`}`

---

#### **6. Как объединить две коллекции с помощью LINQ?**

**Ответ:** Используйте метод `Join`:
`var result = from c in customers `
`             join o in orders on c.Id equals o.CustomerId`
`              select new { c.Name, o.OrderId };`

---

#### **7. Чем `First` отличается от `FirstOrDefault`?**

**Ответ:**
- `First`: Возвращает первый элемент или выбрасывает исключение, если коллекция пуста.
- `FirstOrDefault`: Возвращает первый элемент или `null`/значение по умолчанию, если коллекция пуста.

---

#### **8. Что такое `AsEnumerable` и когда его использовать?**

**Ответ:** Преобразует источник данных к `IEnumerable`, чтобы остановить обработку на стороне базы данных (например, для LINQ to SQL).

---

#### **9. Как оптимизировать сложные LINQ-запросы?**

**Ответ:**
1. Используйте метод `ToList()` только после завершения всех операций.
2. Избегайте дублирующих фильтров.
3. Разделяйте запросы на небольшие части.

---

#### **10. Как написать асинхронный LINQ-запрос?**

**Ответ:** Используйте `AsParallel` для многопоточности или комбинируйте с `async/await`.  
**Пример:**
`var result = await Task.Run(() => numbers.AsParallel().Where(n => n % 2 == 0).ToList());`