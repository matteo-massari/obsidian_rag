# Clustering 
Il clustering suddivide gli elementi (vettori o punti n-dimensionali) di un dato insieme (dataset) in k gruppi (cluster) distinti in modo che elementi che mostrano caratteristiche simili vengano raggruppati insieme ed elementi che mostrano caratteristiche poco simili vengano messi in gruppi diversi, per qualche misura di similarità. 
Le caratteristiche sono rappresentate dalle componenti dei vettori o dalle n dimensioni.

La similarità tra due punti viene quantitativamente stimata attraverso una specifica metrica, calcolata attraverso funzione definita su due punti. (distanza euclidea) 

Una volta stabilito come assegnare i punti ad ognuno dei k cluster si può calcolare la similarità di un cluster come somma delle similarità tra ogni punto del cluster ed uno specifico punto, detto **centroide** del cluster.  
Inoltre si può calcolare la similarità di un assegnazione di tutti i punti ai k cluster come somma delle similarità dei k cluster.
![[Raggruppamento.png]]

Il problema è detto intrattabile perché, non è noto alcun algoritmo di complessità polinomiale, in quanto la ricerca della soluzione ottima potrebbe richiedere tempi eccessivamente lunghi.
Quindi ricorriamo agli **algoritmi euristici**, i quali non identificano la soluzione ma tentano di trovare la soluzione più vicina a quella ottima (garantendo una complessità bassa).


## K-means
K-means è un algoritmo di clustering euristico utilizzato per raggruppare un insieme di dati in gruppi omogenei chiamati cluster
L'obbiettivo principale è minimizzare la somma dei quadrati delle distanze tra ciascun punto e il centroide del cluster a cui appartiene, quindi la sua metrica è nota come "Within-Cluster Sum of Squares" (WCSS), la quale a sua volta sfrutta la distanza euclidea 
$$
\text{Distanza Euclidea} = \sqrt{(x_1 - c_1)^2 + (x_2 - c_2)^2} 
$$
Dove x<sub>1</sub>​ e x<sub>2</sub> sono le coordinate del punto x nei due attributi, e c<sub>1​</sub> e c<sub>2​</sub> sono le coordinate del centroide c nei due attributi.

### Funzionamento
1) Si sceglie il dataset
2) Si fissa a priori il numero K di cluster (centroidi) desiderati (elbow method)
3) si fissa l'errore massimo (differenza tra l'errore corrente e l'errore precedente)
4) L'algoritmo quindi:
	- Fissa per la prima iterazione l'errore precedente a 0 
	- Sceglie k punti casualmente, cioè i centroidi iniziali
	- Inizia l'iterazione
		1) ogni punto del dataset viene assegnato al centroide più vicino
		2) Calcola l'errore corrente come somma degli errori di ogni cluster
		3) L'errore precedente nella prima iterazione è 0
		4) Se l'errore massimo è inferiore ad una soglia prestabilita l’algoritmo termina
		5) in caso contrario:
			- il valore dell'errore corrente è assegnato a errore precedente  
			- si calcolano le nuove posizioni dei centroidi di ogni cluster. Questi nuovi centroidi sono calcolati prendendo la posizione media (il valore medio) di tutte le posizioni dei punti assegnati al cluster.
