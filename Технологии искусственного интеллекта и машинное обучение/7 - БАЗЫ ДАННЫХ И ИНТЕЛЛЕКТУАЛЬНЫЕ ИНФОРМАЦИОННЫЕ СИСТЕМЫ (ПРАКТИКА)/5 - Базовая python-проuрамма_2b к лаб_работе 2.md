```
import sqlite3
import os
import  xml.etree.ElementTree as ET

# Путь к файлу базы данных SQLite
db_file = 'library_2b.db'

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

# Чтение XML-файла
tree = ET.parse("data.xml")  # Убедись, что у тебя есть этот файл
root = tree.getroot()

# Функция для вставки данных
def insert_data(table, columns, values):
    placeholders = ", ".join("?" * len(values))
    query = f"INSERT INTO {table} ({', '.join(columns)}) VALUES ({placeholders})"
    cursor.execute(query, values)

# Заполняем таблицу Genre
for genre in root.find("genres").findall("genre"):
    insert_data("Genre", ["GenreID", "GenreName", "Description"], [
        int(genre.find("GenreID").text),
        genre.find("GenreName").text,
        genre.find("Description").text
    ])

# Заполняем таблицу Author
for author in root.find("authors").findall("author"):
    insert_data("Author", ["AuthorID", "Name", "BirthYear", "Nationality"], [
        int(author.find("AuthorID").text),
        author.find("Name").text,
        int(author.find("BirthYear").text),
        author.find("Nationality").text
    ])

# Заполняем таблицу Book
for book in root.find("books").findall("book"):
    insert_data("Book", ["Title", "Author", "Genre", "PublicationYear", "ISBN"], [
        book.find("Title").text,
        int(book.find("Author").text),
        int(book.find("Genre").text),
        int(book.find("PublicationYear").text),
        book.find("ISBN").text
    ])

# Заполняем таблицу BookInstance
for instance in root.find("book_instances").findall("book_instance"):
    insert_data("BookInstance", ["Book", "InstanceNumber", "InstanceStatus", "Condition"], [
        int(instance.find("Book").text),
        int(instance.find("InstanceNumber").text),
        instance.find("InstanceStatus").text,
        instance.find("Condition").text
    ])


# Сохранение изменений
conn.commit()


print("Данные из XML-файла успешно загружены в базу!")


# Получение результатов запроса и вывод на экран
cursor.execute("SELECT * FROM Author")
results = cursor.fetchall()

# Обработка результатов
if results:  # Проверяем, есть ли данные
    for row in results:
        print(row)
else:
    print("Нет данных для вывода.")




# Подсчёт количества авторов
cursor.execute("SELECT COUNT(*) FROM Author")
author_count = cursor.fetchone()[0]
print(f"Количество авторов: {author_count}")



# Вычисление среднего года рождения авторов
cursor.execute("SELECT AVG(BirthYear) FROM Author")
avg_year = cursor.fetchone()[0]
print(f"Средний год рождения авторов: {round(avg_year)}")


# Нахождение самого молодого года рождения
cursor.execute("SELECT MIN(BirthYear) FROM Author")
youngest_year = cursor.fetchone()[0]

if youngest_year is not None:
    # Получение всех авторов с самым молодым годом рождения
    cursor.execute("""
        SELECT Name, BirthYear 
        FROM Author 
        WHERE BirthYear = ?
    """, (youngest_year,))
    
    youngest_authors = cursor.fetchall()

    print("Самые молодые авторы:")
    for author in youngest_authors:
        name, birth_year = author
        print(f"{name}, Год рождения: {birth_year}")
else:
    print("Не найдено авторов.")

# Не забудьте закрыть соединение с базой данных, когда закончите работу
conn.close()




```