# Esercizi [[Algoritmi di ordinamento]]

Dato il codice del bubble sort ottimizzato e della funzione swap, modificarli in modo che la funzione bubble_sort restituisca il numero di iterazioni effettuate.
```python
def swap(l, i, j):
    temp = l[i]
    l[i] = l[j]
    l[j] = temp


def bubble_sort(l):
    count = 0
    for i in reversed(range(len(l))):
        count+=1
        swap_done = False
        for j in range(0, i):
            if l[j] > l[j + 1]:
                swap(l, j, j + 1)
                swap_done = True
        if swap_done == False:
            break
    return count
                
```
---
Dato il codice del bubble sort e della funzione swap, modificarli in modo che la funzione bubble_sort restituisca il numero di scambi effettuati.

```python
def swap(l, i, j):
    temp = l[i]
    l[i] = l[j]
    l[j] = temp


def bubble_sort(l):
    count = 0
    for i in reversed(range(len(l))):
        for j in range(0, i):
            if l[j] > l[j + 1]:
                swap(l, j, j + 1)
                count += 1
    return count
```
---
Date le funzioni quick_sort e swap, scrivere una funzione **sortListofLists(l)** che prende in input una lista di liste e la ordina. Una lista di liste è ordinata quando ogni lista è ordinata e le liste sono ordinate tra loro secondo un ordinamento lessicografico.

```python
def sortListofLists(l):
    for list in l:
        quick_sort(list)
    quick_sort(l)
```
---
Dato il codice del selection sort modificalo per ordinare una lista di interi primariamente in base alla parità e secondariamente in base all'ordinamento naturale (ossia prima tutti in numeri pari ordinati in maniera crescente e poi tutti i dispari ordinati in maniere crescente)

```python
def swap(l, i, j):
    temp = l[i]
    l[i] = l[j]
    l[j] = temp


def selection_sort(l):
    for i in range(len(l)):
        min_index = i 
        for j in range(i + 1, len(l)):
            if l[min_index] % 2 > l[j] % 2 or l[min_index] % 2 == l[j] % 2 and l[min_index] > l[j]:
                min_index = j
        swap(l, i, min_index)
```
