---
title: 2D Simplex Noise の数学
description: 2022年に米国特許 US6867776B2 が満了し，普段使いがしやすくなった Kenneth Perlin 考案の Simplex Noise を題材に，2D のアルゴリズムを数式から整理する
date: 2026-09-03T09:00:00+09:00
categories:
  - math
  - computergraphics
draft: false
---

2D simplex noise では，入力点を Skew した座標系に変換して格子領域を求め，これを Unskew して元の座標系における基準位置を求める．基準位置から入力点が属する simplex を決定し，simplex を構成する 3 つの格子点との相対位置を求めた上で，各格子点からの寄与を評価する．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/belhmqu6mr?embed"></iframe>
<figcaption></figcaption>
</figure>
</div>

## Skew と格子座標の決定

入力点を

$$
\boldsymbol{x}
=
\begin{bmatrix}
x\\
y
\end{bmatrix}
$$

とすると，Skew 変換によって，

$$
\boldsymbol{x}^{'}
=
\begin{bmatrix}
x^{'}\\
y^{'}
\end{bmatrix}
=M\cdot\boldsymbol{x}
=
\begin{bmatrix}
1+F_{2} & F_{2}\\
F_{2} & 1+F_{2}
\end{bmatrix}
\cdot\boldsymbol{x}
=
\begin{bmatrix}
x+(x+y)\cdot F_{2}\\
y+(x+y)\cdot F_{2}
\end{bmatrix}
$$

となる．ここで $F_{2}=\frac{\sqrt{3}-1}{2}$ であり，導出は Appendix A で扱う．

Skew 変換後の座標から整数格子座標を

$$
i=\lfloor x^{'}\rfloor
,\quad
j=\lfloor y^{'}\rfloor
$$

として求める．

これにより入力点が属する格子領域が定まる．

## Unskew と simplex の決定

求めた格子座標 $(i, j)$ を Unskew し，元の座標系における格子領域の基準位置を求める．

$$
\boldsymbol{X}_{0}
=
\begin{bmatrix}
X_{0}\\
Y_{0}
\end{bmatrix}
=M^{-1}\cdot
\begin{bmatrix}
i\\
j
\end{bmatrix}
=
\begin{bmatrix}
i-(i+j)\cdot G_{2}\\
j-(i+j)\cdot G_{2}
\end{bmatrix}
$$

ここで $G_{2}=\frac{3-\sqrt{3}}{6}$ であり，導出は Appendix B で扱う．

入力点への相対位置を

$$
\boldsymbol{d}_{0}
=
\begin{bmatrix}
x_{0}\\
y_{0}
\end{bmatrix}
=
\boldsymbol{x}-\boldsymbol{X}_{0}
$$

とする．

格子領域内の位置は次のように分類される．

$$
\begin{array}{c|c}
\hline
x_{0}\gt y_{0} & \text{lower}\\
x_{0}\lt y_{0} & \text{upper}\\
x_{0}=y_{0} & \text{diagonal}\\
\hline
\end{array}
$$

ここでは diagonal 上の点は upper として扱う．

## 格子点からの相対位置の算出

格子座標 $(0, 0)$ に対応する格子点を基準とし，残りの格子点は次のようになる．

$$
\begin{array}{c|c|c}
& \text{Grid coordinate} & \text{Unskewed displacement}\\
\hline
\text{lower} & (1, 0) &
\begin{bmatrix}
1-G_{2}\\
-G_{2}
\end{bmatrix}\\
\text{upper} & (0, 1) &
\begin{bmatrix}
-G_{2}\\
1-G_{2}
\end{bmatrix}\\
\text{shared} & (1, 1) &
\begin{bmatrix}
1-2G_{2}\\
1-2G_{2}
\end{bmatrix}
\end{array}
$$

各格子点に対する入力点への相対位置は，

$$
\boldsymbol{d}_{1}
=
\begin{bmatrix}
x_{1}\\
y_{1}
\end{bmatrix}
=
\boldsymbol{d}_{0}
-
\boldsymbol{\Delta}_{1}
,\quad
\boldsymbol{d}_{2}
=
\begin{bmatrix}
x_{2}\\
y_{2}
\end{bmatrix}
=
\boldsymbol{d}_{0}
-
\boldsymbol{\Delta}_{2}
$$

となる．ここで，$\boldsymbol{d}_{1}$ は lower または upper ，$\boldsymbol{d}_{2}$ は shared の格子点に対する相対位置である．

## 各格子点の寄与

各格子点の寄与は，格子点からの距離による減衰と，勾配ベクトルと変位ベクトルの内積によって決まる．

### 距離による減衰

格子点 $i$ から入力点までの距離に応じた量を

$$
t_{i}
=r^{2}-\|\boldsymbol{d}_{i}\|^{2}
$$

と定義する．$t_{i}$ は格子点から離れるにつれて小さくなり，影響範囲の境界に向かうと $0$ に近づく．

ここで

$$
r^{2}=0.5
$$

である．導出は Appendix C で扱う．

### 寄与の算出

格子点 $i$ に割り当てられた勾配ベクトルを $\boldsymbol{g}_{i}$ とすると，その寄与は

$$
n_{i}
=
\begin{cases}
t_{i}^{4}\cdot\langle\boldsymbol{g}_{i},\boldsymbol{d}_{i}\rangle & t_{i}\ge 0\\
0 & \text{otherwise}
\end{cases}
$$

となる．

最終的なノイズ値は，3 つの格子点からの寄与を加算し，正規化することで得られる．

$$
n
=C\cdot\sum_{i=0}^{2}n_{i}
$$

ここで $C$ は正規化定数であり，導出は Appendix E で扱う．

## Appendix A: $F_{2}$ の導出

Skew 変換を表す行列を

$$
M(k)
=
\begin{bmatrix}
1+k & k\\
k & 1+k
\end{bmatrix}
$$

と定義する．

このとき，座標 $(x, y)$ を Skew すると，

$$
\boldsymbol{x}^{'}
=M\cdot\boldsymbol{x}
=
\begin{bmatrix}
x+(x+y)\cdot k\\
y+(x+y)\cdot k
\end{bmatrix}
$$

となる．

行列の各列を格子座標の基底とすると，

$$
\boldsymbol{a}
=
\begin{bmatrix}
1+k\\
k
\end{bmatrix}
,\quad
\boldsymbol{b}
=
\begin{bmatrix}
k\\
1+k
\end{bmatrix}
$$

となる．

この 2 つの基底のなす角を $60^{\circ}$ にすることで正三角形を構成する格子配置になる．したがって，

$$
\left\langle\frac{\boldsymbol{a}}{\|\boldsymbol{a}\|},\frac{\boldsymbol{b}}{\|\boldsymbol{b}\|}\right\rangle
=\cos{\frac{\pi}{3}}
=\frac{1}{2}
$$

これを解くと，

$$
k
=\frac{-1\pm\sqrt{3}}{2}
$$

となる．

$k\gt 0$ のとき，2 つのベクトルはともに第一象限を向くので，

$$
F_{2}
=k
=\frac{\sqrt{3}-1}{2}
$$

## Appendix B: $G_{2}$ の導出

Skew 変換を表す行列 $M(k)$ の逆行列は，

$$
M^{-1}(k)
=
\frac{1}{1+2k}
\begin{bmatrix}
1+k & -k\\
-k & 1+k
\end{bmatrix}
$$

である．

Skew 変換後の座標 $(x^{'}, y^{'})$ を元の座標に戻すと，

$$
\boldsymbol{x}
=M^{-1}\cdot\boldsymbol{x}^{'}
=\frac{1}{1+2k}
\begin{bmatrix}
(1+k)x^{'}-ky^{'}\\
-kx^{'}+(1+k)y^{'}
\end{bmatrix}
$$

ここで，

$$
\frac{1+k}{1+2k}
=1-\frac{k}{1+2k}
$$

なので，

$$
\boldsymbol{x}
=
\begin{bmatrix}
\left(1-\frac{k}{1+2k}\right)x^{'}-\frac{k}{1+2k}y^{'}\\
-\frac{k}{1+2k}x^{'}+\left(1-\frac{k}{1+2k}\right)y^{'}
\end{bmatrix}
=
\begin{bmatrix}
x^{'}-\frac{k}{1+2k}(x^{'}+y^{'})\\
y^{'}-\frac{k}{1+2k}(x^{'}+y^{'})
\end{bmatrix}
$$

共通して現れる係数を

$$
G_{2}
=\frac{k}{1+2k}
$$

とおくと，

$$
\boldsymbol{x}
=
\begin{bmatrix}
x^{'}-G_{2}(x^{'}+y^{'})\\
y^{'}-G_{2}(x^{'}+y^{'})
\end{bmatrix}
$$

となる．

ここで $k$ を代入すると，

$$
G_{2}
=\frac{\frac{\sqrt{3}-1}{2}}{1+2\frac{\sqrt{3}-1}{2}}
=\frac{\sqrt{3}-1}{2\sqrt{3}}
=\frac{3-\sqrt{3}}{6}
$$

## Appendix C: 影響範囲の導出

入力点が属する格子領域を求め，その領域の simplex を構成する 3 つの格子点のみを影響対象とする．したがって，各影響範囲を，格子点から対辺までと定める．

2D simplex は正三角形を構成するため，その範囲は三角形の高さとなる．

Appendix B で求めた

$$
G_{2}
=\frac{3-\sqrt{3}}{6}
$$

を用いると，基準となる格子点 $(0, 0)$ から lower の第 2 格子点 $(1, 0)$ までの変位は，

$$
\boldsymbol{\Delta}_{1}
=
\begin{bmatrix}
1-G_{2}\\
-G_{2}
\end{bmatrix}
$$

である．したがって，辺長の 2 乗は，

$$
\|\boldsymbol{\Delta}_{1}\|^{2}
=(1-G_{2})^{2}+G_{2}^{2}
=\frac{2}{3}
$$

ここで，正三角形の頂点から対辺に垂線を下ろすと，辺長の半分を底辺とする直角三角形が得られる．その高さは，

$$
\begin{align*}
h^{2}+\left(\frac{\|\boldsymbol{\Delta}_{1}\|}{2}\right)^{2}
&=\|\boldsymbol{\Delta}_{1}\|^{2}
,\\
h
&=\frac{1}{\sqrt{2}}
\end{align*}
$$

となる．

## Appendix D: $t_i^4$ の妥当性

寄与の算出では，距離による減衰に $t_i^4$ を用いている．ここでは，このべき指数が影響範囲の境界における滑らかさにどのように関係するかを示す．

### 微分可能性の要求

シェーディング等において法線ベクトルを求める際には，ノイズ関数の 1 次導関数が利用される．そのため，格子点の影響範囲の境界において，ノイズ値だけでなくその 1 次導関数も連続であることが望ましい．

寄与を次のように一般化する．

$$
n_i
=
\begin{cases}
t_i^p\cdot\langle\boldsymbol{g}_i,\boldsymbol{d}_i\rangle & t_i\ge0\\
0 & \text{otherwise}
\end{cases}
$$

ここで，

$$
t_i=r^2-\|\boldsymbol{d}_i\|^2
$$

であり，$t_i$ は入力点の座標 $x,y$ に関する 2 次関数である．また，

$$
\langle\boldsymbol{g}_i,\boldsymbol{d}_i\rangle
$$

は $x, y$ に関する 1 次関数である．

影響範囲の境界では $t_i=0$ となるため，まず

$$
F(u)
=
\begin{cases} u^p & u\ge0\\
0 & \text{otherwise}
\end{cases}
$$

という関数の $u=0$ における滑らかさを考える．ここで $p\in\mathbb{Z}_{>0}$ とする．$t_i(x, y)$ は滑らかな関数であり，影響範囲の境界 $t_i=0$ では

$$
\nabla t_i=-2\boldsymbol{d}_i\neq\boldsymbol{0}
$$

である．したがって，$F(t_i(x, y))$ の境界 $t_i=0$ における滑らかさは，$F(u)$ の $u=0$ における滑らかさから評価できる．

### 境界 $u=0$ における滑らかさ

$u>0$ において $F(u)=u^p$ を $k$ 回微分すると，

$$
F^{(k)}(u)
=\frac{p!}{(p-k)!}u^{p-k} \qquad (1\le k\le p)
$$

となる．$u\to0^+$ の極限を考えると，$k\lt p$ では $p-k>0$ であるから，

$$
F^{(k)}(u)\longrightarrow0
$$

となり，これは $u<0$ 側の恒等的に 0 である部分と一致する．

一方，$k=p$ では $F^{(k)}(u)$ は $u$ によらない定数 $p!$ であるから，

$$
F^{(p)}(u)\longrightarrow p!
$$

となり，$u<0$ 側の 0 とは一致しない．なお，さらに微分すると $F^{(p+1)}(u)=0$ である．

また，$F(0)=0$ であり，これは $u<0$ 側の値と一致するため，$F(u)$ は $u=0$ で連続である．

したがって，$F(u)$ は境界 $u=0$ において

$$
C^{p-1}
$$

級となる．

### $p=4$ の場合

今回用いる $p=4$ では，

$$
F(u)
=
\begin{cases} u^4 & u\ge0\\
0 & \text{otherwise}
\end{cases}
$$

である．これを順に微分すると，

$$
F'(u)=4u^3
,\quad
F''(u)=12u^2
,\quad F'''(u)=24u
,\quad F^{(4)}(u)=24
$$

となる．境界 $u=0$ において

$$
F(0)=F'(0)=F''(0)=F'''(0)=0
$$

であり，これらは $u<0$ 側の値および 1 次・ 2 次・ 3 次導関数とそれぞれ一致する．一方，

$$
F^{(4)}(0)=24
$$

となるため，4 次導関数では不連続となる．

以上より，$t_i^4$ を用いることで，影響範囲の境界において寄与は $C^3$ 級となり，少なくともノイズ値とその 1 次導関数が連続に接続される．

## Appendix E: 正規化定数

ノイズ値は，3 つの格子点からの寄与の総和

$$
f(\boldsymbol{x};\boldsymbol{g}_{0},\boldsymbol{g}_{1},\boldsymbol{g}_{2})
=\sum_{i=0}^{2}n_{i}
$$

によって得られる．ここで $\boldsymbol{g}_{i}$ は各格子点に割り当てられた勾配ベクトルである．

出力値を $[-1, 1]$ に正規化するため，この総和の絶対値の最大値を

$$
f_{\max}
=\max_{\boldsymbol{x},\boldsymbol{g}_{0},\boldsymbol{g}_{1},\boldsymbol{g}_{2}}{|f(\boldsymbol{x};\boldsymbol{g}_{0},\boldsymbol{g}_{1},\boldsymbol{g}_{2})|}
$$

とする．ここで $\boldsymbol{x}$ は 1 つの canonical simplex 内を動き，$\boldsymbol{g}_{i}$ は使用する勾配ベクトルの組み合わせについて評価する．

正規化定数は

$$
C
=\frac{1}{f_{\max}}
$$

と定める．したがって，正規化後のノイズ値は

$$
n
=C\cdot f(\boldsymbol{x};\boldsymbol{g}_{0},\boldsymbol{g}_{1},\boldsymbol{g}_{2})
$$

となり，

$$
|n|\le 1
$$

を満たす．

最大値の推定値は，1 つの canonical simplex 内を十分細かくサンプリングして最大値の候補を求め，その候補を初期値として数値最適化を行うことで求めた．数値計算には[このスクリプト](https://colab.research.google.com/drive/1J-RB4R2paFomD1dc9J2UVV9qPaQr2tUA?usp=sharing)を使用した．

今回使用する 2D simplex noise では，8 種類の勾配ベクトル

$$
\mathcal{G}
=
\left\{
\begin{bmatrix}
0\\
\pm 1
\end{bmatrix}
,
\begin{bmatrix}
\pm 1\\
0
\end{bmatrix}
,
\begin{bmatrix}
\pm 1\\
\pm 1
\end{bmatrix}
\right\}
$$

を用いるため， 3 つの格子点に対する勾配の組み合わせは $8^{3}=512$ 通りとなる．これらすべての組み合わせについて simplex 内の $|f|$ の最大値を探索する．

その結果，

$$
f_{\max}
\approx 0.014255562201874
$$

となり，正規化定数は

$$
C
\approx 70.14805770820965
$$

となる．

なお，最大値は simplex のある辺上に現れ，このときの一例は

$$
\boldsymbol{g}_{0}
=\boldsymbol{g}_{1}
=
\begin{bmatrix}
-1\\
-1
\end{bmatrix}
,\quad
\boldsymbol{g}_{2}
=
\begin{bmatrix}
1\\
1
\end{bmatrix}
$$

であり，重心座標は

$$
(u, v, w)
\approx (0, 0.499002, 0.500998)
$$

となる．

## Appendix F: 正規化定数 2

Appendix E では，勾配ベクトルの組み合わせを変えながら，目的関数の最大値を探索していた．ここでは，勾配ベクトルの大きさを

$$
\|\boldsymbol{g}_{i}\|=1
$$

に制限することで，勾配ベクトルについての探索を取り除くことを考える．

$\boldsymbol{x}$ を固定すると，各項に含まれる内積について Cauchy–Schwarz の不等式より

$$
|\langle\boldsymbol{g}_{i},\boldsymbol{d}_{i}\rangle|
\le\|\boldsymbol{g}_{i}\|\|\boldsymbol{d}_{i}\|
=\|\boldsymbol{d}_{i}\|
$$

が成り立つ．

等号は $\boldsymbol{g}_{i}\parallel\boldsymbol{d}_{i}$ のときに成立する．ここでは $\boldsymbol{g}_{i}$ を $\boldsymbol{d}_{i}$ と同じ向きに取ると，

$$
\boldsymbol{g}_{i}
=\frac{\boldsymbol{d}_{i}}{\|\boldsymbol{d}_{i}\|}
$$

となり，

$$
\langle\boldsymbol{g}_{i},\boldsymbol{d}_{i}\rangle
=\|\boldsymbol{d}_{i}\|
$$

を得る．

これを Appendix E の目的関数

$$
f(\boldsymbol{x};\boldsymbol{g}_{0},\boldsymbol{g}_{1},\boldsymbol{g}_{2})
=\sum_{i=0}^{2}t_{i}^{4}\cdot\langle\boldsymbol{g}_{i},\boldsymbol{d}_{i}\rangle
$$

に代入すると，

$$
f(\boldsymbol{x})
=\sum_{i=0}^{2}t_{i}^{4}\cdot\|\boldsymbol{d}_{i}\|
$$

となる．

これにより，勾配ベクトルについての探索は不要となり，$\boldsymbol{x}$ のみについて最大値を探索すればよい．したがって，

$$
f_{\max}
=\max_{\boldsymbol{x}}{f(\boldsymbol{x})}
=\max_{\boldsymbol{x}}{\sum_{i=0}^{2}t_{i}^{4}\cdot\|\boldsymbol{d}_{i}\|}
$$

となる．

数値計算には[このスクリプト](https://colab.research.google.com/drive/1922113Ktui3HbcmgnHRHogMT_u7BCKUO?usp=sharing)を使用した．

その結果，

$$
f_{\max}
\approx 0.010080204702571
$$

となり，正規化定数は

$$
C
\approx 99.20433458508063
$$

となる．

なお，最大値は simplex のある辺上に現れ，重心座標は

$$
(u, v, w)
\approx (0, 0.499002, 0.500998)
$$

となる．

## References

- Perlin, K. (2001). Noise hardware. In M. Olano (Ed.), Real-time shading SIGGRAPH course notes. ACM SIGGRAPH. <https://userpages.cs.umbc.edu/olano/s2002c36/ch02.pdf>
- Gustavson, S. (2005). Simplex noise demystified [Technical report]. Linköping University. <https://github.com/stegu/perlin-noise/blob/master/simplexnoise.pdf>
- briansharpe. (2012, January 13). Simplex noise. <https://briansharpe.wordpress.com/2012/01/13/simplex-noise/>
- *Simplex*. (2026, August 24). In *Wikipedia*. <https://en.wikipedia.org/w/index.php?title=Simplex&oldid=1371056590>
