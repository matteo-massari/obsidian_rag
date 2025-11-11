# Esercizi [[Algoritmi di ricerca]]
Modificare l'algoritmo di ricerca binaria dato per cercare in una lista di tuple ordinata l una tupla che inizi con l'intero k e la restituisce. A tale scopo modificare l'algoritmo di ricerca binaria dato. Se non esiste una tupla che rispetta tale condizione viene restituito False
```python
def binary_search(l, k):

    low=0
    high = len(l)-1

    while low <= high:
        middle = (low+high) // 2
        if l[middle][0]==k:
            return l[middle]
        if k < l[middle][0]:
            high = middle - 1
        else:
            low = middle + 1

    return False

```
---

Modificare l'algoritmo di ricerca binaria per cercare l'indice di una stringa in una lista ordinata di stringhe. Se la stringa non è nella lista viene restituito -1.
``` python
def binary_search(l, k):

    low=0
    high = len(l)-1
    
    while low <= high:
        middle = (low+high) // 2
        if l[middle]==k:
            return middle
        if k < l[middle]:
            high = middle - 1
        else:
            low = middle + 1

    return -1
```
---

Modificare l'algoritmo di ricerca binaria per cercare l'indice un intero in una lista ordinata. Se l'intero è presente allora viene restituito il suo indice, se non è presente viene restituito l'indice del più piccolo intero presente che sia maggiore di quello cercato. Se la lista non contiene elementi maggiori di quello cercato allora la funzione restituisce -1.


```python
def binary_search(l, k):

    low=0
    high = len(l)-1
    result = -1
    while low <= high:
        middle = (low+high) // 2
        if l[middle]==k:
            return middle
        if k < l[middle]:
            high = middle - 1
            result = middle
        else:
            low = middle + 1

    return result
```

---
Definire una funzione **inRange(a, b, l)** che prende in input due stringhe, a e b, ed una lista ordinata di stringhe l. La funzione restituisce la sottolista di l composta dalle sole stringhe s tali che a<=s<=b. Sfruttare la funzione di ricerca binaria data.

``` python
def inRange(a, b, l):
    lista = []
    for s in l:
        if binary_search(l, s) == True and a <= s <= b:
            lista.append(s)
    return lista
```

