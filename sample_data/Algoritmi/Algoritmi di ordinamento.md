# Algoritmi di ordinamento
## Definizioni
Data delle liste [e1, e1, ... en] possono essere definite con:
- Relazione d'ordine largo (gli elementi si ripetono) ≤: 
	- totalmente ordinata: [1,2,2,3,4,5,6,7,7,8] 
	- parzialmente ordinata: [1,3,2,3,5,4,6,7,7,8] 
- Relazione d'ordine stretto (gli elementi non si ripetono) <:
	- totalmente ordinata: [1,2,3,4,5,6,7,8] 
	- parzialmente ordinata: [1,2,3,4,5,8,6,7] 

A seguito di un ordinamento la lista risultante è solamente una permutazione della lista iniziale  
``` python
lista_iniziale = [3, -5, 10, 43, -76, 15, -9, 0, 24, 26, -2, 14, 98, 12, -25]
lista_finale = [-76, -25, -9, -5, -2, 0, 3, 10, 12, 14, 15, 24, 26, 43, 98]
```

Esistono vari algoritmo di ordinamento i quali generano lo stesso risultato usando metodi diversi per cui possono essere diversi in termini di complessità computazionale.

### Metodi built - in di python
- **sort()**
``` python
my_list = [1, 3, 4, 2] 
my_list.sort() 
print(my_list)  
[1, 2, 3, 4]
```
In questo caso ho perso l'ordinamento della lista originale perché ordina la lista in-place, cioè cambiando di posto gli elementi all’interno della lista

- **sorted()** 
```Python 
my_list = [1, 3, 4, 2] 
my_new_list = sorted(my_list) 
print(my_new_list)  
[1, 2, 3, 4]
```
Restituisce una copia della lista ordinata, quindi la lista originale viene conservata in memoria

## Selection sort 
L’algoritmo legge tutti gli elementi della lista da sinistra a destra, individua l'elemento minore e lo sposta a sinistra. Partendo dall’elemento successivo continua il ciclo fino alla fine della lista

```python
 # mi inverte la posizione
def swap(l, i, j): #l = la lista, i e j = indice de  parametri da invertire 
	temp = l[i] 
	l[i] = l[j]  
	l[j] = temp

def selection_sort(l): 
	for i in range(len(l)):
		min_index = i #quello che suppongo sia il numero minore
		for j in range(i + 1, len(l)): #i+1 perchè parto dall'elemento subito dopo quello preso 
			if (l[min_index] > l[j]): 
				min_index = j
		swap(l, i, min_index)`
```

### Complessità
La stima della complessità computazionale di questo script non dipende dagli elementi della lista, infatti dopo aver fissato già la numerosità della lista, lo script andrà a compiere lo stesso numero di processi.
Numero di confronti effettuati: $$ n*\frac{n-1}{2}$$

La complessità (in termini di confronti effettuati) è O(n<sup>2</sup>)

## Insertion sort
L’algoritmo inizia dal secondo elemento della lista, e poi ripetendo per tutti gli altri elementi successivi, lo confronta con l'elemento precedente e se quest'ultimo è maggiore lo sposta di una posizione a destra/li scambio; in tal caso lo confronterà anche con l'elemento precedente e se quest'ultimo è maggiore lo sposta di una posizione a destra fino al raggiungimento di un'elemento maggiore.

``` python
def insertion_sort(l):  
    for i in range(1, len(l)): #prendo il valore da ordinare i
        value = l[i]    # assegno a una variabile il valore   
        j = i - 1       # mi sposto verso sinistra  
        while j >= 0 and value < l[j]: # se l'indice é maggiore uguale a 0 e se il valore di i è minore di j 
            l[j + 1] = l[j]  #sposto il valore avanti di una posizione
            j = j - 1  #vedo il valore precednete 
        l[j + 1] = value #assegno il nuovo value 
```
### Complessità
numero di confronti effettuati nel caso peggiore : $$ n*\frac{n-1}{2}$$

La complessità (in termini di confronti effettuati) è O(n<sup>2</sup>)

## Bubble sort
L’algoritmo, posiziona un puntatore sull'ultimo elemento, e legge il primo elemento da sinistra, confrontandolo con il successivo, se il primo è maggiore scambia i posti; poi confronta il secondo elemento con il successivo se il secondo è maggiore scambia i posti fino al raggiungimento dell'ultimo elemento il quale se è minore effettua l'ultimo scambio e "sposta il puntatore" dall'ultima carta alla penultima. Così la lista sarà riordinata al contrario e la lista diventerà sempre più piccola.
Infine, dopo che la lista sarà completamente riordinata l'algoritmo restituirà una tupla

``` python
def bubble_sort(l):
    for i in reversed(range(len(l))): #reverse perché posiziona un puntatore sull'ultimo elemento 
        for j in range(0, i): #j rappresenta la posizione dell'elemento corrente che viene confrontato con l'elemento successivo.
            if l[j] > l[j + 1]: #se l'elemento corrente l[j] è maggiore del successivo l[j + 1] allora swap
                swap(l, j, j + 1)
