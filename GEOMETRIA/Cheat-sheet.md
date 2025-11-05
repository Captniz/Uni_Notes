---
Date created: 22-10-25 • 14:26
tags:
  - Geometria
Related PDF/DOC:
Related Pages:
---
## Definizioni

| Simbolo                     | Nome                           | Definizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $\mathbb{N}$                | N. Naturali                    | ${1,2,3,4,5}$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| $\mathbb{Z}$                | N. Relativi                    | $0,\pm1,\pm2$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| $\mathbb{Q}$                | N. Razionali                   | $\frac{p}{q} \|\ p,q \in \mathbb{Z},q\neq0$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| $\mathbb{R}$                | N. Reali                       | $p^2,\sqrt{p}$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| $k$                         | Scalare                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| $R^2$                       | Piano                          | Rappresentazione di un **piano** (Coppie di reali)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| $R^3$                       | Spazio                         | Rappresentazione di uno **spazio** (Terne di reali)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| $R^n$                       | Spazio delle N-uple            | $\mathbb{R}^n=\{(a_1,..,a_n\ \|\ a_i\in\mathbb{R},i=1,..,n)\}$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| $M_{m\times n}\mathbb{(R)}$ | Matrici reali $m\times n$      | Insieme delle **matrici reali $m\times n$**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                             | Vettore riga                   | Matrice $(1\times n)$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                             | Vettore colonna                | Matrice $(m\times 1)$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                             | Versore                        | Vettore di **norma 1**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                             | Mat. conformabile              | $A\ conformabile\ B \iff A\in M_{q\times n}(\mathbb{R}),\ B\in M_{n\times k}(\mathbb{R})$<br><br>(*Numero Colonne A = Numero Righe B *)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| $A^T$                       | Mat. trasposta                 | $A^T =B\ trasposta\ di\ A \Rightarrow$ $B$ ottenuta scambiando le righe e le colonne di $A$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                             | Mat. simmetrica                | $A\ simmetrica\iff A=A^T$                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                             | Eqz. lineare                   | Equazione a multiple incognite di **grado 1**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                             | Sistema lineare                | Sistema composto da equazioni lineari                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                             | Sistemi equivalenti            | Sistemi lineari con le stesse soluzioni                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                             | Sistemi lineari omogenei       | Sistema lineare con tutti i **termini noti posti a 0**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                             | Vettori linearmente dipendenti | Due o più vettori si dicono linearmente indipendenti se... <br><br>Nessun vettore può essere espresso come **combinazione lineare** dei restanti vettori.<br><br>$$V \ \ lin.indip\  \iff V=v_1,..,v_n\ ; x\in \mathbb{K}\ \|\ \forall \overrightarrow{v} \in V\ , \overrightarrow{v}\neq \sum_{i=1}^{n}x_i\cdot v_i\ ,\ v_i\neq \overrightarrow{v}$$<br><br>Oppure ...<br>Se il vettore nullo può essere espresso come combinazione lineare dei vettori solo con tutti i coefficienti $x$ **nulli**.<br><br>$$V \ \ lin.indip\  \iff V=v_1,..,v_n\ ; x\in \mathbb{K}\ \|\ \vec{0} =  \sum_{i=1}^{n}x_i\cdot v_i\ \iff \forall x_i =0$$<br> |
|                             |                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |

## Vettori
> Rappresentazione
> $$\begin{array}{lcl}\overrightarrow{AB}\in\mathbb{R}^2 : \{(x_b-x_a),(y_b-y_a)\} \\ \overrightarrow{AB}\in\mathbb{R}^3 : \{(x_b-x_a),(y_b-y_a),(z_b-z_a)\}\end{array}$$

### Operazioni
#### Somma
$$\overrightarrow{v}+\overrightarrow{w} = \{(v_1+w_1),(v_2+w_2)\}$$

Generalmente nella somma valgono queste regole :
- $\overrightarrow{AB}+\overrightarrow{BC}=\overrightarrow{AC}$
- $\overrightarrow{AB}+\overrightarrow{AC}=\overrightarrow{AD}$

#### Moltiplicazione
##### Moltiplicazione per scalare
$$k\ \cdot\ (a,b) = \{(k\ \cdot\ a),(k\ \cdot\ b)\}$$

