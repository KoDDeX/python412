# Креативные задачи по Python: Циклы, строки и списки

## Задачки

**1. Умный поиск в тексте**
Найдите все вхождения слова в тексте с учетом регистра и без. Программа должна:
- Показать позиции всех точных совпадений
- Показать позиции совпадений без учета регистра
- Посчитать общее количество совпадений
- Показать контекст (5 символов до и после)
Пример: "Python это python, а не PYTHON" → 
- Точные совпадения: [0, 9]
- Все совпадения: [0, 9, 19]
- Всего: 3
- Контексты: ["_Python_", "_python_", "_PYTHON"]

**2. Цензурный фильтр**
Замените все "плохие" слова звёздочками, сохраняя первую и последнюю буквы. 
Программа продолжает работу пока не встретит слово "стоп".
Пример: "редиска" → "р****а"

**3. Генератор тегов**
Создайте систему тегов из текста, где:
- Теги создаются из слов длиннее 3 букв
- Каждый тег начинается с #
- Теги не должны содержать спецсимволы
- Подсчитайте топ-3 самых длинных тега
Пример: "Я люблю программирование на Python!" → 
"#люблю #программирование #python"

**4. Парсер смайликов**
Найдите все текстовые смайлики в сообщении и замените их на emoji коды.
:) → 😊
:( → ☹️
;) → 😉
:D → 😀
XD → 😆

**5. Сортировщик символов**
Отсортируйте символы в строке по категориям:
- Заглавные буквы
- Строчные буквы
- Цифры
- Пробелы
- Знаки препинания
Выведите статистику по каждой категории.

**6. Форматировщик списка**
Получите список строк и отформатируйте каждую:
- Удалите пробелы в начале и конце
- Сделайте первую букву заглавной
- Добавьте точку в конце если нет знаков .!?
- Уберите повторяющиеся знаки препинания
Пример: "  привет!!!!!" → "Привет!"

**7. Генератор мема**
Преобразуйте текст в мем-формат:
- Чередуйте регистр букв: пРиВеТ мИр
- Добавьте случайные смайлики в конце предложений
- Замените гласные на цифры похожие на них (а->4, о->0)
Пример: "Привет мир" → "пРиВеТ мИр 0_о"

**8. Поиск палиндромов**
Найдите все палиндромы в тексте:
- Игнорируйте пробелы и знаки препинания
- Учитывайте слова от 3 букв
- Покажите контекст обнаружения
- Сделайте статистику по длине

**9. Текстовый градиент**
Создайте эффект градиента в тексте:
- Каждое следующее слово имеет больше заглавных букв
- Можно добавить символы градиента (░▒▓█)
- Учитывайте пробелы как разделители
Пример: "привет мой мир" → "привет МОй МИР"

**10. Рифмовщик**
Найдите рифмующиеся слова в тексте:
- Слова рифмуются если совпадают последние 2+ буквы
- Группируйте рифмы вместе
- Игнорируйте регистр
Пример: "кошка ложка мошка немножко" → 
Рифма "ошка": кошка, ложка, мошка
Рифма "ножко": немножко

**11. Текстовый фильтр**
Удалите из текста все:
- Повторяющиеся пробелы
- Пустые строки
- Строки короче 3 символов
- Строки состоящие только из цифр
Сохраните статистику изменений.

**12. Генератор никнеймов 2.0**
Создайте крутые никнеймы из слов:
- Замените похожие буквы на цифры (о->0, з->3)
- Добавьте случайные символы из списка (_x.-)
- Чередуйте регистр
- Ограничение длины 15 символов
Пример: "крутой игрок" → "KpyT0u_urp0k"

---
## Решения

### 1. Удаление повторяющихся знаков препинания

```python
text = "Привет!!!!!   Как дела????"

result = ""
prev_char = ""

for char in text:
    # Если символ знак препинания и совпадает с предыдущим - пропускаем
    if char in "!?." and char == prev_char:
        continue
    result += char
    prev_char = char

print(result)  # Привет! Как дела?
```

### 2. Генератор мема

```python
text = "Привет мир"
smiles = ["^_^", "0_o", ":)", "=)"]

result = ""
upper = True  # Флаг для чередования регистра

# Преобразуем каждый символ
for char in text:
    if char.isalpha():
        if upper:
            result += char.upper()
        else:
            result += char.lower()
        upper = not upper
    else:
        result += char

# Заменяем гласные на цифры
result = result.replace("а", "4").replace("о", "0")

# Добавляем случайный смайлик
import random
result += " " + random.choice(smiles)

print(result)
```

### 3. Поиск палиндромов

