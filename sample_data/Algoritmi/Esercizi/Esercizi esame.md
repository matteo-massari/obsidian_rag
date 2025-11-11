# Esercizi esame

Considerare le seguenti funzioni che implementano gli algoritmi di ordinamento "bubble sort" e "bubble sort con check di terminazione". Modificarle in modo che ognuna restituisca il numero di CONFRONTI effettuati e il numero di SCAMBI effettuati (si consiglia di utilizzare la funzione _ "return x, y_").  

Successivamente implementare la funzione _ **bubble_sort_comparison**_ _ _ **(input_list)** __ che riceve in input una lista di interi **_ input_list_**, la ordina attraverso i due algoritmi e infine stampa (usando la funzione **_print()_**) la somma dei confronti e degli scambi effettuati dall'algoritmo "bubble sort" se il numero dei confronti effettuati da "bubble sort" è minore rispetto al "bubble sort con check di terminazione", **altrimenti** stampa la somma dei confronti e degli scambi effettuati dall'algoritmo "bubble sort con check di terminazione".

(indicazione: se ad esempio x è il numero di confronti effettuati e y è il numero di scambi effettuati, la stampa deve avvenire attraverso **print(x+y)**).
```python
def swap(l, i, j):
    temp = l[i]
    l[i] = l[j]
    l[j] = temp

def bubble_sort(l):
    swaps = 0
    comparisons = 0
    for i in reversed(range(len(l))):
        for j in range(0, i):
            comparisons += 1
            if l[j] > l[j + 1]:
                swap(l, j, j + 1)
                swaps+=1
    return comparisons, swaps


def bubble_sort_with_check(l):
    swaps = 0
    comparisons = 0
    for i in reversed(range(len(l))):
        swap_done = False
        for j in range(0, i):
            comparisons += 1
            if l[j] > l[j + 1]:
                swap(l, j, j + 1)
                swaps += 1
                swap_done = True
        if swap_done == False:
            break
    return comparisons, swaps

def bubble_sort_comparison(input_list):
    list_copy = input_list.copy() #se no la prima chiamata modifica la lista, quindi faccio una copia
    comparisons1, swaps1 = bubble_sort(list_copy)  #comparisons1, swaps1 nuovo nome, bubble_sort è il vecchio nome. (list_copy) è la lista su cui opera
    comparisons2, swaps2 = bubble_sort_with_check(input_list)  #comparisons2, swaps2 nuovo nome, bubble_sort è il vecchio nome. (input_list) è la lista su cui opera

    if comparisons1 < comparisons2:
        print(comparisons1 + swaps1)
    else:
        print(comparisons2 + swaps2
```
---

Nel codice sottostante sono riportale le implementazioni delle seguenti funzioni:

- **_sequential_search_with_ordered_list(input_list, k)_**, che effettua una **ricerca sequenziale** dell'intero **_k_** nella lista **_input_list_**, che si assume sia già ordinata
- **_ binary _ search(input_list, k  ) _**, che effettua una **ricerca binaria** dell'intero **_ k _** nella lista **_ input _ list _**, che si assume sia già ordinata

Le suddette funzioni restituiscono **_True_** o **_False_** a seconda che l'elemento **_ k_** sia stato trovato o meno nella lista.

Modificare entrambe le funzioni in modo che in luogo di **_True_** o **_False_** restituiscano il numero di istruzioni che eseguono. Per il conteggio delle istruzioni si consideri che ogni esecuzione di un riga di codice corrisponde all'esecuzione di una istruzione, escludendo l'istruzione _**return**_ (attenzione: contare anche le volte in cui vengono eseguiti i test di uscita nelle istruzioni _while_)

Successivamente completare la funzione **_the_best(input_list)_** in modo che:

- cerchi l'elemento 4890 nella lista **_input_list_** sia con la **ricerca sequenziale** che con la **ricerca binaria**, quindi:
- restituisca la stringa **_"sequential"_** se il numero di istruzioni eseguite dalla **ricerca sequenziale** è minore del numero di istruzioni eseguite dalla **ricerca binaria**;
- altrimenti deve restituire la stringa _**"binary"**_.

Successivamente completare la funzione **_the_best_ever(list_of_lists)_**, dove _**list_of_lists**_ è una lista di liste di interi, in modo che**_:  
_**

