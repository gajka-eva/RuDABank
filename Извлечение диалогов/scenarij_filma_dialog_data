!pip install PyPDF2

import PyPDF2
import pandas as pd
import re

# Открываем PDF-файл в режиме чтения
with open('/content/***.pdf', 'rb') as pdf_file:
    pdf_reader = PyPDF2.PdfReader(pdf_file)

    # Инициализируем пустую строку для форматированного текста
    formatted_text = ""

    # Итерируемся по страницам PDF
    for page in pdf_reader.pages:
        # Извлекаем текст с текущей страницы
        page_text = page.extract_text()

        # Добавляем имя (в верхнем регистре) перед каждой репликой
        formatted_page_text = ""
        lines = page_text.split('\n')
        speaker = None
        for line in lines:
            if line.isupper():
                if speaker is not None:
                    formatted_page_text += f"{speaker}\n"
                speaker = line
            else:
                formatted_page_text += f"{line}\n"
        if speaker is not None:
            formatted_page_text += f"{speaker}\n"

        # Добавляем текст с текущей страницы к общему форматированному тексту
        formatted_text += formatted_page_text

# Выводим полученный форматированный текст
print(formatted_text)

# Разделяем текст на диалоги с использованием регулярных выражений
dialogs = re.split(r'([А-ЯЁ]+)\s', formatted_text)
dialogs = [dialog.strip() for dialog in dialogs if dialog.strip()]

# Инициализируем списки для имен и слов
names = []
words = []

# Переменная для хранения текущего диалога
current_name = ""
current_words = []

# Перебираем диалоги и извлекаем имена и слова
for dialog in dialogs:
    if dialog.isupper():
        # Если текущее имя не пустое, добавляем его и соответствующие слова в списки
        if current_name:
            names.append(current_name)
            words.append(' '.join(current_words))
        current_name = dialog
        current_words = []
    else:
        # Добавляем слова к текущему диалогу
        current_words.append(dialog)

# Добавляем последний диалог
if current_name:
    names.append(current_name)
    words.append(' '.join(current_words))

# Создаем датасет из списков имен и слов
df = pd.DataFrame({'Имя': names, 'Слова': words})

# Выводим датасет
df.head()

# Сохраняем датасет
df.to_csv('dialog.csv')
