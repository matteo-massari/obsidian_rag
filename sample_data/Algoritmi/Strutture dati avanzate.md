# Strutture dati avanzate
## Alberi
Un albero è una struttura dati gerarchica che consiste in un insieme di nodi collegati tra loro da archi direzionati. 
Un nodo è un contenitore di dati, che ha relazioni di parentela con gli altri nodi:
- **nodo radice**: Il nodo in cima all'albero
- **nodi genitori e nodi figli**: 
- **nodi ascendenti e nodi discendenti**

### Definizioni 
|                                                                                                                     |                                                                                      |
| ------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| **Cammino**: una sequenza di nodi tali che ogni nodo della sequenza, eccetto l’ultimo, è il genitore del successivo | **Il grado** di un nodo: è pari al numero di nodi figli.                             |
|                                                                                                                     |                                                                                      |
| **Lunghezza**: di un cammino è data dal numero di archi che collegano i nodi del cammino                            | **L’altezza dell’albero** è la lunghezza del cammino più lungo presente nell’albero. |
|                                                                                                                     |                                                                                      |
| **Il livello** di un nodo corrisponde alla lunghezza del cammino dal nodo radice a quel nodo (la radice è 0).       | **Profondità** di un nodo: lunghezza del cammino dal nodo radice a se stesso         |
|                                                                                                                     |                                                                                      |
![[nomenclatura alberi 1.png]]
![[Nomenclatura alberi 2.png]]
![[nomenclatura alberi 3.png]]

### Operazioni
- **add_child(node, data)**: crea un nuovo nodo che contiene i dati data, lo aggiunge come nodo figlio al nodo node e restituisce un riferimento al nuovo nodo
- **remove_child(node, child_node)**: elimina il nodo child_node dall’insieme dei nodi figli del nodo node
- **get_children(node)**: restituisce la lista dei nodi figli del nodo node, restituisce una lista vuota se il nodo node non ha figli
- **set_data(node, data)**: inserisce i dati data come contenuto del nodo node
- **get_data(node)**: restituisce i dati contenuti nel nodo node, restituisce null se il nodo non contiene dati

### Implementazioni 
``` python
import queue_with_deque

class ThreeNode:

    def __init__(self, data=None):
	    self._data = data   # rappresenta il dato che desideri memorizzare nel nodo.
        self._children = [] # è una lista che viene utilizzata per memorizzare i nodi figli del nodo corrente

    def add_child(self, data=None):
        new_child = ThreeNode(data)
        self._children.append(new_child)
        return new_child
#crea un nuovo nodo che contiene i dati data, lo aggiunge come nodo figlio al nodo node e restituisce un riferimento al nuovo nodo

    def remove_child(self, child_node):
        for i in range(len(self._children)):
            if self._children[i] is child_node:
                del(self._children[i])
                break
#elimina il nodo child_node dall’insieme dei nodi figli del nodo node

    def get_children(self):
        return self._children
#restituisce la lista dei nodi figli del nodo node, restituisce una lista vuota se il nodo node non ha figli, si usa per iterare.
#L'output che esce, è sono un riferimento agli oggetti

	def get_children_data(self): 
		return [child.get_data() for child in self._children]
# è un metodo che restituisce il dato del nodo
    
    def get_data(self):
        return self._data
#restituisce i dati data come contenuto del nodo node,  restituisce null se il nodo non contiene dati.
    
    def set_data(self, data):
        self._data = data
#inserisce i dati contenuti nel nodo node
            

```

### Algoritmi di scorrimento alberi n-ari
La lettura dei nodi di un albero o la ricerca di un nodo in un albero, vista la struttura non lineare degli alberi, può essere effettuata seguendo differenti ordini di accesso ai nodi. Vi sono due principali strategie di accesso ai nodi che li visitano in ordine differente:

Vi sono due principali strategie di accesso ai nodi che li visitano in ordine differente:
- **Depth-First Search** (ricerca in profondità) con due principali varianti:
	- **preordine**
	- **postordine**  
- **Breadth-First Search** (ricerca in ampiezza)

#### Depth-First Search (ricerca in profondità)
##### **Preordine**
I nodi sono visitati accedendo ai dati di un nodo, poi ai figli del nodo, e successivamente agli altri nodi dello stesso livello.
###### Implementazione python
```python
def print_dfs_preorder(self): 
	# comando da applicare al nodo in questione
	#qua posso anche metterci un'assegnazione, in quanto itera sul nodo corrente 
	print(self.get_data()) 

# Mi permette di iterare sui figli, (dato che la ricorsione mi fa ripartire la funzione,) ogni volta che la richiamo su una dei figli, tale figlio diventa il nodo corrente (a cui applico get_data)
	for child in self.get_children():  
	# La successiva iterazione for itera sui suoi figli
	#get_children ci restituisce la lista dei figli da destra verso sinistra 
		child.print_dfs_preorder() 
```