- usando la funzione **_the_best(**_input_list_**)_** su ogni lista contenuta in **list_of_lists**, conti quante volte vince (cioè esegue meno istruzioni) la **ricerca sequenziale** e quante volte vince la **ricerca binaria**; 
- infine deve restituire la stringa **_"sequential"_** se il numero di vittorie della **ricerca sequenziale** è maggiore del numero di vittorie dalla **ricerca binaria**, altrimenti deve restituire la stringa _**"binary"**_**.**

```python
import math
import random

def sequential_search_with_ordered_list(input_list, k):
    count = 0
    count += 1
    i = 0
    count += 1
    while i < len(input_list):
        count += 1
        if input_list[i] == k:
            return count
        count += 1
        if input_list[i] > k:
            return count
        count += 1
        i = i + 1
    count += 1 # next i < len
    return count

def binary_search(input_list, k):
    count  = 0
    count += 1
    low=0
    count += 1
    high = len(input_list) - 1
    count += 1
    while low <= high:
        count += 1
        middle = (low+high) // 2
        count += 1
        if input_list[middle]==k:
            return count
        count += 1
        if k < input_list[middle]:
            count += 1
            high = middle - 1
        else:
            count += 1
            low = middle + 1
    count += 1  # while low <= high
    return count

def the_best(input_list):
    ss = sequential_search_with_ordered_list(input_list, 4760)
    bs = binary_search(input_list, 4760)
    if ss<bs:
        return "sequential"
    return "binary"

def the_best_ever(list_of_lists):
    ss_winner=0
    bs_winner = 0
    for l in list_of_lists:
        if the_best(l)=="sequential":
            ss_winner+=1
        else:
            bs_winner +=1

    if ss_winner > bs_winner:
        return "sequential"
    return "binary"
```
---

La funzione **_hash(k)_** presente nel codice sottostante è una funzione hash che riceve in input un intero _k_restituisce un intero.

Completa le seguenti funzioni:  

- **_conflict(a,b)_**, dove i parametri di input _**a**_ e **_b_** sono due interi, in modo che restituisca **_True_** se la chiave **_a_** e la chiave _**b**_ generano un conflitto usando la suddetta funzione hash, altrimenti **_False_**;  
    
- **_check_conflict(),_** in modo che restituisca **_True_** se la specifica chiave **_4890_** genera un conflitto con almeno una chiave compresa tra **_0_** e **_100_** usando la suddetta funzione hash;  
    
- **_get_conflicting_keys(low, high)_**, dove i parametri di input _**low**_ e **_high_** sono due interi**,** in modo che restituisca (attraverso l'istruzione **_return_**) l'insieme delle chiavi comprese tra **_low_** e **_high_** per le quali si genera un conflitto con una qualsiasi altra chiave inclusa nello stesso intervallo; per la verifica dei conflitti può usare la funzione **_conflict(a,b)_****.** Ad esempio se la chiave x∈_[__**l****ow**_, _**high**)_ genera in conflitto con un'altra chiave y∈_[__**l****ow**_, **_high_**_)_ allora x e y devono essere inclusi nell'insieme restituito**,** e se la chiave z∈_[_**_low_**, _**high**)_ non genera nessun conflitto con nessun'altra chiave in _[__low_, _high__]_ allora z non deve essere compresa nell'insieme. Si sottolinea che i dati restituiti non devono contenere chiavi duplicate **.**

```python
from math import floor

def hash(k):
    return floor(70*((0.357840 * k)%1))
    
def conflict(a,b):
    return hash(a)==hash(b)
    
    
def check_conflict():
    for i in range(0, 101):
        if hash(0)==hash(i):
            return True
    return False    

def get_conflicting_keys(low, high):
    conflicting_keys = set()
    for i in range(low, high-1):
        for j in range(i+1, high):
            if conflict(i,j):
                conflicting_keys.add(i)
                conflicting_keys.add(j)
                break
    return conflicting_keys
```
---

Il codice successivo fornisce un'implementazione dell'algoritmo di Dijkstra, dove la funzione 

**_dijkstra(ad_matrix, start_node)_**

riceve in input gli argomenti  

- **_ad_matrix_**: è la matrice di adiacenza di un grafo pesato, i cui _n_ nodi sono rappresentati dagli interi da _0_a _n-1_
- _**_start_node_**:_ è un intero corrispondente all'_id_ del nodo sorgente

e restituisce in ordine i seguenti oggetti:

- una **lista** contenente i pesi dei cammini minimi dal nodo sorgente ad ogni altro nodo (l’elemento i-simo rappresenta il peso del cammino che unisce il nodo sorgente al nodo _i_)
- un **dizionario** contenente i nodi che precedono ogni altro nodo lungo i percorsi minimi

Completa le due seguenti funzioni:

**1)** _**furthest_node(s, graph_ad_matrix)**,_ dove 

