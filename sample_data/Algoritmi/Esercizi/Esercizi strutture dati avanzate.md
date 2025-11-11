# Esercizi [[Strutture dati avanzate]]
## Alberi binari 
1) Aggiungere alla classe Node le funzioni **addLeft(v)** e **addRight(v)** che aggiungono il nodo v rispettivamente come figlio sinistro e destro al nodo su cui vengono invocate. Se un figlio in quella posizione fosse già presente lo sostituiscono.
``` python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
    
    def addLeft(self, v):
        self._left = v
    
    def addRight(self, v):
        self._right = v
    
```

2) Implementare nella classe Node un metodo **onlyLeft()** che restituisce True se tutti i nodi dell'albero binario radicato nel nodo su cui si chiama la funzione hanno solamente il figlio sinistro o non hanno nessun figlio.
``` python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
        
    def onlyLeft(self):
        if self._left != None and self._right == None:
            return self._left.onlyLeft() and True
        if self._left == None and self._right != None:
            return True
        if self._right == None and self._right == None:
            return True
        if self._right != None and self._right != None:
            return False
    
```

3) Modificare le funzioni **addLeft(v)** e **addRight(v)** scritte in precedenza di modo che se il figlio fosse già presente, il nuovo nodo viene aggiunto come prima foglia a sinistra o ultima foglia a destra, rispettivamente.
	Suggerimenti: è simile ad inserire un nodo nella prima o ultima posizione di un albero binario di ricerca

``` python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
        
    def addLeft(self, v):
        if self._left == None:
            self._left = v
        else:
            self._left.addLeft(v)
    
    def addRight(self, v):
        if self._right == None:
            self._right = v
        else:
            self._right.addRight(v)
```

4) Implementare nella classe Node la funzione **search(n)** che cerca nell'albero binario un nodo il cui campo _data sia uguale a n e di conseguenza restituisce True o False
``` python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
        
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
    
    def search(self, n):
        if self._data == n:
            return True
        else:
            found = False
            if self._left != None:
                found = self._left.search(n)
            if self._right != None and found == False:
                found = self._right.search(n)
            return found

#Soluzione mia
# 1) Prima genero una lista con unalgoritmo di scorrimento
# 2) poi faccio un prgramma search seplicissimo usando la funzione di ricerca built-in di python
	def scorrimento(self):
        result = [] 
        if self._left:
            result += self._left.scorrimento()
        result.append(self._data)
        if self._right:
            result += self._right.scorrimento()
        return result
        
    def search(self, n):
        lista = self.scorrimento()
        
        if n in lista:
            return True
        return False
```

5) Definire nella classe Node una funzione **countNodes()** che calcola e restituisce il numero di nodi nel sottoalbero radicato al nodo su cui la funzione viene invocata

```python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
        
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
        
    def countNodes(self):
        count = 1
        if self._left != None:
            count += self._left.countNodes()
        if self._right != None:
            count += self._right.countNodes()
        return count
```

6) Definire nella classe Node una funzione **isPath()** che verifica se l'albero è un cammino. Un cammino è definito come un grafo in cui tutti i nodi hanno grado (numero di vicini) uguale a 2 eccetto per due nodi che hanno grado 1 (gli estremi del cammino). Essendo l'albero binario radicato dovete verificare che tutti i nodi, eccetto una foglia, abbiano un solo figlio. Un solo nodo è considerato un cammino.

```python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
        
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
        
    def isPath(self):
        count = False #imposto il contatore
        # questi due if mi indicano la reiterazione
        if self._left == None and self._right != None:
            count = self._right.isPath()
        if self._right == None and self._left != None:
            count = self._left.isPath()
        
        # quando arrivo al punto limite verifico se ci sta una diramamzione 
        if self._right == None and self._left == None:
            count = True
        if self._right != None and self._left != None:
            count = False
        return count
```
Definire nella classe Node una funzione **depth()** che calcola la profondità dell'albero radicato nel nodo su cui viene invocata. La profondità di un albero è definita come la profondità massima tra i suoi nodi. La radice ha profondità 0.

```python
class Node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right
        
    def __str__(self):
        s = "(" + str(self._data) + ", "
        if not self._left == None:
            s += self._left.__str__()
        s += ", "
        if not self._right == None:
            s += self._right.__str__()
        return s + ")"
    
    def depth(self):
        count_left = 0
        count_right = 0
        if self._left != None:
            count_left = self._left.depth() + 1
        if self._right != None:
            count_right = self._right.depth() + 1
        return max(count_left,count_right)
```

