# Algoritmo di ricerca
## Ricerca sequenziale
Funzione per la ricerca dell’elemento k nella lista l

```python
def ricerca_sequenziale(l, k):
	i = 0 ##inizializza una variabile 0, variabile utilizzata per accedere agli altri elementi della lista l
	while i < len(l): #eseguito fintanto che i è minore della lunghezza della lista l
		if l[i] == k:  ##controlla se l'elemento all'indice i è uguale a k
			return True
		i=i+1 #aumenta i di 1 per per passare all'elemento successivo
	return False
```
La complessità della ricerca sequenziale è **O(n)**.

## Ricerca sequenziale su lista ordinata
Funzione per la ricerca dell’elemento k nella lista l sapendo che la lista è ordinata

```python
def sequential_search_with_ordered_list(input_list, k):
    i = 0
    while i < len(input_list):
        if input_list[i] == k:
            return True
        if input_list[i] > k: #Se raggiungo un elemento della lista chè è maggiore di k sicuramente k non sarà nella lista, quindi esco dalla funzione restituendo False
            return False
        i = i + 1
    return False
```
Differentemente dalla ricerca sequenziale, il caso medio migliora, ma la complessità rimane **O(n)**.

## Ricerca binaria
Se la lista è ordinata è possibile usare la ricerca binaria (o dicotomica)
```python
def binary_search(l, k):
    low = 0                   # Inizializza l'indice inferiore
    high = len(l) - 1         # Inizializza l'indice superiore

    while low <= high:        # Continua finché l'indice inferiore è minore o uguale all'indice superiore
        middle = (low + high) // 2  # Calcola l'indice medio

        if l[middle] == k:    # Se l'elemento al centro è uguale a k, restituisci True
            return True

        if k < l[middle]:     # Se k è più piccolo dell'elemento al centro
            high = middle - 1 # Imposta l'indice superiore a medio - 1
        else:
            low = middle + 1  # Altrimenti, imposta l'indice inferiore a medio + 1

    return False             # Se il ciclo while termina senza trovare k, restituisci False

```
Se l'algoritmo non trova l'elemento richiesto, l'algoritmo si ferma perché high e low si andranno ad  equiparare
### Complessità
Assumendo che n sia pari, ad ogni iterazione del ciclo while la ricerca si restringe ad una sottolista di ampiezza al massimo pari alla metà (n/2) della lista dell’iterazione precedente
![[Merge Sort.png]]
Una ricerca corrisponde ad effettuare un percorso che va dalla radice verso le foglie fino a che:
- il valore k viene trovato oppure  
- raggiungo una foglia senza aver trovato il valore k

Poiché l’albero è bilanciato l’altezza dell’albero sarà log<sub>2</sub>n, quindi il ciclo **while** effettuerà al log<sub>2</sub>n. Quindi la complessità **O(log<sub>2</sub>n)**
Nel caso n sia dispari, ad ogni iterazione del ciclo while la ricerca si restringe ad una sottolista di ampiezza al massimo pari a n/2 arrotondato all’intero superiore, ma ciò non influisce sulla complessità, quindi per ogni n la complessità è **O(log<sub>2</sub>n)**
