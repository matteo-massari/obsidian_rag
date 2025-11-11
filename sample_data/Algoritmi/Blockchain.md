# Blockchain
La **blockchain** è una struttura, fa rifermento anche alla tecnologia che consente la creazione e la gestione di registri elettronici distribuiti.
![[Blockchain.png|400]]

## Nascita della blockchain
La blockchain nasce dalla necessita di una moneta che fosse:
- esclusivamente elettronica
- peer-to-peer

Nel 2008, fu pubblicato un articolo da Satoshi Nakamoto, pseudonimo di un autore sconosciuto, che introdusse la **tecnologia blockchain** come base per implementare una moneta elettronica peer-to-peer, dando vita nel gennaio 2009 al Bitcoin.

## Doppia spesa
Nei pagamenti in forma elettronica senza intermediari, un problema al quale si può incorrere è la doppia spesa: quando un utente tenta di spendere gli stessi fondi digitali due volte.

La blockchain ha attuato delle manovre di sicurezza per evitare ciò:
- **Documento elettronico firmato digitalmente**; 
	il quale attesta il pagamento e chi lo esegue e lo percepisce
- **Registro pubblico distribuito**;
	cosi da permettere a tutti gli utenti di verificare i documenti e quindi i precedenti trasferimenti ed essere sicuri che la controparte possa sostenere il pagamento 
- **Catena dei blocchi**:
	- **Indica una linea temporale dei pagamenti**
		Stabilendo l'ordine dei documenti è possibile far fronte a possibili doppie spese avvenute nello stesso momento, Infatti per regola il nuovo proprietario è colui che ha ricevuto i digital coins attraverso la transazione che è stata registrata prima nel registro. 
	- **Crea un registro inalterabile**
		Il registro pubblico potrebbe essere vulnerabile a azioni malevoli dagli utilizzatori (i quali essendo un registro publico, ha i dati che possono essere modificati o eliminati) , minando la linea temporale dei pagamenti.

## Funzionamento
### Registro pubblico distribuito
Un registro publico è un sistema che contiene tutti i documenti relativi alle transazioni effettuate. I digital coins si considerano effettivamente trasferiti solo se il documento relativo alla transazione è presente nel registro.  
Per evitare che i registri pubblici possano andare offline, si ricorre a registri elettronici distribuiti. Tali sistemi sono composti da più server presenti su una rete (che quindi possono comunicare tra loro), ognuno dei quali ospita un copia di tutti (o di parte) dei documenti. I documenti quindi sono condivisi e disponibili su più server.

I server costituiscono i “nodi” del registro distribuito, e consentono a chiunque di collegare un proprio calcolatore al registro così da farlo diventare uno dei server del registro, si rafforza la decentralizzazione del registro ed il modello di gestione è equo per tutti, in quanto a tutti è concesso di avere un ruolo pari agli altri.

### Catena dei blocchi
Il registro pubblico è formato da una catena di blocchi:
1) le transazioni vengono inserite in blocchi ed ogni blocco viene “agganciato” al precedente
![[Blockchain 1.png]]

2) L'aggancio avviene scegliendo una specifica [[Funzioni Hash|funzione hash]] f da utilizzare per costruire la catena di blocchi. Quando si deve aggiungere un nuovo blocco viene inserito in esso il valore hash dell’ultimo blocco presente nella catena (cioè l’output della della funzione f calcolato fornendo come input la stringa che rappresenta l’ultimo blocco presente nella catena). In questo modo ogni blocco della catena conterrà l’hash del blocco precedente, cosi da garantire la temporalità delle transazioni
![[Blockchain 2.png]]

3) Per la verifica dell'integrità della catena basterà confrontare l'hash presente in ogni blocco con l’hash ricalcolato tramite f del blocco precedente presente in quel momento nella catena
![[blockchain 3.png]]

Però questo sistema non protegge l'ultimo blocco dall'alterazione o dalla cancellazione, per questo è stati implementato il proof of work.

#### Proof of work
Il **Proof of Work** è un protocollo di consenso utilizzato nella tecnologia blockchain per garantire la sicurezza e la validità delle transazioni all'interno della rete.

