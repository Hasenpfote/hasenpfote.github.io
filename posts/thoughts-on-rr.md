---
title: "ロシアンルーレットについての考察"
description: "確率的に処理を打ち切るロシアンルーレットという手法についての考察をする"
categories:
  - computerscience
date: 2025-06-02T00:00:00+00:00
draft: false
---

ロシアンルーレットは，確率的に処理を打ち切ることで計算コストを削減しつつ，期待値を保つ手法であり，今回はその動作原理を確認してみる．

## ロシアンルーレットの定義

確率変数 $X$ と $\zeta\sim U(0,1)$ が独立であり，$0\lt q\lt 1$ とする． このとき，次のように定義する．

$$
X_{rr}
=
\begin{cases}
\frac{X}{q} & \zeta\lt q\\
0 & \text{otherwise}
\end{cases}
$$

この期待値は,

$$
\begin{align*}
E[X_{rr}]
&=E\left[\frac{X}{q}\middle|\zeta\lt q\right]\,P(\zeta\lt q) + E[0|\zeta\ge q]\,P(\zeta\ge q)\\
&=\frac{1}{q}\,E[X]\,q + E[0]\,(1-q)
=E[X]
\end{align*}
$$

となり，期待値は保存される．この性質により，確率的な打ち切りを行っても，統計的に偏りのない推定が可能になる．

### 補足: 期待値について

期待値に全確率の定理を適用すると,

$$
\begin{align*}
E[X]
&=\sum_{i} x_{i}\,P(X=x_{i})\\
&=\sum_{i} x_{i} \sum_{j} P(X=x_{i}|Y_{j})\,P(Y_{j}) \quad \text{(Law of total probability)}\\
&=\sum_{j} \left(\sum_{i} x_{i}\,P(X=x_{i}|Y_{j})\right) P(Y_{j})\\
&=\sum_{j} E[X|Y_{j}]\,P(Y_{j})
\end{align*}
$$

::: {.callout-note title="条件付き確率による全確率の定理"}
標本空間 $\Omega$ が互いに排反な $N$ 個の事象 $B_{1},\dots,B_{N}$ によって分割されているとする．このとき，任意の事象 $A$ に対して次が成り立つ．

$$
P(A)
=\sum_{i} P(A\cap B_{i})
$$

さらに $P(B_{i})\gt 0$ であるとき，条件付き確率の定義より,

$$
P(A|B_{i})
=\frac{P(A\cap B_{i})}{P(B_{i})}
$$

これを用いると，$P(A)$ は次のように表せる．

$$
P(A)
=\sum_{i} P(A|B_{i})\,P(B_{i})
$$
:::

## 指示関数を使った表記

$\zeta\sim U(0,1)$ に対して，指示関数 $1_{\zeta\lt q}$ を

$$
1_{\zeta\lt q}
=
\begin{cases}
1 & \zeta\lt q\\
0 & \text{otherwise}
\end{cases}
$$

と定義すると，$X_{rr}=\frac{X}{q}\,1_{\zeta\lt q}$ と書ける．$X$ と $\zeta$ は独立であることから,

$$
E[X_{rr}]
=E\left[\frac{X}{q}\,1_{\zeta\lt q}\right]
=E\left[\frac{X}{q}\right]\,E[1_{\zeta\lt q}]
=\frac{1}{q}\,E[X]\,P(\zeta\lt q)
=\frac{1}{q}\,E[X]\,q
=E[X]
$$

## 繰り返しと打ち切り

次のような絶対収束する無限級数を考える．

$$
S
=\sum_{k=0}^{\infty} a_{k}
$$

この和を有限の試行回数で評価したいとする．そこで，各項 $a_{k}$ を確率 $q$ で次に進めるとし，打ち切り確率を $1-q$ とする．このような設定で，確率変数 $N\sim G(1-q)$ に従って，次のような近似的な和を定義できる．

$$
S_{rr}
=\sum_{k=0}^{N} \frac{a_{k}}{P(N\ge k)}
$$

幾何分布 $G(1-q)$ の性質から $P(N\ge k)=q^{k}$ なので,

$$
S_{rr}
=\sum_{k=0}^{N} \frac{a_{k}}{q^{k}}
$$

この期待値は,

$$
\begin{align*}
E[S_{rr}]
&=E\left[\sum_{k=0}^{\infty} \frac{a_{k}}{q^{k}}\,1_{k\le N}\right]\\
&=\sum_{k=0}^{\infty} \frac{a_{k}}{q^{k}}\,E\left[1_{k\le N}\right]\\
&=\sum_{k=0}^{\infty} \frac{a_{k}}{q^{k}}\,P(N\ge k)\\
&=\sum_{k=0}^{\infty} \frac{a_{k}}{q^{k}}\,q^{k}
=\sum_{k=0}^{\infty} a_{k}
=S
\end{align*}
$$

### 例: 指数関数のマクローリン展開

指数関数のマクローリン展開は次のように表される．

$$
e^{x}
=\sum_{k=0}^{\infty} \frac{x^{k}}{k!}
$$

この展開に対しても，近似的な和を定義できる．

$$
S_{rr}
=\sum_{k=0}^{N} \frac{x^{k}}{k!\,P(N\ge k)}
$$

この期待値は,

$$
E[S_{rr}]
=E\left[\sum_{k=0}^{\infty} \frac{x^{k}}{k!\,P(N\ge k)}\,1_{k\le N}\right]
=\sum_{k=0}^{\infty} \frac{x^{k}}{k!}
=e^{x}
$$

## まとめ

今回は，固定された $q$ を用いてロシアンルーレットの基本的な性質を確認した．実際の応用，例えばパストレーシングでは，経路の貢献度に応じて $q$ を動的に調整することで，より柔軟かつ効率的な処理が行われる．

## Appendix: notebook

- <https://gist.github.com/Hasenpfote/3703d424966523a2e3aba4e1007529d9>

## References

- Pharr, M., Jakob, W., & Humphreys, G. (2016). Russian roulette and splitting. In *Physically based rendering: From theory to implementation* (3rd ed.). Morgan Kaufmann. <https://pbr-book.org/3ed-2018/Monte_Carlo_Integration/Russian_Roulette_and_Splitting>
