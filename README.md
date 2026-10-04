# Russian Profanity Dataset (RU-MAT)

Датасет русской ненормативной лексики для фильтрации контента в чатах, ботах и социальных сетях. Включает основные морфологические формы, приставочные вариации и завуалированные написания.

## О датасете

- Язык: русский, частично английский
- Объём: около 1300 уникальных слов и словоформ
- Формат: `words.txt`, одно слово на строку, кодировка UTF-8
- Регистр: приведён к нижнему
- Лицензия: MIT

## Назначение

Датасет предназначен для:

- мат-фильтров в Telegram-ботах и чатах
- модерации пользовательского контента
- обучения моделей определения токсичности
- исследований в области русской лексикографии

## Дисклеймер

Файл содержит реальные оскорбительные слова и ненормативную лексику. Использовать только для фильтрации и исследований.

## Структура репозитория

```
.
├── words.txt          основной файл со словами
├── merge_words.py     скрипт объединения нескольких txt в один
├── fix_bom.py         утилита удаления BOM
├── README.md
└── LICENSE
```

## Что включено

| Категория       | Примеры                                              |
|-----------------|------------------------------------------------------|
| Мат на хуй      | хуй, хуя, хую, хуе, хуё, хуи, хуйня, хуесос          |
| Мат на пизд     | пизда, пиздец, пиздеть, пиздюк                       |
| Мат на еб/ёб    | ебать, ёбнуть, заебал, наебал                        |
| Мат на бля      | бля, блять, блядь, блядство                          |
| Мат на муд      | мудак, мудила, мудозвон                              |
| Прочее          | сука, шлюха, говно, жопа                             |
| Английский      | spam, porno                                          |

## Быстрый старт

Python-пример фильтра:

```python
import re

with open("words.txt", "r", encoding="utf-8") as f:
    MATS = [
        line.strip().lower()
        for line in f
        if line.strip() and not line.startswith("#")
    ]

def has_mat(text: str) -> bool:
    if not text:
        return False
    clean = re.sub(r"[^а-яёa-z0-9]", "", text.lower())
    for word in MATS:
        w = re.sub(r"[^а-яёa-z0-9]", "", word)
        if w and w in clean:
            return True
    return False

print(has_mat("привет"))   # False
print(has_mat("х у й"))    # True
print(has_mat("пиздец"))   # True
```

Склеить несколько txt-файлов в один:

```bash
python merge_words.py
```

## Вклад

Pull Request приветствуется. Требования к словам в `words.txt`:

- одно слово на строку
- нижний регистр
- без знаков препинания
- комментарии начинаются с `#`

## Источники

Часть слов собрана из открытых источников:

- LDNOOBW/List-of-Dirty-Naughty-Obscene-and-Otherwise-Bad-Words
- for-publishing/bad-words
- catmoez/Large-Obscenity-Lexicon

## Лицензия

MIT. См. файл LICENSE.
```

---

## 2. README для Hugging Face — `README.md`

Первая часть — YAML-шапка, её обязательно оставить. Ниже — обычный markdown.

```markdown
---
language:
  - ru
  - en
license: mit
task_categories:
  - text-classification
tags:
  - profanity
  - toxicity
  - text-filtering
  - russian
  - moderation
  - nsfw
size_categories:
  - n<1K
configs:
  - config_name: default
    data_files:
      - split: train
        path: words.txt
---

# Russian Profanity Dataset (RU-MAT)

Датасет русской ненормативной лексики для фильтрации контента и модерации. Включает основные морфологические формы, приставочные вариации и завуалированные написания.

## О датасете

- Языки: русский, английский
- Объём: около 1300 уникальных слов
- Формат: текстовый файл, одно слово на строку
- Регистр: нижний
- Кодировка: UTF-8

## Назначение

- мат-фильтры для ботов и чатов
- модерация пользовательского контента
- обучение моделей определения токсичности

## Дисклеймер

Датасет содержит реальные оскорбительные слова. Использовать только для фильтрации и исследований.

## Использование

Загрузка через datasets:

```python
from datasets import load_dataset

ds = load_dataset("your-username/russian-profanity")
print(ds["train"][0])
# {'word': 'выхуяривание'}
```

Прямое чтение файла:

```python
import requests

url = "https://huggingface.co/datasets/your-username/russian-profanity/resolve/main/words.txt"
words = requests.get(url).text.splitlines()
```

Пример фильтра:

```python
import re

def has_profanity(text, words):
    clean = re.sub(r"[^а-яёa-z0-9]", "", text.lower())
    return any(
        re.sub(r"[^а-яёa-z0-9]", "", w) in clean
        for w in words if w.strip()
    )

print(has_profanity("х у й", words))    # True
print(has_profanity("привет", words))   # False
```

## Структура данных

Один файл `words.txt`:

- одна строка — одно слово
- строки, начинающиеся с `#`, являются комментариями
- пустые строки игнорируются

## Файлы

| Файл       | Описание               |
|------------|------------------------|
| words.txt  | основной список слов   |
| README.md  | описание датасета      |

## Вклад

Pull Request приветствуется. Формат:

- одно слово на строку
- нижний регистр
- без знаков препинания
