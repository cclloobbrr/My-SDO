```
import sqlite3
import os
import sys

# Путь к файлу базы данных SQLite
db_file = 'library.db'


# Если файл базы данных существует, просто подключаемся к нему
# и создаем объект курсор для выполнения SQL-запросов
if  os.path.isfile(db_file):
    conn = sqlite3.connect(db_file)
    cursor = conn.cursor()
else:
    print(f"Файл базы данных {db_file} не существует.")
    sys.exit()  # Завершаем программу



# *********************************
# Использование вложенных запросов
# *********************************

print()
# Объединение имени названия книги и имени автора (таблицы Book, Author )
# При условии что у книги один автор
# Использование оператора "=" в подзапросе
print('********* SELECT (SELECT from Author) from Book *********')
cursor.execute('''
    SELECT Book.Title, 
        (SELECT Author.Name 
        FROM Author 
        WHERE Author.AuthorID = Book.Author) 
FROM Book;
''')
# Получаем результаты
results = cursor.fetchall()
# Печать результатов
for i, result in enumerate(results, start=1):  # начинаем нумерацию с 1
  print(f'{i}. Книга: {result[0]}    Автор: {result[1]} ')  # Печатаем номер и название книги
print()



# Напишите вложенный запрос, который выводит название жанров и всех книг каждого жанра.
# Используйте таблицы Book и Genre.
# Пример запроса должен вернуть список жанров и соответствующие им книги.







# Вложенный запрос на выбор книг конкретного автора
print('******* SELECT from Book (SELECT from Author)  *******')
name = 'М. Горький'
cursor.execute('''
    SELECT Title  FROM Book 
    WHERE Author = (SELECT AuthorID FROM Author WHERE Name = ?)
''', (name,))  # Передаем имя как параметр через переменную name

# Получаем результаты
results = cursor.fetchall()
# Печать результатов
print(f'Автор книг: {name}')
for i, result in enumerate(results, start=1):  # начинаем нумерацию с 1
  print(f'{i}. {result[0]}')  # Печатаем номер и название книги
print()



# Напишите вложенный запрос, который выводит название указанного жанра и всех книг этого жанра.
# Используйте таблицы Genre и Book.
# Передайте жанр в запрос как параметр через переменную genre










# Не забудьте закрыть соединение с базой данных, когда закончите работу
conn.close()
```