# Cheat Sheet: Funzioni Trigonometriche e Ciclometriche

Un formulario essenziale e compatto su definizioni, proprietà, valori notevoli e formule utili delle **funzioni trigonometriche (dirette e inverse)**.

---

## 1. Panoramica delle Funzioni

### Funzioni Dirette
| Funzione | Nome | Dominio | Insieme Immagine | Periodo |
| :--- | :--- | :--- | :--- | :--- |
| $\sin(x)$ | Seno | $\mathbb{R}$ | $[-1, 1]$ | $2\pi$ |
| $\cos(x)$ | Coseno | $\mathbb{R}$ | $[-1, 1]$ | $2\pi$ |
| $\tan(x)$ | Tangente | $\mathbb{R} \setminus \left\{\frac{\pi}{2} + k\pi\right\}$ | $\mathbb{R}$ | $\pi$ |
| $\cot(x)$ | Cotangente | $\mathbb{R} \setminus \{k\pi\}$ | $\mathbb{R}$ | $\pi$ |

### Funzioni Inverse (Ciclometriche)
| Funzione | Nome | Dominio | Insieme Immagine (Codominio) |
| :--- | :--- | :--- | :--- |
| $\arcsin(x)$ | Arcoseno | $[-1, 1]$ | $\left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$ |
| $\arccos(x)$ | Arccoseno | $[-1, 1]$ | $[0, \pi]$ |
| $\arctan(x)$ | Arcotangente | $\mathbb{R}$ | $\left(-\frac{\pi}{2}, \frac{\pi}{2}\right)$ |

---

## 2. Relazioni Fondamentali

### Definizioni di Base
* $\tan(x) = \frac{\sin(x)}{\cos(x)}$
* $\cot(x) = \frac{\cos(x)}{\sin(x)} = \frac{1}{\tan(x)}$
* $\sec(x) = \frac{1}{\cos(x)}$ *(Secante)*
* $\csc(x) = \frac{1}{\sin(x)}$ *(Cosecante)*

### Identità Trigonometrica Fondamentale (Prima legge)
$$\sin^2(x) + \cos^2(x) = 1$$

Da cui derivano:
* $\sin(x) = \pm\sqrt{1 - \cos^2(x)}$
* $\cos(x) = \pm\sqrt{1 - \sin^2(x)}$
* $1 + \tan^2(x) = \frac{1}{\cos^2(x)} = \sec^2(x)$

---

## 3. Valori Notevoli

| $\theta$ (gradi) | $\theta$ (rad) | $\sin(\theta)$ | $\cos(\theta)$ | $\tan(\theta)$ |
| :---: | :---: | :---: | :---: | :---: |
| $0^\circ$ | $0$ | $0$ | $1$ | $0$ |
| $30^\circ$ | $\frac{\pi}{6}$ | $\frac{1}{2}$ | $\frac{\sqrt{3}}{2}$ | $\frac{\sqrt{3}}{3}$ |
| $45^\circ$ | $\frac{\pi}{4}$ | $\frac{\sqrt{2}}{2}$ | $\frac{\sqrt{2}}{2}$ | $1$ |
| $60^\circ$ | $\frac{\pi}{3}$ | $\frac{\sqrt{3}}{2}$ | $\frac{1}{2}$ | $\sqrt{3}$ |
| $90^\circ$ | $\frac{\pi}{2}$ | $1$ | $0$ | Non definita |
| $180^\circ$ | $\pi$ | $0$ | $-1$ | $0$ |
| $270^\circ$ | $\frac{3\pi}{2}$ | $-1$ | $0$ | Non definita |

---

## 4. Proprietà di Simmetria e Parità

### Pari e Dispari
* **Dispari:** $\sin(-x) = -\sin(x)$
* **Pari:** $\cos(-x) = \cos(x)$
* **Dispari:** $\tan(-x) = -\tan(x)$
* **Dispari:** $\arctan(-x) = -\arctan(x)$
* **Né pari né dispari:** $\arccos(-x) = \pi - \arccos(x)$

### Angoli Complementari e Supplementari
* $\sin\left(\frac{\pi}{2} - x\right) = \cos(x)$
* $\cos\left(\frac{\pi}{2} - x\right) = \sin(x)$
* $\sin(\pi - x) = \sin(x)$
* $\cos(\pi - x) = -\cos(x)$

---

## 5. Formule Principali

### Addizione e Sottrazione
$$\sin(a \pm b) = \sin(a)\cos(b) \pm \cos(a)\sin(b)$$
$$\cos(a \pm b) = \cos(a)\cos(b) \mp \sin(a)\sin(b)$$
$$\tan(a \pm b) = \frac{\tan(a) \pm \tan(b)}{1 \mp \tan(a)\tan(b)}$$

### Duplicazione
$$\sin(2x) = 2\sin(x)\cos(x)$$
$$\cos(2x) = \cos^2(x) - \sin^2(x) = 2\cos^2(x) - 1 = 1 - 2\sin^2(x)$$
$$\tan(2x) = \frac{2\tan(x)}{1 - \tan^2(x)}$$

### Bisezione
$$\sin\left(\frac{x}{2}\right) = \pm\sqrt{\frac{1 - \cos(x)}{2}}$$
$$\cos\left(\frac{x}{2}\right) = \pm\sqrt{\frac{1 + \cos(x)}{2}}$$

---

## 6. Proprietà delle Funzioni Inverse

Per i rispettivi domini di appartenenza:
* $\sin(\arcsin(x)) = x \quad \text{per } x \in [-1, 1]$
* $\arcsin(\sin(x)) = x \quad \text{per } x \in \left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$
* $\cos(\arccos(x)) = x \quad \text{per } x \in [-1, 1]$
* $\tan(\arctan(x)) = x \quad \text{per } x \in \mathbb{R}$

### Identità Utili tra Funzioni Inverse
$$\arcsin(x) + \arccos(x) = \frac{\pi}{2}$$
$$\arctan(x) + \operatorname{arccot}(x) = \frac{\pi}{2}$$
$$\arctan(x) = \arcsin\left(\frac{x}{\sqrt{1+x^2}}\right)$$

---

## 7. Derivate e Integrali Fondamentali (Analisi)

| Funzione $f(x)$ | Derivata $f'(x)$ | Integrale $\int f(x)\,dx$ |
| :--- | :--- | :--- |
| $\sin(x)$ | $\cos(x)$ | $-\cos(x) + C$ |
| $\cos(x)$ | $-\sin(x)$ | $\sin(x) + C$ |
| $\tan(x)$ | $1 + \tan^2(x) = \frac{1}{\cos^2(x)}$ | $-\ln\vert\cos(x)\vert + C$ |
| $\arcsin(x)$ | $\frac{1}{\sqrt{1 - x^2}}$ | $x\arcsin(x) + \sqrt{1 - x^2} + C$ |
| $\arccos(x)$ | $-\frac{1}{\sqrt{1 - x^2}}$ | $x\arccos(x) - \sqrt{1 - x^2} + C$ |
| $\arctan(x)$ | $\frac{1}{1 + x^2}$ | $x\arctan(x) - \frac{1}{2}\ln(1 + x^2) + C$ |
