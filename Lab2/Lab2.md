# Характеристики положения и рассеяния

Необходимо сгенерировать выборки размером 10, 100, 1000 элементов. Для каждой выборки вычислить следующие статистические характеристики:

1. выборочное среднее
2. выборочная медиана
3. полусумма экстремальных выборочных элементов
4. полусумма квартилей
5. усечённое среднее

Повторить такие вычисления 1000 раз для каждой выборки и найти среднее характеристик положения их квадратов. Вычислить оценку дисперсии. Представить полученные данные в виде таблиц.


```python
from scipy.stats import norminvgauss, laplace, poisson, cauchy, uniform
import numpy as np
import math as m
```


```python
sizes = [10, 100, 1000]
NORMAL, CAUCHY, LAPLACE, POISSON, UNIFORM = "NormalNumber", "CauchyNumber", "LaplaceNumber", "PoissonNumber", "UniformNumber"
NUMBER_OF_REPETITIONS = 1000
```

Функция вычисления выборочного среднего


```python
def mean(selection):
    return np.mean(selection)
```

Функция вычисления выборочной медианы


```python
def med(selection):
    return np.median(selection)
```

Функция вычисления полусуммы экстремальных выборочных элементов


```python
def z_R(selection, size):
    z_R = (selection[0] + selection[size - 1]) / 2
    return z_R
```

Функция вычисления выборочной квартили порядка р


```python
def z_p(selection, np):
    if np.is_integer():
        return selection[int(np)]
    else:
        return selection[int(np) + 1]
```

Функция вычисления полусуммы квартилей


```python
def z_Q(selection, size):
    z_1 = z_p(selection, size / 4)
    z_2 = z_p(selection, 3 * size / 4)
    return (z_1 + z_2) / 2
```

Функция вычисления усечённого среднего


```python
def z_tr(selection, size):
    r = int(size / 4)
    sum = 0
    for i in range(r + 1, size - r + 1):
        sum += selection[i]
    return (1 / (size - 2 * r)) * sum 
```

Функция вычисления дисперсии


```python
def dispersion(selection):
    return np.std(selection) ** 2 
```

Функция построения таблицы для нормального распределения


```python
def NormalNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_Q_list, z_tr_list]
    for size in sizes:
        E, D = [], []    
        for i in range(NUMBER_OF_REPETITIONS):
            distribution = norminvgauss.rvs(1, 0, size=size)
            mean_list.append(mean(distribution))
            med_list.append(med(distribution))
            z_R_list.append(z_R(distribution, size))
            z_Q_list.append(z_Q(distribution, size))
            z_tr_list.append(z_tr(distribution, size))
        for lis in all_list:
            E.append(round(mean(lis), 6))
            D.append(round(dispersion(lis), 6))
        print(E)
        print(D)
    return
NormalNumbers()
```

    [-3.2e-05, -0.008031, 0.036451, -0.021004, -0.012014]
    [0.101239, 0.091199, 0.480519, 0.475769, 0.172801]
    [-0.002052, -0.004684, 0.012205, -0.011495, -0.008917]
    [0.05601, 0.050246, 0.478019, 0.50419, 0.097187]
    [-0.001219, -0.003105, 0.006667, -0.010076, -0.006475]
    [0.037681, 0.033809, 0.475955, 0.497166, 0.065463]
    

Функция построения таблицы для распределения Коши


```python
def CauchyNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_Q_list, z_tr_list]
    for size in sizes:
        E, D = [], []    
        for i in range(NUMBER_OF_REPETITIONS):
            distribution = cauchy.rvs(size=size)
            mean_list.append(mean(distribution))
            med_list.append(med(distribution))
            z_R_list.append(z_R(distribution, size))
            z_Q_list.append(z_Q(distribution, size))
            z_tr_list.append(z_tr(distribution, size))
        for lis in all_list:
            E.append(round(mean(lis), 6))
            D.append(round(dispersion(lis), 6))
        print(E)
        print(D)
    return
CauchyNumbers()
```

    [-1.787828, -0.025951, -5.130831, -3.948137, -2.645886]
    [4390.971006, 0.403047, 71558.568616, 21478.02574, 2724.386192]
    [-2.946291, -0.011733, -2.919987, 7.274195, -5.064231]
    [11499.487958, 0.214055, 37287.430564, 194370.447689, 37068.769842]
    [-2.272925, -0.007192, -1.740728, 4.70099, -4.116761]
    [7794.088368, 0.143556, 25043.951311, 129732.737357, 25183.866103]
    

