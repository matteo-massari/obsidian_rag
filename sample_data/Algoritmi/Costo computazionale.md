# Costo computazionale
Il costo computazionale è la quantità di risorse utilizzante durante l'esecuzione.
Le due tipologie di risorse:
- Tempo: quantità di tempo richiesto per l’esecuzione
- Spazio : quantità di memoria necessaria

I casi di riferimento sono sono: 
- Per il Tempo:  
	- **caso peggiore**: costo maggiore per usare un determinato input
	- caso migliore: costo minore per usare un determinato input
	- **caso medio**: media del tempo utilizzato al variare dell’input

- Per lo Spazio:  
	- **caso peggiore**: maggiore spazio di memoria utilizzata
	- caso migliore: minore spazio di memoria utilizzata
	- caso medio: media dello spazio utilizzato al variare dell’input

## Tempo
Il tempo di esecuzione può essere calcolato come somma dei tempi per eseguire tutte le istruzioni per completare l'esecuzione di un programma, e dipende da vari fattori tra cui:
- la costruzione fisica della macchina 
- i dati di input del programma
Dato che questi fattori sono variabili, per il calcolo si ricorre a modelli di costo che non dipendono da una specifico calcolatore su cui sarà eseguito il programma. Questo modello è basato sulla definizione di una macchina ideale sulla quale, per le caratteristiche fisiche, il tempo di esecuzione di ogni istruzione è fisso e noto a-priori, mentre per le basi di input è basato sulla stima della complessità nel caso peggiore, medio o migliore al variare della dimensione dei dati di input.

Costo:
- **istruzioni semplici** con costo pari ad 1:
	- istruzioni di assegnazione (ad esempio x = 10)  
	- espressioni (ad esempio x=a+b) 
	- istruzioni (ad esempio assert, pass, del, print, return, yield, raise, break, continue)
- le **istruzioni condizionali** hanno il costo della condizione + if se è vero/else se è falso
- l'**esecuzione di un ciclo** (come for, while) ha un costo pari al costo della condizione di uscita moltiplicato il numero di volte che viene valutata + il costo del blocco moltiplicato il numero di volte che viene eseguito

### Esempio con la ricerca sequenziale
Questa funzione cerca dalla lista I l'elemento k
``` python
l = [30, 3, 62, 17, 34, 5, 96] 
k = 30
```

``` python
def ricerca_sequenziale(l,k):
    i=0
    trovato=False
    while i<len(l):#finché la variabile di len è minore di l (numero) l'esercizio salta
        if l[i]==k:
            trovato=True
            break #per uscire dal ciclo while
        i=i+1
    return trovato
```

Per questo programma, ho bisogno dello specifico valore della lista per i cicli k quindi se ho una lista di 7 elementi avrò un costo computazionale iniziale è pari a 7 
Il programma mi chiede quando k è uguale a tale valore 30, che essendo il primo valore, mi valore mi dara uno sforzo computazionale minimo

``` python
l = [30, 3, 62, 17, 34, 5, 96] 
k = 30


def ricerca_sequenziale(l,k):
    i=0                   #costo 1*1 = 1
    trovato=False         #costo 1*1 = 1
    while i<len(l):       #costo 1*1 = 1
        if l[i]==k:       #costo 1*1 = 1
            trovato=True  #costo 1*1 = 1
            break         #costo 1*1 = 1
        i=i+1             #costo 1*0 = 0 
    return trovato        #costo 1*1 = 1 
                          #costo totale = 7 
``` 

Infatti se dovessi cambiare K, il costo computazionale aumenterebbe. Quindi il caso meno costoso è il caso in qui devo cercare il primo elemento, al contrario se devo cercare un'elemento che non esiste il costo è massimo.