##### Postordine
I nodi sono visitati accedendo prima ai figli del nodo, poi ai dati del nodo, e dopo agli altri nodi dello stesso livello
###### Implementazione python
```python
def print_dfs_postorder(self): 
	for child in self.get_children():
		child.print_dfs_postorder() 
	print(self.get_data())
```

#### **Breadth-First Search** (ricerca in ampiezza)
I nodi sono visitati in ordine di livello e da sinistra verso destra, quindi prima la radice, poi tutti i nodi di livello 1 da sinistra e destra, poi tutti i nodi di livello 2 fino alla fine della lista
##### Implementazione python
```python
def print_bfs(self):  
	nodes = queue_with_deque.Queue() 
	node = self  
	while node is not None:
		print(node.get_data())  
	for child in node.get_children():
		nodes.enqueue(child) 
	node = nodes.dequeue() #Utilizza una coda per inserire i nodi secondo l’ordine in cui devono essere visitati
```

![[Albero.png|inlR]]

| Preordine | Postordine | Ricerca in ampiezza |
| --------- | ---------- | ------------------ |
| 1. a      | 1. e       | 1. a               |
| 2. b      | 2. f       | 2. b               |
| 3. e      | 3. b       | 5. c               |
| 4. f      | 4. g       | 8. d               |
| 5. c      | 5. h       | 3. e               |
| 6. g      | 6. c       | 9. f              |
| 7. h      | 7. i       | 10. g              |
| 8. d      | 8. l       | 4. h              |
| 9. i      | 9. d       | 6. i               |
| 10. l     | 10. a      | 7. l               |


Ecco una breve guida in formato Markdown che spiega come funziona la dicitura per rappresentare alberi binari:

### Notazione di Base di Alberi Binari

La notazione di base consiste nel rappresentare un albero binario con parentesi tonde, dove ogni nodo è racchiuso tra parentesi tonde e separato dai suoi figli da virgole. Ecco un esempio:

```
(1, (2, , ), (3, (4, , ), (5, , )))
```

In questo esempio:
- `1` è la radice dell'albero.
- `2` è un figlio sinistro di `1`, mentre `3` è un figlio destro di `1`.
- `4` è un figlio sinistro di `3`, mentre `5` è un figlio destro di `3`.

Le virgole vuote rappresentano nodi senza figli.

#### Albero Semplice
Un albero binario con un solo nodo può essere rappresentato come:

```
(1, , )
```
```
  1
```

#### Albero Lineare
Un albero binario lineare, dove ogni nodo ha un solo figlio, può essere rappresentato come:

```
(1, (2, (3, (4, , ), ), ), )
```
```
    1
     \
      2
       \
        3
         \
          4
```

#### Albero Bilanciato

```
(1, (2, (3, , ), (4, , )), (5, (6, , ), (7, , )))
```
```
     1
   /   \
  2     5
 / \   / \
3   4  6  7
```

#### Albero Sbilanciato

```
(1, (2, (3, , ), (4, , )), (5, (6, , (7, , 8)), ))
```
```
    1
   / \
  2   5
 / \   \
3   4   6
         \
          7
           \
            8
```

---
## Alberi binari di ricerca 
### Definizione
Un albero è binario se ogni nodo ha al più due figli (figlio destro e sinistro)
Inoltre può ulteriormente essere definito pieno se ogni nodo ha due figli oppure è una foglia (che è l’unico nodo a non avere né figlio destro, né sinistro)

Un **albero binario di ricerca** ha le seguenti caratteristiche:
- Ogni nodo contiene un elemento di un insieme su cui è definita una relazione d’ordine (come ≤)
- Per ogni nodo x dell’albero (indicando con x.data l’elemento contenuto nel nodo x), si ha:
	- Se y è un nodo del sottoalbero sinistro: y.data ≤ x.data
	- Se y è un nodo del sottoalbero destro : x.data ≤ y.data

