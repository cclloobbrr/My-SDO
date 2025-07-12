```
import numpy as np
import time


N = 1000000
r = 1

def rejection_sampling():
    while True:
        x = r * (np.random.rand() * 2 - 1)
        y = r * (np.random.rand() * 2 - 1)
        if x * x + y * y < r * r:
            return x, y

start_time = time.time()

xx = np.zeros(N)
yy = np.zeros(N)
for i in range(N):
    xx[i], yy[i] = rejection_sampling()

#print(xx, type(xx))

l = 2 * (r*r - xx*xx - yy*yy) ** 0.5
l11 = np.average(l)
'''
rho = r * np.random.rand(N)
phi = np.random.rand(N) * 2 * np.pi
xx = rho * np.cos(phi)
yy = rho * np.sin(phi)

l = 2 * (r*r - xx*xx - yy*yy) ** 0.5
l12 = np.average(l)

phi1 = np.random.rand(N) * 2 * np.pi
phi2 = np.random.rand(N) * 2 * np.pi

x2_1 = r * np.cos(phi1)
y2_1 = r * np.sin(phi1)

x2_2 = r * np.cos(phi2)
y2_2 = r * np.sin(phi2)

l = ((x2_2 - x2_1)**2 + (y2_2 - y2_1)**2)**0.5
l2 = np.average(l)
'''


end_time = time.time()
print(f'Затрачено времени {end_time - start_time} секунд')


print('l11 =', l11)
#print('l12 =', l12)
#print('l2 =', l2)
```