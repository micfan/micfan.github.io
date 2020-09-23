---
title:  "证明: 非完全平方数的正平方根是无理数"
categories: math
---
## 求证

证明非完全平方数的正平方根是无理数. 例如 $$\sqrt{2}, \sqrt{3}, \sqrt{5}, ...$$

## 证明

若 $$n \in \mathbb{N^+}$$ 非完全平方数，则 $$\sqrt{n}$$ 是无理数.

利用反证法。设若 $$\sqrt{n}$$ 是有理数，则可设 $$\sqrt{n} = \frac{p}{q}$$ 其中 $$p, q$$ 是互质的正整数，对等号两侧分别求平方, 于是 $$p^2 = nq^2$$.

因为 $$p, q$$ 互质，则依裴蜀定理（Bézout's theorem），存在整数 $$a, b$$ 使得 

$$ap + bq = 1$$

对上式两侧同时乘以 $$p$$，则有 

$$p = ap^2 + bpq = anq^2 + bpq = (anq + bp)q$$

这意味着 $$\sqrt{n} = \frac{p}{q} = anq + bp \in \mathbb{N^+}$$ 与 $$n$$ 非完全平方数矛盾. 证毕.