```
### Complessità
La complessità è O(n<sup>2</sup>)
Numero di confronti effettuati
$$ n*\frac{n-1}{2}$$

### Bubble sort ottimizzato
Il bubble sort può essere **ottimizzato**; se durante una serie di confronti (dall'inizio della lista alla fine) non è effettuato nessun scambio significa gli elementi sono in ordine e l’algoritmo si interrompe:
```python 
def bubble_sort_with_check(l):
    for i in reversed(range(len(l))):
        swap_done = False #asssegno alla variabile booleana False
        for j in range(0, i):
            if l[j] > l[j + 1]:
                swap(l, j, j + 1)
                swap_done = True #se swap_done rimane FALSE ferma il programma
        if swap_done == False:
            break
```
#### Complessità
La complessità è O(n<sup>2</sup>)
Numeri di confronti effettuati nel caso migliore
$$n-1$$
Numero di confronti effettuati nel caso peggiore
$$ n*\frac{n-1}{2}$$

## Merge Sort
L’algoritmo divide la lista a metà ottenendo due sotto-liste, che sono ulteriormente divise fino ad ottenere liste di due elementi. Tali liste vengono ordinate effettuando un singolo confronto, in seguito vengono unite a due a due e riordinate, questo processo è ripetuto fino ad arrivare alle ultime due liste rimaste.

```python 
def merge_lists(l, start, middle, high):
    start2 = middle + 1

    while start < start2 <= high:  # start e start2 sono gli inizi delle due liste

        if l[start] <= l[start2]: #Se l'elemento alla posizione start è <= a start2, incre menta start
            start += 1
        else: #se start è maggiore di start2 sposta gli elementi a destra
            value = l[start2] #value memorizza il valore dell'elemento alla posizione start2
            index = start2 #inizializza, index tiene traccia della posizione dell'elemento che deve essere spostato
            # Shift all the elements
            while index != start: #while itera finquando index non è uguale a start
                l[index] = l[index - 1]
                index -= 1 #decrementa index di 1
            l[start] = value
            # Update indexes
            start += 1 #tengono traccia ognuno dei rispettivi indici
            start2 += 1

def recursive_merge_sort(l, low, high):
    if low < high: #continua ad iterare fin quando l'indice di partenza è minore dell'indice di fine
        middle = (low + high) // 2 #calcola l'indice centrale della lista
        recursive_merge_sort(l, low, middle) #richiama il mergesort per la prima metà della lista
        recursive_merge_sort(l, middle + 1, high) #chiama mergesort per seconda metà della lista
        merge_lists(l, low, middle, high) #unisce le due liste ordinate

def merge_sort(l):
    recursive_merge_sort(l, 0, len(l) - 1) #chiama ricorsivamente la funzione merge sort per riodinare la lista, l'indice di partenza è 0 e l'indice di fine è lunghezza-1
```

### Complessità
![[Livello alberi.png]]
Per ogni livello K (altezza dell'albero) il numero di liste raddoppia rispetto al livello K -1, ma la lunghezza di ogni lista si dimezza, quindi il numero di confronti totali effettuati dalla funzione merge_list() per tutte le liste di un intero livello non può superare n -1, quindi la complessità per le operazioni di un livello è O(n). 
Poiché i livelli totali sono log2 (n+1) e la complessità delle operazioni di ordinamento per ogni livello è O(n), si ha che la complessità del merge sort è O(n* log2n). 

## Quick sort
L’algoritmo sceglie un elemento della lista che assume il ruolo di pivot, ossia di perno; quindi, riorganizza la lista in modo da avere a sinistra del pivot tutti gli elementi minori o uguali al pivot e a destra del pivot tutti gli elementi maggiori del pivot; quindi, rifaccio lo stesso per la lista a sinistra del pivot e per la lista a destra del pivot.

```python
def partition(l, low, high): #lista con 2 indici
    pivot = l[high] #elementi maggiori del pivot
    p = low #elementi minori o uguali al pivot

    for j in range(low, high): 
        if l[j] <= pivot:
            l[p], l[j] = l[j], l[p]
            p = p + 1
    l[p], l[high] = l[high], l[p]
    return p

def recursive_quick_sort(l, low, high): #ricorsiva il partizionamento degli elenchi
    pi = partition(l, low, high)
    if low < pi - 1:
        recursive_quick_sort(l, low, pi - 1)
    if pi + 1 < high:
        recursive_quick_sort(l, pi + 1, high)

def quick_sort(l):
    recursive_quick_sort(l, 0, len(l) - 1)