Generalmente per la moltiplicazione per scalare dei vettori valgono queste regole :
$$k\ \cdot\ \overrightarrow{AB} = \begin{cases} 
\overrightarrow{AB'}, & \mbox{if } t>0 \\ 
\overrightarrow{BA}, & \mbox{if } t=-1 \\ 
\overrightarrow{B'A}, & \mbox{if } t<0 
\end{cases}$$

##### Prodotto scalare
$$
\overrightarrow{v}\ \cdot\ \overrightarrow{w} = (v_1\cdot w_1\ +\ v_2\cdot w_2\ ) 
$$
##### Prodotto vettoriale
$$
\begin{array}{lcl}
\overrightarrow{v},\overrightarrow{w}\in\mathbb{R}^2\ \|\ \  \overrightarrow{v}\ \times\ \overrightarrow{w} = \{(v_1w_2),(v_2w_1) \} \\
\overrightarrow{v},\overrightarrow{w}\in\mathbb{R}^3\ \|\ \  \overrightarrow{v}\ \times\ \overrightarrow{w} = \{(v_2w_3-v_3w_2),(v_1w_3-v_3w_1),(v_1w_2-v_2w_1)\}
\end{array}
$$

#### Norma
$$\|\overrightarrow{v}\|=\sqrt{v_1^2+{...}+v_n^2}\ :\ \overrightarrow{v}\in\mathbb{R}^n $$
Equivalente alla [[#Lunghezza di un vettore|lunghezza]].

Se $\|\overrightarrow{v}\|=1$ allora $\overrightarrow{v}$ è un **versore**.

#### Normalizzazione ( Versore )
Cioè trasformazione in versore.

$$\overrightarrow{v'} = \frac{1}{\|\ \overrightarrow{v}\ \|}\overrightarrow{v}$$

#### Combinazione lineare
$$
k_1,..,k_n\in\mathbb{K}\ campo\ ;v,..v_n\in\mathbb{V}\ definito\ su\ \mathbb{K}\ |\ (v_1\cdot k_1+\ ...\ +v_n\cdot k_n)
$$

### Dati e proprietà

#### Opposti
$$
opposto\ \overrightarrow{AB}\ =-\overrightarrow{AB}=\overrightarrow{BA}
$$

Inoltre...
$$\overrightarrow{AB}+\overrightarrow{BA}=\emptyset$$

#### Vettori perpendicolari
$$\overrightarrow{v}\perp\overrightarrow{w}\iff(\overrightarrow{v}\cdot\overrightarrow{w})=0 \iff \|\overrightarrow{v}+\overrightarrow{w}\|=\|\overrightarrow{v}\|^2+\|\overrightarrow{w}\|^2$$

#### Vettori paralleli
$$\overrightarrow{v}\parallel\overrightarrow{w} \iff\overrightarrow{v}=k\cdot\overrightarrow{w}$$

#### Vettori linearmente dipendenti
##### Disuguaglianza Cauchy-Shwartz

$$\overrightarrow{v},\overrightarrow{w}\ \ linearmente\ indip\iff|v\cdot w|\le \|v\|\cdot\|w\|$$

##### Verifica
Due o più vettori si dicono linearmente indipendenti se... 

Nessun vettore può essere espresso come **combinazione lineare** dei restanti vettori.

$$V \ \ lin.indip\  \iff V=v_1,..,v_n\ ; x\in \mathbb{K}\ |\ \forall \overrightarrow{v} \in V\ , \overrightarrow{v}\neq \sum_{i=1}^{n}x_i\cdot v_i\ ,\ v_i\neq \overrightarrow{v}$$

Oppure ...

Se il vettore nullo può essere espresso come combinazione lineare dei vettori solo con tutti i coefficienti $x$ **nulli**.

$$V \ \ lin.indip\  \iff V=v_1,..,v_n\ ; x\in \mathbb{K}\ |\ \vec{0} =  \sum_{i=1}^{n}x_i\cdot v_i\ \iff \forall x_i =0$$
#### Proiezione ortogonale di un vettore su un altro

Proiezione di $v$ su $w$ :
$$pr_w(v) = \frac{v\cdot w}{\|\ w\ \|^2}w\ |\ v,w\in\mathbb{R}^n$$

#### Distanza tra destinazione di due vettori ( Distanza fra punti  )
$$
d(\ \overrightarrow{v},\ \overrightarrow{w}\ ) = \|\overrightarrow{v}-\overrightarrow{w}\|
$$

Ovviamente questa vale anche per i **punti**.

#### Lunghezza di un vettore
$$|\ \overrightarrow{v}\ |= \begin{cases} 
\sqrt{v_1^2+v_2^2}\ : & \overrightarrow{v}\in\mathbb{R}^2 \\ 
\sqrt{v_1^2+v_2^2+v_3^2}\ : & \overrightarrow{v}\in\mathbb{R}^3 
\end{cases}$$

#### Coseno dell'angolo tra due vettori
$$
\cos{\Theta} = \frac{\overrightarrow{v}\cdot\overrightarrow{w}}{\|\overrightarrow{v}\|\cdot\|\overrightarrow{w}\|}
$$

Vale $0$ se sono **perpendicolari**.

#### Aree e volumi
##### Area di un parallelogramma con lati due vettori
$$
A_p=|\ \overrightarrow{v}\times\overrightarrow{w}\ |

$$

##### Volume di un parallelepipedo con lati tre vettori
$$
V_p=|\ \overrightarrow{u}\cdot(\ \overrightarrow{v}\times\overrightarrow{w}\ )\ |
$$

Inoltre...
$$
h_p = |\ \overrightarrow{u}\ |\ \cdot\ |\ \cos\Theta\ |
$$


## Rette
> Rappresentazione
> $$r=\begin{cases}
ax_1+by_1=c &:& r\in\mathbb{R}^2 \\
y=mx+q &:& r\in\mathbb{R}^2 \\
\begin{cases}
ax+by+cz=d \\
kx+vy+uz=w
\end{cases} &:& r\in\mathbb{R}^3 \\
\end{cases}$$

In $\mathbb{R}^2$ le rette sono rappresentabili o con la forma **cartesiana** o con la forma **normale**. Mentre in $\mathbb{R}^3$ vengono rappresentate come un **intersezione tra piani**. 

### Operazioni

%%TODO%%

### Dati e proprietà

#### Distanza tra rette
$$
\begin{array}{lcl}
\text{Sia: }\\\
\overrightarrow{AB} \text{ il vettore direzione di }r\\
\overrightarrow{CD} \text{ il vettore direzione di }r'\\
\overrightarrow{XY} \text{ la differenza tra due punti di } r \text{ e }r'\text{ (Es: AC,AD,BC,BD)} \\
\\
\text{Allora:}\\

d(r,r')=\frac{\|v\times XY\|}{\|v\|} : v \text{ vettore direzione di }r\text{ o } r'
\end{array}
$$