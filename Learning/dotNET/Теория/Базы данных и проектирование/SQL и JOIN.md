### 1. Что такое SQL?

SQL (Structured Query Language) — это язык структурированных запросов, предназначенный для работы с реляционными базами данных. Он позволяет выполнять операции по созданию, чтению, обновлению и удалению данных (CRUD — Create, Read, Update, Delete).

### 2. Какие задачи решает SQL?

SQL используется для:

- Запроса данных (`SELECT`)
- Вставки новых записей (`INSERT`)
- Обновления существующих данных (`UPDATE`)
- Удаления записей (`DELETE`)
- Определения структуры базы (`CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`)
- Управления правами (`GRANT`, `REVOKE`)
- Управления транзакциями (`BEGIN TRANSACTION`, `COMMIT`, `ROLLBACK`)

### 3. Что означает SQL в контексте .NET? (ORM)

В .NET SQL обычно используется в связке с **ORM (Object-Relational Mapping)**, например, Entity Framework Core (EF Core). ORM позволяет работать с базой данных через объекты, не используя чистый SQL-код. Вместо написания SQL-запросов можно работать с LINQ-запросами, а ORM преобразует их в SQL.

**Пример работы ORM (EF Core)**:

```csharp
var users = dbContext.Users.Where(u => u.Age > 18).ToList();
```

Этот LINQ-запрос ORM превратит в SQL:

```sql
SELECT * FROM Users WHERE Age > 18;
```

### 4. Что такое JOIN?

`JOIN` используется для объединения данных из нескольких таблиц на основе общих полей (обычно ключей).

### 5. Виды JOIN

1. **INNER JOIN** – объединяет только те записи, у которых есть соответствие в обеих таблицах.
2. **LEFT JOIN** – возвращает все записи из левой таблицы и только совпадающие из правой.
3. **RIGHT JOIN** – возвращает все записи из правой таблицы и только совпадающие из левой.
4. **FULL OUTER JOIN** – объединяет все записи из обеих таблиц (очень медленный, так как возвращает все комбинации).
5. крос джоин
6. селф джоин

**Пример:**

```sql
SELECT Orders.OrderID, Customers.Name 
FROM Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
```

### 6. Основные замечания

- **Отсутствие индексов** – сильно замедляет запросы (`WHERE`, `JOIN`, `ORDER BY`).
- **FOREIGN KEY** – гарантирует целостность данных, но может замедлять `INSERT` и `DELETE`.
- **Нормализация** – уменьшает дублирование данных, но усложняет запросы.
- **Избыточные `SELECT *`** – лучше указывать только нужные колонки (`SELECT column1, column2`).
- **Использование `WHERE` вместо `HAVING`** – `HAVING` выполняется после агрегации, `WHERE` – до.

### 7. Нюансы многократного LEFT JOIN

- **NULL-значения** – если в правой таблице нет соответствующих данных, поля будут `NULL`. Нужно проверять это в коде:
    
    ```sql
    SELECT Orders.OrderID, Customers.Name 
    FROM Orders
    LEFT JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
    ```
    
- **Фильтрация по `WHERE` может обнулить строки** – например:
    
    ```sql
    SELECT * FROM Orders
    LEFT JOIN Customers ON Orders.CustomerID = Customers.CustomerID
    WHERE Customers.Name = 'John';  -- Уберет все строки, где `Customers.Name` NULL
    ```
    
    **Решение** – проверять `NULL` явно:
    
    ```sql
    WHERE (Customers.Name = 'John' OR Customers.Name IS NULL)
    ```
    