---
Date created: 24-09-26 • 16:17
tags:
Related PDF/DOC:
  - "[[2-geometria_1_old.pdf]]"
Related Pages:
---
# Esempi di calcolo geometrico della cinematica
Prendiamo un esempio un manipolatore che si muove su un piano cartesiano composto da 2 link.


> [!example] Schema del robot
> ![[EMBED/2-geometria_1_old.png]]
[[2-geometria_1_old.pdf#page=2&rect=187,412,464,761|2-geometria_1_old, p.2]]

%%
Dove ...
- $l_1,l_{2}$ : Link del robot
- $q_1$ : Rotazione di $l_{1}$ rispetto al asse $X$ ( *Angolo tra $l_1$ e $X$* ). 
- $q_{2}$ : Rotazione di $l_2$ rispetto all'asse parallelo a $l_1$ ( *Angolo tra $l_1$ e $l_2$* ).
%%

Inoltre sia ...
$$
E=
\begin{bmatrix}
p_{x} \\
p_{y} \\
\phi
\end{bmatrix} = \text{Posizione e orientamento del effettore}
$$

$$
G=\begin{bmatrix}
q_{1} \\
q_{2}
\end{bmatrix}=\text{Configurazione delle giunture}
$$

Vogliamo trovare $E$ come funzione di $G$.

---

## Cinematica diretta
Seguendo la cinematica diretta possiamo trovare semplicemente :
$$\begin{array}{left}
p_{x}=l_{1}\cos(q_{1})+l_{2}\cos (q_{1}+q_{2}) \\
p_{x}=l_{1}\sin(q_{1})+l_{2}\sin (q_{1}+q_{2}) \\
\phi = q_{1}+q_{2}
\end{array}$$


> [!important]- Reminder su funzioni trigonometriche
> ![[_Cheat sheet trigonometria]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

## Cinematica inversa

Invece per la cinematica inversa invece, abbiamo che ...
$$q_{1}+q_{2}=\phi$$
quindi ...
$$
\begin{array}{left} 
p_{x}-l_{2}\cos (\phi) = l_{1}\cos (q_{1}) \\
p_{y}-l_{2}\sin (\phi) = l_{1}\sin (q_{1}) 
\end{array}\implies
\begin{array}{left} 
q_{1} = \arctan_{2}\left( \frac{p_{x}-l_{2}\cos(\phi)}{l_{1}},\frac{p_{y}-l_{2}\sin(\phi)}{l_{1}} \right) \\
q_{2} = \phi -q_{1}
\end{array}
$$


> [!info] $\arctan_{2}$ 
> La normale funzione $\arctan(z)$ accetta **un solo argomento** ( $z = y/x$ ). Questo crea due grandi problemi in un piano cartesiano :
> - Possibile divisione per $0$.
> - Perdita di significato del quadrante.
>   
> $\arctan_{2}$ accetta **due argomenti separati** ( $y$ e $x$ ) e mantiene quindi le informazioni sui quadranti. 
> 
>
> > [!warning] Input
> > All'interno di $\arctan_{2}$  <mark class="hltr-red">si ha comunque una divisione, questo vuol dire che fattori comuni in $y$ e $x$ vengono annullati.</mark>
> >$$\arctan_{2}(\cos \theta,\sin \theta) =\arctan_{2}(K\cdot\cos \theta ,K\cdot\sin \theta)$$
> 
> Restituisce l'angolo  nell'intervallo $(-\pi, \pi]$ ( $-180^\circ \to +180^\circ$ ).
^TANGENTE
### Con $\phi$ libero
Avendo $\phi$ libero dobbiamo cambiare il nostro approccio.

Applicando il **teorema del coseno** abbiamo che ...

$$
p_{x}^2+p_{y}^2 = l_{1}^2+l_{2}^2 -2l_{1}l_{2}\cos(180°-q_{2}) 
$$

risolviamo poi per $\cos(180°-q_{2}) = -\cos(q_{2})$ ...


$$
\cos(q_{2})=\frac{p_{x}^2+p_{y}^2 - l_{1}^2-l_{2}^2}{2l_{1}l_{2}} 
$$

Infine troviamo $q_2$ attraverso l'$\arccos$ :

$$
q_{2} = \pm \arccos\left( \frac{p_{x}^2+p_{y}^2-l_{1}^2-l_{2}^2}{2l_{1}l_{2}} \right)
$$



> [!important]- Teorema del coseno
> Teorema che permette di trovare la lunghezza di un qualunque lato di un triangolo tramite la formula :
> $$r^2 = l_1^2 + l_2^2 - 2 l_1 l_2 \cos(\theta_{r})$$
> Dove ...
> - $r$ : Lato da trovare.
> - $l_{1},l_{2}$ : Altri due lati.
> - $\theta_{r}$ : Angolo <mark class="hltr-orange">opposto</mark> al lato da trovare.

Una volta trovato $q_2$ trovare $q_{1}$ è banale attraverso l'$\arctan$ :
$$
q_{1}=\arctan \left( \frac{p_{y}}{p_{x}} \right) - \arctan \left( \frac{l_{2}\sin(q_{2})}{l_{1}+l_{2}\cos (q_{2})} \right)
$$

---

# Generalizzazione del calcolo cinematico

Sia un manipolatore con $n$ giunture $q = [q_{1}\dots q_{n}]^T$ e sia $p$ la posa dell'effettore.

Dobbiamo generalizzare le formule per i casi di ...
- Cinematica diretta : $p=f(q)$
- Cinematica inversa : $q=f^{-1}(p)$

## Vettori e frame di riferimento
Sia un punto $P$ generico su un piano cartesiano.
Sia inoltre un *frame* $O_{0} - x_{0}y_{0}$.

Possiamo definire $P$ attraverso un vettore con origine in $O_{0}$ attraverso la dicitura ...

$$
P=\begin{bmatrix}
x_{p}^0 \\
y_{p}^0
\end{bmatrix}
$$

( *$x^0$, $0$ indica il frame di riferimento* )

Uno stesso punto può essere associato a più vettori e di conseguenza a più frame.

$$
P=\begin{bmatrix}
x_{p}^0 \\
y_{p}^0
\end{bmatrix}==\begin{bmatrix}
x_{p}^1 \\
y_{p}^1
\end{bmatrix}

$$


> [!example] Schema della definizione di un punto tramite frame
> ![[EMBED/2-geometria_1_old 1.png]]
[[2-geometria_1_old.pdf#page=8&rect=144,261,328,544|2-geometria_1_old, p.8]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Matrice di rotazione | Coordinate in un frame rispetto ad un altro
Consideriamo ora due frame con la stessa origine :
- $O-x_{}y_{}$
- $O'-x'y'$ ruotato rispetto a $O$ di $\alpha$ gradi

Inoltre abbiamo il punto $P$ con coordinate polari $(\rho,\theta)$ (*distanza, rotazione*) rispetto ad $O$. 


> [!example] Esempio
> ![[EMBED/2-geometria_1_old 2.png]]
[[2-geometria_1_old.pdf#page=10&rect=241,448,475,742|2-geometria_1_old, p.10]]

Con questi dati possiamo già vedere che ...
$$
\begin{array}{c}

\begin{array}{l}
p_{x}=\rho \cos \theta \\
p_{y}=\rho \sin \theta
\end{array} 
&\text{and}&
\begin{array}{l}
p_{x}'=\rho \cos (\theta-\alpha) \\
p_{y}'=\rho \sin (\theta-\alpha)
\end{array}
\end{array}
$$

Tuttavia applicando le [[_Cheat sheet trigonometria#Addizione e Sottrazione|operazioni di somma e differenza trigonometriche]] possiamo arrivare a trovare $p'$ da $p$ :
$$
\begin{array}{l}

p'_x = \rho \Big( \cos(\theta)\cos(\alpha) + \sin(\theta)\sin(\alpha) \Big) \\
p'_x = \big(\rho \cos(\theta)\cos(\alpha)\big) + \big(\rho \sin(\theta)\sin(\alpha)\big) \\
p'_x = p_{x}\cos\alpha + p_{y}\sin \alpha

\end{array}
$$

(*Lo stesso si applica per $p_{y}$ con la formula di sottrazione per il seno : $p'_x = -p_{x}\sin\alpha + p_{y}\cos \alpha$*).

Abbiamo quindi ...

$$
p'=R(\alpha)\cdot p \mid R(\alpha) = \begin{bmatrix}
\cos \alpha & \sin \alpha \\
-\sin \alpha & \cos \alpha
\end{bmatrix}
$$

Dove $R(\alpha)$ è detta <mark class="hltr-orange">matrice di rotazione</mark>.

---

## Matrici di rotazione
>Una matrice di rotazione $R$ rappresenta una rotazione attorno a un asse nello spazio necessaria per allineare gli assi del sistema di riferimento con i corrispondenti assi del sistema solidale al corpo.

In geometria, una matrice di rotazione ...
- Descrive l'orientamento reciproco tra due sistemi di riferimento (*sistemi di coordinate*). 
  I suoi vettori colonna sono i coseni direttori degli assi del sistema ruotato rispetto a quello originale.
- Rappresenta la trasformazione di coordinate tra due sistemi di riferimento ruotati.
- È l'operatore che consente le rotazioni all'interno dello stesso sistema di riferimento.
### Proprietà delle matrici di rotazione
Le matrici di rotazione rispettano queste caratteristiche :
- I vettori $x',y',z'$ sono **ortonormali**.
	-  ${x'}^T x' = {y'}^T y' = {z'}^T z' = 1$
	- ${x'}^T y' = {x'}^T z' = {y'}^T z' = 0$
- $R^T R = I_3 \longrightarrow R^T = R^{-1}$
- $\det R = 1$ per sistemi destrorsi
- $\det R = -1$ per sistemi sinistrorsi

Inoltre si hanno le seguenti proprietà :
1. **Composizione** : Moltiplicare due rotazione produce una nuova matrice di rotazione.
2. **Associatività** :  $R_1 (R_2 R_3) = (R_1 R_2) R_3$
3. Ogni rotazione ha un'inversa unica.
4. La rotazione *degenere* $I_3$ è una rotazione.
5. Le rotazioni <mark class="hltr-red">NON</mark> sono **commutative** : $R_1 R_2 \neq R_2 R_1$

Queste proprietà qualificano le rotazioni come <mark class="hltr-orange">un gruppo algebrico non abeliano</mark> ( *cioè non commutativo* ) sotto composizione ( *moltiplicazione* ), denominato <mark class="hltr-orange">$SO(3)$</mark>.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Posa di un corpo rigido 
> I robot sono modellati come un insieme di corpi rigidi, dove un punto preliminare definisce la *posa*.


Consideriamo un generico corpo rigido. Assumiamo che ci sia un sistema di riferimento attaccato al corpo $O_{r}$.

Per la definizione di corpo rigido, <mark class="hltr-orange">le coordinate dei punti del corpo sono fisse all'interno sistema di riferimento ad esso attaccato $O_r$</mark>. 

Pertanto, la *configurazione* del corpo nello spazio è <mark class="hltr-red">univocamente definita</mark> se definiamo il sistema di riferimento solidale *$O_r$* rispetto a un sistema di riferimento fisso $O_{s}$ nello spazio.

#### Matrice di rotazione di un corpo rigido


Il frame $O_f$ è univocamente definito dalla posizione della sua origine, rappresentata da un vettore vincolato (*bounded*) e dalle coordinate dei suoi tre vettori direzione espressi attraverso il frame $O_s$.

$$
\begin{array}{c}
O^{r} = \begin{bmatrix}
O^r_{x} \\
O^r_{y} \\
O^r_{z}
\end{bmatrix} & \text{and} & \begin{array}{left}
x^r=x^r_{x}x+x^r_{y}y+x^r_{z}z \\
y^r=y^r_{x}x+y^r_{y}y+y^r_{z}z \\
z^r=z^r_{x}x+z^r_{y}y+z^r_{z}z
\end{array}
\end{array}
$$


Con i dati riguardanti le coordinate dei vettori direzione unitari di $O_s-xyz$ e $O_r-x^ry^rz^r$, possiamo creare la matrice di rotazione $R$ di $O_r$ rispetto a $O_s$. 

$R$ ci dice com'è orientato $O_r$ rispetto a $O_s$ e <mark class="hltr-red">non ha alcuna informazione su centro o posizione dei due frame</mark>.


##### Matrice di rotazione con coordinate
Una possibilità è comporre la matrice con le coordinate dei vettori direzione (*vettori unità*).

$$R = \begin{bmatrix} x_x^r & y_x^r & z_x^r \\ x_y^r & y_y^r & z_y^r \\ x_z^r & y_z^r & z_z^r \end{bmatrix}$$

Si può anche riassumere $R$ con un vettore composto dai tre vettori direzione :
$$
R=\begin{bmatrix}
x^r \\
y^r \\
z^r
\end{bmatrix}
$$

> [!example] Esempio di significato di un elemento di $R$ con rappresentazione a coordinate
> Per esempio $x_{y}^r$ rappresenta la <mark class="hltr-orange">componente $y$ del vettore unitario direzione $x\in O_{r}$ espresso all'interno di $O_s$</mark>.

##### Matrice di rotazione con prodotti scalari

Possiamo anche rappresentare $R$ come una **matrice di prodotti scalari** di cui ogni elemento rappresenta il coseno dell'angolo tra i due assi ( *Uno in $O_r$ e l'altro in $O_s$* ).

$$R = \begin{bmatrix} {x^r}^T x & {y^r}^T x & {z^r}^T x \\ {x^r}^T y & {y^r}^T y & {z^r}^T y \\ {x^r}^T z & {y^r}^T z & {z^r}^T z \end{bmatrix}$$


> [!example] Esempio di significato di un elemento di $R$ con rappresentazione a prodotti scalari
> Per esempio $x^{rT}y$ rappresenta il <mark class="hltr-orange">coseno dell'angolo tra l'asse $x^r$ e $y$</mark>.


> [!info]- Dicitura $x^T$ e prodotti scalari
> La definizione geometrica del prodotto scalare tra due vettori è ...
> $$\mathbf{a} \cdot \mathbf{b} = \Vert{}\mathbf{a}\Vert{} \Vert{}\mathbf{b}\Vert{} \cos(\theta)$$
> 
> Tuttavia in questo caso dato che trattiamo di vettori unitari le loro magnitudini $\Vert{}\mathbf{a}\Vert{}$ e $\Vert{}\mathbf{b}\Vert{}$ si cancellano.
> 
> Rimaniamo quindi con ...
> $$\mathbf{a} \cdot \mathbf{b} =(1) \cdot (1) \cdot \cos(\theta_{\mathbf{a}, \mathbf{b}}) = \cos(\theta_{\mathbf{a}, \mathbf{b}})$$
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
> 
> All'interno delle matrici il prodotto scalare è rappresentato tramite la notazione :
> $$\mathbf{a}^T \mathbf{b}$$

##### Sintesi delle rappresentazioni
le due rappresentazioni di $R$ sono equivalenti per le proprietà dei vettori unitari ( *versori* ). Pertanto ...

$$x^r_x = \mathbf{x}^{rT} \mathbf{x} = \cos(\theta_{x^r, x})$$

- È la **coordinata $x$** del versore dell'asse $x^r$ nel frame $O_s$.
- È il **coseno dell'angolo** $\theta$ tra l'asse $x^r$  e l'asse $x$.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Rotazioni elementari

Le rotazioni di un angolo $\alpha$ attorno ad un asse di $O_s$ vengono rappresentati facilmente; queste rotazioni vengono dette *elementari*. 

Prendiamo in esempio una rotazione attorno all'asse $z$ di angolo $\alpha$, questa viene rappresentata come ...

$$
\begin{array}{l}
x^r=\begin{bmatrix}
\cos \alpha \\
\sin \alpha \\
0 
\end{bmatrix} & 
 \begin{array}{l}
y^r=\begin{bmatrix}
-\sin \alpha \\
\cos \alpha \\
0 
\end{bmatrix}
\end{array} &  
\begin{array}{l}
z^r=\begin{bmatrix}
0 \\
0 \\
1 
\end{bmatrix}
\end{array}
\end{array}
$$

Con rotazione risultante :

$$
R_z(\alpha) = \begin{bmatrix} \cos(\alpha) & -\sin(\alpha) & 0 \\ \sin(\alpha) & \cos(\alpha) & 0 \\ 0 & 0 & 1 \end{bmatrix}
$$

Allo stesso modo, possiamo trovare la matrice di rotazione di un angolo $\beta$ attorno a $y$ e di un angolo $\gamma$ attorno a $x$:

$$R_y(\beta) = \begin{bmatrix} \cos(\beta) & 0 & \sin(\beta) \\ 0 & 1 & 0 \\ -\sin(\beta) & 0 & \cos(\beta) \end{bmatrix}$$

$$R_x(\gamma) = \begin{bmatrix} 1 & 0 & 0 \\ 0 & \cos(\gamma) & -\sin(\gamma) \\ 0 & \sin(\gamma) & \cos(\gamma) \end{bmatrix}$$

Inoltre <mark class="hltr-red">possiamo ottenere la rotazione inversa ( $\alpha \to -\alpha$ ) trovando la matrice trasposta della rotazione</mark> : 



$$R_k(-\theta) = R_k^T(\theta), \quad k = x, y, z$$
^50a45e

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Rappresentazione di un punto o vettore
#### Posizione
Consideriamo il caso in cui le due origini $O$ e $O'$ coincidano.


> [!example] Esempio
> ![[EMBED/2-geometria_1_old 3.png]]
[[2-geometria_1_old.pdf#page=20&rect=211,489,496,772|2-geometria_1_old, p.20]]

Un generico punto $P$ può essere rappresentato come:

$$p = \begin{bmatrix} p_x \\ p_y \\ p_z \end{bmatrix} \text{ in }O-xyz$$

$$p' = \begin{bmatrix} p_x' \\ p_y' \\ p_z' \end{bmatrix}\text{ in }O'-x'y'z'$$

Poiché sia $p$ che $p'$ si riferiscono allo stesso punto, possiamo scrivere ...

$$p = p_x' x' + p_y' y' + p_z' z' = \begin{bmatrix} x' & y' & z' \end{bmatrix} p' = R p'$$

Quello che facciamo è moltiplicare il punto $p'$ per la rotazione $R$ in modo da riottenere le coordinate del sistema di riferimento $O-xyz$. 

Da questo possiamo facilmente dedurre che vale anche [[#^50a45e|l'inverso tramite la trasposta]] :

$$p' = R^T p$$


> [!example]- Esercizio esempio
> [[2-geometria_1_old.pdf#page=22|2-geometria_1_old, p.22]]

Possiamo anche vedere il problema in questo modo ...
$$
p=p'+OO'
$$
con $OO'$ vettore che unisce le due origini.

Questo vale grazie alla somma tra vettori.
#### Rotazione

Un altro modo di vedere una matrice di rotazione è come un operatore matriciale che consente la rotazione di un vettore attorno a un asse arbitrario nello spazio, <mark class="hltr-orange">rimanendo nello stesso frame</mark>.

Sia $R$ una matrice di rotazione e $p'$ un vettore espresso in $O-xyz$. Vogliamo ruotare $p'$ secondo $R$ ottenendo $p$ :

$$p = R p'$$
( *$p$ sempre in $O-xyz$* )

Possiamo dire inoltre che l'operazione di rotazione preserva la norma del vettore :

$$\|p\|^2 = p^T p =  (Rp')^T R p' = R^Tp'^T R p' = I_{3}{p'}^T p'= {p'}^T p' = \|p'\|^2$$
( *Per la $4^a$ trasformazione usiamo $R^TR=I_3$* )


> [!important]- Norma
> La norma può rappresentare grandezze diverse dipendentemente dal contesto :
> - **Distanza** : Misura la distanza in linea retta dall'origine fino al punto $P(p_x, p_y, p_z)$.
> - **Lunghezza** ( *nostro caso* ): Se $p$ rappresenta una freccia o *link*, la norma ne rappresenta la lunghezza fisica.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Composizione
> Una rotazione può essere espressa come una sequenza di rotazioni parziali, ciascuna definita rispetto a quella precedente.
> 

Il sistema di riferimento a partire dal quale avviene la rotazione è chiamato **sistema di riferimento corrente (current frame)**. La sequenza si ottiene *post-moltiplicando* le matrici associate a ciascuna rotazione.

Partiamo da un esempio.

Siano tre frame con la stessa origine e tre coordinate associate ai frame che puntano a un punto $P$.

- $O-x^0y^0z^0 \to p^0$
- $O-x^1y^1z^1\to p^1$
- $O-x^2y^2z^2\to p^2$

( *Chiamati $F_{0},F_{1},F_{2}$ per semplicità* )

Sia inoltre $R_i^j$ la rotazione del frame $i$ rispetto a $j$.

Abbiamo quindi :
- $p^1=R^1_{2}\cdot p^2$
- $p^0=R^0_{1}\cdot p^1$
- $p^0=R^0_{2}\cdot p^2$

Per sostituzione e comparazione possiamo anche dire semplicemente che ...

$$p^1=R^1_{2}\cdot p^2 \to R^0_{2}=R_{1}^0\cdot R_{2}^1$$

La relazione $R^0_{2}=R_{1}^0\cdot R_{2}^1$ è interpretabile come ...
1. Ruotiamo $F_{0}$ per allinearlo con $F_{1}$ ( *$R_{1}^0$* )
2. Ruotiamo ancora il risultato ( *che sarebbe equivalente a $F_{1}$ dato che sono allineati* ) per allinearlo con $F_{2}$ ( *$R_{2}^1$* )

Possiamo generalizzare quindi come :
$$R_i^j = (R_j^i)^{-1} = (R_j^i)^T$$

> [!example]- Esempio Pratico
> [[2-geometria_1.pdf#page=14|2-geometria_1, p.11]]

#### Composizione di rotazioni elementari
Le composizioni possono avvenire facilmente ruotando attorno agli assi elementari, ma necessitano delle considerazioni speciali.

Consideriamo due frame ruotati ( *stessa origine* ) 
- $F_{0} : O-x_{0}y_{0}z_{0}$
- $F_{1} : O-x_{1}y_{1}z_{1}$

e un punto generico $P=R^0_1p^1$.

Sia inoltre un applicazione lineare generica (*trasformazione di una matrice*) $A$ definita in $F_0$.

Per il generico punto $P$ abbiamo che la sua trasformazione per $A$ vale ...
$$\begin{array}{left} 
q^0=Ap^0 \\
q^1=R_{0}^1Ap^0=R_{0}^1AR_{1}^0p^1=(R_{1}^0)^{-1}AR_{1}^0p^1
\end{array}$$

Questo ci va a significare che per ottenere una rotazione rispetto agli assi elementari dobbiamo prima invertire le rotazioni passate.

> [!example] Esempio di composizione di rotazioni elementari
> ![[EMBED/2-geometria_1.png]]
[[2-geometria_1.pdf#page=15&rect=57,53,309,140|2-geometria_1, p.12]]

In questo esempio siano le due rotazioni
- $R^0_1 = R_y(\phi)\text{ rotazione attorno }y_{0}$
- $\bar {R^{1}_2} = R_z(\theta)\text{ rotazione attorno }z_{0}\text{ RIMUOVENDO LA ROT }R^0_{1}$

Abbiamo quindi che ...
$$R_{2}^0 = R^0_1 \bar R^1_2 = R_{z}({\theta})R_{y}({\phi}) $$

> [!warning] Notazione barrata e rotazioni attorno ad assi mobili e fissi
> Definiamo intanto le due rotazioni attorno ai diversi tipi di assi :
> - <mark class="hltr-purple">Mobili</mark> :  Rotazione attorno all'asse del frame generato dalla rotazione precedente. *Rotazioni in sequenza*.
>   Si presenta come una post-moltiplicazione :
>    $$R_2^0 = R_1^0 R_z(\theta) \mid R^1_{2} = R_{z}(\theta)$$
>  - <mark class="hltr-orange">Fissi</mark> : Rotazione attorno all'asse del frame originale ( *o assi dello spazio* ). 
>    Si presenta come una pre-moltiplicazione :
>    $$R_2^0 = R_z(\theta) R_1^0$$
>   Dove $R_{z}(\theta)$ è definito secondo il frame $0$.
>   
>   
>   La pre o post moltiplicazione è riferita alla prima rotazione ($R^0_{1}$) : Se si lavora con assi fissi si mette la rotazione **A SINISTRA** rispetto a $R^0_{1}$, al contrario con rotazioni sugli assi correnti si mette **A DESTRA**.
> 
> ---
>  Per utilizzare le rotazioni attorno agli assi elementari bisogna sempre riferirsi al frame $0$, matematicamente questo avviene invertendo le rotazioni precedenti (*Mettere a sinistra o a destra con pre-post-moltiplicazione non ha valore come espressione*).
>  
>  Usiamo per questo la notazione **barrata** che rappresenta la inversione delle rotazioni precedenti per favorire la corrente.
>  
>  Abbiamo quindi che ...
>  $$\bar R^1_2=\overbrace{(R^0_{1})^{-1}}^{\text{Inversione}}\cdot\underbrace{R_{z}(\theta)}_{\text{Rotazione}}\cdot\overbrace{R^0_{1}}^{\text{Ripristino}}$$
>  esplicitando ...
>  $$\begin{array}{left} R^0_{1}=R_{y}(\phi) \\  R^0_{1'}=R_{z}(\theta)\\  R_{2}^0 = R^0_1 \bar R^1_2=R^0_{1}(R^0_{1})^{-1}R_{z}(\theta)R^0_{1}=R_{z}(\theta)R^0_{1} = R_{z}({\theta})R_{y}({\phi})\end{array}$$
>  

Bisogna sempre ricordarsi che nelle rotazioni <mark class="hltr-red">CAMBIARE ORDINE DELLE OPERAZIONI (ROTAZIONI) O ASSE DI RIFERIMENTO CAMBIA TOTALMENTE IL RISULTATO.</mark> 


> [!example]- Esempio dell'incorrettezza del cambio dell'ordine delle operazioni
> [[2-geometria_1.pdf#page=14|2-geometria_1, p.11]]


> [!example] Esempio finale completo
> ![[EMBED/2-geometria_1 1.png]]
[[2-geometria_1.pdf#page=20&rect=20,14,351,232|2-geometria_1, p.17]]

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

### Parametrizzazione di matrici di rotazione

Una rotazione è un elemento di $SO(m)$ che è completamente specificato da $\frac{m(m-1)}{2}$ parametri:
* 1 parametro per $SO(2)$
* 3 parametri per $SO(3)$

Per esempio, una matrice di rotazione 3D ha **9 elementi**, ma una rotazione è completamente specificata da soli **tre parametri** : <mark class="hltr-orange">tre rotazioni attorno a tre assi diversi, tali che due rotazioni adiacenti non siano attorno allo stesso asse.</mark>  

Come risultato, ci sono **12 parametrizzazioni con tre rotazioni**:
$$\begin{array}{l} \\
3\ \times & \text{Uno qualsiasi degli assi} \\
2\ \times & \text{[...] escluso l'asse della prima rot.} \\
2= & \text{[...] escluso l'asse della seconda rot.} \\
12
\end{array}$$

#### Angoli Euler e RPY
Gli angoli di Eulero sono gli angoli relativi a <mark class="hltr-red">tre</mark> rotazioni attorno agli assi <mark class="hltr-orange">correnti</mark>.  
( *Solitamente si hanno tre rotazioni ma solo due assi usati. Eg. $XYX$, $ZXZ$, ...*)

> [!example] Esempio di rotazione Euler
> Abbiamo una rotazione attorno agli assi <mark class="hltr-orange">correnti</mark> $ZYZ$.
> $$R (ϕ, θ, ψ) = R_z (ϕ)R_{y'} (θ)R_{z''} (ψ)$$ 



Al contrario, gli angoli RPY sono composti da tre rotazioni, uno per ogni asse $XYZ$, relative agli <mark class="hltr-orange">assi fissi</mark>. RPY sta per  **Roll ($\psi$), Pitch ($\theta$), Yaw ($\phi$)** ( *Rollio, Beccheggio, Imbardata* ).


> [!example] Roll, Pitch, Yaw schema
> ![[EMBED/2-geometria_1 2.png]]
[[2-geometria_1.pdf#page=24&rect=149,87,214,146|2-geometria_1, p.21]]


> [!example] Esempio di rotazione RPY
> Abbiamo una rotazione qualsiasi, rappresentata attraverso gli assi <mark class="hltr-orange">fissi</mark> $XYZ$.
> $$R = R_z(\phi) R_y(\theta) R_x(\psi)$$
> 
> In questo caso, poiché la rotazione è attorno ad assi fissi, dobbiamo pre-moltiplicare ($XYZ \to ZYX$).

<hr style="width: 70%; margin-left: auto;margin-right: auto;">




Prendendo in esempio la rotazione RPY precedente abbiamo che $R$ vale ...
$$R =  R_z(\phi) R_y(\theta) R_x(\psi)= \begin{bmatrix} c_\phi c_\theta & c_\phi s_\theta s_\psi - s_\phi c_\psi & c_\phi s_\theta c_\psi + s_\phi s_\psi \\ s_\phi c_\theta & s_\phi s_\theta s_\psi + c_\phi c_\psi & s_\phi s_\theta c_\psi - c_\phi s_\psi \\ -s_\theta & c_\theta s_\psi & c_\theta c_\psi \end{bmatrix}$$

( *Con $c_\alpha = \cos(\alpha)$, $s_\alpha = \sin(\alpha)$* )

da questa possiamo risolvere il problema inverso, cioè trovare gli angoli delle rotazioni. Abbiamo tuttavia due

Inoltre, data una matrice di Rotazione $R \in SO(3)$ ...
$$R = \begin{bmatrix} r_{11} & r_{12} & r_{13} \\ r_{21} & r_{22} & r_{23} \\ r_{31} & r_{32} & r_{33} \end{bmatrix}$$
possiamo risolvere il problema inverso e trovare due soluzioni:

- Per $\theta \in (-\pi/2, \pi/2)$ ( *$-90<\theta<90$* )
$$\begin{cases} \phi = \text{atan2}(r_{21}, r_{11}) \\ \theta = \text{atan2}(-r_{31}, \sqrt{r_{32}^2 + r_{33}^2}) \\ \psi = \text{atan2}(r_{32}, r_{33}) \end{cases}$$

- Per $\theta \in (\pi/2, 3\pi/2)$ ( *$90<\theta<270$* )
$$\begin{cases} \phi = \text{atan2}(-r_{21}, -r_{11}) \\ \theta = \text{atan2}(-r_{31}, -\sqrt{r_{32}^2 + r_{33}^2}) \\ \psi = \text{atan2}(-r_{32}, -r_{33}) \end{cases}$$

( *[[#^TANGENTE| Link approfondimento atan2]]* )


> [!warning] Due soluzioni e implicazioni
> Le due soluzioni sono date dal fatto che per trovare $\theta$
> $$\cos(\theta) = \pm\sqrt{1 - r_{31}^2} = \pm\sqrt{r_{32}^2 + r_{33}^2}$$
> In particolare, il $\pm$ ci dice che esiste una soluzione positiva nella prima metà e una negativa nella seconda metà dove si ha $\cos$ negativo.
> 
> Questo comporta che <mark class="hltr-red">ogni $\cos \theta$ nella rotazione diventi negativo</mark>. 
> 
> <hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
> 
> Dal punto di vista fisico/visuale questo si spiega tramite la **ridondanza cinematica** :
> 
> Nello spazio 3D, è possibile ottenere esattamente la stessa orientazione finale di un oggetto utilizzando due combinazioni rotazioni completamente diverse.
> 
> Queste due diverse rotazioni dipendono dall'*asse centrale*, cioè quello di mezzo nella rotazione ( *$R(XYZ)\to \text{asse centrazle : }Y$* ) :
> - <mark class="hltr-orange">Soluzione 1 (Posa Frontale/Standard)</mark>: Si inclina l'asse centrale in *avanti* ( *in questo caso di $\theta$ gradi* ) all'interno del suo intervallo standard ($−π/2,π/2$).
>- <mark class="hltr-purple">Soluzione 2 (Posa Posteriore/Rovesciata)</mark>: Si inclina l'asse centrale oltre i 90∘ nell'intervallo $(π/2,3π/2)$ ( *quindi si ha che l'angolo di inclinazione finale $\theta'=\theta+\pi$ ($180°+\theta$)* ) e si compensa ruotando gli altri due assi di $180°$ ($±π$).
>  In sintesi quindi, ruotiamo ogni angolo di $180°$ aggiuntivi.



##### Gimbal Lock
Una caratteristica delle rappresentazioni RPY, e in generale delle rappresentazioni basate su Eulero, è che i tre angoli agiscono come <mark class="hltr-orange">tre gradi di libertà indipendenti</mark>.

Tuttavia, prendendo in esempio una rotazione $XYZ$ , quando $Y$ è ruotato ad un angolo di $\pm\frac{\pi}{2}\ (\pm90°)$, gli assi di rotazione di $X$ e $Z$ <mark class="hltr-red">diventano allineati</mark>. Questo si traduce nella <mark class="hltr-red">perdita di un grado di libertà</mark>.

Questo fenomeno avviene data la gerarchia delle rotazioni composte secondo gli assi correnti : in una $R(XYZ)$ le singole rotazioni si influenzano sequenzialmente l'una con l'altra ...
- $R_{x}(\psi)\xrightarrow{\text{influenza}}R_{y}(\theta)R_{z}(\phi)$
- $R_{y}(\theta)\xrightarrow{\text{influenza}}R_{z}(\phi)$
- $R_{z}(\phi)\text{ sta a se}$

Agendo sull'<mark class="hltr-orange">asse di mezzo</mark>, qualunque esso sia, e ruotandolo di $\pm90°$ in una direzione si entra nello stato di **gimbal lock**.

L'asse esterno e interno <mark class="hltr-orange">non sono fisicamente allineati nello stesso tempo</mark>; sono allineati nel senso che, quando *arriva il tempo* di compiere una rotazione attorno a loro, entrambi sono nella stessa posizione e ruotano l'oggetto attorno allo stesso asse corrente.

In questo stato quindi, una rotazione attorno ad un asse può essere replicata da una rotazione attorno all'altro. Infatti, quando $\theta = \frac{\pi}{2}$:

$$R =\begin{bmatrix}
\dots
\end{bmatrix} = \begin{bmatrix} 0 & s_{(\phi+\psi)} & c_{(\phi+\psi)} \\ 0 & c_{(\phi+\psi)} & -s_{(\phi+\psi)} \\ -1 & 0 & 0 \end{bmatrix}$$

Da $R$ possiamo vedere che <mark class="hltr-orange">$\phi(Z)$ e $\psi(X)$ non agiscono indipendentemente</mark>. 

In una situazione normale, le rotazioni ottenute con una combinazione di $\phi$ e $\psi$ possono essere realizzate con una singola rotazione di angolo $\beta = (\phi+\psi)$.

La mappa inversa dalla matrice di rotazione agli angoli RPY ( *cioè trovare gli angoli da $R$* ) è *singolare* in questo punto, il che significa che ci sono <mark class="hltr-red">infinite combinazioni di $\phi$ e $\psi$ che producono $R$</mark>.

Durante una configurazione singolare, <mark class="hltr-orange">piccole variazioni nell'orientamento fisico possono produrre salti discontinui negli angoli RPY calcolati</mark>. Questo è un problema di primaria importanza per i sistemi fisici come i robot, che richiedono un controllo fluido e continuo.

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Assi aggiuntivi di rotazione | Parametrizzazione asse-angolo 

Possiamo adottare una parametrizzazione *ridondante* utilizzando un asse $k = [k_x, k_y, k_z]$ ( *$\|k\|=1$ dato che parliamo di asse* ) ed un angolo $\theta$, ottenendo quattro parametri complessivi ( *$k_{x},k_{y},k_{z},\theta$ al contrario di RPY con $\phi,\theta,\psi$* ).


> [!error] Valori di $k$
> I valori <mark class="hltr-red">$k_x,k_y,k_z$ NON SONO DEI VETTORI DI COORDINATE.</mark>
> Sono invece degli <mark class="hltr-orange">scalari</mark> tali che $\|k\|=1$, dato che parliamo di assi.
> 


La rotazione $R_k(\theta)$ può essere trovata trasformando le coordinate; in particolare possiamo trovare un nuovo sistema $O-x_1 y_1 z_1$, con $z_1$ coincidente con $k$. 

Ruotiamo $O-x_0 y_0 z_0$ di $\alpha$ attorno all'asse $z$ e di $\beta$ attorno all'asse $y$.  


> [!example] Schema dell'asse $k$
> ![[EMBED/2-geometria_1 4.png]]
[[2-geometria_1.pdf#page=36&rect=122,118,244,227|2-geometria_1, p.28]]

Abbiamo quinidi ... 
$$R_1^0 = R_z(\alpha) R_y(\beta)$$

Se una trasformazione $A$ ( *Eg. una rotazione* ) è espressa 
nel sistema $O-x_1 y_1 z_1$, come in $q^1 = A p^1$, sappiamo che :

$$q^0 = R_1^0 A (R_1^0)^{-1} p^0 = R_z(\alpha) R_y(\beta) A \big(R_z(\alpha) R_y(\beta)\big)^{-1}$$

quindi ...
$$R_k(\theta) = \overbrace{R_z(\alpha) R_y(\beta)}^{R^0_{1}}\cdot \overbrace{R_z(\theta)}^{A} \cdot\overbrace{R_y(-\beta) R_z(-\alpha)}^{(R^0_{1})^{-1}}$$


> [!important]- Cinematica inversa : Trovare $\alpha$ e $\beta$
> Usando un po' di trigonometria, possiamo trovare gli angoli $\alpha$ e $\beta$:
$$\sin(\alpha) = \frac{k_y}{\sqrt{k_x^2 + k_y^2}}, \quad \cos(\alpha) = \frac{k_x}{\sqrt{k_x^2 + k_y^2}}$$
$$\sin(\beta) = \sqrt{k_x^2 + k_y^2}, \quad \cos(\beta) = k_z$$
>
$$R_k(\theta) = \begin{bmatrix} k_x^2(1-c_\theta)+c_\theta & k_x k_y(1-c_\theta)-k_z s_\theta & k_x k_z(1-c_\theta)+k_y s_\theta \\ k_x k_y(1-c_\theta)+k_z s_\theta & k_y^2(1-c_\theta)+c_\theta & k_y k_z(1-c_\theta)-k_x s_\theta \\ k_x k_z(1-c_\theta)-k_y s_\theta & k_y k_z(1-c_\theta)+k_x s_\theta & k_z^2(1-c_\theta)+c_\theta \end{bmatrix}$$

Anche se abbiamo quattro parametri per la definizione di $R$ abbiamo comunque solo <mark class="hltr-orange">3 gradi di libertà</mark>; questo perchè si ha il vincolo $\Vert k\Vert=1 \implies k_x^2 + k_y^2 + k_z^2 = 1$.

Questo vuol dire che quando scegliamo il valore $k_{x},k_{y},k_{z}$ le prime due scelte sono libere <mark class="hltr-red">mentre la terza è matematicamente vincolata</mark>. Per esempio ...
$$k_{z}=1-k_{y}-k_{x}$$

Abbiamo quindi realmente solo due gradi di libertà dati dalle coordinate e uno dato dall'angolo di rotazione.


Inoltre, dalla formula di Rodrigues:
$$R_k(\theta) = \begin{bmatrix} k_x^2(1-c_\theta)+c_\theta & k_x k_y(1-c_\theta)-k_z s_\theta & k_x k_z(1-c_\theta)+k_y s_\theta \\ k_x k_y(1-c_\theta)+k_z s_\theta & k_y^2(1-c_\theta)+c_\theta & k_y k_z(1-c_\theta)-k_x s_\theta \\ k_x k_z(1-c_\theta)-k_y s_\theta & k_y k_z(1-c_\theta)+k_x s_\theta & k_z^2(1-c_\theta)+c_\theta \end{bmatrix}$$

Possiamo vedere che $R_{-k}(-\theta) = R_k(\theta)$, cioè se invertiamo sia l'asse che l'angolo, otteniamo la stessa rotazione.

Questo significa che la parametrizzazione asse-angolo <mark class="hltr-orange">non è globalmente invertibile</mark> : non è possibile, data una qualunque matrice di rotazione $R$, risalire a un set **unico** ( *un set legato ad una singola rotazione* ) di parametri $k$ e $\theta$. 


> [!important]- Cause della non-invertibilità globale
> Dire che la rotazione non è globalmente invertibile implica che ci siano uno o più punti dove una rotazione $R$ viene mappata a più di un set $(k,\theta)$.
> 
> Questi casi sono :
> > Inversione asse-angolo
>    $$R_{-k}(-\theta) = R_k(\theta)$$
> 
> Ogni rotazione può essere mappata a due set: $(k,\theta)$ e $(-k,-\theta)$. 
> 
> 
> > Rotazione identità   
>    $$\theta = 0$$
>  
>  Con angolo $0$ si ha che $R=I$ e questo implica che $k$ può essere un **qualunque vettore** nello spazio. SI hanno quindi infinite possibilità per $k$ ( *singolarità* ).
>  
> > Rotazione di $180°$
> > $$R_{k}(\pi) = R_{-k}(\pi)$$
> 
> Ruotare di un angolo pari a $180°$ ( *$\pi$* ) produce la stessa rotazione per $k$ e $-k$.
>  
>  > Rotazioni periodiche
>  > $$\forall k\geq 1\mid R_{\mathbf{k}}(\theta) = R_{\mathbf{k}}(\theta + 2\pi k) $$
>   
> Ovviamente, aggiungendo $360°$ di rotazione si ritorna allo stesso angolo, seppur l'angolo di rotazione non sia lo stesso.


Tuttavia, a differenza degli angoli RPY questa parametrizzazione è <mark class="hltr-purple">localmente invertibile</mark> : dato un set di vincoli che definiscono una *regione locale* è possibile l'inversione di una qualunque rotazione $R$.


> [!important]- Vincoli necessari all'inversione locale
> Si applicano due vincoli standard alle parametrizzazioni asse-angolo per evitare le ridondanze causate da $R_{-k}(-\theta) = R_k(\theta)$ :
> > Restrizione dell'angolo
> > $$\theta \in (0,\pi)$$
> 
> Escludendo $\theta \lt 0$ rimuoviamo anche il problema causato da $R_k(\theta)=R_{-k}(-\theta)$. Per raggiungere le rotazioni oltre i $180°$ ( *$\pi$* ) basta **invertire l'asse di rotazione $k$**.
> 
> > Evitare i punti confine
> > $$\theta\ne 0,\pi$$
> 
> Evitando i due punti confine rimuoviamo i problemi legati ad essi. Questo <mark class="hltr-red">NON VUOL DIRE CHE LA ROTAZIONE NON ESISTE</mark>, semplicemente all'interno di formule matematiche e/o codice viene trattato come un edge-case con un comportamento speciale.


##### Formula di Rodrigues
> La formula di Rodrigues provvede un metodo efficiente per ruotare un vettore $v$ qualsiasi nello spazio attorno ad un asse $k$ per un angolo $\theta$.  

(*$v$ può essere un vettore con una direzione o un punto, entrambi nello spazio. Il signifcato è secondario.*)  

Si possono avere due risultati dalla formula :
 - Il vettore <mark class="hltr-purple">$v_{rot}$</mark> ruotato
 - La rotazione/trasformazione <mark class="hltr-orange">$R_{k}(\theta)$</mark>

Nel primo caso la formula è la seguente ...
$$\mathbf{v}_{\text{rot}} = \mathbf{v}\cos\theta + (\mathbf{k} \times \mathbf{v})\sin\theta + \mathbf{k}(\mathbf{k} \cdot \mathbf{v})(1 - \cos\theta)$$ 
Per raggiungere il secondo caso invece dobbiamo rimuovere ( *fattorizzare* ) $\mathbf v$. 

Tuttavia per motivi legati alle proprietà del <mark class="hltr-blue">prodotto vettoriale</mark> ( <mark class="hltr-red">DIVERSO DAL PRODOTTO SCALARE TRA VETTORI</mark> ), questo è difficile.

Convertire $(\mathbf{k} \times \mathbf{v})$ in una moltiplicazione scalare tra matrici $\mathbf {Kv}$ risolve questo problema.

> [!error] Matrice anti-simmetrica $\mathbf K$
> 
> La matrice $\mathbf{K}$ ( *anche scritta $[\mathbf k]_{\times}$* ) è detta formalmente *matrice anti-simmetrica*.
> 
> Tuttavia in ambito di robotica è detta anche **matrice del prodotto vettoriale**, dato che il suo scopo principale è convertire un prodotto vettoriale in una moltiplicazione tra matrici; come in questo caso.
> 
> $\mathbf K$ è definita come ...
> $$\mathbf [\mathbf{k}]_{\times} = \begin{bmatrix} 0 & -k_z & k_y \\ k_z & 0 & -k_x \\ -k_y & k_x & 0 \end{bmatrix}$$
> 
> e ha le proprietà :
> - $\mathbf K \mathbf v = (\mathbf k \times \mathbf v)$
> - $\mathbf K^T=-\mathbf K$
> - Diagonale a $0$
> - $\mathbf{K}^2 = \mathbf{k}\mathbf{k}^T - \mathbf{I} \quad \implies \quad \mathbf{k}\mathbf{k}^T = \mathbf{I} + \mathbf{K}^2$
> 
> ---
> $\mathbf K$ ha sempre questa forma, quindi basta ricordarla.
> 
> Per completezza, in questo caso possiamo trovarla conoscendo il risultato di $(\mathbf v \times \mathbf k)$ possiamo trovare la formula inversa dato che ...
>    $$(\mathbf v \times \mathbf k)= \mathbf K \mathbf v =  \begin{bmatrix} k_y v_z - k_z v_y \\ k_z v_x - k_x v_z \\ k_x v_y - k_y v_x \end{bmatrix}$$
>    da cui ...
>    $$\begin{bmatrix} k_y v_z - k_z v_y \\ k_z v_x - k_x v_z \\ k_x v_y - k_y v_x \end{bmatrix} = \begin{bmatrix} M_{11} & M_{12} & M_{13} \\ M_{21} & M_{22} & M_{23} \\ M_{31} & M_{32} & M_{33} \end{bmatrix} \begin{bmatrix} v_x \\ v_y \\ v_z \end{bmatrix}$$
>    a questo punto basta risolvere $M_{ab}$ attraverso delle semplici formule inverse.

Una volta che abbiamo $\mathbf K$ possiamo fattorizzare $\mathbf v$ e ottenere ...
$$\mathbf{R} = \mathbf{I} + (\sin\theta)\mathbf{K} + (1 - \cos\theta)\mathbf{K}^2$$

dato che $\mathbf {Rv} = \mathbf {v_{rot}}$ .

> [!info]- Passi per la fattorizzazione
> Partendo dalla formula per il vettore con $\mathbf K$ ...
> $$\mathbf{Rv} = \mathbf{v}\cos\theta + \mathbf{Kv}(\sin\theta) + \mathbf{k}(\mathbf{k} \cdot \mathbf{v})(1 - \cos\theta)$$
> raggruppiamo il terzo termine ...
> $$\mathbf{Rv} = (\cos\theta)\mathbf{v} + (\sin\theta)\mathbf{Kv} + (\mathbf{k}\mathbf{k}^T)(1 - \cos\theta)\mathbf{v}$$
> fattorizzo $\mathbf v$ ...
> $$\mathbf{R} = (\cos\theta)\mathbf{I} + (\sin\theta)\mathbf{K} + (\mathbf{k}\mathbf{k}^T)(1 - \cos\theta)$$
> per le proprietà della matrice $\mathbf K$ anti-simmetrica raggruppiamo a potenza...
> $$\mathbf{R} = (\cos\theta)\mathbf{I} + (\sin\theta)\mathbf{K} + (\mathbf{K^2}+\mathbf{I})(1 - \cos\theta)$$
> espandiamo il terzo termine ...
> $$\mathbf{R} = (\cos\theta)\mathbf{I} + (\sin\theta)\mathbf{K} + \mathbf{K^2}(1 - \cos\theta)+\mathbf{I}(1 - \cos\theta)$$
> semplifico primo e quarto termine ...
> $$\mathbf{R} = \mathbf{I} + (\sin\theta)\mathbf{K} + \mathbf{K^2}(1 - \cos\theta)$$

<hr style="width: 70%; margin-left: auto;margin-right: auto;">

#### Quaternioni
> Siano $1$ e $i, j, k$ gli elementi di base.  
> Valgono i seguenti assiomi: 
> - $i^2 = j^2 = k^2 = 1$
> - $ijk = -1$.  
> 
> Un quaternione è definito come un elemento di uno spazio vettoriale sui reali, della forma ... $$q = q_0 + q_1 i + q_2 j + q_3 k$$
> Una notazione sintetica distingue la **parte scalare** $q_0$ e la **parte vettoriale** : $$q = q_1 i + q_2 j + q_3 k\mid q = q_0 + q$$Una notazione equivalente è $[q_0, q]$.
> 


Consideriamo un numero reale $x \in \mathbb{R}$. Il suo inverso è definito come ...
$$\frac{1}{x} \cdot x = 1 \implies x^{-1} = \frac{1}{x}$$

Consideriamo ora un elemento di $\mathbb{R}^2$, $x = \begin{bmatrix} a \\ b \end{bmatrix}$.  
Possiamo trovare il suo inverso usando i numeri complessi. 


Definiamo $z = a + ib$, dove $i$ è l'unità immaginaria. Sia $z^* = a - ib$ il *complesso coniugato*. Abbiamo:
$$\left. z_1 = \frac{z^*}{|z|^2}\ \middle|\ z \cdot z_1 = \frac{z \cdot z^*}{|z|^2} = \frac{a^2 + b^2}{a^2 + b^2} = 1 \implies z^{-1} = z_1\right.$$

Questo non vale per $\mathbb{R}^3$, tuttavia vale per $\mathbb{R}^4$ usando i **quaternioni**.


> [!important]- Numeri Complessi
> > Siano $1$ ed $i$ gli elementi di base. 
> > Vale il seguente assioma: $i^2 = -1$.  
> > 
> > Un numero complesso è definito come un elemento di uno spazio vettoriale sui reali, della forma ...$$z = x + iy$$con $x, y \in \mathbb{R}$.
>
> Questa definizione ci dice due cose principali :
> - $x,y\in \mathbb R$
> - Un numero complesso è rappresentabile come vettore
> 
> Un numero complesso è rappresentabile in un diagramma cartesiano con assi basati sugli elementi di base.
> 
> In particolare ...
> -  $x$ è la coordinata sull'asse <mark class="hltr-orange">reale</mark> legata all'<mark class="hltr-orange">elemento base 1</mark>
> -  $y$ è la coordinata sull'asse <mark class="hltr-purple">immaginario</mark> legata all'<mark class="hltr-purple">elemento base $i$</mark>
>
>Questo significa anche che valgono le operazioni standard dei vettori per i numeri complessi ...
>- **Addizione** : $(x_1 + i y_1) + (x_2 + i y_2) = (x_1 + x_2) + i(y_1 + y_2)$
>- **Moltiplicazione per uno scalare** : $(x_1 + i y_1) + (x_2 + i y_2) = (x_1 + x_2) + i(y_1 + y_2)$
>- **Moltiplicazione scalar**e : $(x_{1}+iy_{1})\cdot(x_{2}+iy_{2})=x_{1}x_{2}+y_{1}y_{2}$
>
><hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
> Serve anche ricordare la <mark class="hltr-blue">forma di eulero</mark> dei numeri complessi :
> $$z = r e^{i\theta}$$
> dove ...
> - $r$ è la **lunghezza/magnitudine** : $r = \vert{}z\vert{} = \sqrt{x^2 + y^2}$
> - $\theta$ è l'**angolo del vettore** rispetto a $x$ : $\theta = \text{atan2}(y, x)$
>
><hr style="width: 70%; margin-left: auto;margin-right: auto;">
>
> E' importante capire che i valori immaginari dei numeri complessi ( *in questo caso $i$* ) <mark class="hltr-red">NON HANNO UN VALORE, SONO INVECE DEFINITI MATEMATICAME</mark>.
> 
> Questo significa che anche se diciamo che $i=\sqrt{-1}$  e sia che il risultato di $\sqrt{-1}$ sia un valore $\beta$ allora $i=\beta$.
> 
> Un esempio pratico che servirà in seguito è che definiamo le unità base dei quaternioni come $i,j,k,1$ per cui vale ...
> $$i^2=j^2=k^2=ijk=-1$$
> 
> Che se svolgiamo le operazioni non ha senso dato che non esiste modo in cui ...
> 
$$\begin{array}{c}i,j,k=\sqrt{-1} & \text{and} & i\cdot j\cdot k=-1\end{array}$$
>
> Tuttavia è una definizione valida per i numeri complessi.

##### Prodotto tra quaternioni

Data la definizione dell'assioma sulle basi dei quaternioni, possiamo  derivare le seguenti moltiplicazioni tra gli elementi ( *derivate tramite trasformazioni aritmetiche* ) :
$$\begin{array}{l} 
ij=k, & ji=-k \\
jk=i, & kj=-i \\
ki=j, & ik=-j
\end{array}$$

Queste relazioni si rifanno ai risultati dati dal *prodotto vettoriale*.

Dato che si parla di vettori valgono anche l' operazione di **prodotto scalare** :
 $$\left.\begin{array}{l}\mathbf u=\mathbf u_{1}x+\mathbf u_{2}y \\
 \mathbf v=\mathbf v_{1}x+\mathbf v_{2}y
 \end{array}\ \middle|\ \mathbf u\cdot \mathbf v=\mathbf u_{1}\mathbf v_{1}+\mathbf u_{2}\mathbf v_{2}\right.$$
 
da cui deriva la formula per il *coseno* dell'angolo tra i vettori $\mathbf u\cdot \mathbf v=\vert \mathbf u\vert\vert \mathbf v\vert \cos \theta$;

e l'operazione di **prodotto vettoriale** :
 $$\left.\begin{array}{l}
 \mathbf u=\mathbf u_{1}x+\mathbf u_{2}y+\mathbf u_{3}z \\
 \mathbf v=\mathbf v_{1}x+\mathbf v_{2}y+\mathbf v_{3}z
 \end{array}\ \middle|\  
 
s = a\times b = \mathbf s_{1}x+\mathbf s_{2}y+\mathbf s_{3}z = \begin{cases} s_1 = u_2 v_3 - u_3 v_2 \\ s_2 = u_3 v_1 - u_1 v_3 \\ s_3 = u_1 v_2 - u_2 v_1 \end{cases}
 
 \right.$$
 
 da cui deriva la formula per il *seno* dell'angolo tra i vettori $\vert\mathbf s\vert=\vert \mathbf u\vert\vert \mathbf v\vert \sin \theta$.
<hr style="width: 40%; margin-left: auto;margin-right: auto;">


Possiamo quindi usare la notazione sintetica per definire un prodotto tra quaternioni in questo modo :

(*Per chiarezza $\mathbf p,\mathbf q$ sono i quaternioni originali non separati, mentre $p,q$ sono la parte vettoriale della short-hand*)

$$\mathbf{pq} = (p_0 + p)(q_0 + q) = p_0 q_0 + p_0 q + q_0 p + pq$$

dove il prodotto della parte vettoriale viene espanso come $$ pq = -p \cdot q + p \times q$$

Possiamo quindi scrivere la formula completa di un prodotto tra quaternioni come ... $$\mathbf{pq} = (p_0 q_0 - p \cdot q) + (p_0 q + q_0 p + p \times q)$$
Il prodotto di quaternioni è ...
- <mark class="hltr-orange">Associativo</mark> : $(pq)o=p(qo)$
- <mark class="hltr-red">NON</mark><mark class="hltr-purple"> commutativo</mark> : $pq \neq qp$

##### Coniugato di un quaternione
Il coniugato di un quaternione viene definito come ...
$$q = q_0 + q_1 i + q_2 j + q_3 k \implies q^* = q_0 - q_1 i - q_2 j - q_3 k$$
in linea con il coniugato dei numeri complessi.


Applicando questa definizione otteniamo che ...
$$q q^* = (q_0 + q)(q_0 - q) = q_0^2 + q_1^2 + q_2^2 + q_3^2$$

##### Lunghezza di un Quaternione
La quantità ... $$|q| = \sqrt{q q^*} = \sqrt{q_0^2 + q_1^2 + q_2^2 + q_3^2}$$ è definita come la **lunghezza o norma** del quaternione.

##### Inverso di un quaternione
In base alla definizione precedente, un quaternione ( *eccetto 0* ) ha un <mark class="hltr-orange">unico</mark> inverso ...
$$q^{-1} = \frac{q^*}{|q|^2} \implies q \frac{q^*}{|q|^2} = \frac{q q^*}{|q|^2} = 1$$

La relazione precedente si semplifica ulteriormente quando il quaternione ha lunghezza unitaria ( *$=1$* ).

##### Quaternioni unitari e proprietà | Rotazioni tramite quaternioni
Sia la forma di Eulero per un numero complesso ...
$$z = r \big(cos (θ) + i \sin (\theta)\big)$$

Moltiplicando due n.complessi in questa forma otteniamo :
$$z_1 z_2 = \rho_1 \rho_2 (\cos(\theta_1 + \theta_2) + j\sin(\theta_1 + \theta_2))$$

Possiamo vedere che moltiplicare un n.complesso per un altro con **norma unitaria** ( *modulo unitario $\vert z \vert=1$* ), risulta in una <mark class="hltr-orange">rotazione</mark>.  

Invece, moltiplicare un numero complesso per il suo coniugato <mark class="hltr-purple">"annulla" la rotazione</mark>, allineando il risultato con l'asse reale.

<hr style="width: 40%; margin-left: auto;margin-right: auto;">

Consideriamo ora un quaternione di lunghezza unitaria $q = [\eta, \epsilon]$, dove ...
- $\eta$ : parte scalare 
- $\epsilon = [ \epsilon_x, \epsilon_y, \epsilon_z ]$ : parte vettoriale

Possiamo esprimere un quaternione di lunghezza unitaria come :
$$q = \left[\cos\left(\frac{\theta}{2}\right), \sin\left(\frac{\theta}{2}\right) k\right]$$
dove...
- $k = \frac{\epsilon}{|\epsilon|}$ vettore unitario 
- $\theta = 2\,\text{atan2}(|\epsilon|, \eta)$

Il quaternione di lunghezza unitaria $q = \left[\cos\left(\frac{\theta}{2}\right), \sin\left(\frac{\theta}{2}\right) k\right]$ <mark class="hltr-red">rappresenta una rotazione dell'angolo $\theta$ attorno all'asse definito dal vettore unitario $k$</mark>.


Possiamo vedere questo considerando un quaternione $p=[0,\mathbf p]$ puro, cioè rappresentante un vettore $\mathbf p$.

Il vettore ruotato $\mathbf p'$, rappresentato dal quaternione $p'$, può essere trovato con la formula ...

$$p' = [0,\mathbf p']= qpq^*$$

tramite passaggi algebrici ( *omessi perchè numerosi e complessi* ) possiamo anche vedere che la parte vettoriale $\mathbf p'$ vale ...

$$\mathbf p' = (\eta^2 - |\epsilon|^2)\mathbf  p + 2\eta(\epsilon \times \mathbf p) + 2(\epsilon \cdot \mathbf  p)\epsilon$$

che con la sostituzione dei valori contenuti in $q$ abbiamo :

$$\mathbf p' = \cos(\theta)\mathbf  p + \sin(\theta)(k \times \mathbf  p) + (1 - \cos(\theta))(k \cdot \mathbf  p) k$$

Possiamo vedere che è la [[#Formula di Rodrigues]].

<hr style="width: 40%; margin-left: auto;margin-right: auto;">


Riassumendo, 
dato un quaternione di lunghezza unitaria ...
$$q = [\eta, \epsilon] = \left[\cos\left(\frac{\theta}{2}\right), \sin\left(\frac{\theta}{2}\right) k\right]$$

possiamo associarlo ad una rotazione dell'angolo $\theta$ attorno al vettore unitario $k$.

I parametri sono legati da:
- $k = \frac{\epsilon}{|\epsilon|}$
- $\theta = 2\,\text{atan2}(|\epsilon|, \eta)$

La corrispondente matrice di rotazione è data da :
$$R_k(\theta) = \begin{bmatrix} k_x^2(1-c_\theta)+c_\theta & k_x k_y(1-c_\theta)-k_z s_\theta & k_x k_z(1-c_\theta)+k_y s_\theta \\ k_x k_y(1-c_\theta)+k_z s_\theta & k_y^2(1-c_\theta)+c_\theta & k_y k_z(1-c_\theta)-k_x s_\theta \\ k_x k_z(1-c_\theta)-k_y s_\theta & k_y k_z(1-c_\theta)+k_x s_\theta & k_z^2(1-c_\theta)+c_\theta \end{bmatrix}$$
con $c_\theta = \cos(\theta)$ e $s_\theta = \sin(\theta)$.

I parametri di un quaternione di lunghezza unitaria corrispondono a <mark class="hltr-orange">tre gradi di libertà</mark>, che possono essere espressi in due modi diversi :
*  $\eta, \epsilon_x, \epsilon_y, \epsilon_z$, con il vincolo $\eta^2 + \epsilon_x^2 + \epsilon_y^2 + \epsilon_z^2 = 1$.
* $\theta, k_x, k_y, k_z$, con il vincolo $k_x^2 + k_y^2 + k_z^2 = 1$.

##### Cinematica inversa per quaternioni
Sia una matrice di rotazione ...
$$R = \begin{bmatrix} r_{11} & r_{12} & r_{13} \\ r_{21} & r_{22} & r_{23} \\ r_{31} & r_{32} & r_{33} \end{bmatrix}$$

possiamo risolvere il problema inverso :

$$\begin{array}{c}\eta = \frac{1}{2}\sqrt{r_{11} + r_{22} + r_{33} + 1}\\\\\epsilon = \frac{1}{2}\begin{bmatrix} \text{sgn}(r_{32} - r_{23})\sqrt{r_{11} - r_{22} - r_{33} + 1} \\ \text{sgn}(r_{13} - r_{31})\sqrt{r_{22} - r_{33} - r_{11} + 1} \\ \text{sgn}(r_{21} - r_{12})\sqrt{r_{33} - r_{11} - r_{22} + 1} \end{bmatrix}\end{array}$$

dove $\text{sgn}(x)$ è definita come :
$$\text{sgn}(x) = \begin{cases} 1 & x \ge 0 \\ -1 & x < 0 \end{cases}$$

##### Composizione di rotazioni su quaternioni
Consideriamo due rotazioni tramite quaternioni $q_0$ e $q_1$.  
La prima rotazione trasforma un vettore $p$ in $p'$, e la seconda trasforma $p'$ in $p''$.  

Possiamo rappresentare queste rotazioni usando quaternioni puri $p = [0, p]$, $p' = [0, p']$, e $p'' = [0, p'']$.

Abbiamo:
$$\begin{array}{l}p' = q_0 p q_0^*\\ p'' = q_1 p' q_1^* = q_1 (q_0 p q_0^*) q_1^* = (q_1 q_0) p (q_0^* q_1^*) = (q_1 q_0) p (q_1 q_0)^*\end{array}$$

L'ultimo passaggio vale perché <mark class="hltr-orange">il coniugato di un prodotto è il prodotto dei coniugati in ordine inverso, cioè $(q_1 q_0)^* = q_0^* q_1^*$.</mark>

Pertanto, la composizione di due rotazioni è equivalente al **prodotto dei rispettivi quaternioni**. 

##### Operazioni e proprietà dei quaternioni relative alle rotazioni

I quaternioni forniscono operazioni efficienti che corrispondono direttamente a quelle usate con le matrici di rotazione.

| Operazione                  | Matrici di Rotazione  | Quaternioni Unitari                                                                                                                |
| :-------------------------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| **Inversione**              | $R^{-1} = R^T$        | $q^{-1} = (\eta, -\epsilon)$                                                                                                       |
| **Composizione**            | $R = R_1 R_2$         | $q = q_1 q_2$                                                                                                                      |
| **Formula di Composizione** | Matrix Multiplication | $q_1 q_2 = (\eta_1 \eta_2 - \epsilon_1 \cdot \epsilon_2, \; \eta_1 \epsilon_2 + \eta_2 \epsilon_1 + \epsilon_1 \times \epsilon_2)$ |

Per esempio :
* **Inversione:** L'inverso di un quaternione di lunghezza unitaria è il suo coniugato, che si trova semplicemente negando la parte vettoriale.
* **Composizione:** La composizione di due rotazioni è un semplice prodotto di quaternioni. Il quaternione risultante rappresenta la rotazione combinata. Questo è più efficiente e numericamente più stabile rispetto alla composizione di matrici di rotazione.

Inoltre abbiamo le seguenti proprietà :

* **Rappresentazione Compatta:** I quaternioni usano quattro numeri per rappresentare una rotazione, rispetto ai nove di una matrice.
* **Assenza di Gimbal Lock:** A differenza degli angoli di Eulero, i quaternioni forniscono una rappresentazione continua e non singolare delle rotazioni, evitando completamente il problema del Gimbal Lock. 
* **Composizione Efficiente:** La composizione di due rotazioni è ottenuta tramite una semplice moltiplicazione di quaternioni.
* **Interpolazione Diretta:** I quaternioni permettono un'interpolazione fluida e a velocità costante tra due rotazioni ( *interpolazione sferica lineare o $\text{slerp}$* ).
* **Non-Unicità:** Una singola rotazione può essere rappresentata da due quaternioni, $q$ e $-q$. Sebbene questa sia una forma di ridondanza, si tratta di un problema minore che non sminuisce i benefici pratici dei quaternioni.

---

## Trasformazioni omogenee
Abbiamo stabilito che la posa di un corpo rigido è definita dalla <mark class="hltr-purple">posizione</mark> e dall'<mark class="hltr-orange">orientamento</mark> di un sistema di riferimento solidale ad esso.  


> [!example] Esempio di posa
> ![[EMBED/2-geometria_1 5.png]]
[[2-geometria_1.pdf#page=75&rect=117,158,248,236|2-geometria_1, p.50]]


Abbiamo poi considerato solo la rotazione ( *origini dei sistemi coincidono* ), ignorando la traslazione .  
Torniamo ora al caso generale, in cui sono presenti sia la rotazione che la traslazione.

Un punto $p^1$ nel sistema $O_{1}$ può essere espresso nel sistema $O_{0}$ come ...
$$p^0 = o_1^0 + R_1^0 p^1$$

di cui la relazione inversa è :
$$R_1^0 p^1 = p^0 - o_1^0$$

Mentre $p_{1}$ viene espresso come :
$$p^1 = (R_1^0)^{-1}(p^0 - o_1^0) = R_1^{0T}(p^0 - o_1^0) = R_0^1(p^0 - o_1^0) = R_0^1 p^0 - R_0^1 o_1^0$$

### Rappresentazioni e trasformazioni omogenee

Serve un modo rappresentare traslazione e rotazione in maniera compatta. 

Per fare questo usiamo una *rappresentazione omogenea* $\tilde{p}$ di $p$ associandogli una coordinata con valore $1$.

$$\tilde{p} = \begin{bmatrix} p \\ 1 \end{bmatrix}$$

Possiamo ora costruire una *matrice di trasformazione omogenea* in questo modo :

$$A_1^0 = \begin{bmatrix} R_1^0 & o_1^0 \\ 0^T & 1 \end{bmatrix}$$

A questo punto, la trasformazione di un punto dal sistema $O_{1}$ al sistema $O_{0}$ può essere scritta come una moltiplicazione matrice-vettore ... 
$$\tilde{p}^0 = A_1^0 \tilde{p}^1$$

La coordinata inversa, invece, è trovata invertendo la matrice ...
$$\tilde{p}^1 = A_0^1 \tilde{p}^0 = (A_1^0)^{-1} \tilde{p}^0$$

quest'ultima può essere derivata in questo modo :
$$A_0^1 = (A_1^0)^{-1} = \begin{bmatrix} R_1^{0T} & -R_1^{0T} o_1^0 \\ 0^T & 1 \end{bmatrix}$$

<hr style="width: 40%; margin-left: auto;margin-right: auto;">

A differenza delle matrici di rotazione, le trasformazioni omogenee <mark class="hltr-orange">non sono ortogonali</mark>, cioè $A^{-1} \neq A^T$.

Il formalismo *omogeneo* ci permette di generalizzare le proprietà del gruppo di rotazione per includere le traslazioni (*Cioè espandere proprietà e/o operazioni delle rotazioni alle traslazioni*).

Eg. una sequenza di trasformazioni negli assi correnti può essere scritta come un semplice prodotto di matrici:
$$\tilde{p}^0 = A_1^0 A_2^1 \cdots A_n^{n-1} \tilde{p}^n$$

L'insieme di tutte le trasformazioni omogenee forma un gruppo chiamato <mark class="hltr-blue">Special Euclidean Group</mark>, indicato come $SE(3)$. 

Questo gruppo è il prodotto semidiretto $SE(3) = \mathbb{R}^3 \rtimes SO(3)$.

