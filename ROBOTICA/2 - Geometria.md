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
> Restituisce l'angolo  nell'intervallo $(-\pi, \pi]$ ( $-180^\circ \to +180^\circ$ ).

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