![[BTS.png|350]]
### Operazioni 
- **Inserimento di un nodo (add_node)**: Aggiungi un nuovo nodo all'albero binario di ricerca seguendo le regole dell'albero binario
- **Ricerca di un nodo (search_node)**: Trova un nodo con un dato specifico nell'albero binario di ricerca.
- **Rimozione di un figlio (remove_child)**: Rimuovi un nodo specifico dalla lista dei nodi figli del nodo genitore. Questa operazione richiede di aggiornare correttamente i collegamenti dei nodi rimanenti.
- **Ottenere il figlio sinistro (get_left_child)**: Restituisci il riferimento al nodo figlio sinistro del nodo corrente.
- **Ottenere il figlio destro (get_right_child)**: Restituisci il riferimento al nodo figlio destro del nodo corrente.
- **Ottenere i dati del nodo (get_data)**: Restituisci i dati contenuti nel nodo corrente.
- **Impostare i dati del nodo (set_data)**: Aggiorna i dati contenuti nel nodo corrente con nuovi dati forniti come parametro.
### Implementazione
L'implementazione avviene all'interno di una classe
#### Costruttore
- Un nodo di un albero binario può essere realizzato sostituendo la lista children[ ] con due variabili, chiamiamole left e right, che contengono i riferimenti al figlio sinistro e destro del nodo. 

```python
class BSTNode:

    def __init__(self, data=None, left=None, right=None):
        self._data = data   # memorizza il dato associato al nodo corrente
        self._left = left   # riferimento al figlio sinistro
        self._right = right # riferimento al filgio destro
```

#### Inserimento dei nodi
- Negli alberi binari di ricerca i nodi devono essere inseriti rispettando le relazioni di ordinamento tra i nodi dell’albero.
![[BTS add_node.png| 200]]

```python
def add_node(self, data): #self 
    current_node = self
    while True: #ad ogni esecuzione del ciclo esplora un nodo di livello più basso 
	    if data <= current_node._data:
	        if current_node._left is None:
		        current_node._left= BSTNode(data)
			        return current_node._left
		    else:
	            current_node = current_node._left
		else:
			if current_node._right is None:
	            current_node._right= BSTNode(data)
                return current_node._right
            else:
                current_node = current_node._right
```

- **Ciclo while**: ad ogni esecuzione del ciclo esplora un nodo di livello più basso
- **1° if**: cerca il punto in cui inserire il nodo nel sottoalbero di sinistra
- **2° if**: cerca il punto in cui inserire il nodo nel sottoalbero di destra
##### Funzionamento dettagliato
1. Nel metodo **add_node** il primo parametro **self** indica che è un metodo di istanza della classe, il secondo parametro **data** rappresenta il valore del nodo che si desidera aggiungere all'albero.

2. Il primo passo nel metodo è assegnare **self** alla variabile **current_node**. Questo suggerisce che **self** rappresenta il nodo radice dell'albero o il nodo corrente in cui si sta valutando la posizione in cui inserire il nuovo nodo.

3. Successivamente, viene avviato un ciclo **while True**, che continuerà fino a quando non si verifica una delle condizioni di terminazione.

4. All'interno del ciclo, viene effettuato un controllo per determinare se il valore del nodo da inserire (**data**) è minore o uguale al valore del nodo corrente (**current_node._data**).

5. Se il valore è minore o uguale, si procede con la valutazione del sottoalbero sinistro.

6. Se il sottoalbero sinistro (**curren t _ node. _ left**) è **None**, significa che non c'è un nodo a sinistra e quindi si può creare un nuovo nodo con il valore **data** e assegnarlo a **current _ node. _ left**. Successivamente, si restituisce **current _node. _ left**, ovvero il nuovo nodo creato.

7. Se invece il sottoalbero sinistro non è **None**, significa che esiste già un nodo e quindi si imposta **current_node** sul nodo sinistro (**current_node = current_ node. _ left**) e si continua con la valutazione del successivo nodo.

8. Se il valore del nodo da inserire (**data**) è maggiore del valore del nodo corrente (**current _ node. _ data**), si passa alla valutazione del sottoalbero destro.

9. Se il sottoalbero destro (**current _ node. _ right**) è **None**, significa che non c'è un nodo a destra e quindi si crea un nuovo nodo con il valore **data** e lo si assegna a **current _ node. _ right**. Successivamente, si restituisce **current _ node. _ right**, ovvero il nuovo nodo creato.

10. Se il sottoalbero destro non è **None**, significa che esiste già un nodo e quindi si imposta **current_node** sul nodo destro (**current_node = current_node._ right**) e si continua con la valutazione del successivo nodo

#### Ricerca di nodi
Gli alberi binari consentono di sfruttare la ricerca binaria per cercare nodi nell'albero sulla base dei dati che contengono.  
```python
def search_node(self, data_to_search):
    if self._data == data_to_search:
        return self
    if data_to_search < self._data:
        if  self._left is None:
            return None
        return self._left.search_node(data_to_search)
    else:
        if  self._right is None:
            return None
        return self._right.search_node(data_to_search)
```

