# Strutture dati complesse
Una [[Strutture dati|struttura dati]] definisce una specifica modalità per organizzare i dati di una collezione, le relative modalità di accesso e di gestione; le strutture dati si focalizzano sull’organizzazione dei dati e sulle operazioni su di esse piuttosto che sul tipo di dati che ospitano.
Le differenti organizzazioni dei dati rendono ogni tipo di struttura dati più o meno adatta alla memorizzazione ed all’elaborazione di specifiche collezioni di dati, anche in funzione dello specifico tipo di elaborazioni da effettuare.

Esistono vari tipi di strutture dati astratte che specificano le operazioni eseguibili con quella struttura e l’effetto che hanno sui vari dati, ma non specificano in che modo le operazioni possono essere effettuate. 

Quindi una struttura dati astratta fornisce una specifica ma non un’implementazione della struttura dati, esistono diversi tipi di implementazione; l’importante è che l’implementazione rispetti la specifica (e quindi la struttura). Oltre alle liste, altre strutture dati astratte comunemente usate sono le pile, le code, [[Strutture dati avanzate|gli alberi, i grafi, i dizionari]].

## Principio di ordinamento
### Principio LIFO
![[stack.png]]
Last In First Out: l'ultimo elemento che viene inserito o aggiunto a una struttura dati è il primo ad essere rimosso o elaborato. Una convenzione utile da sapere è che l'ultimo elemento inserito è l'elemento in cima alla pila, quindi accedendo all'elemento con l'indice **-1**, si ottiene proprio l'elemento desiderato (cioè l’ultimo elemento inserito).
### Principio FIFO
![[code.png]]
First In First Out: il primo elemento che viene inserito o aggiunto a una struttura dati è il primo ad essere rimosso o elaborato

## Stack
Una pila (stack) è una struttura dati in cui i dati sono possono essere inseriti e rimossi secondo il principio “**LIFO**” (last-in first-out, LIFO).
### Operazioni
- **push(e)**: inserisce l’elemento e in cima alla pila  
- **pop()**: rimuove l’elemento in cima alla pila e lo restituisce, restituisce null se la pila è vuota
- **top()**: restituisce (senza rimuoverlo) l’elemento in cima alla pila, restituisce null se è la pila è vuota
- **is_empty()**: verifica se la pila è vuota e in tal caso restituisce true, altrimenti false 
- **len()**: restituisce il numero di elementi presenti nella pila

### Implementazioni
``` python
class Stack:

    def __init__(self):
        self._data = [] 

    def push(self, e):
        self._data.append(e)
# inserisci l'elemento e in cima alla pila

    def pop(self):
        if self.is_empty():
            return None
        return self._data.pop()
# rimuove l'elemento in cima alla pila e lo restituisce (restituisce Null se la pila è vuota)
    
    def top(self):
        if self.is_empty():
            return None
        return self._data[-1]
# restituisce, senza rimuoverlo, l'elemento in cima alla pila (restituisce Null se la pila è vuota)

    def len(self):
        return len(self._data)
# restituisce il numero di elementi presenti nella pila

    def is_empty(self):
        return len(self._data) == 0
# verifica se la pila è vuota e in tal caso restituisce True, altrimenti False
```

## Coda
Una coda è una struttura dati in cui i dati possono essere inseriti e rimossi secondo il principio FIFO.
### Operazioni
Le operazioni di base sono:
- **enqueue(e)**: inserisce l'elemento e all'inizio della coda
- **dequeue()**: rimuove e restituisce l'elemento in testa alla coda (restituisce Null se è vuota)
- **first()**: restituisce e non rimuove l'elemento in testa alla coda (restituisce Null se è vuota)
- **is_empty()**: verifica se la coda è vuota
- **len()**: restituisce il numero di elementi presenti nella coda

### Implementazioni
```python
class Queue:

    def __init__(self):
        self._data = []

    def enqueue(self, e):
        self._data.append(e)
# inserisce l'elemento e all'inizio della coda

    def dequeue(self):
        if self.is_empty():
            return None
        return self._data.pop(0)
# rimuove e restituisce l'elemento in testa alla coda (restituisce Null se è vuota)

    def first(self):
        if self.is_empty():
            return None
        return self._data[0]
# restituisce e non rimuove l'elemento in testa alla coda (restituisce Null se è vuota)

    def len (self):
        return len(self._data)
# restituisce il numero di elementi presenti nella coda

    def is_empty(self):
        return len(self._data) == 0
# verifica se la coda è vuota
```
## Coda doppia
Una coda doppia una struttura di dati che si compone come una coda ma che consente l'inserimento e la rimozione di elementi sia all'inizio che alla fine della coda. Potrebbe essere più semplice vederla come una lista in cui è possibile inserire il rimuovere elementi solo all'inizio o alla fine della lista.
### Operazioni
- **append(e)**: inserisce l’elemento e a destra della coda (fine della lista)
- **appendleft(e)**: inserisce l’elemento e a sinistra della coda (inizio della lista)
- **pop()**: rimuove e restituisce l'elemento a destra della coda (fine della lista), restituisce null se è vuota
- **popleft()**: rimuove e restituisce l'elemento a sinistra della coda (inizio della lista), restituisce null se è vuota
- **first()**: restituisce (non rimuovere) l’elemento a sinistra alla coda (inizio della lista), restituisce null se è vuota
- **last()**: restituisce (non rimuovere) l’elemento a destra alla coda (fine della lista), restituisce null se è vuota
- **is_empty()**: verifica se la coda è vuota
- **len()**: restituisce il numero di elementi presenti nella coda.