- **_s_** è l'id del nodo sorgente
- __**graph_ad_matrix**__ è la matrice di adiacenza del grafo

in modo che, usando la funzione **_dijkstra(ad_matrix, start_node)_**, calcoli i cammini minimi da _**s**_ a ogni altro nodo del grafo e restituisca (usando la funzione **_return_**) il **peso** del cammino minimo **più lungo**;

**2) _furthest_path(graph_ad_matrix_1, graph_ad_matrix_2, graph_ad_matrix_3)_**, dove  

- **graph_ad_matrix_1, graph_ad_matrix_2** e **graph_ad_matrix_3** sono le matrici di adiacenza di tre grafi differenti

in modo che, usando la funzione _****furthest_node(s, graph_ad_matrix)****_, calcoli per ognuno dei tre grafi il **peso** del cammino minimo più lungo considerando come sorgente sempre il nodo **0**; quindi restituisca (usando la funzione **return**) il **peso** del **più lungo** tra i tre cammini minimi ottenuti.


```python
import numpy as np

"""Dijkstra's algorithm"""
def dijkstra(ad_matrix, start_node):
    n = len(ad_matrix)
    unvisited_nodes = set(range(n))
    previous_nodes = {}
    path_weights = np.full(n,float('inf'))
    path_weights[start_node] = 0

    while unvisited_nodes:
        current_min_node = None
        # Iterate over nodes in unvisited_nodes
        for node in unvisited_nodes:
            if current_min_node is None:
                current_min_node = node
            elif path_weights[node] < path_weights[current_min_node]:
                current_min_node = node

        unvisited_nodes.remove(current_min_node)

        for node in unvisited_nodes:
            if ad_matrix[node,current_min_node]:
                # the node is a neighbor
                weight = path_weights[current_min_node]+ad_matrix[node,current_min_node]
                if weight < path_weights[node]:
                    path_weights[node]=weight
                    previous_nodes[node]=current_min_node

    return path_weights, previous_nodes

def furthest_node(s, graph_ad_matrix):
    path_weights, previous_nodes=dijkstra(graph_ad_matrix, s)
    max_weight = path_weights[0]
    for i in range(1, len(path_weights)):
        if path_weights[i] > max_weight:
            max_weight = path_weights[i]
    return max_weight

#oppure
def furthest_node(s, graph_ad_matrix):
    path_weights = dijkstra(graph_ad_matrix, s)[0]
    return max(path_weights)

def furthest_path(graph_ad_matrix_1, graph_ad_matrix_2, graph_ad_matrix_3):

    dist_1 = furthest_node(0, graph_ad_matrix_1)
    dist_2 = furthest_node(0, graph_ad_matrix_2)
    dist_3 = furthest_node(0, graph_ad_matrix_3)

    return max(dist_1, dist_2, dist_3)
```
---

Nel codice sottostante è riportata l'implementazione dell'algoritmo kmeans. In particolare la funzione
**_kmeans(data_set, number_of_clusters)_**
ha come parametri di input:
- _**data_set**:_ è il set di dati di input  
- _**number_of_clusters**:_ è un intero che specifica il numero di cluster da creare
e restituisce in output tre oggetti nel seguente ordine:
- la lista dei centroidi identificati dall'algoritmo
- la lista dei centroidi assegnati per ogni punto  
- la lista degli errori, ossia una lista che contiene nella posizione _i-sima_ la distanza del punto _i-simo_ del dataset dal centroide assegnato

Completa la funzione **_min_error()_**, la cui definizione è già presente alla fine del codice, in modo che sfruttando la suddetta funzione **_kmeans(data_set, number_of_clusters)_** fornendo come primo parametro di input la variabile **_dataset_** (che troverai già definita nel codice), calcoli l'errore totale (cioè somma della lista degli errori) che si ottiene con un numero di cluster pari a 3, poi pari a 4 e poi pari 5. Infine deve restituire (attraverso l'istruzione **_return_**) il minore dei tre errori totali calcolati.