Ricordando le caratteristiche degli alberi binari di ricerca, possiamo adottare un approccio di ricerca ricorsiva. Considerando la disposizione dei dati in un albero binario di ricerca, l'algoritmo sfrutta la proprietà che i nodi con valori minori si collocano nel sottoalbero sinistro, mentre quelli con valori maggiori nel sottoalbero destro.
Il costo della ricerca è dove **O(h)** è l’altezza dell’albero
##### Funzionamento dettagliato
1. Nel metodo **search_node** il primo parametro **self** indica che è un metodo di istanza della classe, il secondo parametro **data_to_search** rappresenta il valore del nodo che si desidera aggiungere all'albero. 

2. Il primo `if` verifica se il dato del nodo corrente `self._data` è uguale al dato che stiamo cercando `data_to_search`. Se sono uguali, ciò indica che abbiamo individuato il nodo corrispondente e restituiamo l'oggetto nodo corrente utilizzando `return self`.

3. Il secondo if controlla se  il dato che stiamo cercando è minore del dato del nodo corrente, in tal caso, la ricerca si sposta verso il sottoalbero sinistro .

4. il terzo if annidato nel secondo controlla se il sottoalbero sinistro (**left**) esiste. Se non esiste, significa che il dato che stiamo cercando non è presente nell'albero, quindi restituiamo **None**.

5. Se il sottoalbero sinistro esiste, la funzione `return self._left.search_node(data_to_search)` richiama la funzione `search_node` in modo ricorsivo sul sottoalbero sinistro `self._left`, al fine di continuare la ricerca nel ramo sinistro dell'albero.

6. L'alternativa al secondo if è il primo else,  il quale se il dato che stiamo cercato è maggiore o uguale al dato del nodo corrente, ci spostiamo verso il sottoalbero destro.

7. Allo stesso modo nel quarto if (annidato nel else) controlla se il sottoalbero destro (`_right`) esiste. Se non esiste, significa ancora una volta che il dato che stiamo cercando non è presente nell'albero, quindi restituiamo `None`.

8.  Se il sottoalbero destro esiste, la funzione `return self._right.search_node(data_to_search)`, richiama la funzione `return self._right.search_node(data_to_search)` ricorsivamente sul sottoalbero destro (`self._right`) per continuare la ricerca nel ramo destro dell'albero.

##### Fattore di bilanciamento
Il **fattore di bilanciamento** β di un nodo è la differenza tra l’altezza del sottalbero destro e l’altezza del suo sottalbero sinistro (se un sottoalbero non è presente possiamo considerare come sua altezza -1) 

Un albero si dice bilanciato in altezza se ogni suo nodo ha fattore di bilanciamento in valore assoluto ≤ -1  

| Fattore do bilanciamento | Albero                                      |
| ------------------------ | ------------------------------------------- |
| L'albero ha altezza h=4; β = (3)-(0) = 3, quindi l'albero è sbilanciato | ![[albero sbilanciato.png]] |
| L'albero ha altezza h=4; β = -(4)-(3) = 1, quindi l'albero è bilanciato | ![[Albero bilanciato.png]] |

L'obbiettivo di avere un'albero bilanciato consiste nell'avere una complessità di O(log n)
#### Altre implementazioni

```python
def remove_child(self, child_node):
    if self._left is child_node:
        del self._left
    elif self._right is child_node:
        del self._right
 
def get_left_child(self):
    return self._left

def get_right_child(self):
    return self._right

def get_data(self):
    return self._data
 
def set_data(self, data):
    self._data = data 
```

---
## Grafi 
### Definizione
Un grafo G è una coppia (V, E) dove:
- **V**: è detto insieme dei nodi (o vertici) ed un insieme non vuoto
- **E**: è detto insieme degli archi ed è un un insieme di coppie (x, y) dove x e y sono elementi dell’insieme V
#### Rappresentare un grafo
- Notazione grafica
- Notazione insiemistica
- [[Matrici|Matrice di adiacenza]]:
	- è una matrice nxn, dove n è il numero di nodi del grafo
	- 1 nella posizione (i, j), (con i indice di riga e j indice di colonna) denota la presenza di un arco che va dal nodo i al nodo j 
	- la matrice risulterà simmetrica solo se il grafo non è orientato
- [[Strutture dati|Lista di adiacenza]]
	- Le adiacenze sono rappresentate da un insieme di liste, una per ogni nodo 
	- Ogni lista include i vertici adiacenti ad uno specifico nodo

| Notazione grafica    | Notazione insiemistica         |
| -------------------- | ------------------------------ |
| ![[grafo 1.png]]     | ![[Notazione isiemistica.png]] |
| **Matrice di adiacenza**   |   **Lista di adiacenza**  |
| ![[Schermata 2023-09-01 alle 14.35.55.png]]  |   ![[Schermata 2023-09-01 alle 14.36.32 1.png]]                             |