### Punto di disambiguazione tra la teoria e gli esercizi

Gli alberi binari, anche se possono essere trattati con la stessa implementazione degli alberi n-ari, sono trattati con la classe node (o chiamata anche treeNode).
Dato che negli appunti manca un'algoritmo di scorrimento per la classe node:

>[!hint] Può essere adattato in base all'esercizio

```python
class node:
    def __init__(self, data=None, left=None, right=None):
        self._data = data
        self._left = left
        self._right = right

    def scorrimento(self):
        # Funzione per l'attraversamento in ordine dell'albero binario
        result = [] #raccoglie i nodi nell'ordine corretto
        if self._left:
            result += self._left.scorrimento()
        result.append(self._data)
        if self._right:
            result += self._right.scorrimento()
        return result
```


## Alberi n-ari

1) Definire nella classe Tree una funzione **gradoMassimo()** che dato un albero (tramite la sua radice) restituisce il grado del nodo con grado maggiore all'interno dell'albero

>[!info] il grado di un nodo è il numero di archi che incidono su quel nodo. Per convenzione negli alberi si omette l'arco verso il genitore, quindi il grado è pari al numero di nodi figli.

```python
def gradoMassimo(self):
        max = len(self.get_children())
        for child in self.get_children():
            grado = child.gradoMassimo()
            if grado > max:
                max = grado
        return max
```

2) Definire nella classe Tree una funzione **somma()** che dato un albero (tramite la sua radice) restituisce la somma dei valori contenuti nei nodi.
``` python
def somma(self):
	tot = self.get_data()
    for child in self.get_children():
        tot += child.somma()
    return tot
```

3) Definire nella classe Node una funzione **contaValori(k)** che calcola il numero di nodi il cui campo _ data contiene il valore k

```python
def contaValori(self, k):
	count = 0
	if self.get_data() == k:
		count = 1
	for child in self.get_children():
		count += child.contaValori(k)
	return count
```

4) Definire nella classe Tree una funzione **valoreAggregato()** che modifica l'albero in modo che in ogni nodo sia contenuto il suo valore originale più la somma dei valori nel sotto albero originale.

```python
def valoreAggregato(self):
	for child in self.get_children():
		self.set_data(self.get_data()+child.valoreAggregato())
	return self.get_data()

def valoreAggregato(self):
        valore_aggregato = self.get_data()
        for child in self.get_children():
            valore_aggregato += child.valoreAggregato()
            self.set_data(valore_aggregato)
        return self.get_data()
```

## Grafi
1) Definire nella classe Grafo una funzione **degree(v)** che dato l'id (intero) di un vertice v restituisce una tupla di due elementi, il cui primo valore è il numero di archi entranti e il secondo è il numero di archi uscenti dal vertice v.

> [!warning] Il grafo è un grafo diretto definito tramite una matrice di adiacenza

```python
class Grafo:
    def __init__(self, matrix=[[]]):
        self.matrix = matrix
        self.V = len(matrix)
    
    def degree(self, v):
        indeg = 0
        outdeg = 0
        for riga in self.matrix:
            indeg += riga[v]
        for value in self.matrix[v]:
            outdeg += value
        return (indeg, outdeg)

    def degree(self, v):
    #v essendo il nodo di interesse, viene utilizzato per indicizzare la matrice di adiacenza
    # per trovare gli archi entranti logicamente faccio:
    # 1) tengo fermo le colonne al nodo che voglio analizzare v
    # 2) faccio scorrere solo i che mi mi contera tutte le combinazioni tra v e i dandomi la somma degli archi entranti 
        entry = sum(self.matrix[i][v] for i in range(self.V) )

		# stesso ragionaemnto per gli archi uscenti
        exit = sum(self.matrix[v][j] for j in range(self.V) )
        return (entry, exit)	
```

2) Definire nella classe Grafo una funzione **isSimple()** che verifica se il grafo è semplice.
>[!info] 
>Il grafo è un grafo indiretto definito tramite matrice di adiacenza.
>DEFINIZIONE: Un grafo si dice semplice se non contiene **archi multipli** (più di un arco tra un coppia di vertici) e **loop** (archi che partono da un vertice e tornano sullo stesso vertice).


```python
class Grafo:
    def __init__(self, matrix=[[]]):
        self.matrix = matrix
        self.V = len(matrix)
    
    def isSimple(self):
        simple = True
        for i in range(len(self.matrix)):
            for j in range(len(self.matrix[0])):
                if self.matrix[i][j] > 1 or i == j and self.matrix[i][j] > 0:
                    simple = False
        return simple
```

