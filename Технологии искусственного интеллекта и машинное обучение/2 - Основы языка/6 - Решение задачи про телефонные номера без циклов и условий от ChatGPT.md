# Решение задачи про телефонные номера без циклов и условий от ChatGPT

```
def normalize(phone):
    # Удаляем все ненужные символы
    phone = phone.translate(str.maketrans('', '', '-()'))
    # Определяем код страны: заменяем 8 на 7 или добавляем код 495
    phone = phone * (len(phone) != 7) + ("495" + phone) * (len(phone) == 7)
    # Возвращаем последние 10 символов
    return phone[-10:]


target = '8(495)430-23-97'
phone_book = ['+7-4-9-5-43-023-97','4-3-0-2-3-9-7','8-495-430','8(918)5238495']


# Нормализуем и сравниваем
normalized_target = normalize(target)
results = list(map(lambda contact: ["NO", "YES"][(normalize(contact) == normalized_target)], phone_book))

# Выводим результаты
print('\n'.join(results))
```