#### Grafo pesato
Un grafo pesato G è una coppia (V, E) dove:
- V è un insieme non vuoto;  
- E è un un insieme di coppie (x, y) dove x e y sono elementi dell’insieme V
- w è una funzione di peso w:E→R<sup>+</sup> (associa ad ogni arco un peso)
	**Peso di un cammino**: 
	Date un  grafo pesato (G = (V, E)) con una funzione di peso (w : E -> R<sup>+</sup>), si definisce peso di un cammino la somma dei pesi degli archi che collegano la successione dei nodi del cammino

| Notazione grafica    | Notazione insiemistica         |
| -------------------- | ------------------------------ |
| ![[Schermata 2023-09-01 alle 15.16.52.png]]     |![[Schermata 2023-09-01 alle 15.17.00.png]]  |
| **Matrice di adiacenza**   |   **Lista di adiacenza**  |
|  ![[Schermata 2023-09-01 alle 15.17.10.png]] |           ![[Schermata 2023-09-01 alle 15.17.18.png]]                     |

#### Grafo orientato
Un grafo orientato G (o diretto o di grafo) è una coppia (V, E) dove
- **V**: è un insieme non vuoto;  
- **E**: è un un insieme di coppie ordinate (x, y) dove x e y sono elementi dell’insieme V

| Notazione grafica    | Notazione insiemistica         |
| -------------------- | ------------------------------ |
| ![[Schermata 2023-09-01 alle 15.19.37.png]]   | ![[Schermata 2023-09-01 alle 15.21.12.png]] |
| **Matrice di adiacenza**   |   **Lista di adiacenza**  |
|  ![[Schermata 2023-09-01 alle 15.19.44.png]] |            ![[Schermata 2023-09-01 alle 15.19.58.png]]                    |

#### Altre definizioni 
- Un nodo v è detto **adiacente** al nodo u se esiste un arco (u,v)
- Un arco (u,v) è detto **incidente** ai nodi v e u  
- Si definisce **grado di un nodo** il numero di archi incidenti
- Un grafo è detto **completo** se è presente un arco tra ogni coppia di nodi. Se n è il numero di nodi del grafo, Il numero di archi in un grafo completo è n(n-1)/2
- Se in un grafo il numero di archi è dello stesso ordine di grandezza del numero dei nodi allora il grafo è detto **sparso**
- se il numero di archi è elevato rispetto al numero di archi allora è detto **denso**
![[Schermata 2023-09-01 alle 15.24.01.png]]

### Operazioni 
- **add_node(n)**: aggiunge un nuovo nodo al grafo
- **remove_node(n)**: elimina il nodo n dal grafo e tutti gli archi incidenti al nodo n
- **get_nodes()**: restituisce la lista dei nodi del grafo con le liste dei relativi archi adiacenti
- **add_edge(n1, n2, w)**: aggiunge al grafo l’arco (n1, n2) con peso w
- **remove_edge(n1, n2)**: rimuove dal grafo l’arco (n1, n2)
- **adiacent(n)**: restituisce la lista dei nodi adiacenti al nodo n
- **len()**: restituisce il numero di nodi del grafo

### Implementazione
#### Lista di adiacenza con i dizionari  
Tale implementazione utilizza un dizionario principale (self._ nodes) che memorizza i nodi come chiavi. Per ciascuna (chiave), c'è un dizionario associato come valore. Tale dizionario memorizza le liste dei nodi adiacenti a quel nodo.
Quindi ogni chiave all'interno del dizionario associato rappresenta un nodo adiacente, e il valore associato a quella chiave può rappresentare un peso dell'arco che collega il nodo principale a quel nodo adiacente.
```
{'A': {'B': 0, 'C': 2}, 'B': {}, 'C': {}}
```
Ad A è connesso B e C con un arco di peso 0 a B e con un'arco di peso 2 a C, Mentre B e C non sono connessi a niente

