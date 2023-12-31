## Основы модуля `re`

Модуль `re` в Python предоставляет возможности для работы с регулярными выражениями, которые являются мощным инструментом для поиска и обработки текстовой информации. В данной части урока мы познакомимся с основами использования модуля `re`, включая импорт и основные функции, а также создание и компиляцию регулярных выражений.

### Импорт модуля `re`

Для начала работы с модулем `re` необходимо импортировать его в свой код. Это можно сделать следующим образом:

```python
import re
```

После импорта модуль `re` становится доступным, и мы можем использовать его функциональность.

## Основные функции модуля `re`

Модуль `re` предоставляет несколько основных функций для работы с регулярными выражениями:

- `re.search()`: Эта функция ищет первое совпадение регулярного выражения в строке. Если совпадение найдено, оно возвращается в виде объекта `match`, который можно использовать для получения информации о совпадении.
- `re.findall()`: Эта функция ищет все совпадения регулярного выражения в строке и возвращает их в виде списка.
- `re.finditer()`: Аналогично `re.findall()`, но возвращает итератор, который можно использовать для обхода совпадений.

## Создание и компиляция регулярных выражений

Для создания регулярных выражений в Python используется стандартный синтаксис, который состоит из символов и конструкций для поиска определенных паттернов в тексте. Например, выражение `\d+` будет искать последовательность одной или более цифр.

Для создания регулярного выражения и его компиляции в объект можно использовать следующий синтаксис:

```python
# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"((\d{4})-\d{2})-(\d{2})")
```

Здесь `r` перед строкой означает "сырую" строку, что полезно при работе с регулярными выражениями, чтобы избежать интерпретации служебных символов как специальных управляющих последовательностей.

После компиляции регулярного выражения в объект `pattern`, мы можем использовать его для поиска совпадений в строках и выполнять различные операции с текстом.

Компиляция регулярных выражений нужна для того, чтобы использовать флаги. Каждый флаг выполняет определенную функцию и может быть полезен в разных сценариях обработки данных. Вот некоторые из них:

1. **re.IGNORECASE (или re.I)**: Этот флаг позволяет игнорировать регистр символов при поиске текста. 
2. **re.DOTALL (или re.S)**: Этот флаг позволяет символу точки (`.`) сопоставлять любой символ, включая символ новой строки. Это полезно, когда вы хотите переносить текст через несколько строк, используя только одно регулярное выражение.

```python
import re

pattern = re.compile(r'abc', re.IGNORECASE)

```

> Полный перечень флагов можно найти в [документации](https://docs.python.org/3.11/library/re.html#flags).
> 

## Объект `match` в модуле `re`

В модуле `re` в Python объект `match` представляет собой результат поиска регулярного выражения в тексте. Этот объект содержит информацию о совпадении, а также позволяет извлекать данные из найденных совпадений. Давайте рассмотрим основные характеристики объекта `match`.

Объект `match` содержит информацию о совпадении между регулярным выражением и текстом. Вот некоторые из основных характеристик этого объекта:

- `group()`: Метод, который возвращает строку, соответствующую совпадению. Если объект `match` был создан после успешного поиска, этот метод вернет строку с найденным текстом.
- `group(num)`: Метод, который позволяет получить совпадение по номеру или имени скобочной группы (если регулярное выражение содержит скобочные группы).
- `start()`: Метод, который возвращает начальную позицию совпадения в тексте.
- `end()`: Метод, который возвращает конечную позицию совпадения в тексте (не включая символ, следующий за совпадением).

Пример использования объекта `match`:

```python
import re

# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"(\d{4})-(\d{2})-(\d{2})")

text = "Дата начала: 2023-09-25. Дата окончания: 2023-09-28"

# Ищем первое совпадение в тексте
match_obj = pattern.search(text)

# Проверяем, было ли найдено совпадение
if match_obj:
    print("Найдено совпадение:", match_obj.group())  # Найдено совпадение: 2023-09-25
    print("Начальная позиция:", match_obj.start())  # Начальная позиция: 6
    print("Конечная позиция:", match_obj.end())  # Конечная позиция: 16

else:
    print("Совпадений не найдено")
```

Объекты `match` позволяют более детально изучать и работать с найденными совпадениями в тексте при использовании регулярных выражений.

Скобочные группы позволяют не только найти соответствия шаблону, но и извлечь конкретные подстроки из текста, соответствующие этим группам. Давайте рассмотрим, как это работает и какие методы и функции можно использовать для доступа к данным, захваченным скобочными группами.

После успешного сопоставления регулярного выражения с текстом, вы можете использовать метод `group()` объекта, возвращаемого методом `match()`, чтобы получить строку, соответствующую скобочной группе.

Пример:

```python
import re

# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"(\d{4})-(\d{2})-(\d{2})")

text = "Дата начала: 2023-09-25. Дата окончания: 2023-09-28"

# Ищем первое совпадение в тексте
match_obj = pattern.search(text)

year = match_obj.group(1)
month = match_obj.group(2)
day = match_obj.group(3)

print(f"День: {day}, Месяц: {month}, Год: {year}")  # День: 25, Месяц: 09, Год: 2023
```

### Методы `findall()` и `finditer()`

Если в тексте может быть несколько соответствий вашему регулярному выражению и вы хотите получить все соответствия, вы можете использовать метод `findall()`. Этот метод вернет список всех найденных совпадений.

Пример:

```python
import re

# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"\d{4}-\d{2}-\d{2}")

text = "Дата начала: 2023-09-25. Дата окончания: 2023-09-28"

# Ищем все совпадения в тексте
match_obj = pattern.findall(text)

print(f"Все найденные даты: {match_obj}")  # Все найденные даты: ['2023-09-25', '2023-09-28']
```

Метод `findall()` возвращает список строк, но при наличии запоминающих групп, он возвращает список кортежей. Где каждый кортеж состоит из запоминающих групп. 

```python
import re

# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"(\d{4})-(\d{2})-(\d{2})")  # выражение содержит запоминающие группы

text = "Дата начала: 2023-09-25. Дата окончания: 2023-09-28"

# Ищем все совпадения в тексте
match_obj = pattern.findall(text)

print(f"Все найденные даты: {match_obj}")  # Все найденные даты: [('2023', '09', '25'), ('2023', '09', '28')]
```

Если нужно иметь список строк, то группы нужно сделать не запоминающими. Попробуйте сделать это самостоятельно и проверить, что при наличии незапоминающих скобочных групп метод `findall()` возвращает список строк.

Когда в выражении присутствуют именованные скобочные группы, удобно пользоваться методом `finditer()`. Метод `re.finditer` аналогичен `re.findall` только возвращает итератор. Итератор позволяет получить больше информации обо всех совпадениях шаблона, поскольку он предоставляет объекты совпадений `match` вместо строк.

```python
import re

# Создаем объект pattern для дат формата YYYY-MM-DD
pattern = re.compile(r"(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})")  # выражение содержит именованные группы

text = "Дата начала: 2023-09-25. Дата окончания: 2023-09-28"

for match_obj in pattern.finditer(text):
    print(f"Найденная дата: {match_obj.groupdict()}")
```

В результате будет выведена следующая информация:

```
Найденная дата: {'year': '2023', 'month': '09', 'day': '25'}
Найденная дата: {'year': '2023', 'month': '09', 'day': '28'}
```

В этом примере вместо того, чтобы обращаться к каждой именованной группе по отдельности `match_obj.group("year")`, `match_obj.group("month")` и `match_obj.group("day")`, использован метод `groupdict()` возвращающий словарь со всеми именованными группами.