### Implementazioni
#### K-means
```python
def kmeans(data_set, number_of_clusters, max_error=1e-4):
# - data_set: contiene i dati di input
# - number_of_clusters: è il numero di cluster da formare
# - max_error è il massimo valore accettato della differenza tra l’errore precedente e l’errore corrente

#Inizializzazione dell'algoritmo:
# - L'errore precedente viene impostato a 0 per la prima iterazione
    previous_error = 0 
# - Lo stop è impostato a False
    stop = False #Diventa True quando l'algoritmo deve terminare
    current_iteration = 0 #Contatore delle iterazioni
# - Viene creato un array "centroids" vuoto con dimensioni [number_of_clusters, 2]
    centroids = np.empty([number_of_clusters, 2]) #Memorizza le coordinate dei centroidi 

#Inizzializzazione dei centroidi:
    i=0 #indice per accedere alle posizioni dei centroidi
    
# - Sceglie casualmente i centroidi tra gli elementi del set di dati
    random_indexes = random.sample(range(0, len(data_set)), number_of_clusters) # random_indexes crea un vettore che conterrà un campione casuale (random.sample) di indici dai dati nel range da 0 a `len(data_set)`

# - Assegna le coordinate:
    for index in random_indexes: #iterizza su random_indexes
        centroids[i] = data_set[index] #assegna in indice i, alla lista centroids, il dato presente nel data_set con index iterato dal for in random_indexes 
        i+=1 #aumenta di uno il counter
        
# Iterazione principali dell'algoritmo:
    while True:
        current_iteration += 1

# - Assegna i centroidi e calcola gli errori di assegnazione (formula euclidea)
        assigned_centroids, assignment_errors = centroid_assignation(data_set, centroids)

# - verifica se la differenza tra l’errore corrente e precedente è sotto la soglia di tolleranza scelta
        total_error = sum(assignment_errors) # calcola l'errore totale effettuando la somma di tutti gli errori
		
	# - Controlla se L'errore è sotto la soglia minima:
		# - abs calcola il valore assoluto della differenza tra l'errore precedente e l'errore corrente
        if abs(previous_error - total_error) <= max_error: 
	        
	    # - se l'errore è minore dell'errore massimo il while si ferma
            break 
        
        #In caso contrario:
        # - l'errore attuale diventa l'errore precedente
        previous_error = total_error 
        
        # - ricalcola la posizione dei centroidi
        centroids = calculate_new_centroid_positions(data_set, assigned_centroids, number_of_clusters) #richiama la funzione scritta sotto
	
	# Se l'errore minimo è sceso sotto una certa soglia, il break chiude il ciclo while
    return centroids, assigned_centroids, assignment_errors  
    # restituisce la lista dei centroidi, dei centroidi assegnati ad ogni punto e i relativi errori di assegnazione

```

#### Assegnazione dei centroidi
La funzione centroid_assignation assegna ad ogni punto il centroide più vicino secondo una specifica metrica di errore, tale funzione è richiamata nel Kmeans 
```python
def centroid_assignation(data_set, centroids): 
# - centroids è la lsita dei centroidi con le relative coordinate creata dalla funzione kmeans
    
# Inizzializzazione
    n = len(data_set) # calcola la lunghezza del dataset
    assigned_centroids = np.empty(n, dtype=int) #conterrà la lista dei centroidi assegnati ad ogni punto
    assignment_errors = np.empty(n, dtype=float) #conterrà gli errori di assegnazione dei centroidi per ogni punto


# Iterazione su ogni punto nel dataset per assegnare il centroide più vicino e calcolare l'errore: 
	# Prima iterazione: 
	# - Itera su tutto il data_set, confrontando solamente il primo centroide 
    for i in range(len(data_set)):
        nearest_centroid = 0 # Inizialmente l'indice del centroide più vicino è 0
    
	# - Per ogni punto, viene calcolata la distanza (errore) tra il punto stesso e tutti i centroidi presenti.
        nearest_centroid_error = np.sqrt(np.sum((centroids[0, :] - data_set[i, :]) ** 2)) #formula per il calcolo dell'errore
        
        # Seconda iterazione:
        # - Itera sui restanti centroidi 
        for j in range(1, len(centroids)): # quell'uno in range esclude il primo, che è già stato considerato nella prima iterazione
        
        # - Calcola l'errore tra il punto considerato e ciascun centroide
            error = np.sqrt(np.sum((centroids[j, :] - data_set[i, :]) ** 2))

		# - confronta gli errori con un if per trovare il centroide più vicino
            if error < nearest_centroid_error: # Se l'errore calcolato per un centroide è inferiore all'errore precedentemente calcolato per altri centroidi: 
            # Assegnazioni Finali:
            # - Il centroide attuale diventa il nuovo centroide più vicino al punto considerato
                nearest_centroid = j 
	            
            # - L'errore associato al nuovo centroide più vicino viene aggiornato
                nearest_centroid_error = error

	# Assegnazioni definitive 
	# Terminto il ciclo for:
	# - viene assegnato con l'indice del centroide più vicino
        assigned_centroids[i]=nearest_centroid 
    # - viene assegnato con l'errore associato al centroide più vicino
        assignment_errors[i]=nearest_centroid_error
	
	# restituisce una lista con i centroidi assegnati per ogni punto e la lista dei relativi errori
    return assigned_centroids, assignment_errors
```