```python
class Graph:

	def __init__(self):
		self._nodes = {}

	def add_node(self, n):
		if n not in self._nodes:
			self._nodes[n] = { }
# aggiunge un nuovo nodo al grafo
	
	def remove_node(self, n):
		if n in self._nodes:
			self._nodes.pop(n)
# elimina il nodo n dal grafo e tutti gli archi incidenti al nodo n
	
	def get_nodes(self):
		return self._nodes.keys()
# restituisce la lista dei nodi del grafo con le liste dei relativi archi adiacenti
	
	def add_edge(self, n1, n2, w=0):
		self.add_node(n1)
		self.add_node(n2)
		self._nodes[n1][n2] = w
# aggiunge al grafo l’arco (n1, n2) con peso w
	
	def remove_edge(self, n1, n2):
		if n2 in self._nodes[n1] :
			self._nodes[n1].pop(n2)
# rimuove dal grafo l’arco (n1, n2)
	
	def adjacent(self, n):
		if n in self._nodes:
			return self._nodes[n]
# restituisce la lista dei nodi adiacenti al nodo n
	
	def len(self):
		return len(self._nodes)
# restituisce il numero di nodi del grafo
```

#### Matrice di adiacenza
> [!warning] Gli algoritmi di scorrimento sono gli stessi che possiamo usare per scorrere banalmente una matrice o una lista ma riportarli ci permette di capire come si interfacciano al costruttore

```python
class Grafo:
    def __init__(self, matrix=[[]]): # Inizialmente alla matrice viene asseganta una lista vuota
        self.matrix = matrix #memorizza la matrice di adiacenza che verrà passata quando sarà richiamata una funzione all'interno della classe
        self.V = len(matrix) # calcola il numero di vertici nel grafo e lo memorizza nell'attributo V. len(matrix) restituisce la lunghezza della lista esterna della matrice di adiacenza. Quindi, self.V conterrà il numero di righe (o colonne) nella matrice di adiacenza, che rappresenta il numero di vertici nel grafo.

    def scorrere_matrice_adiacenza(self):
        for riga in range(self.V):
            for colonna in range(self.V):
                peso = self.matrix[riga][colonna]
                if peso != 0:
                    print(f"Arco da Vertice {riga} a Vertice {colonna} con peso {peso}")
```
#### Liste di adiacenza
Ogni vertice ha la propria lista di adiacenza (contenuta in un'insieme di liste di adiacenza), quindi per iterare su quel nodo basta metterlo come indice della lista di liste di adiacenze che vengono fornite al momento degli input

```python
# Lista di liste 
[[1, 2], # Vertice 0 è adiacente a 1 e 2
[0, 2],  # Vertice 1 è adiacente a 0 e 2
[0, 1],  # Vertice 2 è adiacente a 0 e 1
[]]      # Vertice 3 non ha adiacenti

```

```python
class Grafo:
    def __init__(self, list=[[]]):
        self.list = list
        self.V = len(list)

	def scorrere_lista_adiacenza(self): 
		for vertice in range(self.V): 
			adiacenti = self.list[vertice] 
			print(f"Vertice {vertice}: {adiacenti}")

	def scorrere_lista_su_un_nodo:
		for vertice in self.list[v]: #itero solo sulla lista di v
		            if vertice == w:
		                return True
		        return False
```

### Comandi utili nei grafi
#### Archi
Dato un grafo rappresentato da una matrice di adiacenza **v** rappresenta l'indice del vertice per il quale si vuole calcolare: ([[Espressioni Condizionali]])
- il numero di **archi entranti** è dal vettore colonna (i),
```python
entry = sum(self.matrix[i][v] for i in range(self.V))
# In range perchè mi sevrono che i diventa un'indice
```
- il **numero di archi** uscenti è dal vettore riga (j),
```python
exit = sum(self.matrix[v][j] for j in range(self.V))
```
#### Archi multipli 
```python
self.matrix[i][j] > 1
```
#### Loop
```python
i == j and self.matrix[i][j] == 1:
```


### Problema dei cammini di peso minimo di un grafo
Date un grafo pesato (G = (V, E)) con una funzione di peso (w : E -> R<sup>+</sup>), si vuole determinare il cammino semplice di peso minimo che va da un nodo sorgente **s** a un nodo destinazione **d**

**Esempio**
Nel grafo è evidenziato uno dei possibili cammini semplici dal nodo s=1 al nodo d=4, ossia {1, 5, 3, 4}, il cui peso è 3+1+2=6
![[Grafo.png|400]]

Le due possibili soluzioni sono:
-  Forza bruta:
- Algoritmo di Dijkstra
#### [[Cammini e connessioni#Forza bruta |Forza bruta]]
1) Si decide un nodo di sorgente s e un qualsiasi nodo destinazione d
2) crea l'elenco di tutti i possibili percorsi semplici 
3) Per ogni percorso calcola il peso
4) sceglie il cammino con il peso minore
##### Implementazione
```python
def MinWeightPath_ForceBrute(G, s, v):
    min_weight = infinity
    best_path = null

    for each path P from s to v:
        weight = calculateWeight(P)

        if weight < min_weight:
            min_weight = weight
            best_path = P

    return best_path
```

