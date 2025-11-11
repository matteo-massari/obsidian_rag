# Funzioni Hash
Una funzione hash associa ad ogni elemento k del dominio un elemento v di un codominio di dimensioni limitate
L’elemento k viene è chiave, il dominio è detto universo delle chiavi, l’elemento associato v è detto valore hash

Sono particolarmente utili le funzioni hash in cui:
- il dominio include stringhe di lunghezza arbitraria 
- il codominio:
	1) corrisponde all’insieme dei numeri interi compresi tra 0 ed un valore massimo prefissato
	2) corrisponde all’insieme delle stringhe di lunghezza prefissata

![[Domini hash.png]]


## Funziona hash tra numeri interi
Questa funzione hash associa un elemento del dominio degli interi ad un elemento del sottoinsieme degli interi compresi tra 0 e n-1:

**f(k) = k mod n** 

**nota bene**
	- tale funzione associa un qualsiasi numero intero k al risultato dell’operazione k mod n)
	- l’operazione k mod n in Python corrisponde all’operazione k%n che calcola il resto della divisione tra gli interi k e n.

Esempio
f(k) = k mod 4 con codominio = {0, 1, 2, 3}

## Funzioni hash tra numeri interi con le stringhe
Questa funzione hash associa un elemento del dominio delle stringhe ad un elemento del sottoinsieme degli interi compresi tra 0 e n-1:

**f(k) = unicode_sum(k) mod n**

unicode_sum(k) è una funzione che associa alla stringa k la somma dei valori interi corrispondenti ai codici unicode di ogni carattere della stringa

**Esempio**
f(k) = unicode_sum(k) mod 6 con codominio = {0, 1, 2, 3, 4, 5}

```python
def my_hash_function(s): 
	unicode_sum = 0  
	for i in range(len(s)):
		unicode_sum += ord(s[i])
	return unicode_sum % 6
```

## Funzioni hash tra stringhe
Questa funzione hash associa un elemento del dominio delle stringhe ad un elemento del sottoinsieme delle stringhe di lunghezza 1 composte dai caratteri {c,d,e,f,g,h}

**f(k) = char( (unicode_sum(k) mod 6) + 99)**

unicode_sum(k) è una funzione che associa alla stringa k la somma dei valori intero corrispondente al codice unicode di ogni carattere della stringa e chr() è la funzione che associa all’intero corrispondente ad un codice unicode il relativo carattere

**Esempio**
``` python
def my_hash_function(s):
	unicode_sum = 0  
	for i in range(len(s)):
		unicode_sum += ord(s[i])
		return chr((unicode_sum % 6) + 99)
```