##### Funzionamento 
1. Quando un nodo chiede l’inserimento di un nuovo blocco nella catena deve fornire una prova di lavoro.
2. Un blocco è considerato valido dagli altri nodi ed aggiunto alla blockchain se all’interno contiene un valore, detto nonce, tale che una funzione hash, calcolata sulla stringa che descrive il blocco unita al valore nonce, restituisce un hash con k zeri iniziali. 
    Quindi il nodo che vuole aggiungere un blocco deve “lavorare” per trovare il valore nonce che genera un hash con k zeri iniziali. Può fare ciò calcolando la funzione f: fa variare il valore nonce dal valore 0 incrementandolo di 1 fino a che non si ottiene un valore hash con k zeri iniziali.  
    La quantità di zeri iniziali decide in media quanto un calcolatore deve lavorare.

L'operazione di ricerca del nonce viene chiamata mining e un nodo che effettua il mining è detto miner.  
La falsificazione del blocco non avviene perché ricalcolare il nonce richiede un ulteriore lavoro, inoltre l'aggiunta di nuovi blocchi causa un'incremento di lavoro, incrementando il livello di sicurezza (più la catena è lunga e più è valida). Quindi alterare un blocco richiederebbe una capacità di calcolo enorme.

#### Come operano i nodi del registro
1. Le nuove transazioni da inserire sono trasmesse a tutti i nodi del registro.
2. Ogni nodo raccoglie le nuove transazioni e prepara un nuovo blocco che le contiene
3. Ogni nodo inizia ad effettuare il mining del nuovo blocco
4. Non appena un nodo trova un nonce corretto lo aggiunge al blocco e invia il blocco a tutti gli altri nodi.
5. Alla ricezione del blocco gli altri nodi verificano il nonce e verificano che nessuna transazioni sia un tentativo di doppia spesa.
6. In caso positivo aggiungono il blocco alla propria catena.
![[Proof of work.png]]
Il Mining ha un costo in termini di Cpu e di energia elettrica, quindi ogni volta che il miner trova il nonce, avrà una ricompensa.
![[fee miner.png]]
### Transazioni Bitcoin
- Una transazione ha uno o più input e uno o più output  
- Gli input di una nuova transazione devono far riferimento agli output di transazioni esistenti attraverso le quali colui che ha ricevuto i bitcoin effettua la nuova transazione.  
- Se in una transazione la somma degli output è minore della somma degli input si genera un resto (bitcoin non spesi) che può essere usato dal proprietario in una successiva transazione.  
- Se in una transazione la somma degli output è maggiore della somma degli input la transazione viene rifiutata.  
- Un output di una transazione può essere utilizzato solo una volta come input di una successiva transazione. Ogni riferimento da parte di una transazione successiva allo stesso output è considerato un tentativo di doppia spesa.

![[Transazione bitcoin.png]]

### Implementazione in python
```python
class Blockchain:

    def __init__(self, zeros = 1):
        self.unconfirmed_transactions = []
        self.chain = []
        self.zeros = zeros
        # create the block 0
        self.create_genesis_block()


    def create_genesis_block(self):
        genesis_block = Block(0, [], None, "0")
        self.mine_block(genesis_block)
        self.chain.append(genesis_block)


    def verify_chain(self):
        for i in range(1, len(self.chain)):
            if self.chain[i].previous_hash != self.chain[i-1].get_hash():
                return False
        return  True


    def mine_block(self, block):

        hash = block.get_hash()
        while not hash.startswith('0' * self.zeros):
            block.nonce += 1
            hash = block.get_hash()

        return hash


    def add_new_transaction(self, transaction):
        # add a new transaction to the list of unconfirmed transactions
        self.unconfirmed_transactions.append(transaction)


    def create_and_add_new_block(self):
        if not self.unconfirmed_transactions:
            return None

        # get the last block of the current chain
        last_block = self.chain[-1]
        # create a new block with all the unconfirmed transactions
        new_block = Block(index=last_block.index + 1,
                          transactions=self.unconfirmed_transactions,
                          timestamp=datetime.now(),
                          previous_hash=last_block.get_hash())
        # mine the block
        self.mine_block(new_block)
        # add the bloch to the chain
        self.chain.append(new_block)

        self.unconfirmed_transactions = []
        return new_block.index


    def print_chain(self):
        for block in self.chain:
            print("\tBlock Id:", block.index)
            print("\t\tBlock timestamp:",block.timestamp)
            print("\t\tBlock previous_hash:",block.previous_hash)
            print("\t\tBlock nonce:",block.nonce)
            print("\t\tBlock transactions:",block.transactions)
            print("\t----")
```
