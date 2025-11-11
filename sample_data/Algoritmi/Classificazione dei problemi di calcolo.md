# Classificazione dei problemi di calcolo

## Classi di complessità computazionale
**P (Polynomial Time)**:
- è definita come la classe dei problemi risolubili attraverso un algoritmo di complessità polinomiale.
- oppure come classe dei problemi risolubili da una macchina di Turing deterministica in tempo polinomiale.
- tutti gli algoritmi trattati 

**NP (Non-deterministic Polynomial Time)**:
- è la classe dei problemi per i quali non esiste un algoritmo di complessità polinomiale in grado di risolverli, ma esiste un’algoritmo per verificare il risultato in tempo polinomiale
- oppure come la classe dei problemi risolubili da una macchina di Turing non-deterministica in tempo polinomiale.
- Problemi
	- Esistenza di un cammino hamiltoniano
	- Esistenza di una cricca di dimensione k 
	- Esistenza di un insieme indipendente di dimensione k 

**EXPTime (Exponential Time)**
- classe dei problemi risolubili attraverso un algoritmo di complessità O(2<sup>p(n)</sup>), con p(n) polinomio in n.
- Non esiste un algoritmo ne per risolverlo ne per verificare il risultato in tempo polinomiale
- Problemi:
	- problema del commesso viaggiatore 
	- Massima cricca
	- massimo insieme indipendente

### Rapporti di inclusione
**P ⊆ NP ⊆ EXPTime** 
![[rapporti di inclusione.png]]
Non sappiamo se P⊂NP o se P=NP

## Cammini Hamiltoniani
Un cammino Hamiltoniano è un cammino che tocca tutti i vertici del grafo una e una sola volta.
Un ciclo Hamiltoniano vi è se dato un cammino hamiltoniano, se vi è un arco che collega l'ultimo nodo con il primo nodo del cammino
![[Ciclo hamiltoniano.png]]
#### Possibile implementazione
Un algoritmo che risolve tale problema può essere costruito così:
1. Costruzione di tutte le possibili permutazioni dell’insieme dei nodi del grafo (complessità di n!)
2. Verifica, per ognuna delle permutazioni, se la sequenza di nodi della permutazione è un ciclo hamiltoniano, la complessità diventa polinomiale (dipende dalla quantità di nodi)
La complessità computazionale è O(n!)
### Problema del commesso viaggiatore
Il problema del commesso viaggiatore chiede di trovare il percorso più breve che passa attraverso n città e rientra alla città di partenza, toccando ogni città una sola volta. 
Le n città possono essere rappresentate dagli n nodi di un grafo e le strade che collegano coppie di città possono essere rappresentate tramite gli archi del grafo.
Per risolvere questo problema è possibile usare la ricerca dei cilci hamiltoniani di peso minore
##### Possibile implementazione
1. Fissa un nodo iniziale s.
2. Identifica tutti i cicli Hamiltoniani che iniziano in s e finiscono in s.
3. Calcola il peso di ciascun ciclo identificato.
4. Seleziona il ciclo con il peso minore tra quelli calcolati.
###### Complessità computazionale
Considerando il caso peggiore con un grafo completo:
   - Il numero di cicli Hamiltoniani possibili è (n−1)! (permutazioni dei nodi diversi da s).
   - La complessità dell'algoritmo sarà quindi O((n)!)
Non è stato ancora trovato un algoritmo di complessità polinomiale che lo risolva.

## Problema della cricca e problema dell’insieme indipendente
Tali problemi sono problema di ottimizzazione combinatoria che riguarda la ricerca della sottostruttura in un grafo completo.
### Problema della cricca
Dato un grafo G=(V, E) non orientato, si definisce cricca (clique) un sottoinsieme C di nodi di V tale che ogni nodo di C sia adiacente ad ogni altro nodo di C. (in giallo sono i sottoinsiemi)
![[Cricca.png]]
- **Problema della cricca di dimensione k**: verificare se un grafo contiene una cricca formata da k nodi
- **Problema della cricca di dimensione k**: trovare la cricca di un grafo con il maggior numero di nodi.

