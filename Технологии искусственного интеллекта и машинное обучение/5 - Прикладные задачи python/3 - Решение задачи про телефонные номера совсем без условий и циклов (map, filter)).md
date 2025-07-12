```
# -*- coding: utf-8 -*-
"""
Created on Thu Oct 24 15:58:37 2024

@author: ubkprep
"""
import itertools


def clean(x):
    clean.counter += 1
    return clean.counter, x.replace('-','').replace(')','').replace('(','')

clean.counter = 0

phone_book = ['8(495)430-23-97', '+7-4-9-5-43-023-97', '4-3-0-2-3-9-7', 
              '8-495-430', '89185238495']

phone_book= list(map(clean, phone_book))
#print(phone_book, type(phone_book))

shorts = filter(lambda x: len(x[1]) == 7, phone_book)
start_with_8 = filter(lambda x: len(x[1]) == 11, phone_book)
fulls = filter(lambda x: len(x[1]) == 12, phone_book)

m1 = map(lambda x: (x[0], '+7495' + x[1]), shorts)
m2 = map(lambda x: (x[0], '+7' + x[1][1:]), start_with_8)

phone_book = sorted(itertools.chain(fulls, m1, m2))

phone = phone_book.pop(0)[1]
#print(phone, type(phone))
#print(phone_book, type(phone_book))

a1 = map(lambda x: x[1] == phone, phone_book)

table = ['NO\n', 'YES\n']

answer = ''.join(map(lambda x: table[x], a1))
print(answer)
```