``` python
l = [30, 3, 62, 17, 34, 5, 96] 
k = 96


def ricerca_sequenziale(l,k):
    i=0                   #costo 1*1 = 1
    trovato=False         #costo 1*1 = 1
    while i<len(l):       #costo 1*7 = 7
        if l[i]==k:       #costo 1*7 = 7
            trovato=True  #costo 1*1 = 1
            break         #costo 1*1 = 1
        i=i+1             #costo 1*6 = 6
    return trovato        #costo 1*1 = 1 
                          #costo totale = 25
``` 

##### Ricerca del caso peggiore
``` python

# n è la dimensione della lista

def ricerca_sequenziale(l,k):
    i=0                   #costo 1*1 = 1
    trovato=False         #costo 1*1 = 1
    while i<len(l):       #costo 1*(n+1) = n+1
        if l[i]==k:       #costo 1*n = n
            trovato=True  #costo 1*0 = 0
            break         #costo 1*0 = 0
        i=i+1             #costo 1*n = n
    return trovato        #costo 1*1 = 1 
                          #costo totale = 3*n+4
``` 

### In conclusione
Per il calcolo del costo = costo * volte = costo comando 
**Esempio**
7 è il costo minimo e 25 è il costo massimo, di conseguenza 16 sarà il costo medio
Ma dato che i numeri sono infiniti per avere questa stima impongo un range di valori. Mentre se questo è impossibile, dobbiamo usare una somma pesata per valutare il costo medio;

$$
\sum_{i=0}^{m-1}p_{i}\times c_{i}
$$
- p = la probabilità di scegliere k uguale all’i-simo elemento di una lista di m di elementi
- c = il costo per cercare l’i-simo elemento della lista

### Esempio con le matrici 
```python
def somma_di_matrici(m1, m2): 
	m_ris = []  #costo 1·1 volta = 1
	for i in range(0, len(m1)): #costo 1·n volte = n
		nuova_riga = []  #costo 1·n volte = n
		for j in range(0, len(m1[0])): #costo 1 · n volte · n volte = n·n
				nuova_riga.append(m1[i][j] + m2[i][j]) #costo 1 · n volte · n volte = n·n
			m_ris.append(nuova_riga) # costo 1·n volte = n
	return m_ris #costo 1·1 volta = 1
	#costo totale: 2·n 2+3·n+2
```
La doppia moltiplicazione nel secondo for é giustificata in quanto, ogni volta che viene eseguito il primo blocco viene seguito il secondo blocco per un numero n di volte (il numero delle variabili.) 
![[Costo del caso peggiore.png]]
## Costo asintotico e notazione asintotica
Lo studio del costo asintotico consiste nell'analizzare il comportamento della funzione di costo “o” al crescere della dimensione dell'input al fine di identificare una funzione asintotica che la limiti superiormente. “n” è la dimensione degli input, quindi studiamo cosa accade al crescere di “n”.
### Definizione
Siano f(n) e g(n) due funzioni positive definite per n positivo.  
F(n) ∈ O(g(n)), oppure diciamo che f(n) è un O-grande di g(n) se e solo se esistono due valori c>0 e n<sub>0</sub> ≥ 0 tale che per ogni n≥ n<sub>0</sub> si ha: f(n) ≤ c * g(n)

Al crescere di n la funzione tende a diventare minore o uguale a c * g(x) per un qualsiasi valore di c e che rimanga tale (non ci interessano gli specifici valori di c e n<sub>0</sub> che rendono vera la condizione).

Quando la complessità di un algoritmo è elevata il tempo atteso di esecuzione del programma potrebbe diventare eccessivamente elevato al crescere della dimensione dell'input. Un problema si definisce trattabile se esiste un algoritmo in grado di risolverlo di complessità al massimo polinomiale.
### Pratica
Nella pratica, è sufficiente un approccio che stima un ordine di riferimento per complessità, quindi evitando la complessità esatta e quindi tutta la parte teorica:
1. calcolare la funzione f(n) del caso peggiore
2. per ottenere una funzione g(n) si eliminano i termini costanti, e il termini n con la potenza inferiore al n massimo
3. O(g(n)) è l'ordine di riferimento stimato

![[Complessità.png|600]]
