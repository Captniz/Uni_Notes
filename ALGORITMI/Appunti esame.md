## Notazione O grande
### Definizione
La Big-O risponde a domande come:
> _“Se l’input raddoppia, quanto peggiora il tempo di esecuzione?”_

Non misura il tempo **esatto**, ma l’**ordine di grandezza**.

---

Dire che un algoritmo è **O(f(n))** significa che per input grandi, il suo costo cresce **al massimo** come una costante $c\cdot f(n)$, ignorando costanti e termini meno importanti.


> [!example] Esempio
> 
`T(n) = 3n² + 10n + 5  →  O(n²)`
Perché per n grandi domina **n²**.


Grandezze più comuni :

- **O(1) – Costante**
- **O(log n) – Logaritmica**
- **O(n) – Lineare**
- **O(n log n) – Lineare-logaritmica**
- **O(n²) – Quadratica**
- **O(2ⁿ) – Esponenziale**

---

### Calcolo

1. Si ignorano le costanti : `O(5n) = O(n)`
2. Conta il termine dominante : `O(n² + n) = O(n²)`
3. Cicli annidati moltiplicano : `for i in n:   for j in n:     → O(n²)`
4. Cicli consecutivi si sommano : `ciclo O(n) ciclo O(n) → O(n)`
---

### Altre notazioni
#### Big O
> Limite superiore o caso peggiore.
> “L’algoritmo non sarà mai più lento di così (a grandi n).”

Descrive il **massimo** costo di un algoritmo quando l’input cresce.

---

#### Big $\Omega$
> Limite inferiore o caso migliore.
> “L’algoritmo impiega almeno questo tempo.”

Descrive il **minimo** costo di un algoritmo quando l’input cresce.

---

#### Big $\Theta$
> Limite esatto o crescita esatta.
> “L’algoritmo Cresce esattamente come f(n).”

Quando **limite superiore e inferiore coincidono**.

---

## Calcolo della complessità di algoritmi 
Per un algoritmo non ricorsivo:

1. **Individua l’operazione fondamentale**.
    - Confronto.
    - Assegnamento.
    - Accesso a un array.
2. **Conta quante volte viene eseguita**.
3. **Tieni solo il termine dominante**.
4. **Scrivi $\Theta$** (o $O$ se richiesto).


> [!error] NON
> - Sommare costanti.
> - Contare assegnamenti singoli.
> - Scrivere `Θ(n + n²)`.

---

Tabella dei costi degli elementi :

| Elemento                             | Costo                                           | Appunto                                           |
| ------------------------------------ | ----------------------------------------------- | ------------------------------------------------- |
| Ciclo singolo (*fino a $n$*)         | $\Theta(n)$                                     |                                                   |
| Cicli consecutivi (*fino a n*)       | $\Theta(n)$                                     | Si prende il maggiore dei 2                       |
| Cicli annidati (*fino a n*)          | $\Theta(n^2)$                                   | Prodotto tra i due cicli                          |
| Cicli dipendenti (*i to n / j to i*) | $\Theta(n^2)$                                   |                                                   |
| Cicli dimezzanti                     | $\Theta(\log n)$                                | Es. ricerca dicotomica                            |
| Cicli con incrementi $c\neq1$        | $\Theta(n)$                                     | Se $c$ costante                                   |
| If                                   | $\Theta(max(A,B))$                              | Si prende il ramo con il costo peggiore tra i due |
| Cicli con break                      | **Best case**: $Θ(1)$<br>**Worst case**: $Θ(n)$ | Es. ricerca di un elemento                        |

---
Tabella dei costi dell'accesso ai dati :

| Operazione            | Complessità |
| --------------------- | ----------- |
| Accesso array         | $Θ(1)$      |
| Ricerca lineare       | $Θ(n)$      |
| Ricerca binaria       | $Θ(\log n)$ |
| Stack push/pop        | $Θ(1)$      |
| Queue enqueue/dequeue | $Θ(1)$      |
| Lista collegata       | $Θ(n)$      |
| Hash table (medio)    | $Θ(1)$      |
| Albero bilanciato     | $Θ(\log n)$ |

---
Algoritmi noti :

| Algoritmo                              | Complessità   |
| -------------------------------------- | ------------- |
| Bubble / Selection / Insertion (worst) | $Θ(n²)$       |
| Merge Sort                             | $Θ(n \log n)$ |
| Quick Sort (medio)                     | $Θ(n \log n)$ |
| Quick Sort (worst)                     | $Θ(n²)$       |
| BFS / DFS                              | $Θ(V + E)$    |
| Dijkstra (heap)                        | $Θ(E \log V)$ |