#### Calcolo la nuova posizione dei centroidi
L'ultima funzione richiamata dal k-means calcola la posizione dei centroidi effettuando la media dei valori delle coordinate dei punti assegnati al centroide
```python
def calculate_new_centroid_positions(data_set, assigned_centroids, number_of_clusters):

	# Inizzializzazione: 
	# - #crea una matrice vuota chiamata `sum_of_positions` con dimensioni `[number_of_clusters, 2]`. Sarà utilizzata per accumulare le posizioni dei punti assegnati a ciascun centroide.
    sum_of_positions = np.zeros([number_of_clusters, 2],float) 

	# - crea un vettore chiamato `points_per_centroids` con dimensione `number_of_clusters`, inizializzato con tutti zeri. è utilizzato per tenere traccia di quanti punti sono stati assegnati a ciascun centroide.
    points_per_centroids = np.zeros(number_of_clusters,int) 


    # Somma delle posizioni dei punti in base ai centroidi assegnati:
    # - scorre attraverso ogni punto nel dataset
    for i in range(len(data_set)):
    # - Somma le posizioni
        sum_of_positions[assigned_centroids[i]]+= data_set[i] 
    # - Conta i punti assegnati per ogni centroide   
        points_per_centroids[assigned_centroids[i]] += 1 

    # Calcolo delle nuove posizioni dei centroidi:
    # - inizializza la matrice con dimensioni `[number_of_clusters, 2]`
    centroid_positions = np.zeros([number_of_clusters, 2], float)
	
	# - Calcola la somma media cosi da assicurarci che i centroidi si spostino gradualmente verso il centro effettivo dei punti all'interno del cluster, migliorando così la precisione del clustering (rende l'algorimto più efficace)
    for i in range(number_of_clusters):
    
    # - dividendo la somma delle posizioni dei punti assegnati per un centroide per il numero di punti assegnati a quel centroide.
		centroid_positions[i]=sum_of_positions[i]/points_per_centroids[i]
     
     #Contiene le nuove posizioni calcolate per i centroidi in base ai punti assegnati.   
    return centroid_positions 
```

#### Output
I punti sono assegnati tramite gli indici.
**Centroidi finali**:
I centroidi finali rappresentano il centro di ciascun cluster identificato dall'algoritmo K-Means. In questo esempio, sono presenti due centroidi e sono rappresentati come coppie di coordinate (x e y):
```python
# In questo esempio abbiamo solo due K, (k = 0, K = 1)
[[1.5, 2.5] [6.33333333, 7.33333333]]
```

**Assegnazione dei punti ai centroidi**:
Ogni punto nel dataset è stato assegnato a uno dei cluster identificati. Gli indici rappresentano l'appartenenza dei punti ai cluster. Ad esempio, il primo punto è stato assegnato al cluster 0, il secondo punto al cluster 0 e così via:
```python
[0 0 1 1 1]
```

**Errori di assegnazione**:
Gli errori di assegnazione sono calcolati come la distanza tra ciascun punto e il centroide al quale è assegnato. Ad ogni errore è associato l'indice del punto nel dataset. Ad esempio:
```python
[0.5 0.5 0.33333333 0.33333333 1.33333333]
```

### Complessità 
La complessità dell’implementazione che abbiamo visto è O(t  n  k  d):
- t è il numero di iterazioni effettuate  
- n è il numero di punti  
- k è il numero di cluster da formare  
- d è la dimensione dei punti

Il ciclo principale (istruzione `while True:`) viene eseguito t volte;
- la funzione `centroid_assignation` contenuta nel ciclo principale esegue:
	- un ciclo `for` (istruzione: `for i in range(len(data_set)):`) che viene eseguito n volte
	- un ciclo `for` innestato (istruzione: `for j in range(1, len(centroids)):`) che viene eseguito k volte

- La funzione `calculate_new_centroid_positions` contenuta nel ciclo principale esegue:
	- un ciclo `for` (istruzione: `for i in range(len(data_set)):`) che viene eseguito n volte
	- un ciclo `for` (istruzione: `for i in range(number_of_clusters):`) che viene eseguito k volte

## Metodo del gomito
Il metodo del gomito è usato per stabilire il numero ideale di cluster da utilizzare
### Funzionamento 
- Si calcola la metrica di errore al variare del numero di cluster, ossia si esegue l’algoritmo con un numero di cluster pari a 1, 2, 3, ..., n e si genera il grafico con la relativa curva
- Quando la curva decresce in modo quasi lineare, si ottiene il numero ottimale dei cluster
![[Metodo del gomito.png|400]]