```python
import random
import numpy as np
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs

def calculate_new_centroid_positions(data_set, assigned_centroids, number_of_clusters):
    sum_of_positions = np.zeros([number_of_clusters, 2],float)
    points_per_centroids = np.zeros(number_of_clusters,int)

    #sum all point positions depending on the assigned centroids
    for i in range(len(data_set)):
        sum_of_positions[assigned_centroids[i]]+= data_set[i]  # sum the positions
        points_per_centroids[assigned_centroids[i]] += 1 # count the assigned points for each centroid

    #calculate the average position for each coordinate
    centroid_positions = np.zeros([number_of_clusters, 2], float)
    for i in range(number_of_clusters):
        centroid_positions[i]=sum_of_positions[i]/points_per_centroids[i]

    return centroid_positions

def centroid_assignation(data_set, centroids):
    n = len(data_set)
    assigned_centroids = np.empty(n, dtype=int)
    assignment_errors = np.empty(n, dtype=float)

    for i in range(len(data_set)):
        nearest_centroid = 0
        nearest_centroid_error = np.sqrt(np.sum((centroids[0, :] - data_set[i, :]) ** 2))
        for j in range(1, len(centroids)):
            # error estimation
            error = np.sqrt(np.sum((centroids[j, :] - data_set[i, :]) ** 2))
            if error < nearest_centroid_error:
                nearest_centroid = j
                nearest_centroid_error = error

        assigned_centroids[i]=nearest_centroid
        assignment_errors[i]=nearest_centroid_error

    return assigned_centroids, assignment_errors

def kmeans(data_set, number_of_clusters):
    random.seed(0)
    max_error=1e-4
    # inizialization
    previous_error = 0
    stop = False
    current_iteration = 0
    centroids = np.empty([number_of_clusters, 2])

    # select random data points as centroids
    i=0
    random_indexes = random.sample(range(0, len(data_set)), number_of_clusters)
    for index in random_indexes:
        centroids[i] = data_set[index]
        i+=1

    while True:

        current_iteration += 1

        # assign centroids and calculate the errors
        assigned_centroids, assignment_errors = centroid_assignation(data_set, centroids)

        # error check
        total_error = sum(assignment_errors)
        if abs(previous_error - total_error) <= max_error:
            break

        previous_error = total_error
        # update centroid position
        centroids = calculate_new_centroid_positions(data_set, assigned_centroids, number_of_clusters)

    return centroids, assigned_centroids, assignment_errors

def min_error():
    dataset, _ = make_blobs(n_samples=1000, centers=3, random_state=40, cluster_std=1.3)
    #inserisci il codice qui sotto
    centroids, assignment, errors = kmeans(dataset, 3)
    err_3 = sum(errors)
    centroids, assignment, errors = kmeans(dataset, 4)
    err_4 = sum(errors)
    centroids, assignment, errors = kmeans(dataset, 5)
    err_5 = sum(errors)
    return(min(err_3, err_4, err_5))

#oppure
def min_error():
    dataset, _ = make_blobs(n_samples=1000, centers=3, random_state=40, cluster_std=1.3)
    kmeans3= sum(kmeans(dataset, number_of_clusters=3)[2])
    kmeans4= sum(kmeans(dataset, number_of_clusters=4)[2])
    kmeans5= sum(kmeans(dataset, number_of_clusters=5)[2])
    return min(kmeans3, kmeans4, kmeans5)
```
---

Di seguito viene fornita una semplice implementazione di una blockchain in cui un blocco contiene un attributo _data_, che contiene i dati del blocco, e l'attributo _previous_hash__,_ che contiene i valore dell'hash del blocco precedente. Viene già fornita anche una funzione, _create_blockchain_, che crea una blockchain di test, ossia una lista di blocchi. Completare la funzione _verify_chain_, che riceve in input una blockchain e effettua la verifica di integrità della catena verificando che ogni hash presente in un blocco sia uguale all'hash del blocco precedente.