##### Complessità 
Il caso peggiore considerabile è un grafo completo (ogni nodo collegato con qualsiasi altro nodo). 

In un grafo completo con n nodi, è possibile calcolare il numero di cammini tra due nodi distinti che hanno esattamente k nodi interni (con 0 ≤ k ≤ n-2). Questo può essere fatto utilizzando una formula che si basa su osservazioni intuitive:

- Il numero di cammini tra un nodo s e un nodo d senza nodi interni è 1 (costituito dall'unico arco che collega s e d).
- Il numero di cammini tra un nodo s e un nodo d con 1 nodo interno è n-2 (sono i cammini che vanno dal nodo s a tutti gli altri n-2 nodi diversi da d e poi raggiungono il nodo d).
- Il numero di cammini tra un nodo s e un nodo d con 2 nodi interni è (n-2) * (n-3) (sono i cammini che vanno dal nodo s a tutti gli altri n-2 nodi diversi da d, ognuno dei quali poi si collega a tutti gli altri n-3 nodi diversi da d e alla fine raggiungono il nodo d).
- In generale, il numero di cammini con k nodi interni è dato da (n-2) * (n-3) * ... * (n-(k+1)), che può essere semplificato in una formula generale.
![[Schermata 2023-09-01 alle 15.51.07.png|500]]

Quindi la complessità è **O(n!)**

#### [[Cammini e connessioni#Algoritmo di Dijkstra|Algoritmo di Dijkstra]]
Se i<sub>1</sub> e i<sub>2</sub> sono nodi per i quali passa un cammino minimo che va dal nodo s al nodo d, allora il suo “sottocammino” che va da i<sub>1</sub> a i<sub>2</sub> deve essere anch’esso un cammino minimo da i<sub>1</sub> a i<sub>2</sub>. Per cui non è necessario esplorare tutti i possibili cammini dalla sorgente alla destinazione, ma solo un sottoinsieme di essi.
##### Funzionamento e implementazione 
###### Inizializzazione
L'algoritmo usa tre insiemi di supporto (implementati come liste):
- **unvisited_nodes**:
	nodi non visitati
- **previous_nodes**:
	contiene per ogni nodo n del grafo il nodo precedente lungo il cammino minimo progressivamente individuato per raggiungere n
- **path_weights**:
	Insieme dei pesi progressivamente individuati dal nodo s ogni altro nodo del grafo

```python
# Prende in input:
# - matrice di adiacenza "ad_matrix"
# - nodo di partenza "start_node
# - una variabile booleana "plot" che indica se si desidera tracciare il grafico durante l'esecuzione dell'algoritmo. (da saltare e si riferisce all'ultima if)
def dijkstra(ad_matrix, start_node, plot):
	#inizzializzazione delle variabili 
    n = len(ad_matrix) # leggo l'ordine del grafo
    unvisited_nodes = set(range(n)) # nodi non visitato dall'algoritmo
    previous_nodes = {} # dizionario dei nodi precedenti nel percorso più breve
    path_weights = np.full(n,float('inf'))# contiene i pesi del nodo
    path_weights[start_node] = 0
```

###### Ciclo
```python
#L'algoritmo individua il nodo k con il peso del percorso minimo corrente tra i nodi non visitati, in modo da poterlo successivamente selezionare per ulteriori operazioni nell'algoritmo di Dijkstra. 
    while unvisited_nodes: 
#Il cilo principipale finisce quando finiscono i nodi da visitare, quindi nel momento tale in cui unvisited_nodes (nel costruttore) risulterà vuoto

#Lo inizzializza per usarlo come flag per tenere traccia del nodo con il peso minore, assegnandolo a none è come se lo svuotasssi
        current_min_node = None 

#ciclo innestato, viene eseguito un ciclo for che itera su ogni nodo presente nell'insieme degli unvisited_nodes (nodi non ancora visitati) per individuare il nodo adiacente a k (o vicino) con il minor peso dell’arco che li collega:
        for node in unvisited_nodes:

#Se la flag è vuota:
            if current_min_node is None:
	
	# - Imposta la flag sul nodo iterato (preso in esame)
                current_min_node = node

    # - Se non è vuota confronta il peso dell'arco del nodo, con il nodo salvato in current_min_node, se il peso è minore: 
            elif path_weights[node] < path_weights[current_min_node]:
		# - Memorizzo il nodo preso in esame in current_min_node
                current_min_node = node

# A ciclo for concluso rimuovere il nodo corrente (current_min_node) dall'insieme dei nodi non visitati (unvisited_nodes). Questa operazione è necessaria per tener traccia dei nodi che sono stati visitati durante l'algoritmo di Dijkstra.
        unvisited_nodes.remove(current_min_node)


#Il codice sottostante ha lo scopo di esaminare i nodi non visitati adiacenti al nodo corrente (current_min_node) e aggiornare i loro pesi del percorso e nodi precedenti se viene trovato un percorso più breve

# L'algoritmo esegue un ciclo for per identificare i vicini del nodo current_min_node tra i nodi non ancora visitati:
        for node in unvisited_nodes:

# Se esiste un arco tra il nodo corrente (current_min_node) e il nodo considerato (node) nell'ad_matrix, il nodo considerato è un vicino del nodo corrente. 
#(controlla l'esistenza dell'arco verificando che il valore nella matrice di adiacenza di node sia diverso da o)
            if ad_matrix[node,current_min_node]:

# Viene calcolato il peso del percorso sommando il peso del percorso fino al nodo corrente (path_weights[current_min_node]) con il peso dell'arco tra il nodo corrente e il nodo considerato (ad_matrix[node, current_min_node]).
                weight = path_weights[current_min_node]+ad_matrix[node,current_min_node]

# Se il peso del percorso calcolato è inferiore al peso del percorso attualmente registrato per il nodo considerato (path_weights[node]), viene aggiornato il peso del percorso e viene impostato "current_min_node" come nodo precedente per il nodo considerato.
                if weight < path_weights[node]:
                    path_weights[node]=weight
                    previous_nodes[node]=current_min_node
	

#questo pezzo traccia un grafico (può essere saltato)
        if plot:
           plot_graph(ad_matrix, unvisited_nodes, previous_nodes, "Dijkstra Algorithm - Visited nodes:" + str(n - len(unvisited_nodes)))

# Vengono restituiti due diversi valori 
    return path_weights, previous_nodes
# path_weights = un array che contiene i pesi dei percorsi minimi dai nodi di partenza a tutti gli altri nodi.
# previous_nodes = un dizionario che tiene traccia dei nodi precedenti nel percorso più breve.
```

###### Output
Se dovessi richiamare un algoritmo di Dijkstra, il suo output sarebbe: 
```python
array([0., 1., 2.]), {1: 0, 2: 0} 
```

Per cui avremmo il primo array, che definisce il pesi dei percorsi minimi, mentre il secondo elemento è un dizionario che tiene traccia dei nodi precedenti 
Quindi se voglio richiamare, solo un'output assegno:
[0] per i pesi [1] per i nodi precedenti.

Pesi
Djikstra calcola il peso cumulativo del percorso minimo da un nodo di partenza (nodo iniziale) a tutti gli altri nodi nel grafo. Il risultato di Dijkstra è una lista (o un array) di pesi, in cui ogni elemento rappresenta il peso del percorso minimo dal nodo di partenza al corrispondente nodo di destinazione.
```python
pesi = dijkstra(self.matrix, a)[0]
print(pesi)
[0. 1. 2.]
```

[1] per i nodi precedenti
```python
nodi_precedenti = dijkstra(self.matrix, a)[1]
print(nodi_precedenti)
{1: 0, 2: 0}
```

Nel dizionario `{1: 0, 2: 0}`, l'associazione `1: 0` indica che il predecessore del nodo 1 nel percorso più breve è il nodo 0, e l'associazione `2: 0` indica che il predecessore del nodo 2 è anch'esso il nodo 0 nel percorso più breve.

Per trasformare l'output nel percorso vero e proprio da un punto a ad un'altro punto bisogna implementare questo programma. 

>[!info] Partendo dall'ultimo nodo ricostruisce al contrario il percorso più piccolo
```python
def find_shortest_path(previous_nodes, start_node, end_node):
    path = [] #Inseriamo i nodi
    current_node = end_node #Assegnando l'ultimo nodo a current_node, servirà in seguito ad aggiungerlo per primo nella lista 
    
    while current_node != start_node: #finche il nodo corrente è diverso dal nodo iniziale permette di risalire il percorso al contrario
        path.insert(0, current_node) #con il comando insert inseriamo in posizione 0 il nodo in esame, serve per posizionarlo sempre davanti, facendo scorrere gli altri nodi
        current_node = previous_nodes[current_node] #otteniamo dal set di dizionari previous_node il nodo prima e lo assegnamo a current_node

    path.insert(0, start_node) #inseriamo il nodo iniziale come primo elemento della lista
    return path
```

##### Complessità
Nell'algoritmo sono iterati due nodi **for**, dentro un **while** che si ripete n volte. Quindi i **for** essendo eseguite al massimo n·n volte, genera una complessità computazionale di **O(n<sup>2</sup>)**.