3) Definire nella classe Grafo una funzione **esiste(v, w)** che dati gli id (interi) dei vertici v e w verifica se esiste l'arco (v, w)
>[!info] Il grafo è un grafo indiretto gestito tramite liste di adiacenza
>

```python
class Grafo:
    def __init__(self, list=[[]]):
        self.list = list
        self.V = len(list)
        
    def esiste(self, v, w):
        for i in self.list[v]:
            if i == w:
                return True
        return False
```

4) Definire nella classe Grafo una funzione **undirected()** che verifica se il grafo, gestito tramite liste di adiacenza, sia effettivamente un grafo indiretto, ossia che per ogni arco (v, w) sia presente nelle liste l'arco (w, v)
```python
class Grafo:
    def __init__(self, list=[[]]):
        self.list = list
        self.V = len(list)
    
    def esiste(self, v, w):
        for i in self.list[v]:
            if i == w:
                return True
        return False
    
    def undirected(self):
        for i in range(len(self.list)):
            for j in self.list[i]:
                if not self.esiste(j, i):
                    return False
        return True

    def undirected(self):
        for v in range(self.V): #arco di partenza
            for w in self.list[v]: #arco di arrivo
                # Verifica se l'arco inverso (w, v) non esiste
                if not self.esiste(w, v):
                    return False
        return True

```

### Djikstra 

> [!warning] Attenzione
> Aggiungere **import numpy as np** prima di ogni codice

5) Definire nella classe Grafo una funzione **minPath(a, b)** che, sfruttando l'algoritmo di Dijkstra, restituisce il cammino minimo dal vertice **a** al vertice **b** come una lista di vertici

Questo algoritmo ci restituisce il percorso vero e proprio:  
```python
class Grafo:
    def minPath(self, a, b):
        path = []
        previous_nodes = dijkstra(self.matrix, a)[1] #contrariamente da quello spiegato il strutture e dati avanzate, qua non mettiamo in input previous_node ma lo ricaviamo chiamando direttamente dijkstra
        while b != a:
            path.insert(0, b)
            b = previous_nodes[b]
        path.insert(0, a)
        return path
```

6) Definire nella classe Grafo una funzione **all_pairs_shortest_paths()** che, sfruttando l'algoritmo di Dijkstra, restituisce una matrice (implementata come lista di liste) tale che all'elemento i, j sia associato il valore del cammino minimo tra i nodi i e j

```python
def all_pairs_shortest_paths(self):
	apsp = []
	for i in range(self.V):
		apsp.append([0]*self.V)
	for i in range(self.V):
		shortest_paths = dijkstra(self.matrix, i)[0]
		for j in range(i+1, self.V):
			apsp[i][j] = shortest_paths[j]
			apsp[j][i] = shortest_paths[j]
	return apsp
```

7) Definire nella classe Grafo una funzione **biconnesso(a, b)** che dati due vertici verifica se esistono due cammini distinti tra essi e restituisce un booleano.
	Sfruttare l'algoritmo di Dijkstra per risolvere il problema, in particolare path_weights[ j ] calcolato a partire dal vertice i varrà **np.inf** (valore infinito della libreria numpy) se non esiste un cammino tra i vertici i e j


```python
def biconnesso(self, a, b):
        # Per prima cosa cerco il cammino minimo tra a e b
        (path_weights, previous_nodes) = dijkstra(self.matrix, a)
        
        # Se non c'è restituisco False
        if path_weights[b] == np.inf:
            return False
        
        # Trovo quali sono i nodi del cammino minimo tra a e b diversi da a e b
        path = []
        node = previous_nodes[b]
        while node != a:
            path.insert(0, node)
            node = previous_nodes[node]
        
        # Rimuovo questi nodi dal grafo in modo che il nuovo cammino minimo non passi da questi
        # Attenzione: se il path è vuoto vuol dire che c'è un arco tra a e b e devo rimuoverlo
        for node in path:
            for i in range(self.V):
                self.matrix[i][node] = 0
                self.matrix[node][i] = 0
        if path == []:
            self.matrix[a][b]=0
            self.matrix[b][a]=0
        
        # Cerco nuovamente il cammino minimo tra a e b
        path_weights = dijkstra(self.matrix, a)[0]
        
        # Se non c'è restituisco False, altrimenti restituisco True
        if path_weights[b] == np.inf:
            return False
        else:
            return True
```
