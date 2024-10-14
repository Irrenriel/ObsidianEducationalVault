---
title: Contextlib
tags:
  - Python
  - Модули
  - Функции
  - Асинхронность
---
---
### Модуль `contextlib` в Python

#### Описание:
`contextlib` — это мощный модуль Python для создания и управления контекстными менеджерами. Он предлагает набор инструментов для упрощения работы с ресурсами, которые необходимо корректно открывать и закрывать, например, файлы, соединения с базами данных, транзакции. Кроме того, модуль предоставляет возможности для создания как синхронных, так и асинхронных контекстных менеджеров.

---

### Полный список функций и декораторов `contextlib`

1. **`contextmanager`** — Декоратор для создания контекстных менеджеров.
2. **`closing`** — Контекстный менеджер для объектов с методом `close()`.
3. **`suppress`** — Подавление исключений.
4. **`redirect_stdout` и `redirect_stderr`** — Перенаправление потоков вывода и ошибок.
5. **`ExitStack`** — Управление несколькими контекстами.
6. **`AbstractContextManager`** — Базовый класс для создания контекстных менеджеров.
7. **`ContextDecorator`** — Превращение функций в контекстные менеджеры.
8. **`nullcontext`** — Контекстный менеджер, который ничего не делает.
9. **`AsyncExitStack`** — Асинхронное управление несколькими контекстами.
10. **`aclosing`** — Асинхронный контекстный менеджер для объектов с `aclose()`.
11. **`asynccontextmanager`** — Декоратор для создания асинхронных контекстных менеджеров.

---

### Подробное описание функций и декораторов:

#### 1. **`contextmanager`**

Декоратор `@contextmanager` преобразует обычную функцию в контекстный менеджер, используя оператор `yield` для разделения на "до" и "после" выполнения основного блока кода.

Пример:
```python
from contextlib import contextmanager

@contextmanager
def open_file(file_name, mode):
    file = open(file_name, mode)
    try:
        yield file
    finally:
        file.close()

with open_file('file.txt', 'w') as f:
    f.write('Hello, World!')
```

---

#### 2. **`closing`**

`closing` полезен для работы с объектами, у которых есть метод `close()`, но они не поддерживают использование `with`. Этот контекстный менеджер гарантирует вызов метода `close()` по завершению работы с объектом.

Пример:
```python
from contextlib import closing
import urllib.request

with closing(urllib.request.urlopen('http://example.com')) as page:
    for line in page:
        print(line)
```

---

#### 3. **`suppress`**

Контекстный менеджер `suppress` подавляет указанные исключения, позволяя программе продолжить выполнение без ошибок.

Пример:
```python
from contextlib import suppress

with suppress(FileNotFoundError):
    open('non_existent_file.txt')
```

---

#### 4. **`redirect_stdout` и `redirect_stderr`**

Эти контекстные менеджеры используются для перенаправления стандартного вывода (`stdout`) или потока ошибок (`stderr`) в другой объект (например, файл или строковый буфер).

Пример:
```python
from contextlib import redirect_stdout

with open('output.txt', 'w') as f:
    with redirect_stdout(f):
        print('This will be written to output.txt')
```

---

#### 5. **`ExitStack`**

`ExitStack` позволяет динамически управлять несколькими контекстными менеджерами в одном блоке `with`. Это особенно полезно, когда количество контекстов зависит от условий.

Пример:
```python
from contextlib import ExitStack

with ExitStack() as stack:
    files = [stack.enter_context(open(f'file{i}.txt', 'w')) for i in range(3)]
    for i, f in enumerate(files):
        f.write(f'File {i}\n')
```

---

#### 6. **`AbstractContextManager`**

`AbstractContextManager` — это базовый класс, предоставляющий стандартную реализацию метода `__enter__()`. Для создания пользовательских контекстных менеджеров достаточно реализовать метод `__exit__()`.

Пример:
```python
from contextlib import AbstractContextManager

class MyContextManager(AbstractContextManager):
    def __exit__(self, exc_type, exc_value, traceback):
        print("Exiting the context")

with MyContextManager():
    print("Inside the context")
```

---

#### 7. **`ContextDecorator`**

`ContextDecorator` позволяет функции одновременно быть как декоратором, так и контекстным менеджером. Это упрощает применение одного и того же кода как в виде декоратора, так и через оператор `with`.

Пример:
```python
from contextlib import ContextDecorator

class mycontext(ContextDecorator):
    def __enter__(self):
        print("Entering context")
        return self
    
    def __exit__(self, exc_type, exc_value, traceback):
        print("Exiting context")

@mycontext()
def my_function():
    print("Inside function")

my_function()
```

---

#### 8. **`nullcontext`**

`nullcontext` используется, когда вам нужен контекстный менеджер, который не выполняет никаких действий, что может быть полезно в ситуациях, когда код работает как с контекстом, так и без него.

Пример:
```python
from contextlib import nullcontext

with nullcontext():
    print("Nothing to manage here")
```

---

#### 9. **`AsyncExitStack`**

`AsyncExitStack` — это асинхронная версия `ExitStack`, используемая для работы с асинхронными контекстными менеджерами.

Пример:
```python
import asyncio
from contextlib import AsyncExitStack

async def async_file_manager():
    async with AsyncExitStack() as stack:
        files = [await stack.enter_async_context(open(f'file{i}.txt', 'w')) for i in range(3)]
        for i, f in enumerate(files):
            await f.write(f'File {i}\n')

asyncio.run(async_file_manager())
```

---

#### 10. **`aclosing`**

`aclosing` — это асинхронный контекстный менеджер для объектов с методом `aclose()`, таких как асинхронные генераторы или соединения.

Пример:
```python
import asyncio
from contextlib import aclosing

async def async_gen():
    for i in range(5):
        yield i

async def main():
    async with aclosing(async_gen()) as agen:
        async for item in agen:
            print(item)

asyncio.run(main())
```

---

#### 11. **`asynccontextmanager`**

`asynccontextmanager` — это декоратор, позволяющий создать асинхронный контекстный менеджер на основе асинхронной функции с использованием `async def` и `yield`.

Пример:
```python
from contextlib import asynccontextmanager

@asynccontextmanager
async def my_async_cm():
    print("Entering")
    try:
        yield
    finally:
        print("Exiting")

async def main():
    async with my_async_cm():
        print("Inside")

import asyncio
asyncio.run(main())
```

---

### Заключение:

Модуль `contextlib` предоставляет полный набор инструментов для работы с контекстными менеджерами как в синхронных, так и асинхронных приложениях. Он значительно упрощает создание контекстных менеджеров, позволяет управлять несколькими контекстами и даёт гибкость для динамического управления ресурсами.

---

### Полезные ссылки:

- [Официальная документация Python: Модуль contextlib](https://docs.python.org/3/library/contextlib.html)
- [Руководство по контекстным менеджерам](https://realpython.com/python-with-context-managers/)

---
