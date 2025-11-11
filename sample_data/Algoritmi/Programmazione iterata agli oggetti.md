# Programmazione orientato agli oggetti
Python consente l'utilizzo della programmazione orientata agli oggetti, tramite il quale possiamo creare, oltre a variabili e funzioni, anche oggetti. 
Un oggetto può contenere dati e può disporre di un insieme di funzioni, che hanno l'obiettivo primario di operare sui dati dell'oggetto stesso.
	Definizione utile
	**Istanza** = singolo oggetto creato a partire da una classe

## Classi
Una **classe** è una struttura astratta che definisce un tipo di oggetto. Quindi è un modello che specifica gli attributi e i metodi che gli oggetti di quella classe avranno, definendo come dovrà essere creato un'oggetto
- **attributi**: variabili che contengono i dati dell'oggetto
	- **Attributi di istanza**: sono specifici di un'istanza particolare di una classe.
	- **Attributi di classe**: sono condivisi da tutte le istanze di una classe
- **metodi**: funzioni all'interno delle classi che operano sui dati 

```python
class nome_classe:
	#indentiamo e inseriamo i primi attributi 
	nome_attributo = variabile #attributo di classe
	# creiamo un metodo
	def metodo(...):
		print(questo è un metodo)

```

```python
class SavingsAccount:
	balance = 0.0
	
	def deposit(...): 
		...
	def withdraw(...): 
		...
	def compute_interest(...): 
		...
```

## Oggetti
Un **oggetto** è un'istanza di una classe. É creato utilizzando la definizione della classe come modello e contiene valori specifici per gli attributi definiti dalla classe. Gli oggetti sono le entità attive che possono eseguire operazioni definite dai metodi della classe (nel codice sono esterne alla classe).

```python
#comandi base
#creo un oggetto di una classe
nome_oggetto = NomeClasse() 

#accedere ad un'attributo di un'oggetto
nome_oggetto.nome_attributo

#eseguo un metodo di un'oggetto
nome_oggetto.nome_metodo()

#eliminare un oggetto
del nome_oggetto
```

Ogni oggetto creato avrà i suoi dati memorizzati in variabili proprie, in quanto ogni oggetto dispone di una copia privata di ogni attributo, il cui valore può variare indipendentemente dagli altri oggetti. Infatti ogni oggetto è contenuto in un area di memoria differente

```python 
savings_account_1 = SavingsAccount() 
value = savings_account_1.balance 
print(value)

```

## Utilizzo della programmazione orientato agli oggetti
### Implementazione dei metodi
Seguendo l'esempio della banca, implementiamo una funzione *deposit* che aggiunge una quantità di denaro al saldo del conto deposito:

```python
class SavingsAccount:
	balance = 0.0
	
	def deposit(self,amount): # Header
		self.balance += amount # Corpo
```

**Header**
- self = Il primo parametro di input di un metodo, python passa automaticamente l'istanza dell'oggetto (cioè l'oggetto stesso) come primo argomento al metodo. Attraverso `self`, possiamo accedere e manipolare gli attributi e i metodi dell'oggetto all'interno del metodo.
- amount = indica la quantità di denaro da aggiungere
**Corpo**
- self.balance += amount = denota l’attributo balance dell’oggetto referenziato da self, quindi l’istruzione self.balance+= amount aggiunge la quantità di denaro amount alla variabile balance dell’oggetto 


### Creazione di un oggetto ed utilizzo di un metodo
Creiamo un oggetto della classe SavingsAccount, chiamandolo savings_account_1, ed aggiungiamo una quantità di denaro pari a 10 chiamando il metodo deposit

```python
class SavingsAccount: 
	balance = 0.0
	def deposit(self, amount): 
		self.balance+= amount

#Creiamo l'oggetto saving account con balance = 0.0
savings_account_1 = SavingsAccount() 

#Aggiunge all’attributo balance dell’oggetto savings_account_1 il valore 10 
savings_account_1.deposit(10) #con il punto possiamo accedere agli atttributi 
```

Questo processo lo possiamo ripetere per tutte le istanze che desideriamo 

**Nota importante**
	Riprendendo la definizione di self; l’oggetto su cui viene effettuata la chiamata diventa il primo parametro del metodo. Quindi savings_account_1 risulterà il primo parametro su cui farà riferimento self. 


### Metodo del costruttore
Il metodo del costruttore viene automaticamente chiamato ogni volta che viene chiamato un'oggetto, ed è utile per creare ed inizializzare gli attributi di un'oggetto con più flessibilità, con questo metodo stabiliamo gli attributi di istanza

```python
class SavingsAccount: 
	def __init__(self): #notazione per definire il costruttore
		self.balance = 0.0 #attributo di istanza
		self.rate = 0.0 #attributo di istanza
```

È possibile inizializzare un attributo anche con un valore che viene stabilito al momento della creazione dell’oggetto
A tal fine si può aggiungere un parametro al costruttore, il cui valore sarà passato al momento della creazione dell’oggetto.
```python
class SavingsAccount: 
	def __init__(self, account_rate): #notazione per definire il costruttore, aggiungiamo accont_rate come parametro
		self.balance = 0.0 
		self.rate = account_rate #sarà definito al momento della creazione dell'oggetto

savings_account_1 = SavingsAccount(4.0) # 4.0 verrà assegnato a rate
```
SavingsAccount(4.0) avrà  balance = 0.0 e rate = 4.0  

Inoltre è possibile creare una lista nel costruttore:
```python
class SavingsAccount: 
	def __init__(self): 
		self.balance = 0.0 
		self.rate = 0.0
		self.operations = [] #creo una lista vuota
```

### Ereditarietà
Il meccanismo di ereditarietà consente di creare una classe (B) basata su un’altra classe (A), cioè di creare una classe B che include gli attributi e i metodi già definiti in un’altra classe A e che aggiunge ulteriori attributi e/o metodi.
In tal caso la classe A sarà detta superclasse (o classe base o genitore) e la classe B sarà detta sottoclasse (o classe derivata, figlia o discendente)

```python
class nome_classe:

class nome_sottoclasse (nome_classe):
```
La sottoclasse erediterà le caratteristiche della classe genitore (inserita tra parentesi)

In questo caso la superclasse è la classe SavingsAccount che abbiamo già creato, la  sottoclasse di SavingsAccount sarà GoldSavingsAccount.
```python
class GoldSavingsAccount(SavingsAccount):
	gold_rate = 0.0
```

Se volessimo sostituire un metodo o un'attributo presente nella classe genitore con uno nella classe figlia, basta dargli lo stesso nome, e quest'ultimo verra automaticamente sostituito quando verrà chiamato su un oggetto della classe GoldSavingsAccount

```python
class GoldSavingsAccount(SavingsAccount):
	gold_rate = 0.0
	def compute_interest(self):  
		interest = self.balance * self.rate/100 
		if self.balance > 250:
			interest += self.balance * self.gold_rate / 100
		self.deposit(interest)
```

L'ultimo passaggio è creare un nuovo costruttore specifico

```python
class GoldSavingsAccount(SavingsAccount):
	gold_rate = 0.0
		
	def __init__(self, gold_account_rate, account_rate):
		super().__init__(account_rate) #inizializziamo prima gli attributi della superclasse
		self.gold_rate = gold_account_rate #inizializza i suoi attributi
		
	def compute_interest(self):  
		interest = self.balance * self.rate/100 
		if self.balance > 250:
			interest += self.balance * self.gold_rate / 100
		self.deposit(interest)
```