---
## Calcolo della complessità di algoritmi ricorsivi
### Master Theorem
#### Forma Base
Serve per risolvere **ricorrenze divide-et-impera** della forma:
$$
T(n)=a T ⁣(\frac{n}{b})+f(n)$$

Il master theorem base non funziona quando :

- $f(n)$ non è polinomiale.
- $f(n)=n^{\log_b a} \log \log n$.
- $f(n)$ è troppo irregolare.

---

Per dedurre la complessità si confronta $f(n)$ con ...
$$
n^{\log_b a}$$

che rappresenta il **costo totale dei nodi interni dell’albero di ricorsione**.

A questo punto si hanno 3 casi :

$$\begin{cases}
f(n)=O(n^{\log_b​a − ε})\ |\ ε>0  & \implies & T(n)=Θ(n^{\log_b​a})​\\
f(n)=\Theta(n^{\log_b​a})  & \implies & T(n)=Θ(n^{\log_b​a}\log_bn)\\
f(n)=\Omega(n^{\log_b​a + ε})\ |\ ε>0  & \implies & T(n)=Θ(f(n))​\\
\end{cases}$$


> [!error] CONDIZIONE DI REGOLARITA'
> Per la <mark class="hltr-red">terza</mark> condizione deve valere la **condizione di regolarità** :
> 
> Deve esistere una costante $c$, tale che $0<c<1$ e un intero $n_0$ tali che ...
> $$∀n≥n_0 : a f (\frac{n}{b} )≤cf(n)$$

---
#### Forma estesa

A differenza della forma base la forma estesa funziona con qualsiasi forma di $f(n)$.


Questa forma ha varie possibilità la più comune è :

$f(n)=\Theta\left(n^{\log_b a} \cdot g(n)\right)$

allora:

| Crescita di (g(n))                                | Risultato                           |
| ------------------------------------------------- | ----------------------------------- |
| $g(n)$ decrescente (*es. $1/\log n$*)             | $\Theta(n^{\log_b a})$              |
| $g(n)$ = $\log^k n$                               | $\Theta(n^{\log_b a} \log^{k+1} n)$ |
| $g(n)$ crescente lentamente (*es. $\log \log n$*) | $\Theta(n^{\log_b a} \cdot g(n))$   |
| $g(n)$ polinomiale                                | $\Theta(f(n))$                      |

---

### Espansione
> Si usa per ricorrenze del tipo $T(n-c)$.

I passaggi per la risoluzione sono :
1. Espandi la ricorrenza più volte.
2. **Trovi il pattern**.
3. Arrivi al caso base.

---
## Algoritmi dati

![[EMBED/03-strutture 1.png]]
[[03-strutture.pdf#page=6&rect=8,39,356,230|03-strutture, p.4]]


![[EMBED/07-hashing.png]]

[[07-hashing.pdf#page=5&rect=9,152,355,226|07-hashing, p.3]]

### Alberi
#### DFS

![[EMBED/05-alberi.png | center | 400 ]]

---
#### BFS

![[EMBED/05-alberi 1.png | center | 400]]


---

#### Ricerca
##### Iterativa

![[EMBED/06-abr.png | center | 400]]


##### Ricorsiva
![[EMBED/06-abr 2.png | center | 400]]


---
#### Inserimento

![[EMBED/06-abr 3.png | center | 400]]


![[EMBED/06-abr 4.png | center | 400]]


---

#### Cancellazione

![[EMBED/06-abr 5.png | center | 400]]



---

#### Caratteristiche Red-Black
![[EMBED/06-abr 6.png | center | 400]]


---

#### Rotazione red-black ( Sinistra )

![[EMBED/06-abr 7.png | center | 400]]


---

### Grafi

#### Rappresentazione

##### Matrice di adiacenza
![[EMBED/09-grafi.png | center | 400]]


##### Liste di adiacenza

![[EMBED/09-grafi 1.png | center | 400]]


---

#### Visita generica

![[EMBED/09-grafi 3.png | center | 400]]


---
#### BFS
![[EMBED/09-grafi 4.png | center | 400]]


---

#### DFS

![[EMBED/09-grafi 6.png | center | 400]]


---

#### Erdos (Distanza tra nodi)

![[EMBED/09-grafi 5.png | center | 400]]


---

#### Presenza di cicli - NON orientato

![[EMBED/09-grafi 9.png | center | 400]]



---

#### Presenza di cicli - orientato

![[EMBED/09-grafi 11.png | center | 400]]