(indicazione: per calcolare l'hash di un blocco è possibile usare la funzione _get_hash(obj)_ passando il blocco come argomento di input _obj_)

```python
import string
import random
from hashlib import sha256

def get_hash(obj):
    return sha256(str(obj.__dict__).encode()).hexdigest()

class Block:
    def __init__(self, data, previous_hash):
        self.data = data
        self.previous_hash = previous_hash

def create_blockchain():
    blockchain = []

    data = "".join(random.choices(string.ascii_lowercase, k=5))
    genesis_block = Block(data, "0")
    blockchain.append(genesis_block)

    for i in range(10):
        data = random.choice(string.ascii_letters)
        previous_hash = get_hash(blockchain[i])
        blockchain.append(Block(data, previous_hash))

    return blockchain

def verify_chain(blockchain):
    for i in range(1, len(blockchain)):
        previous_block = blockchain[i-1]
        block = blockchain[i]
        
        if get_hash(previous_block) != block.previous_hash:
            return False
    return True
```
---
Completa il seguente programma in modo che effettui il mining del tuo nome. L'obiettivo è quello di trovare un valore di _nonce_ tale che la stringa formata dall'unione del tuo nome e _nonce_ restituisca un valore hash che inizi con due '0' (quindi del tipo "00....").

_Nota: è sufficiente completare il codice esclusivamente all'interno della funzione mine() nei punti in cui sono presenti i puntini_

```python
from hashlib import sha256

def get_hash(string):
    return sha256(string.encode()).hexdigest()

def mine():
    nome = "mionome"
    nonce = 0
    while not get_hash(nome + str(nonce)).startswith("00"):
        nonce +=1

    return nome + str(nonce)

```



Nel codice sottostante è riportata l'implementazione di una pila attraverso la classe Stack con i relativi metodi push0, pop 0, top0, lenO e is empty0. Assumendo che la pila contenga solo valori interi, si richiede di effettuare quanto descritto nei due punti seguenti. 
1) Modificare l'implementazione della pila in modo che quando si tenti di inserire un elemento uguale a quello in cima venga scartato 
2) Implementare il metodo della classe pop half sums(self), la cui intestazione è già presente nel codice, in modo tale che estragga tutti gli elementi presenti nella pila e calcoli la somma della prima metà degli elementi estratti e la somma della seconda metà degli elementi estratti (in altre parole se la pila contiene n elementi deve calcolare la somma dei primi n/2 elementi e la somma degli elementi rimanenti). Deve restituire quindi (tramite l'istruzione return x. y) una tupla contenente le due somme calcolate. Se la coda non contiene elementi la funzione deve restituire il valore 0. Si noti che la funzione pop_half sums(self) è interna alla classe Stack.

**Svolgimento**
1. **Modificare il metodo `push`** in modo che scarti gli elementi duplicati rispetto a quello in cima alla pila.
2. **Implementare il metodo `pop_half_sums`** che estragga tutti gli elementi, calcoli le somme delle due metà e restituisca una tupla con le due somme.

Ecco il codice aggiornato:

```python
class Stack:
    def __init__(self):
        self._data = []

    def push(self, e):
        if self.is_empty() or e != self.top():
            self._data.append(e)

    def pop(self):
        if self.is_empty():
            return None
        return self._data.pop()

    def top(self):
        if self.is_empty():
            return None
        return self._data[-1]

    def len(self):
        return len(self._data)

    def is_empty(self):
        return len(self._data) == 0

    def pop_half_sums(self):
        if self.is_empty():
            return 0

        elements = []
        while not self.is_empty():
            elements.append(self.pop())

        # Dividere gli elementi in due metà
        n = len(elements)
        half = n // 2

        # Calcolare la somma della prima metà
        sum_first_half = sum(elements[:half])

        # Calcolare la somma della seconda metà
        sum_second_half = sum(elements[half:])

        return sum_first_half, sum_second_half
```

### Spiegazione delle modifiche

1. **Modifica del metodo `push`**:
    - Controlliamo se la pila è vuota (`self.is_empty()`) o se l'elemento `e` da inserire è diverso dall'elemento in cima (`self.top()`).
    - Se uno dei due controlli è vero, l'elemento viene aggiunto alla pila. Altrimenti, viene scartato.

2. **Implementazione del metodo `pop_half_sums`**:
    - Se la pila è vuota, restituisce `0`.
    - Altrimenti, estrae tutti gli elementi dalla pila e li memorizza in una lista `elements`.
    - Divide la lista in due metà.
    - Calcola le somme delle due metà.
    - Restituisce le due somme come una tupla.

Questo codice garantisce che gli elementi duplicati consecutivi non vengano aggiunti alla pila e che il metodo `pop_half_sums` calcoli correttamente le somme delle due metà degli elementi estratti dalla pila.