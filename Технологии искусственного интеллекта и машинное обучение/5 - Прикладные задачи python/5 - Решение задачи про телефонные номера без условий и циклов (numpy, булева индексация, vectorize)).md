```
import numpy as np


def clean(x):
    return x.replace('-','').replace('(','').replace(')','')


def replace_8_by_plus_7(x):
    return x.replace('8', '+7', 1)


ph = ['8(495)430-23-97','+7-4-9-5-43-023-97','4-3-0-2-3-9-7','8-495-430','8(918)5238495']

np_ph = np.vectorize(clean)(ph)

#print(np_ph, type(np_ph))
np_l = np.vectorize(len)(np_ph)

#print(np_l, type(np_l))

b7 = np_l == 7
np_ph[b7] = '+7495' + np_ph[b7]
#print(np_ph[b7])

b11 = np_l == 11
np_ph[b11] = np.vectorize(replace_8_by_plus_7)(np_ph[b11])
#print(np_ph[b11])
#print(np_ph)

table = ['NO', 'YES']
yes_no = np.vectorize(lambda x: table[np_ph[0] == x])(np_ph[1:])
print(*yes_no, sep='\n')


```