Функция построения таблицы для распределения Лапласа


```python
def LaplaceNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_Q_list, z_tr_list]
    for size in sizes:
        E, D = [], []    
        for i in range(NUMBER_OF_REPETITIONS):
            distribution = laplace.rvs(size=size, scale=1 / m.sqrt(2), loc=0)
            mean_list.append(mean(distribution))
            med_list.append(med(distribution))
            z_R_list.append(z_R(distribution, size))
            z_Q_list.append(z_Q(distribution, size))
            z_tr_list.append(z_tr(distribution, size))
        for lis in all_list:
            E.append(round(mean(lis), 6))
            D.append(round(dispersion(lis), 6))
        print(E)
        print(D)
    return
LaplaceNumbers()
```

    [0.022501, 0.011001, 0.011483, 0.023003, 0.027397]
    [0.103582, 0.077859, 0.490929, 0.47279, 0.169248]
    [0.012271, 0.005379, 0.021852, 0.021828, 0.015041]
    [0.057048, 0.041891, 0.492694, 0.492542, 0.095815]
    [0.008403, 0.003924, 0.022277, 0.022505, 0.010596]
    [0.038374, 0.028103, 0.487406, 0.505289, 0.064555]
    

Функция построения таблицы распределения Пуассона


```python
def PoissonNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_Q_list, z_tr_list]
    for size in sizes:
        E, D = [], []    
        for i in range(NUMBER_OF_REPETITIONS):
            distribution = poisson.rvs(10, size=size)
            mean_list.append(mean(distribution))
            med_list.append(med(distribution))
            z_R_list.append(z_R(distribution, size))
            z_Q_list.append(z_Q(distribution, size))
            z_tr_list.append(z_tr(distribution, size))
        for lis in all_list:
            E.append(round(mean(lis), 6))
            D.append(round(dispersion(lis), 6))
        print(E)
        print(D)
    return
PoissonNumbers()
```

    [9.979, 9.8525, 9.9655, 10.044, 9.997167]
    [0.975799, 1.373994, 4.68356, 5.018564, 1.779575]
    [9.98473, 9.8535, 9.9615, 10.0425, 9.986043]
    [0.534271, 0.786788, 4.841018, 5.073944, 0.982735]
    [9.990781, 9.902, 9.955, 10.057, 9.99035]
    [0.359408, 0.529563, 4.769308, 5.025751, 0.661672]
    

Функция построения таблицы равномерного распределения


```python
def UniformNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_Q_list, z_tr_list]
    for size in sizes:
        E, D = [], []    
        for i in range(NUMBER_OF_REPETITIONS):
            distribution = uniform.rvs(size=size, loc=-m.sqrt(3), scale=2 * m.sqrt(3))
            mean_list.append(mean(distribution))
            med_list.append(med(distribution))
            z_R_list.append(z_R(distribution, size))
            z_Q_list.append(z_Q(distribution, size))
            z_tr_list.append(z_tr(distribution, size))
        for lis in all_list:
            E.append(round(mean(lis), 6))
            D.append(round(dispersion(lis), 6))
        print(E)
        print(D)
    return
UniformNumbers()
```

    [-0.003058, -0.008189, -0.008308, -0.058391, -0.000635]
    [0.10312, 0.240584, 0.509498, 0.516179, 0.171744]
    [-0.001261, -0.005273, -0.013241, -0.026719, -0.001152]
    [0.056234, 0.134553, 0.506669, 0.513444, 0.095636]
    [-0.000621, -0.003033, -0.005643, -0.017729, -0.000709]
    [0.037829, 0.090714, 0.499291, 0.501763, 0.064392]
    
