```
import sqlite3
import os
import json

# Путь к файлу базы данных SQLite
db_file = 'library_2a.db'

# ****************************
# эта команда нужна для отладки (для повторных запусков)
# Если файл БД уже существует, то удалить существующий файл БД, 
# что бы предотвратить повторную запись тех же самых данных
if  os.path.isfile(db_file):
  os.remove(db_file)
# **************************** 

# Проверка существования файла базы данных
if not os.path.isfile(db_file):
    # Если файл базы данных не существует, создаем его и подключаемся
    conn = sqlite3.connect(db_file)

    # Создание объекта курсора для выполнения SQL-запросов
    cursor = conn.cursor()
  
    # Создание таблиц и внесение данных в базу данных

    # Создание таблицы Genre
    cursor.execute('''CREATE TABLE IF NOT EXISTS Genre (
        GenreID INTEGER PRIMARY KEY,
        GenreName TEXT NOT NULL UNIQUE,
        Description TEXT
    )''')

    # Создание таблицы Author
    cursor.execute('''CREATE TABLE IF NOT EXISTS Author (
        AuthorID INTEGER PRIMARY KEY,
        Name TEXT NOT NULL,
        BirthYear INTEGER CHECK (BirthYear <= 2010),
        Nationality TEXT
    )''')

    # Создание таблицы Book
    cursor.execute('''CREATE TABLE IF NOT EXISTS Book (
        BookID INTEGER PRIMARY KEY,
        Author INTEGER NOT NULL,
        Genre INTEGER NOT NULL,
        Title TEXT NOT NULL,
        PublicationYear INTEGER NOT NULL,
        ISBN TEXT UNIQUE,
        FOREIGN KEY (Author) REFERENCES Author(AuthorID),
        FOREIGN KEY (Genre) REFERENCES Genre(GenreID)
    )''')

    # Создание таблицы BookInstance 
    cursor.execute('''CREATE TABLE IF NOT EXISTS BookInstance (
        BookInstanceID INTEGER PRIMARY KEY,
        Book INTEGER NOT NULL,
        InstanceNumber INTEGER NOT NULL DEFAULT 1,
        InstanceStatus TEXT,
        Condition TEXT,
        FOREIGN KEY (Book) REFERENCES Book(BookID)
    )''')

    # Сохранение изменений 
    conn.commit()


else:
    # Если файл базы данных существует, просто подключаемся к нему
    conn = sqlite3.connect(db_file)
    # и создаем объект курсор для выполнения SQL-запросов
    cursor = conn.cursor()

# Чтение JSON-файла
with open("data.json", "r", encoding="utf-8") as file:
    data = json.load(file)

# Заполнение таблицы Genre
for genre in data["genres"]:
    cursor.execute("INSERT INTO Genre (GenreID, GenreName, Description) VALUES (?, ?, ?)", (genre["GenreID"], genre["GenreName"], genre["Description"]))

# Заполнение таблицы Author
for author in data["authors"]:
    cursor.execute("INSERT INTO Author (AuthorID, Name, BirthYear, Nationality) VALUES (?, ?, ?, ?)", (author["AuthorID"], author["Name"], author["BirthYear"], author["Nationality"]))

# Заполнение таблицы Book
for book in data["books"]:
    cursor.execute("INSERT INTO Book (Title, Author, Genre, PublicationYear, ISBN) VALUES (?, ?, ?, ?, ?)", (book["Title"], book["Author"], book["Genre"], book["PublicationYear"], book["ISBN"]))

# Заполнение таблицы BookInstance
for instance in data["book_instances"]:
    cursor.execute("INSERT INTO BookInstance (Book, InstanceNumber, InstanceStatus, Condition) VALUES (?, ?, ?, ?)", (instance["Book"], instance["InstanceNumber"], instance["InstanceStatus"], instance["Condition"]))
# Сохранение изменений
conn.commit()


print("Данные из JSON-файла успешно загружены в базу!")


# Получение результатов запроса и вывод на экран
cursor.execute("SELECT * FROM Book")
results = cursor.fetchall()

# Обработка результатов
if results:  # Проверяем, есть ли данные
    for row in results:
        print(row)
else:
    print("Нет данных для вывода.")



# Подсчёт количества книг
cursor.execute("SELECT COUNT(*) FROM Book")
book_count = cursor.fetchone()[0]
print(f"Количество книг: {book_count}")

# Вычисление среднего года публикации книг
cursor.execute("SELECT AVG(PublicationYear) FROM Book")
avg_year = cursor.fetchone()[0]
print(f"Средний год публикации книг: {round(avg_year)}")


# Нахождение минимального года публикации
cursor.execute("SELECT MIN(PublicationYear) FROM Book")
min_year = cursor.fetchone()[0]
print(f"Минимальный год публикации: {min_year}")


# Нахождение максимального года публикации
cursor.execute("SELECT MAX(PublicationYear) FROM Book")
max_year = cursor.fetchone()[0]
print(f"Максимальный год публикации: {max_year}")



# Не забудьте закрыть соединение с базой данных, когда закончите работу
conn.close()




```