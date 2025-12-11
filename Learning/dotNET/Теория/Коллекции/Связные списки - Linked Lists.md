**Коллекция элементов (узлов), где  каждый элемент хранит ссылку одновременно на следующий и на предыдущий элемент.**

**Особенности:**
- Эффективно вставлять/удалять элементы в середине.
- Используется класс `LinkedList<T>`.

```C#
LinkedList<string> people = new LinkedList<string>();


var employees = new List<string> { "Tom", "Sam", "Bob" };
 
LinkedList<string> people = new LinkedList<string>(employees);
foreach (string person in people)
{
    Console.WriteLine(person);
}
```

### Свойства LinkedList

Класс LinkedList определяет следующие свойства:
- Count: количество элементов в связанном списке
- First: первый узел в списке в виде объекта `LinkedListNode<T>`
- Last: последний узел в списке в виде объекта `LinkedListNode<T>`
