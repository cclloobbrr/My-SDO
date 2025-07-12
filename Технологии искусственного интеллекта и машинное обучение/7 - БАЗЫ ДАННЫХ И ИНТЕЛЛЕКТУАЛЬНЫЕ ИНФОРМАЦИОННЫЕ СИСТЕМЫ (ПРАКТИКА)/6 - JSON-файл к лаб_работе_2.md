```
{
    "genres": [
        {"GenreID": 1, "GenreName": "роман", "Description": "Художественная проза"},
        {"GenreID": 2, "GenreName": "драма", "Description": "Серьезное литературное произведение"}
    ],
    "authors": [
     {"AuthorID": 1, "Name": "М. Горький", "BirthYear": 1868, "Nationality": "русский"},
     {"AuthorID": 2, "Name": "А.Чехов", "BirthYear": 1860, "Nationality": "русский"}
    ],
    "books": [
     {"Title": "Мать", "Author": 1, "Genre": 1, "PublicationYear": 2006, "ISBN": "978-1234567890"},
     {"Title": "На дне", "Author": 1, "Genre": 2, "PublicationYear": 2002, "ISBN": "978-0987654321"}
    ],
    "book_instances": [
     {"Book": 1, "InstanceNumber": 1, "InstanceStatus": "Доступен", "Condition": "Хорошее"},
     {"Book": 2, "InstanceNumber": 1, "InstanceStatus": "Выдан", "Condition": "Отличное"}
    ]
}
```