```python
text = "А роза упала на лапу Азора. Дед читает. Шалаш стоит."

# Очищаем текст от знаков и пробелов
def clean_text(text):
    result = ""
    for char in text.lower():
        if char.isalpha():
            result += char
    return result

words = text.split()
palindromes = []
stats = {}  # Для статистики по длине

for word in words:
    clean_word = clean_text(word)
    if len(clean_word) >= 3:
        if clean_word == clean_word[::-1]:
            palindromes.append(word)
            length = len(clean_word)
            if length in stats:
                stats[length] += 1
            else:
                stats[length] = 1

print("Найденные палиндромы:", palindromes)
print("Статистика по длине:", stats)
```

### 4. Текстовый градиент

```python
text = "привет мой мир"
words = text.split()
result = []

# С каждым словом увеличиваем количество заглавных букв
for i, word in enumerate(words):
    new_word = ""
    # Количество заглавных букв зависит от позиции слова
    caps_count = int((i + 1) * len(word) / len(words))
    
    for j, char in enumerate(word):
        if j < caps_count:
            new_word += char.upper()
        else:
            new_word += char.lower()
    result.append(new_word)

# Добавляем символы градиента
gradient = "░▒▓█"
final_text = " ".join(result) + " " + gradient

print(final_text)
```

### 5. Рифмовщик

```python
text = "кошка ложка мошка немножко"
words = text.lower().split()
rhymes = []

# Ищем слова с одинаковыми окончаниями
for i in range(len(words)):
    current_rhyme = []
    ending = words[i][-2:]  # Берем последние 2 буквы
    
    for word in words:
        if word.endswith(ending):
            current_rhyme.append(word)
    
    # Добавляем только если нашли минимум 2 рифмующихся слова
    if len(current_rhyme) > 1 and current_rhyme not in rhymes:
        rhymes.append(current_rhyme)

# Выводим результат
for rhyme_group in rhymes:
    ending = rhyme_group[0][-2:]
    print(f'Рифма "{ending}": {", ".join(rhyme_group)}')
```

### 6. Текстовый фильтр

```python
text = """
привет

мир   тут
123
п
   много    пробелов
"""

filtered_lines = []
stats = {
    "removed_spaces": 0,
    "removed_lines": 0,
    "removed_numbers": 0
}

lines = text.split("\n")

for line in lines:
    # Подсчет удаленных пробелов
    spaces_count = line.count(" ")
    clean_line = " ".join(line.split())
    stats["removed_spaces"] += spaces_count - clean_line.count(" ")
    
    # Пропускаем короткие строки и числовые строки
    if len(clean_line) < 3:
        stats["removed_lines"] += 1
        continue
    if clean_line.isdigit():
        stats["removed_numbers"] += 1
        continue
        
    if clean_line:  # Пропускаем пустые строки
        filtered_lines.append(clean_line)

result = "\n".join(filtered_lines)
print(result)
print("Статистика:", stats)
```

### 7. Генератор никнеймов

```python
text = "крутой игрок"
symbols = ["_", "x", ".", "-"]

# Замена букв на цифры
replacements = {
    "о": "0",
    "з": "3",
    "е": "3",
    "а": "4",
    "и": "1",
    "с": "5"
}

result = ""
upper = True

# Преобразуем текст
for char in text.lower():
    if char in replacements:
        result += replacements[char]
    elif char.isalpha():
        if upper:
            result += char.upper()
        else:
            result += char
        upper = not upper
    else:
        result += char

# Добавляем случайный символ
import random
result = result.replace(" ", random.choice(symbols))

if len(result) > 15:
    result = result[:15]

print(result)
```
# Задача 8: Подсчет слов
```python
text = "Мама мыла раму мама"
words = text.lower().split()

total = 0
for word in words:
    if word == "мама":
        total = total + 1

print("Слово мама встречается:", total)
```
# Задача 9: Поиск самого длинного слова
```python
text = "Мама мыла раму окно"
words = text.split()

longest = ""
for word in words:
    if len(word) > len(longest):
        longest = word

print("Самое длинное слово:", longest)
```
# Задача 10: Удаление гласных
```python
text = "Привет мир"
vowels = "аеёиоуыэюя"
result = ""

for char in text.lower():
    if char not in vowels:
        result = result + char

print(result)
```

# Задача 11: Подсчет согласных букв
```python
text = "Программирование"
consonants = "бвгджзйклмнпрстфхцчшщ"
total = 0

for char in text.lower():
    if char in consonants:
        total = total + 1

print("Количество согласных букв:", total)
```
# Задача 12: Переворот строки
```python
text = "Привет мир"
result = ""

for i in range(len(text) - 1, -1, -1):
    result = result + text[i]

print("Перевернутая строка:", result)
```