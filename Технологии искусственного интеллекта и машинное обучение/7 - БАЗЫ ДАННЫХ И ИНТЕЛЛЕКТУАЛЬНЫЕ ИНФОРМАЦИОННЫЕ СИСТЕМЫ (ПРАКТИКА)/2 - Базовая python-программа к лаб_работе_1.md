```
import sqlite3
import os

# Путь к файлу базы данных SQLite
db_file = 'library.db'

# ****************************
# эта команда нужна для отладки (для повторных запусков)
# Если файл БД уже существует, то удалить существующий файл БД, 
# что бы предотвратить повторную запись тех же самых данных
if  os.path.isfile(db_file):
  os.remove(db_file)
# ***************************


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
        VolumeNumber INTEGER NOT NULL DEFAULT 1,
        VolumeStatus TEXT,
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

# Далее можно выполнять операции с базой данных (добавление/чтение данных)
# # Внесение одиночной записи в таблицу Author
author_data = ("М. Горький", 1868, "русский")
cursor.execute("INSERT INTO Author (Name, BirthYear, Nationality) VALUES (?, ?, ?)", author_data)

# Внесение множества данных в таблицу Author
authors_data = [
    ("А. Чехов", 1860, "русский"), 
    ("Т. Шевченко", 1814, "украинец"),
    ("Л. Толстой", 1828, "русский")
]
cursor.executemany("INSERT INTO Author (Name, BirthYear, Nationality) VALUES (?, ?, ?)", authors_data)

# Добавление жанров
genres_data = [
    ("роман", "Произведение прозаического жанра, длинное по объему, описывающее развитие сюжета и психологию персонажей"),
    ("драма", "Произведение, предназначенное для театрального исполнения, с акцентом на драматические события и конфликты"),
    ("повесть", "Произведение прозаического жанра, короткое по объему, часто с ограниченным числом персонажей и событий"),
    ("поэма", "Произведение, представляющее собой лиро-эпическую или эпическую форму, чаще всего написанное в стихотворной форме")
]
cursor.executemany("INSERT INTO Genre (GenreName, Description) VALUES (?, ?)", genres_data)

# Добавление книг
books_data = [
    (1, 1, "Мать", 1996, "978-1234567890"),
    (1, 2, "На дне", 1992, "978-0987654321"),
    (2, 3, "Каштанка", 2017, "978-5432167890"),
    (2, 3, "Вишневый сад", 2017, "978-6432167890"),
    (3, 4, "Гайдамаки", 1930, "978-2345678901"),
    (4, 1, "Война и мир", 2025, "978-3456789012")
]
cursor.executemany("INSERT INTO Book (Author, Genre, Title, PublicationYear, ISBN) VALUES (?, ?, ?, ?, ?)", books_data)

# Добавление экземпляров
instances_data = [
    (1, 1, "Доступен", "Хорошее"),
    (1, 2, "Доступен", "Хорошее"),
    (2, 1, "Доступен", "Хорошее"),
    (2, 2, "Доступен", "Хорошее"),
    (3, 1, "Доступен", "Отличное"),
    (3, 2, "Доступен", "Отличное"),
    (4, 1, "Доступен", "Хорошее"),
    (5, 1, "Доступен", "Удовлетворительное"),
    (6, 1, "Доступен", "Отличное")
]
cursor.executemany("INSERT INTO BookInstance (Book, VolumeNumber, VolumeStatus, Condition) VALUES (?, ?, ?, ?)", instances_data)

# Сохранение изменений 
conn.commit()


# *****************************************
# ЗАПРОСЫ К БД
# ****************************************


# Получение результатов запроса и вывод на экран
cursor.execute("SELECT * FROM Book")
results = cursor.fetchall()

# Обработка результатов
if results:  # Проверяем, есть ли данные
    print ("Содержимое таблицы Book. Вывод всех атрибутов (*)")
    for row in results:
        print(row)
else:
    print("Нет данных для вывода.")
print()   # для отделения результатов разных запросов при выводе


# Выполнение запроса для извлечения данных из таблицы Author
cursor.execute("SELECT * FROM Author")

# Получение первой строки результата
results = cursor.fetchall()
print ("Содержимое таблицы Authors. Вывод всех атрибутов (*)")
# Обработка результатов
if results:  # Проверяем, есть ли данные
    for row in results:
        print(row)
else:
    print("Нет данных для вывода.")




# Написать SQL-запросы на вывод: названий книг и их ISBN.



# Написать SQL-запросы на вывод: названий жанров;


# Написать SQL-запросы на вывод: Фамилий авторов и их годов рождения.



# Написать SQL-запросы на вывод: названий жанров и их краткие описания;




# Написать SQL-запросы на вывод: фамилий всех авторов и их годы рождения;



# Написать SQL-запросы на вывод: фамилий авторов, родившихся до 1850 года; 
# Использовать WHERE



# Написать SQL-запросы на вывод: фамилий авторов, отсортированных по алфавиту (в порядке возрастания); 
# использовать ORDER BY 



# Написать SQL-запросы на вывод: названий книг, отсортированных в алфавитном порядке (в порядке убывания);
# использовать ORDER BY 


# Написать SQL-запросы на вывод: названий книг, изданных до 2000 года;
# Использовать WHERE











# Не забудьте закрыть соединение с базой данных, когда закончите работу
conn.close()
```