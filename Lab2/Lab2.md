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
    all_list = [mean_list, med_list, z_R_list, z_tr_list]
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

    [0.008085, 0.010633, 0.021127, 0.010177]
    [0.100556, 0.089664, 0.472883, 0.166701]
    [0.002114, 0.004009, -0.006275, 0.00312]
    [0.055455, 0.049157, 0.500878, 0.093204]
    [0.001405, 0.002835, -0.004931, 0.001902]
    [0.037304, 0.033088, 0.495314, 0.062814]
    

Функция построения таблицы для распределения Коши


```python
def CauchyNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_tr_list]
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

    [2.966373, -0.016697, 14.37477, -1.265183]
    [14169.629098, 0.375039, 197784.702719, 14489.287834]
    [1.75722, -0.008725, 8.135323, 0.19181]
    [7362.023691, 0.20039, 100646.661657, 7841.246515]
    [0.794621, -0.004712, 5.440703, -0.315861]
    [5126.23835, 0.134454, 67319.596729, 5976.38589]
    

Функция построения таблицы для распределения Лапласа


```python
def LaplaceNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_tr_list]
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

    [0.003678, 0.004527, 0.003016, 0.006422]
    [0.102279, 0.072822, 0.480951, 0.17557]
    [0.003309, 0.00212, 0.018836, 0.00509]
    [0.056237, 0.039424, 0.498501, 0.098564]
    [0.002047, 0.001247, 0.0174, 0.002846]
    [0.037849, 0.026466, 0.505373, 0.066385]
    

Функция построения таблицы распределения Пуассона


```python
def PoissonNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_tr_list]
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

    [10.0015, 9.8715, 10.02, 9.955333]
    [1.057168, 1.450238, 4.8391, 1.783283]
    [10.000775, 9.865, 10.03275, 9.977077]
    [0.576562, 0.828275, 4.919052, 0.987505]
    [10.000717, 9.909667, 10.0535, 9.986116]
    [0.387734, 0.556507, 4.948054, 0.665405]
    

Функция построения таблицы равномерного распределения


```python
def UniformNumbers():
    mean_list, med_list, z_R_list, z_Q_list, z_tr_list = [], [], [], [], []
    all_list = [mean_list, med_list, z_R_list, z_tr_list]
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

    [-0.008948, -0.010579, -0.029984, -0.01652]
    [0.103561, 0.24168, 0.519546, 0.178222]
    [-0.006946, -0.009481, -0.018547, -0.013208]
    [0.056835, 0.134673, 0.521238, 0.098918]
    [-0.004379, -0.006288, -0.011447, -0.008637]
    [0.038249, 0.09087, 0.510967, 0.066629]
    
