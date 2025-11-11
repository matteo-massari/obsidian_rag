# Esercizi [[Esercizi programmazione iterata agli oggetti]]
Definire una classe Punto che ha due attributi chiamati x e y che rappresentano le sue coordinate sul piano cartesiano. La classe ha anche un metodo dist(self, p) che calcola e restituisce la distanza euclidea tra il punto stesso e un secondo punto passato come parametro. Utilizzate math.sqrt per calcolare la radice quadrata.


```python
class Punto: 
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def dist(self, p):
        return ((self.x - p.x) ** 2 + (self.y - p.y) ** 2) ** 0.5
       
```

Definire una classe Rettangolo che ha due attributi di tipo Punto bottom_left e top_right che rappresentano rispettivamente il punto in basso a sinistra e in alto a destra di un rettangolo i cui lati sono paralleli agli assi cartesiani. Definire anche un metodo della classe isQuadrato che restituisce True se il rettangolo è un quadrato e False altrimenti. Infine definire un metodo contains che prende in input un punto e restituisce True se il punto è all'interno del rettangolo (anche sul perimetro è considerato interno).

Sfruttare la classe Punto definita nell'esercizio precedente copia-incollando il codice


```python
class Punto: 
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def dist(self, p):
        return ((self.x - p.x) ** 2 + (self.y - p.y) ** 2) ** 0.5
        

class Rettangolo:
    def __init__(self, bottom_left, bottom_right):
        self.bottom_left = bottom_left
        self.top_right = bottom_right
    
    def isQuadrato(self):
        b = self.bottom_left.x - self.top_right.x
        h = self.bottom_left.y - self.top_right.y
        return b == h
    
    def contains(self,p):
        counter = True
        if  self.bottom_left.x <= p.x <= self.top_right.x and self.bottom_left.y <= p.y <= self.top_right.y:
            return counter
        else:
            counter = False
            return counter
```

Modificare il codice della ricerca binaria per cercare un punto in una lista di punti ordinata primariamente in base alla x e secondariamente in base alla y. Utilizzare e modificare il codice scritto in precedenza per risolvere l'esercizio

```python
class Punto: 
    def __init__(self, x, y):
        self.x = x
        self.y = y


def binary_search(l, k):

    low=0
    high = len(l)-1

    while low <= high:
        middle = (low+high) // 2
        
        if l[middle].x == k.x:
            
            while low <= high:
                middle = (low+high) // 2
        
                if l[middle].y == k.y:
                    return True
        
                if k.y < l[middle].y:
                    high = middle - 1
        
                else:
                    low = middle + 1
        
        if k.x < l[middle].x:
            high = middle - 1
        
        else:
            low = middle + 1

    return False
```

Modificare l'algoritmo di selection sort dato per ordinare in una lista di punti in maniera crescente primariamente in base alle ascisse e secondariamente in base alle ordinate. Sfruttare la classe punto scritta in precedenza e aggiungergli la funzione __ repr __ che trovate definita (questa funzione viene chiamata quando si stampa l'oggetto con print)

```python
class Punto: 
    def __init__(self, x, y):
        self.x = x
        self.y = y
    def __repr__(self):
        return f"Punto({self.x}, {self.y})"


def swap(l, i, j):
    temp = l[i]
    l[i] = l[j]
    l[j] = temp


def selection_sort(l):
    for i in range(len(l)):
        min_index = i  # index of the 1-st element of the unordered sublist
        for j in range(i + 1, len(l)):
            if l[min_index].x > l[j].x or l[min_index].x > l[j].y :
                min_index = j
            
            
            
        swap(l, i, min_index)



# NON MODIFICARE QUESTA FUNZIONE        
def test(l):
    selection_sort(l)
    print(l)
```

Definire una funzione rangeQuery(r, l) che prende in input un rettangolo e una lista ordinata di punti e restituisce una lista contenente tutti i punti che sono all'interno del rettangolo. Sfruttare la funzione di ricerca binaria data e il codice scritto in precedenza

```python
class Punto: 
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def dist(self, p):
        return ((self.x - p.x) ** 2 + (self.y - p.y) ** 2) ** 0.5
    
    # Inserire nella classe Punto
    def __repr__(self):
        return f"Punto({self.x}, {self.y})"

class Rettangolo:
    def __init__(self, bottom_left, bottom_right):
        self.bottom_left = bottom_left
        self.top_right = bottom_right
    
    def isQuadrato(self):
        b = self.bottom_left.x - self.top_right.x
        h = self.bottom_left.y - self.top_right.y
        return b == h
    
    def contains(self,p):
        counter = True
        if  self.bottom_left.x <= p.x <= self.top_right.x and self.bottom_left.y <= p.y <= self.top_right.y:
            return counter
        else:
            counter = False
            return counter

def binary_search(l, k):
    
    low=0
    high = len(l)-1

    while low <= high:
        middle = (low+high) // 2
        if l[middle]==k:
            return True
        if k < l[middle]:
            high = middle - 1
        else:
            low = middle + 1

    return False
    
def rangeQuery(r, l):
    lista = []
    
    for i in l:
        if r.contains(i):
            lista.append(i)
    
    return lista

#oppure 
def rangeQuery(r, l):
    index = binary_search(l, r.bottom_left)
    list = []
    while 0 <= index < len(l) and r.contains(l[index]):
        list.append(l[index])
        index += 1
    return list
```
