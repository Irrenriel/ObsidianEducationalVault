---
title: Itertools
tags:
  - Python
  - Модули
  - Функции
  - Циклы
---
---
### Модуль `itertools`

#### Описание:
Модуль `itertools` предоставляет эффективные инструменты для работы с итераторами, упрощая создание и управление последовательностями. Он включает функции для работы с бесконечными итерациями, комбинированием и перестановками элементов, фильтрацией, накоплением и группировкой данных. Использование `itertools` позволяет писать более компактный, читаемый и производительный код, особенно при работе с большими данными.

---

### Основные функции модуля `itertools`

#### 1. **Бесконечные итераторы:**

- **`itertools.count(start=0, step=1)`**  
  Генерирует бесконечную последовательность чисел, начиная с `start`, увеличиваясь на `step`.
  
  Пример:
  ```python
  import itertools
  for i in itertools.count(10, 2):
      if i > 20:
          break
      print(i)
  ```

- **`itertools.cycle(iterable)`**  
  Бесконечно повторяет элементы переданной последовательности.

  Пример:
  ```python
  colors = ['red', 'green', 'blue']
  cycle_colors = itertools.cycle(colors)
  for _ in range(6):
      print(next(cycle_colors))
  ```

- **`itertools.repeat(object, times=None)`**  
  Возвращает бесконечное количество копий объекта. Если указать параметр `times`, повторение происходит `times` раз.

  Пример:
  ```python
  for item in itertools.repeat('Python', 3):
      print(item)
  ```

---

#### 2. **Комбинирование и перестановки:**

- **`itertools.permutations(iterable, r=None)`**  
  Возвращает все перестановки длины `r` из элементов последовательности. По умолчанию длина равна длине последовательности.

  Пример:
  ```python
  items = ['A', 'B', 'C']
  print(list(itertools.permutations(items)))
  ```

- **`itertools.combinations(iterable, r)`**  
  Возвращает все комбинации длины `r` без повторений.

  Пример:
  ```python
  items = ['A', 'B', 'C']
  print(list(itertools.combinations(items, 2)))
  ```

- **`itertools.combinations_with_replacement(iterable, r)`**  
  Возвращает все комбинации длины `r` с повторениями.

  Пример:
  ```python
  items = ['A', 'B', 'C']
  print(list(itertools.combinations_with_replacement(items, 2)))
  ```

- **`itertools.product(*iterables, repeat=1)`**  
  Вычисляет декартово произведение входных итераторов.

  Пример:
  ```python
  print(list(itertools.product([1, 2], ['a', 'b'])))
  ```

---

#### 3. **Функции для фильтрации и группировки:**

- **`itertools.groupby(iterable, key=None)`**  
  Группирует последовательность по ключу `key`. Элементы последовательности должны быть отсортированы.

  Пример:
  ```python
  data = [('A', 1), ('B', 1), ('B', 2), ('C', 1)]
  for key, group in itertools.groupby(data, key=lambda x: x[0]):
      print(key, list(group))
  ```

- **`itertools.filterfalse(predicate, iterable)`**  
  Возвращает элементы, для которых функция `predicate` возвращает `False`.

  Пример:
  ```python
  def is_even(x):
      return x % 2 == 0

  numbers = [1, 2, 3, 4, 5, 6]
  print(list(itertools.filterfalse(is_even, numbers)))
  ```

---

#### 4. **Функции для работы с несколькими итераторами:**

- **`itertools.chain(*iterables)`**  
  Соединяет несколько итераторов в один.

  Пример:
  ```python
  list1 = [1, 2, 3]
  list2 = ['a', 'b', 'c']
  print(list(itertools.chain(list1, list2)))
  ```

- **`itertools.chain.from_iterable(iterable)`**  
  То же, что и `chain`, но принимает один итератор, содержащий другие итераторы.

  Пример:
  ```python
  list_of_lists = [[1, 2, 3], ['a', 'b', 'c']]
  print(list(itertools.chain.from_iterable(list_of_lists)))
  ```

---

#### 5. **Накопительные и маппинговые функции:**

- **`itertools.accumulate(iterable, func=operator.add)`**  
  Возвращает накопленные значения на основе переданной функции `func`. По умолчанию используется сложение.

  Пример:
  ```python
  import itertools, operator
  data = [1, 2, 3, 4]
  print(list(itertools.accumulate(data)))  # [1, 3, 6, 10]
  print(list(itertools.accumulate(data, operator.mul)))  # [1, 2, 6, 24]
  ```

- **`itertools.starmap(func, iterable)`**  
  Применяет функцию `func` к элементам итератора, распаковывая элементы как аргументы.

  Пример:
  ```python
  data = [(2, 5), (3, 2), (10, 3)]
  print(list(itertools.starmap(pow, data)))  # [32, 9, 1000]
  ```

---

#### 6. **Прочие полезные функции:**

- **`itertools.tee(iterable, n=2)`**  
  Возвращает `n` независимых копий итератора. Каждый результат независим и может быть использован в разных частях программы.

  Пример:
  ```python
  data = [1, 2, 3, 4, 5]
  iter1, iter2 = itertools.tee(data, 2)
  print(list(iter1))  # [1, 2, 3, 4, 5]
  print(list(iter2))  # [1, 2, 3, 4, 5]
  ```

- **`itertools.zip_longest(*iterables, fillvalue=None)`**  
  Объединяет несколько итераторов в один, пока самый длинный итератор не будет исчерпан. Для коротких итераторов используются значения по умолчанию `fillvalue`.

  Пример:
  ```python
  iter1 = [1, 2, 3]
  iter2 = ['a', 'b']
  print(list(itertools.zip_longest(iter1, iter2, fillvalue='?')))
  # [(1, 'a'), (2, 'b'), (3, '?')]
  ```

---

### Заключение:

Модуль `itertools` предоставляет обширный набор функций для работы с итераторами, что упрощает решение таких задач, как генерация комбинаций и перестановок, фильтрация данных и эффективное объединение последовательностей. Эти инструменты позволяют писать производительный код для обработки больших данных или сложных последовательностей, избегая явных циклов.

---

#### Полезные ссылки:
- [Официальная документация Python: Модуль itertools](https://docs.python.org/3/library/itertools.html)
- [Руководство по использованию itertools](https://realpython.com/python-itertools/)

---
