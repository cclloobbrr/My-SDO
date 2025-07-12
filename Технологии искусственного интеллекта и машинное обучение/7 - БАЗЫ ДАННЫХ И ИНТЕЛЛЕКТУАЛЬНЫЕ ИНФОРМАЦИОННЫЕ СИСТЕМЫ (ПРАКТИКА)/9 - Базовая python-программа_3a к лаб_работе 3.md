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
# Использование объединений JOIN
# *********************************

# Объединение имени автора и названия книги (таблицы Author, Book)
print()
print('**********  INNER JOIN  (Author, Book) ***********')

cursor.execute('''
    SELECT Author.Name , Book.Title 
    FROM Author
    JOIN Book ON Author.AuthorID = Book.Author
''')
results = cursor.fetchall()
# Печать результатов
for result in results:
    print(f'Автор: {result[0]}, Название: {result[1]}')
print()



# Напишите запрос, который выводит название жанров и всех книг каждого жанра.
# Используйте таблицы Genre и Book.





# Объединение  названия книги, имени автора, жанра и года публикации 
# (таблицы Book, Author, Genre)

print('********* INNER JOIN (Author, Book, Genre) *********')
cursor.execute('''
        SELECT Book.Title, Author.Name, Genre.GenreName, Book.PublicationYear
        FROM Book
        JOIN Author ON Book.Author = Author.AuthorID
        JOIN Genre ON Book.Genre = Genre.GenreID
    ''')
results = cursor.fetchall()

# Печать результатов
for result in results:
    print(f'Название: {result[0]}, Автор: {result[1]}, Жанр: {result[2]}, Год публикации: {result[3]}')
print()


# подсчет числа экземпляров каждой книги (с использованием алиасов b, bi)
print('********* INNER JOIN (Book, BookInstance)  COUNT  GROUP BY  *********')
cursor.execute('''
    SELECT b.Title, COUNT(bi.BookInstanceID) AS NumberOfCopies
    FROM Book b
    JOIN BookInstance bi ON b.BookID = bi.Book
    GROUP BY b.BookID
''')
# Получение и вывод результатов
results = cursor.fetchall()
for row in results:
    print(f"Книга: {row[0]}, Количество экземпляров: {row[1]}")
print()   



# Напишите запрос, который выводит имена авторов и общее количество экземпляров книг, 
# написанных каждым автором. Используйте таблицы Author  Book и BookInstance.
# Используйте для подсчета экземпляров книг функцию COUNT








# Поиск книг по заданному жанру (таблицы Book и Genre) 
# Использована фильтрация по определенному жанру, например, 'роман'
# Название жанра передается в запрос через переменную genre_name 
genre_name = 'роман'

# SQL-запрос с использованием переменной
cursor.execute('''
SELECT Book.Title, Genre.GenreName
FROM Book
JOIN Genre ON Book.Genre = Genre.GenreID
WHERE Genre.GenreName = ?
''', (genre_name,))  # используем переменную в запросе

# Получение и вывод результатов
print('**********  INNER JOIN  ( Book, Genre для genre_name = роман) ***********')
results = cursor.fetchall()
if results:
    for row in results:
        print(f'Название книги: {row[0]}, Жанр: {row[1]}')
else:
    print(f'Нет книг жанра "{genre_name}".')
print()




# Создайте запрос, который выводит название конкретной книги, 
# имя автора и количество экземпляров этой книги. 
# Используйте таблицы Book, Author и BookInstance.
# Название книги передавайте в запрос через переменную, например title='Мать'





# Создайте запрос, который выводит названия книг и имена авторов для тех книг, 
# у которых нет ни одного экземпляра в библиотеке. 
# Используйте таблицы Book, Author и BookInstance.
# Используйте конструкцию WHERE BookInstance.BookInstanceID IS NULL;




# Не забудьте закрыть соединение с базой данных, когда закончите работу
conn.close()

```