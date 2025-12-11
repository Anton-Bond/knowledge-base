```C#
[CompilerGenerated]
private sealed class DelaysStateMachine : IAsyncStateMachine
{
    //откражает текущее состояние, то есть на каком await выполняется ожидание
    //так возможно восстановление выполнения метода с любого await'a
    public int currentState; 
    public AsyncTaskMethodBuilder taskMethodBuilder;
    //текущий объект ожидания
    private TaskAwaiter taskAwaiter;

    //все параметры метода, как и локальные переменные сохраняются в поля для сохранения между ожиданиями при "отпускании" потока
    public int paramInt;
    private int localInt;

    private void MoveNext()
    {
        int num = currentState;
        try
        {
            TaskAwaiter awaiter5;
            TaskAwaiter awaiter4;
            TaskAwaiter awaiter3;
            TaskAwaiter awaiter2;
            TaskAwaiter awaiter;
            switch (num)
            {
                default:
                    localInt = paramInt;  //до первого await
                    Console.WriteLine(1);  //до первого await
                    awaiter5 = Task.Delay(1000).GetAwaiter();  //до первого await
                    if (!awaiter5.IsCompleted) // сам await. Проверяется, завершился ли метод
                    {
                        num = (currentState = 0); //обновление состояние, чтобы возобновить с нужного места
                        taskAwaiter = awaiter5; //сохранение из локальной в поле, ведь после возобновления локальные переменные не сохранятся
                        DelaysStateMachine stateMachine = this; //чтобы передать по ссылке
                        taskMethodBuilder.AwaitUnsafeOnCompleted(ref awaiter5, ref stateMachine); // за страшным названием скрывается лишь действия для присоединения к объекту ожидания продолжения в виде этого метода
                        return;
                    }
                    goto Il_AfterFirstAwait; //если метод завершен, переходим к коду, который следовал далее
                case 0: //данное состояние будет лишь когда первая асинхронная операция не завершилась к моменту проверки, то есть метод пошел по асинхронному пути выполнения. Если мы здесь, то первая операция завершена и мы возобновляем выполнение метода
                    awaiter5 = taskAwaiter; //восстанавливаем объект ожидания
                    taskAwaiter = default(TaskAwaiter); //обнуляем поле класса
                    num = (currentState = -1); //обновляем состояние
                    goto Il_AfterFirstAwait; //переходим непосредственно к логике из исходного метода
                case 1: // мы здесь, если вторая операция не завершилась сразу, а было присоеденино продолжение, которое как раз и запустилось.
                    awaiter4 = taskAwaiter;
                    taskAwaiter = default(TaskAwaiter);
                    num = (currentState = -1);
                    goto Il_AfterSecondAwait;
                case 2: // аналогично, третья операция не завершилась сразу.
                    awaiter3 = taskAwaiter;
                    taskAwaiter = default(TaskAwaiter);
                    num = (currentState = -1);
                    goto Il_AfterThirdAwait;
                case 3: // а здесь четвертая
                    awaiter2 = taskAwaiter;
                    taskAwaiter = default(TaskAwaiter);
                    num = (currentState = -1);
                    goto Il_AfterFourthAwait;
                case 4: // ну и пятая
                    {
                        awaiter = taskAwaiter;
                        taskAwaiter = default(TaskAwaiter);
                        num = (currentState = -1);
                        break;
                    }

                    Il_AfterFourthAwait:
                    awaiter2.GetResult();
                    Console.WriteLine(5); //код после четвертой асинхронной операции
                    awaiter = Task.Delay(1000).GetAwaiter(); //пятая асинхронная операция
                    if (!awaiter.IsCompleted)
                    {
                        num = (currentState = 4);
                        taskAwaiter = awaiter;
                        DelaysStateMachine stateMachine = this;
                        taskMethodBuilder.AwaitUnsafeOnCompleted(ref awaiter, ref stateMachine);
                        return;
                    }
                    break;

                    Il_AfterFirstAwait: //если мы здесь, то первая операция завершилась так или иначе
                    awaiter5.GetResult(); //соответсвенно результат доступен и мы его получаем
                    Console.WriteLine(2); //выполнение того кода, который шел после первого await
                    awaiter4 = Task.Delay(1000).GetAwaiter(); //Выполнение второй асинхронной операции
                    if (!awaiter4.IsCompleted) 
                    {
                        num = (currentState = 1);
                        taskAwaiter = awaiter4;
                        DelaysStateMachine stateMachine = this;
                        taskMethodBuilder.AwaitUnsafeOnCompleted(ref awaiter4, ref stateMachine);
                        return;
                    }
                    goto Il_AfterSecondAwait;

                    Il_AfterThirdAwait:
                    awaiter3.GetResult();
                    Console.WriteLine(4); //код после третей асинхронной операции
                    awaiter2 = Task.Delay(1000).GetAwaiter(); //четвертая асинхронная операция
                    if (!awaiter2.IsCompleted)
                    {
                        num = (currentState = 3);
                        taskAwaiter = awaiter2;
                        DelaysStateMachine stateMachine = this;
                        taskMethodBuilder.AwaitUnsafeOnCompleted(ref awaiter2, ref stateMachine);
                        return;
                    }
                    goto Il_AfterFourthAwait;

                    Il_AfterSecondAwait:
                    awaiter4.GetResult();
                    Console.WriteLine(3); //код после второй асинхронной операции
                    awaiter3 = Task.Delay(1000).GetAwaiter(); //третья асинхронная операция
                    if (!awaiter3.IsCompleted)
                    {
                        num = (currentState = 2);
                        taskAwaiter = awaiter3;
                        DelaysStateMachine stateMachine = this;
                        taskMethodBuilder.AwaitUnsafeOnCompleted(ref awaiter3, ref stateMachine);
                        return;
                    }
                    goto Il_AfterThirdAwait;
            }
            awaiter.GetResult();
        }
        catch (Exception exception)
        {
            currentState = -2;
            taskMethodBuilder.SetException(exception);
            return;
        }
        currentState = -2;
        taskMethodBuilder.SetResult(); //если бы использовали асинхронные операции, которые возвращают результат, он был бы параметром этого метода
    }

    void IAsyncStateMachine.MoveNext() {...}

    [DebuggerHidden]
    private void SetStateMachine(IAsyncStateMachine stateMachine) {...}

    void IAsyncStateMachine.SetStateMachine(IAsyncStateMachine stateMachine) {...}
}


```