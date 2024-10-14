---
title: Enum
tags:
  - Python
  - Модули
---
---
### Модуль `enum` в Python

#### Описание:
Модуль `enum` в Python предоставляет поддержку для создания перечислений (enum). Перечисления — это способ создать набор именованных констант, каждая из которых связана с уникальным значением. `enum` позволяет задавать не только значения, но и упорядоченные коллекции элементов, улучшая читаемость кода и снижая вероятность ошибок, связанных с использованием "магических чисел" или строк в коде.

---

### Основные компоненты модуля `enum`

#### 1. **Класс `Enum`**

Класс `Enum` используется для создания перечислений. Каждое перечисление является уникальным и неизменным элементом. Значения перечисления задаются при объявлении и могут быть строками, числами или любыми другими объектами.

Пример:
```python
from enum import Enum

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

print(Color.RED)  # Вывод: Color.RED
print(Color.RED.name)  # Вывод: RED
print(Color.RED.value)  # Вывод: 1
```

---

#### 2. **Сравнение и итерация**

Элементы перечисления можно сравнивать по их имени или значению. Также можно перебирать все элементы перечисления с помощью цикла `for`.

Пример:
```python
from enum import Enum

class Direction(Enum):
    NORTH = 1
    SOUTH = 2
    EAST = 3
    WEST = 4

# Сравнение
if Direction.NORTH == Direction.SOUTH:
    print("Неправильно")
else:
    print("Правильно")  # Вывод: Правильно

# Итерация
for direction in Direction:
    print(direction)
# Вывод:
# Direction.NORTH
# Direction.SOUTH
# Direction.EAST
# Direction.WEST
```

---

#### 3. **Атрибуты `name` и `value`**

Каждый элемент перечисления обладает двумя важными атрибутами:
- `name` — это имя элемента перечисления.
- `value` — это значение элемента перечисления.

Пример:
```python
from enum import Enum

class Status(Enum):
    SUCCESS = 200
    NOT_FOUND = 404
    SERVER_ERROR = 500

print(Status.SUCCESS.name)  # Вывод: SUCCESS
print(Status.SUCCESS.value)  # Вывод: 200
```

---

#### 4. **Методы `Enum`**

Модуль `enum` предоставляет несколько полезных методов для работы с перечислениями:

- **`Enum` как итератор**: Все элементы перечисления можно перебирать с помощью цикла.
- **`Enum` как словарь**: Вы можете получить доступ к элементу перечисления через его имя или значение.

Пример:
```python
from enum import Enum

class Weekday(Enum):
    MONDAY = 1
    TUESDAY = 2
    WEDNESDAY = 3

# Получение элемента по имени
day = Weekday['MONDAY']
print(day)  # Вывод: Weekday.MONDAY

# Получение элемента по значению
day = Weekday(2)
print(day)  # Вывод: Weekday.TUESDAY
```

---

#### 5. **Класс `IntEnum`**

`IntEnum` — это подкласс `Enum`, который используется для создания перечислений, чьи значения являются целыми числами. Он поддерживает все операции, которые можно выполнять с числами.

Пример:
```python
from enum import IntEnum

class StatusCode(IntEnum):
    OK = 200
    NOT_FOUND = 404
    INTERNAL_ERROR = 500

print(StatusCode.OK)  # Вывод: StatusCode.OK
print(StatusCode.OK + 100)  # Вывод: 300
```

---

#### 6. **Класс `Flag` и `IntFlag`**

Классы `Flag` и `IntFlag` используются для создания перечислений с битовыми флагами, что позволяет комбинировать значения с помощью побитовых операций.

- **`Flag`** — для работы с флагами на уровне битов.
- **`IntFlag`** — то же, что и `Flag`, но поддерживает арифметические операции.

Пример:
```python
from enum import Flag, auto

class Permissions(Flag):
    READ = auto()
    WRITE = auto()
    EXECUTE = auto()

combo = Permissions.READ | Permissions.WRITE
print(combo)  # Вывод: Permissions.READ|WRITE

# Проверка флага
print(Permissions.READ in combo)  # Вывод: True
```

---

#### 7. **Переопределение методов в перечислениях**

Вы можете переопределять методы и добавлять собственные атрибуты и методы в классы перечислений. Это полезно для задания дополнительной логики или поведения.

Пример:
```python
from enum import Enum

class Planet(Enum):
    MERCURY = (3.303e+23, 2.4397e6)
    EARTH = (5.976e+24, 6.37814e6)
    
    def __init__(self, mass, radius):
        self.mass = mass
        self.radius = radius
    
    def surface_gravity(self):
        G = 6.674 * 10**-11
        return G * self.mass / (self.radius ** 2)

print(Planet.EARTH.surface_gravity())
```

---

#### 8. **Перечисления с одинаковыми значениями**

Перечисления допускают использование одинаковых значений, но такие члены считаются одним и тем же элементом. Вы можете управлять этим поведением с помощью декоратора `@unique`.

Пример:
```python
from enum import Enum, unique

@unique
class TrafficLight(Enum):
    RED = 1
    GREEN = 2
    YELLOW = 3
    # RED = 4  # Ошибка: Повторное значение
```

---

### Заключение:

Модуль `enum` предоставляет мощный инструмент для работы с именованными константами, улучшая читаемость и поддерживаемость кода. Перечисления особенно полезны, когда требуется набор фиксированных значений, которые могут быть заданы и использованы в логике программы. Модуль поддерживает различные виды перечислений, включая битовые флаги, целочисленные значения и расширенные методы.

---

#### Полезные ссылки:
- [Официальная документация Python: Модуль enum](https://docs.python.org/3/library/enum.html)
- [Перечисления в Python](https://realpython.com/python-enum/)

---