```
### Complessità
**Nel caso peggiore** del Quick Sort, si verifica quando il pivot scelto si posiziona costantemente in modo che una delle sotto-liste abbia dimensione n-1, mentre l'altra ha dimensione 1. Questo causa la formazione di un albero ricorsivo (alto) con un'altezza massima di n-1. In questa situazione, la funzione di partizione richiederà n-1 confronti poiché uno degli elementi viene confrontato con tutti gli altri nella lista. Ogni livello k dell'albero avrà un totale di n-k confronti, poiché la lista si riduce di un elemento ad ogni livello. In tal caso il numero totale di livelli può arrivare a n-1 nel peggiore dei casi, quindi la complessità temporale del Quick Sort nel caso peggiore sarà O(n^2).

**Nel caso migliore,** che corrisponde al caso in cui il pivot è sempre in una posizione centrale nella lista da dividere (dividendo la lista in due parti uguali), l’altezza dell’albero raggiunge log2(n). Poiché nel caso migliore i livelli totali sono log2(n) + 1, la complessità del Quick Sort nel caso migliore è O(n* log2(n))

Il Merge sort e il Quick sort sono basati entrambi sull'approccio divide et impera, ma hanno complessità differenti a causa delle loro strategie di suddivisione.
Nel merge sort a ogni interazione le liste sono divise a metà, mantenendo l'albero bilanciato
al contrario nel quicksort la suddivisione delle liste dipende dal valore del pivot che non garantisca un albero bilanciato e quindi una complessità O(n * log2(n)). 
Ma nel caso in cui la scelta del pivot sia sempre ottimale anche nel Quick sort l'albero si mantiene bilanciato e la complessità risulta O(n * log2(n)).
il Quick Sort è generalmente più veloce in situazioni reali grazie all'uso del meccanismo di partizionamento che sposta gli elementi in posizioni corrette più rapidamente rispetto alla divisione a metà utilizzata dal Merge Sort.

## Counting Sort 
Conoscendo il valore massimo degli elementi nella lista, l’algoritmo prepara una nuova lista di lunghezza pari al valore massimo, legge quindi gli elementi della lista da ordinare e da sinistra a destra e mette ogni elemento nella nuova lista nella posizione corrispondente al suo valore.

Quindi prima crea una lista con lunghezza pari al valore massimo, poi inserisce gli elementi della lista originale, nella nuova lista e nella posizione corrispondente al loro valore. Una volta svuotata la lista originaria, reinserisce in ordine gli elementi presenti nella seconda lista.

```python
def counting_sort(l, m): #l è l’elenco da ordinare, m è il valore massimo dell’elenco  
	new_list_size = m + 1 # calcola la dimensione della nuova lista in base al valore massimo
	new_list = [0] * new_list_size #crea una nuova lista con gli elemento inizializzato a 0 per tenere traccia delle occorrenze
	
	#conteggio delle occorrenze
	for i in range(0, len(l)):  
		new_list[l[i]] = 1 #viene eseguito sempre n volte, Imposta il valore nella posizione `l[i]` di `new_list` a 1. Questo passaggio indica che il valore `l[i]` è presente nella lista originale.
		i=0
	
	#riempimento dell'array
	for j in range(0, new_list_size): #questo blocco viene eseguito sempre m + 1 volte, è usato per iterare attraverso la lista 
		if new_list[j] != 0: #controlla se il valore in index non è 0 quindi se il valore `j` è presente nella lista originale
			l[i] = j#se non è 0 allora j è nell’elenco, assegna j a l[i] e aumenta di 1 aggiungendolo alla lista  
			i+=1
```
### Complessità
La complessità è O(n+m) (perché in questo specifico algoritmo) dipende:
- dalla dimensione dell'input n 
- dagli specifici valori degli input, in particolare m; 
quindi, nel caso in cui m appartenga ad O(n) allora possiamo assumere che la complessità è O(n).

#### Counting Sort con duplicati e numeri negativi
Questa variazione è usata per ordinare una lista **l**, prendendo in considerazione i valori negativi e le occorrenze duplicate
(Indico solamente le differenze con il codice originale)
``` python
def counting_sort_including_negatives_and_duplicates(l, min, max): # min è il valore minimo (serve per operare con i numeri negativi)
	new_list_size = max - min + 1 
	new_list = [0] * new_list_size
	for i in range(0, len(l)): 
		new_list[l[i]-min] +=1
		i=0  
		for j in range(0, new_list_size):
			if new_list[j] != 0: 
				while new_list[j] > 0: 
					l[i] = j+min 
					i+=1 
					new_list[j]+=-1
```
- Per i valori duplicati: uso l'elemento esimo della nuova lista per contare quanti elementi di valore k contiene la lista da ordinare;
- Per i valori negativi: effettuo uno shift dei valori della lista allineando il valore minimo allo zero e poi li riporto ai valori originali quando li risposto nella lista.

# Esercizio difficile
```python
def selection_sort(l):
    for i in range(len(l)):
        min_index = i  # index of the 1-st element of the unordered sublist
        for j in range(i + 1, len(l)):
            if l[min_index] > l[j] and l[min_index]%2 == l[j]%2 or l[min_index]%2 == 1 and l[j]%2 == 0:
                min_index = j
        swap(l, i, min_index)
```