### Problema dell’insieme indipendente
Dato un grafo G=(V, E) non orientato, si definisce insieme indipendente (independent set) un sottoinsieme C di nodi di V tale che nessun nodo di C sia adiacente ad un altro nodo di C.
![[Insieme indipendente.png]]
- **Problema dell’insieme indipendente di dimensione k**: Verificare se un dato grafo contenga una insieme indipendente costituito da almeno k nodi.
- **Problema massimo insieme indipendente**: trovare l’insieme indipendente di un grafo con il maggior numero di nodi.

### Applicazioni nel [[Strutture dati avanzate|Grafo del mercato azionario]]
L’analisi delle cricche e degli insiemi indipendenti in un grafo di un mercato azionario può fornire queste informazioni:
- **una cricca** identifica un insieme di titoli i cui rendimenti cambiano in modo correlato:
	Quindi una variazione del rendimento di un titolo potrà molto probabilmente influenzare i rendimenti di tutti gli altri titoli delle cricche cui appartiene

- **un insieme indipendente** identifica un insieme di titoli i cui rendimenti variano principalmente in modo indipendente.
	Quindi i titoli di un insieme indipendente possono essere buoni candidati per costruire un portafoglio diversificato.


## [[Strutture dati avanzate|Grafo del mercato azionario]] 
Un'applicazione dei grafi è quella di costruire modelli per analizzare e comprendere le dinamiche delle correlazioni tra diversi titoli nel mercato azionario.

Un approccio per costruire un modello di questo tipo è il seguente:
- Ogni titolo disponibile sul mercato è rappresentato da un nodo nel grafo. Supponendo che ci siano n titoli, costruiamo quindi un grafo con n nodi.
- Indichiamo con C<sub>i,j</sub> un coefficiente che misura la correlazione tra i rendimenti dei due titoli i e j, per ogni coppia di titoli i e j con i≠j. Questi coefficienti C<sub>i,j</sub> sono compresi nell'intervallo [-1, 1].
- Fissiamo una soglia θ con un valore tra -1 e 1. Creiamo un arco tra i nodi i e j del grafo se e solo se il coefficiente C<sub>i,j</sub> è maggiore o uguale a θ.
![[grafo del mercato azionario.png]]


## Algoritmi Greedy (avidi)
Gli algoritmi greedy sono strategie di risoluzione di problemi che prendono decisioni localmente ottimali in ogni passo, sperando che queste decisioni portino a una soluzione globalmente ottimale. 
### Tecnica di hill climbing
#### Definizione
La tecnica di "hill climbing" è un algoritmo di ricerca e ottimizzazione che mira a trovare una soluzione migliore in uno spazio di ricerca attraverso passi incrementali.

#### Implementazione
Può essere usato per trovare il massimo di una funzione f(x), partendo da un valore casuale di x, mi sposto verso la direzione tale che x cresca sempre di più. Se x smette di crescere allora avrò identificato il suo massimo.
![[Greedy.png]]

#### Esempio
Esempio di un algoritmo di tipo greedy di complessità polinomiale che tenta di trovare una cricca di dimensione maggiore o uguale a k
1. Si sceglie casualmente un nodo u del grafo e lo si aggiunge a un insieme C (inizialmente vuoto).
2. Dato l'insieme dei nodi adiacenti ad u, indicato come Au:
	- Se A<sub>u</sub> contiene almeno un nodo v tale che C ∪ {v} forma una cricca (ossia v è adiacente ad ogni altro nodo in C), si sceglie casualmente uno dei nodi di A<sub>u</sub> che soddisfa questa condizione e lo si aggiunge all'insieme C.
	   - Altrimenti:
	     - Se la dimensione corrente di C è maggiore o uguale a k, allora l'insieme C è la cricca cercata.
	     - Altrimenti, si sceglie casualmente un nodo u tra quelli non ancora selezionati e si torna al punto 2.

#### Limitazioni
- **Convergenza al Massimo Locale**: L'algoritmo può restare bloccato in un massimo locale o in un minimo locale e non raggiungere il massimo globale o il minimo globale.
- **Sensibilità all'Inizializzazione**: La soluzione iniziale può influenzare notevolmente il risultato finale.
- **Manca di Esplorazione Globale**: L'algoritmo si concentra su piccoli cambiamenti locali e potrebbe non esplorare soluzioni più distanti che potrebbero portare a un risultato migliore.
