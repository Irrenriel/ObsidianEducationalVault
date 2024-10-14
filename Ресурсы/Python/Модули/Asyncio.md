---
title: Asyncio
tags:
  - Python
  - Асинхронность
  - Модули
---

### Модуль `asyncio`

#### Описание:
Модуль `asyncio` предоставляет инфраструктуру для написания асинхронных программ, работающих на базе событийного цикла. Он позволяет выполнять множество задач параллельно, используя концепции сопрограмм (корутин), событийных циклов и неблокирующих операций. Асинхронное программирование позволяет писать более производительные программы, которые эффективно используют ресурсы и реагируют на внешние события, такие как сетевые запросы или ввод/вывод.

---

### Основные компоненты модуля `asyncio`

#### 1. **Событийный цикл (`Event Loop`)**

Асинхронные программы управляются событийному циклу — основной конструкции, которая управляет выполнением всех асинхронных задач и событий. Событийный цикл обрабатывает задачи и возвращает управление программе, когда задачи готовы к выполнению.

- **`asyncio.get_event_loop()`** — Возвращает текущий событийный цикл.
- **`asyncio.new_event_loop()`** — Создаёт новый событийный цикл.
- **`asyncio.set_event_loop(loop)`** — Устанавливает новый событийный цикл как текущий.

Пример:
```python
import asyncio

async def say_hello():
    print('Hello')
    await asyncio.sleep(1)
    print('World!')

loop = asyncio.get_event_loop()
loop.run_until_complete(say_hello())
loop.close()
```

---

#### 2. **Асинхронные функции (корутины)**

Асинхронные функции (корутины) — это функции, которые выполняются асинхронно, используя ключевое слово `async` перед `def`. Внутри таких функций для ожидания других асинхронных задач используется ключевое слово `await`.

- **`async def`** — Определяет асинхронную функцию.
- **`await`** — Ожидание завершения асинхронной задачи.

Пример:
```python
import asyncio

async def main():
    print('Start')
    await asyncio.sleep(1)
    print('End')

asyncio.run(main())
```

---

#### 3. **Асинхронные задачи (`Task`)**

Задачи в `asyncio` представляют собой отложенные корутины, которые выполняются параллельно в рамках события. Они создаются с помощью метода `asyncio.create_task()`.

- **`asyncio.create_task(coro)`** — Создаёт задачу для выполнения корутины.

Пример:
```python
import asyncio

async def say(message, delay):
    await asyncio.sleep(delay)
    print(message)

async def main():
    task1 = asyncio.create_task(say('Hello', 1))
    task2 = asyncio.create_task(say('World', 2))

    await task1
    await task2

asyncio.run(main())
```

---

#### 4. **Асинхронные генераторы и контекстные менеджеры**

Асинхронные генераторы и контекстные менеджеры позволяют управлять асинхронными потоками данных или ресурсами. Они определяются с помощью ключевого слова `async` перед `for` и `with`.

- **`async for`** — Итерация по асинхронному генератору.
- **`async with`** — Использование асинхронного контекстного менеджера.

Пример:
```python
import asyncio

class AsyncGen:
    async def __aiter__(self):
        for i in range(5):
            await asyncio.sleep(1)
            yield i

async def main():
    async for item in AsyncGen():
        print(item)

asyncio.run(main())
```

---

#### 5. **Функции работы с задачами и корутинами**

- **`asyncio.gather(*coroutines)`** — Выполняет несколько корутин параллельно и возвращает их результаты.
- **`asyncio.wait(tasks)`** — Ожидает завершения всех задач из списка.
- **`asyncio.as_completed(tasks)`** — Возвращает итератор, который генерирует завершённые задачи по мере их выполнения.

Пример:
```python
import asyncio

async def say(message, delay):
    await asyncio.sleep(delay)
    return message

async def main():
    result = await asyncio.gather(say('Hello', 1), say('World', 2))
    print(result)

asyncio.run(main())
```

---

#### 6. **Функции задержки и тайм-ауты**

- **`asyncio.sleep(delay)`** — Асинхронная задержка выполнения программы на указанный `delay` секунд.
- **`asyncio.wait_for(coro, timeout)`** — Выполняет корутину с указанным тайм-аутом. Если корутина не завершится за это время, будет выброшено исключение `asyncio.TimeoutError`.

Пример:
```python
import asyncio

async def main():
    try:
        await asyncio.wait_for(asyncio.sleep(10), timeout=5)
    except asyncio.TimeoutError:
        print('Timeout!')

asyncio.run(main())
```

---

#### 7. **Асинхронные потоки ввода-вывода**

Модуль `asyncio` предоставляет возможность асинхронной работы с вводом-выводом, включая работу с сетевыми соединениями и файлами. 

- **`asyncio.open_connection(host, port)`** — Открывает TCP-соединение.
- **`asyncio.start_server(callback, host, port)`** — Запускает TCP-сервер.

Пример клиента и сервера:
```python
import asyncio

async def handle_client(reader, writer):
    data = await reader.read(100)
    print(f'Received: {data.decode()}')
    writer.close()

async def main():
    server = await asyncio.start_server(handle_client, '127.0.0.1', 8888)
    async with server:
        await server.serve_forever()

asyncio.run(main())
```

---

#### 8. **Синхронизация: блокировки и семафоры**

Модуль `asyncio` включает механизмы синхронизации, такие как блокировки и семафоры, которые можно использовать для управления доступом к общим ресурсам.

- **`asyncio.Lock()`** — Асинхронная блокировка, используемая для синхронизации задач.
- **`asyncio.Semaphore()`** — Ограничивает количество одновременных асинхронных задач.

Пример использования блокировки:
```python
import asyncio

lock = asyncio.Lock()

async def task(id):
    async with lock:
        print(f'Task {id} is running')
        await asyncio.sleep(1)

async def main():
    await asyncio.gather(task(1), task(2), task(3))

asyncio.run(main())
```

---

#### 9. **Исключения и ошибки**

- **`asyncio.CancelledError`** — Исключение, выбрасываемое при отмене задачи.
- **`asyncio.TimeoutError`** — Исключение при превышении тайм-аута в задачах.

Пример отмены задачи:
```python
import asyncio

async def task():
    await asyncio.sleep(10)

async def main():
    task_obj = asyncio.create_task(task())
    await asyncio.sleep(1)
    task_obj.cancel()
    try:
        await task_obj
    except asyncio.CancelledError:
        print("Task was cancelled")

asyncio.run(main())
```

---

### Заключение:

Модуль `asyncio` является мощным инструментом для асинхронного программирования в Python. Он предоставляет гибкие возможности для работы с параллельными задачами, асинхронными функциями, сетевыми операциями и многими другими аспектами. Использование `asyncio` позволяет значительно повысить производительность программ, особенно в сценариях, связанных с вводом-выводом, сетевыми операциями и асинхронными задачами.

---

#### Полезные ссылки:
- [Официальная документация Python: Модуль asyncio](https://docs.python.org/3/library/asyncio.html)
- [Руководство по асинхронному программированию с asyncio](https://realpython.com/async-io-python/)

---