### Implementazioni
```python
class Deque:

    def __init__(self):
        self._data = deque()

    def append(self, e):
        self._data.append(e)
# inserisce l’elemento e a destra della coda (fine della lista)

    def appendleft(self, e):
        self._data.appendleft(e)
# inserisce l’elemento e a sinistra della coda (inizio della lista)

    def pop(self):
        if self.is_empty():
            return None
        return self._data.pop()
# rimuove e restituisce l'elemento a destra della coda (fine della lista), restituisce null se è vuota

    def popleft(self):
        if self.is_empty():
            return None
        return self._data.popleft()
# rimuove e restituisce l'elemento a sinistra della coda (inizio della lista), restituisce null se è vuota

    def first(self):
        if self.is_empty():
            return None
        return self._data[0]
# restituisce (non rimuovere) l’elemento a sinistra alla coda (inizio della lista), restituisce null se è vuota

    def last(self):
        if self.is_empty():
            return None
        return self._data[-1]
# restituisce (non rimuovere) l’elemento a destra alla coda (fine della lista), restituisce null se è vuota

    def len(self):
        return len(self._data)
# restituisce il numero di elementi presenti nella coda

    def is_empty(self):
        return len(self._data) == 0
# verifica se la coda è vuota
```

## Implementazioni dei dizionari
Le [[Funzioni Hash]] che associano stringhe ad interi sono utilizzate nell’implementazione dei dizionari sfruttando una lista di dimensione predefinita, come descritto in seguito:
- viene stabilita inizialmente una dimensione massima **n** per la lista;
- quando viene inserita una nuova chiave (ad esempio con la funzione set(k)=v) si calcola il valore hash **h** della chiave utilizzando una specifica funzione, quindi hash(), restituirà un intero tra 0 ed n-1
- la coppia [k, v] viene inserita quindi in posizione h della lista che contiene n-1 elementi

Esempio
- La coppia [ABC, 10] e hash(“ABC”) = 7 allora la coppia [ABC, 10] viene inserita nella posizione 7 della lista 
- La coppia [K2Fh9, 140] e hash(“K2Fh9”) = 2 allora la coppia [K2Fh9, 140]] viene inserita nella posizione 2 della lista 
![[Hash 2.png|200]]

Il vantaggio di implementare dei dizionari con le funzioni hash è nel ridotto costo delle funzioni di set(k), get(k) e del(k), in quanto non è necessario alcuno scorrimento della lista per effettuare ricerche di chiavi. Infatti quando si vuole cercare la posizione di una chiave k è sufficiente calcolare il valore hash della chiave attraverso funzione hash(k) e sappiamo subito la posizione della lista in cui si trova la chiave riducendo la complessità computazionale a O(1)
### Conflitti
I conflitti si generano quando una funzione hash restituisce due valori uguali per due chiavi differenti: 

**Esempio**
	La coppia [0H9, 12] ha un hash(“0H9”) = 2 già occupata dalla coppia [K2Fh9, 140] poiché hash(“K2Fh9”) = 2

Per risolvere il conflitto si utilizza il separate chaining
#### Separate chaining
Le due coppie [K2Fh9, 140] e [0H9, 140] sono inserite nella stessa posizione della lista, e ciò avviene creando una lista di coppie che viene inserita come elemento della lista principale. 

| Prima                                | Dopo                                    |
| ------------------------------------------------ | ------------------------------------------------ |
| ![[Hash 3.png]] | ![[Hash pool.png]] |

Quindi ogni elemento della lista principale contiene una lista di coppie; in tal caso ogni elemento della lista principale prende il nome di bucket.

In questo modo è possibile inserire un numero arbitrario di coppie nella lista principale qualunque sia la lunghezza n di tale lista.

Ma ciò comporta che per effettuare un operazione di ricerca di una coppia, dopo aver calcolato il risultato della funzione hash, occorre cercare all’interno della lista di coppie cui appartiene.

In conclusione la complessità delle operazioni di set(k), get(k) e del(k) dipende anche dalla lunghezza delle liste di coppie presenti nei bucket.

Quando i conflitti sono rari la complessità si avvicina ad O(1) , ma quando diventano frequenti può cresce fino a O(m), dove m è il numero di chiavi presenti.

Notiamo che aumentando il numero di bucket la complessità può diminuire, ma la memoria richiesta per mantenere la lista principale cresce, quindi la scelta di n determina un trade-off tra tempo e spazio.

Una funzione hash che distribuisce più uniformemente le chiavi sui bucket contribuisce a ridurre la complessità del caso peggiore in termini di tempo.