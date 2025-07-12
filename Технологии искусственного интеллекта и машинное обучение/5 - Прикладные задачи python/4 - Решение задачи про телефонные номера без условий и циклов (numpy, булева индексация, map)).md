```
import numpy as np


def clean(x):
    xx = x.replace('-','').replace('(','').replace(')','')
    return xx

ph = ['8(495)430-23-97','+7-4-9-5-43-023-97','4-3-0-2-3-9-7','8-495-430','8(918)5238495']


np_ph = np.array(tuple(map(clean, ph)))
#print(np_ph, type(np_ph))
np_l = np.array(tuple(map(len, np_ph)))
#print(np_l, type(np_l))

b7 = np_l == 7
np_ph[b7] = '+7495' + np_ph[b7]
#print(np_ph[b7])

b11 = np_l == 11

work = np_ph[b11]
#print(work, type(work))

work = np.array(tuple(map(lambda x: x.replace('8', '+7', 1), work)))

#print(work)

np_ph[b11] = work

#print(np_ph)

table = ['NO', 'YES']
yes_no = tuple(map(lambda x: table[np_ph[0] == x], np_ph[1:]))
print(*yes_no, sep='\n')

```