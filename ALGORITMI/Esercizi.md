---
Date created: 30-09-25 • 10:44
tags:
  - Algoritmi
Related PDF/DOC:
Related Pages:
---
$$
\exists c\gt0,\ \exists m\ge0\ :\ T(n)\le cg(n), \forall n\ge m
$$

$$
T([n/5])+T([7n/10])+\frac{11}{5}n
$$


## O($n^2$)
### C. base:

$$
T(1) = 1\le c*1^2 <=> \forall c>=1  
$$

### Ip. Induttiva

Formula:
$$
\forall k < n : T(k)\le ck
$$

Risol per O(n^2):

$$T(n^2) = T([n/5]^2)+T([7n/10]^2)+\frac{11}{5}n$$
$$\le c[n/5]^2+c[7n/10]^2+\frac{11}{5}n$$
$$
\le \frac{cn^2}{25} +\frac{cn^249}{100}+\frac{11}{5}n
$$
$$
\le \frac{cn^{2}53}{100}+\frac{11}{5}n \le? cn^2
$$
$$
\le \frac{cn53}{100}+\frac{220}{100} \le? cn
$$
$$
220 \le? cn100-cn53 => 220 \le cn47 => \frac{220}{47n}\le c
$$

Risol per O(n):

$$
T(n) = T([n/5])+T([7n/10])+\frac{11}{5}n 
$$
$$
\le c[n/5] + c[7n/10] + \frac{11}{5}n
$$
$$
\le cn\frac{1}{5} + cn\frac{7}{10} + n\frac{11}{5}
$$
$$
\le cn\frac{9}{10}+n\frac{22}{10} \le?\ cn
$$
$$
c\frac{9}{10}+\frac{22}{10}\le? c
$$
$$
22\le?\ 10c-9c = 22<=c
$$

Ovviamente vale solo per $c\ge22$ 


