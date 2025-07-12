```
import itertools


def clean_and_count(x):
    clean_and_count.counter += 1
    return clean_and_count.counter, x.replace('-', '').replace('(', '').replace(')', '')


clean_and_count.counter = 0

phone = input('Введите номер телефона => ')

print('phone =', phone, type(phone))

phone_book = [phone]
phone_book += ['8(495)430-23-97', '+7-4-9-5-43-023-97', '4-3-0-2-3-9-7',
               '8-495-430', '+79185238495']

print('phone_book =', phone_book, type(phone_book))

phone_book = list(map(clean_and_count, phone_book))
print('phone_book =', phone_book, type(phone_book))

# print(*map(lambda x: x, phone_book))
shorts = list(filter(lambda x: len(x[1]) == 7, phone_book))
print('shorts =', shorts, type(shorts))
start_with_8 = list(filter(lambda x: len(x[1]) == 11 and x[1][0] == '8', phone_book))
print('start_with_8 =', start_with_8, type(start_with_8))
fulls = list(filter(lambda x: len(x[1]) == 12, phone_book))
print('fulls =', fulls, type(fulls))

m1 = map(lambda x: (x[0], '+7495' + x[1]), shorts)
m2 = map(lambda x: (x[0], '+7' + x[1][1:]), start_with_8)

phone_book = sorted(itertools.chain(m1, m2, fulls))
print('phone_book =', phone_book, type(phone_book))
phone = phone_book.pop(0)[1]

print('phone =', phone, type(phone))
print('phone_book =', phone_book, type(phone_book))

answer = ''.join(map(lambda x: 'YES\n' if phone == x[1] else 'NO\n', phone_book))
print(answer)
```