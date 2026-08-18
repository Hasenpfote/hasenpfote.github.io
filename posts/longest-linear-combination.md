---
title: 2本のベクトルの最も長くなる方向
description: 2本のベクトルを混ぜたときに最も長くなる方向を、ノルム最大化と固有値問題から導出する
date: 2026-08-18T09:00:00+09:00
categories:
  - math
  - computergraphics
draft: false
---

エッジ検出などで勾配ベクトル $\boldsymbol{g}_{x}, \boldsymbol{g}_{y}$ を扱っていると，「この 2 本を混ぜたとき，最も長くなる方向はどこか」という問いに自然に行き着く．ここでは，この問いを線形写像とその固有値問題として定式化し，最終的に $\operatorname{atan2}$ と根号で表される閉じた式まで導く．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/gwlpuga9m5"></iframe>
<figcaption>N=2</figcaption>
</figure>
</div>

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/3d/aklvwyvgh0"></iframe>
<figcaption>N=3</figcaption>
</figure>
</div>

## 問題設定 - 2 本のベクトルを混ぜる線形写像

$N$ 次元の任意の 2 本のベクトル

$$
\boldsymbol{p}, \boldsymbol{q}\in\mathbb{R}^{N}
$$

を考える．この 2 本を係数 $v_{1}, v_{2}\in\mathbb{R}$ で混ぜて

$$
\boldsymbol{w}
=v_{1}\,\boldsymbol{p}+v_{2}\,\boldsymbol{q}
$$

というベクトルをつくる．係数をまとめて

$$
\boldsymbol{v}
=
\begin{bmatrix}
v_{1}\\
v_{2}
\end{bmatrix}
,\quad
L
=
\begin{bmatrix}
\boldsymbol{p} & \boldsymbol{q}
\end{bmatrix}
\in\mathbb{R}^{N\times 2}
$$

とすると，

$$
\boldsymbol{w}
=L\boldsymbol{v}
$$

と書ける．  

ここで一歩引いて見ると，$L$ は「2 次元の係数空間 $\mathbb{R}^{2}$ から，$N$ 次元空間 $\mathbb{R}^{N}$ への線形写像」である．$\boldsymbol{v}$ という 2 次元の入力（混ぜ方）を渡すと，$L\boldsymbol{v}$ という $N$ 次元の出力（実際のベクトル）が返ってくる．以降の議論は，突き詰めれば「この線形写像はどの入力方向を最も引き伸ばすか」という問いに帰着する．  

係数 $\boldsymbol{v}$ を自由に大きくすれば $\boldsymbol{w}$ をいくらでも長くできてしまうので，比較のために

$$
\boldsymbol{v}^{\top}\boldsymbol{v}=1
\quad(v_{1}^{2}+v_{2}^{2}=1)
$$

という制約を置く．つまり係数の「大きさ」は固定し，「混ぜる比率」だけを変えたときの最大となる方向を探す．目的は

$$
\max_{\boldsymbol{v}^{\top}\boldsymbol{v}=1}{\|L\boldsymbol{v}\|}
$$

である．

## ノルム最大化を対称行列の問題に変換する

平方根は最大値の位置を変えないので，$\|L\boldsymbol{v}\|^{2}$ を最大化すればよい．

$$
\|L\boldsymbol{v}\|^{2}
=(L\boldsymbol{v})^{\top}(L\boldsymbol{v})
=\boldsymbol{v}^{\top}L^{\top}L\boldsymbol{v}
$$

ここで

$$
A
=L^{\top}L
$$

と置くと，

$$
\|L\boldsymbol{v}\|^{2}
=\boldsymbol{v}^{\top}A\boldsymbol{v}
$$

  **線形写像としての意味づけ**：$L$ が $\mathbb{R}^{2}\to\mathbb{R}^{N}$ の写像だとすると，$L^{\top}$ はその「転置」＝ $\mathbb{R}^{N}\to\mathbb{R}^{2}$ の写像であり，$A=L^{\top}L$ は $\mathbb{R}^{2}\to\mathbb{R}^{2}$ に戻ってくる合成写像 $L^{\top}\circ L$ にあたる．つまり $A$ は，$L$ によって入力 $\boldsymbol{v}$ がどれだけの長さになるかを，もとの 2 次元の中の二次形式 $\boldsymbol{v}^{\top}A\boldsymbol{v}$ として表している．

具体的に成分を書き下すと，

$$
A
=L^{\top}L
=
\begin{bmatrix}
\langle\boldsymbol{p},\boldsymbol{p}\rangle & \langle\boldsymbol{p},\boldsymbol{q}\rangle\\
\langle\boldsymbol{q},\boldsymbol{p}\rangle & \langle\boldsymbol{q},\boldsymbol{q}\rangle
\end{bmatrix}
=
\begin{bmatrix}
I_{xx} & I_{xy}\\
I_{xy} & I_{yy}
\end{bmatrix}
$$

（$I_{xx}=\langle\boldsymbol{p},\boldsymbol{p}\rangle,\,I_{yy}=\langle\boldsymbol{q},\boldsymbol{q}\rangle,\,I_{xy}=\langle\boldsymbol{p},\boldsymbol{q}\rangle$）．内積の対称性 $\langle\boldsymbol{p},\boldsymbol{q}\rangle=\langle\boldsymbol{q},\boldsymbol{p}\rangle$ から自動的に

$$
A=A^{\top}
$$

となる．対角化のために対称行列を選んだのではなく，$\|L\boldsymbol{v}\|^{2}$ を展開すると必然的に対称行列 $L^{\top}L$ が現れる，という順序であることに注意したい．

## 単位円上を動かして極値を探す

制約 $\boldsymbol{v}^{\top}\boldsymbol{v}=1$ は単位円なので，

$$
\boldsymbol{v}(\theta)
=
\begin{bmatrix}
\cos{\theta}\\
\sin{\theta}
\end{bmatrix}
$$

とパラメータ化できる．目的関数を

$$
f(\theta)
=\boldsymbol{v}(\theta)^{\top}A\boldsymbol{v}(\theta)
=\|L\boldsymbol{v}(\theta)\|^{2}
$$

とする．$\boldsymbol{v}$ と直交する

$$
\boldsymbol{v}_{\perp}
=\frac{d\boldsymbol{v}}{d\theta}
=
\begin{bmatrix}
-\sin{\theta}\\
\cos{\theta}
\end{bmatrix}
,\quad
\boldsymbol{v}^{\top}\boldsymbol{v}_{\perp}=0
,\,
\boldsymbol{v}_{\perp}^{\top}\boldsymbol{v}_{\perp}=1
$$

を使うと，$(\boldsymbol{v},\,\boldsymbol{v}_{\perp})$ は正規直交基底になる．$f$ を微分すると

$$
\frac{df}{d\theta}
=\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}+\boldsymbol{v}^{\top}A\boldsymbol{v}_{\perp}
$$

$A=A^{\top}$ より $\boldsymbol{v}^{\top}A\boldsymbol{v}_{\perp}=\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}$ なので，

$$
\frac{df}{d\theta}
=2\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}
$$

極値条件 $\frac{df}{d\theta}=0$ から

$$
\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}
=0
$$

が得られる．

## 極値条件から固有値問題が現れる

$(\boldsymbol{v},\,\boldsymbol{v}_{\perp})$ は $\mathbb{R}^{2}$ の基底なので，$A\boldsymbol{v}$ は必ず

$$
A\boldsymbol{v}
=\alpha\,\boldsymbol{v}+\beta\,\boldsymbol{v}_{\perp}
$$

と分解できる．左から $\boldsymbol{v}_{\perp}^{\top}$ を掛けて正規直交性を使うと $\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}=\beta$ ．一方，極値条件から $\boldsymbol{v}_{\perp}^{\top}A\boldsymbol{v}=0$ なので $\beta=0$ ．したがって

$$
A\boldsymbol{v}
=\alpha\boldsymbol{v}
$$

これは固有値・固有ベクトルの定義そのものなので，$\alpha$ が固有値である．以降，固有値を $\lambda$ と書く．したがって，「ノルムが極値になる混ぜ方 $\boldsymbol{v}$ 」は $A$ の固有ベクトルである．微分で探していた極値方向が，固有値問題として姿を現した瞬間である．

さらに $A\boldsymbol{v}=\lambda\boldsymbol{v}$ の両辺に左から $\boldsymbol{v}^{\top}$ を掛け，$\boldsymbol{v}^{\top}\boldsymbol{v}=1$ を使うと

$$
\boldsymbol{v}^{\top}A\boldsymbol{v}
=\lambda
$$

一方 $\boldsymbol{v}^{\top}A\boldsymbol{v}=\|L\boldsymbol{v}\|^{2}$ だったので，

$$
\lambda
=\|L\boldsymbol{v}\|^{2}
$$

すなわち固有値そのものが「その方向に混ぜたときの長さの 2 乗」を表している．よって

$$
\lambda_{\max}
=\max_{\boldsymbol{v}^{\top}\boldsymbol{v}=1}{\|L\boldsymbol{v}\|^{2}}
,\quad
\|L\boldsymbol{v}_{\max}\|
=\sqrt{\lambda_{\max}}
$$

つまり，$A=L^{\top}L$ の固有値・固有ベクトルを求めるだけで，線形写像 $L$ がどの入力方向を最も強く引き伸ばすか（ $\boldsymbol{v}_{\max}$ ），そしてどれだけ引き伸ばすか（ $\sqrt{\lambda_{\max}}$ ）が，両方とも同時にわかったことになる．

## どちらの固有方向が最大か - 閉じた式を求める

$A$ は $2\times 2$ の対称行列なので，異なる固有値に対応する 2 つの固有ベクトルは直交する．そこで，まず最大・最小を区別せず，互いに直交する 2 つの固有方向を

$$
\boldsymbol{e}_{1}
=
\begin{bmatrix}
\cos{\theta}\\
\sin{\theta}
\end{bmatrix}
,\quad
\boldsymbol{e}_{2}
=
\begin{bmatrix}
-\sin{\theta}\\
\cos{\theta}
\end{bmatrix}
$$

と表す．$\boldsymbol{e}_{2}$ は $\boldsymbol{e}_{1}$ を $90^{\circ}$ 回転した方向なので，角度で言えば $\theta + \frac{\pi}{2}$ の関係にある．

この時点では，$\boldsymbol{e}_{1}$ と $\boldsymbol{e}_{2}$ のどちらが最大方向なのかはまだ決まっていない．ここから $f(\theta)$ の最大値を求めることで，どちらが最大方向なのかを決める．

$f(\theta)=\boldsymbol{v}^{\top}A\boldsymbol{v}$ を展開し，倍角公式

$$
\cos^{2}{\theta}
=\frac{1+\cos{2\theta}}{2}
,\quad
\sin^{2}{\theta}
=\frac{1-\cos{2\theta}}{2}
,\quad
\sin{\theta}\cos{\theta}
=\frac{\sin{2\theta}}{2}
$$

を使うと，

$$
f(\theta)
=\frac{I_{xx}+I_{yy}}{2}+\frac{I_{xx}-I_{yy}}{2}\cos{2\theta}+I_{xy}\sin{2\theta}
$$

となる．

::: {.callout-note}

$$
\begin{align*}
f(\theta)
&=I_{xx}\cos^{2}{\theta}+I_{yy}\sin^{2}{\theta}+2I_{xy}\sin{\theta}\cos{\theta}\\
&=I_{xx}\frac{1+\cos{2\theta}}{2}+I_{yy}\frac{1-\cos{2\theta}}{2}+2I_{xy}\frac{\sin{2\theta}}{2}
\end{align*}
$$

:::

$X=I_{xx}-I_{yy},\,Y=2I_{xy}$ と置くと，後半の

$$
\frac{1}{2}(X\cos{2\theta}+Y\sin{2\theta})
$$

は「単位ベクトル $[\cos{2\theta},\,\sin{2\theta}]^{\top}$ と $[X,\,Y]^{\top}$ の内積」なので，両者が同じ方向を向くときに最大値 $\frac{1}{2}\sqrt{X^{2}+Y^{2}}$ を取る．これより最大となる角度

$$
\theta
=\frac{1}{2}\operatorname{atan2}{(2I_{xy},\,I_{xx}-I_{yy})}
$$

が得られる．

したがって，この $\theta$ に対応する $\boldsymbol{e}_{1}$ が最大方向であり，

$$
\boldsymbol{v}_{\max}
=\boldsymbol{e}_{1}
=
\begin{bmatrix}
\cos{\theta}\\
\sin{\theta}
\end{bmatrix}
$$

となる．一方，これと直交する $\boldsymbol{e}_{2}$ は最小方向なので

$$
\boldsymbol{v}_{\min}
=\boldsymbol{e}_{2}
=
\begin{bmatrix}
-\sin{\theta}\\
\cos{\theta}
\end{bmatrix}
$$

となる．また，最大値は

$$
\lambda_{\max}
=\frac{I_{xx}+I_{yy}+\sqrt{(I_{xx}-I_{yy})^{2}+4I_{xy}^{2}}}{2}
$$

である．直交する方向 $\theta+\frac{\pi}{2}$ では $2\theta$ が $\pi$ だけずれるため，$\cos{2\theta}$ と $\sin{2\theta}$ の符号が反転し，先ほど最大だった項が最小になる．したがって，2 つの固有方向と固有値の対応は

$$
\boldsymbol{e}_{1}
=\boldsymbol{v}_{\max}
\leftrightarrow \lambda_{\max}
,\quad
\boldsymbol{e}_{2}
=\boldsymbol{v}_{\min}
\leftrightarrow \lambda_{\min}
$$

となる．

ただし，

$$
I_{xx}=I_{yy}
,\quad
I_{xy}=0
$$

の場合は $A$ が単位行列の定数倍となり，すべての方向で $f(\theta)$ が同じ値になるため，最大方向は一意に定まらない．

同じ結果は，特性方程式 $\det{(A-\lambda I)}=0$ からも得られる．

$$
\lambda^{2}-(I_{xx}+I_{yy})\lambda+(I_{xx}I_{yy}-I_{xy}^{2})=0
$$

解の公式を使うと，

$$
\lambda_{\max,\min}
=\frac{I_{xx}+I_{yy}\pm\sqrt{(I_{xx}-I_{yy})^{2}+4I_{xy}^{2}}}{2}
$$

微分で求めた極値と，代数的に解いた固有値がぴったり一致することが確認できる．

## 余談 - 固有ベクトルによる対角化

$\boldsymbol{v}_{\max}$ と $\boldsymbol{v}_{\min}$ はともに単位ベクトルで互いに直交しているので，これらを列に並べた行列

$$
R
=
\begin{bmatrix}
\boldsymbol{v}_{\max} & \boldsymbol{v}_{\min}
\end{bmatrix}
$$

は，$R^{\top}R=I$ を満たす直交行列である．対応する固有値を

$$
D
=\operatorname{diag}{(\lambda_{\max},\,\lambda_{\min})}
$$

とする．$R$ の各列が固有ベクトルなので，

$$
A\boldsymbol{v}_{\max}
=\lambda_{\max}\boldsymbol{v}_{\max}
,\quad
A\boldsymbol{v}_{\min}
=\lambda_{\min}\boldsymbol{v}_{\min}
$$

をまとめると，

$$
AR=RD
$$

となる．左から $R^{\top}$ を掛けると，

$$
R^{\top}AR=D
$$

を得る．

これは，固有ベクトルを新しい座標軸として選ぶと，$A$ は対角行列として表せることを意味する．つまり，この座標系では $A$ の作用が各固有ベクトル方向の独立した拡大・縮小として表される．

今回の場合，その 2 つの座標軸が $\boldsymbol{v}_{\max}$ と $\boldsymbol{v}_{\min}$ であり，$A$ はそれぞれの方向を $\lambda_{\max},\,\lambda_{\min}$ 倍する．一方，もとの写像 $L$ による長さの拡大率は，それぞれ $\sqrt{\lambda_{\max}},\,\sqrt{\lambda_{\min}}$ である．

## もとの空間へ - 最大の線形結合

最大固有ベクトル $\boldsymbol{v}_{\max}=[\cos{\theta},\,\sin{\theta}]^{\top}$ を，もとの写像 $L$ に戻すと

$$
\boldsymbol{w}_{\max}
=L\boldsymbol{v}_{\max}
=\cos{\theta}\,\boldsymbol{p}+\sin{\theta}\,\boldsymbol{q}
$$

これが $\boldsymbol{p},\,\boldsymbol{q}$ の線形結合の中で最も長いベクトルであり，その長さは

$$
\|\boldsymbol{w}_{\max}\|
=\sqrt{\lambda_{\max}}
$$

## 勾配への応用

以上の議論では $\boldsymbol{p},\,\boldsymbol{q}$ は任意の 2 本のベクトルだった．エッジ検出などの用途では，これに勾配ベクトルを代入する：

$$
L
=
\begin{bmatrix}
\boldsymbol{g}_{x} & \boldsymbol{g}_{y}
\end{bmatrix}
$$

このとき

$$
A
=L^{\top}L
=
\begin{bmatrix}
\langle\boldsymbol{g}_{x},\boldsymbol{g}_{x}\rangle & \langle\boldsymbol{g}_{x},\boldsymbol{g}_{y}\rangle\\
\langle\boldsymbol{g}_{x},\boldsymbol{g}_{y}\rangle & \langle\boldsymbol{g}_{y},\boldsymbol{g}_{y}\rangle
\end{bmatrix}
=
\begin{bmatrix}
I_{xx} & I_{xy}\\
I_{xy} & I_{yy}
\end{bmatrix}
$$

となり，最大となる方向とその強さは

$$
\theta
=\frac{1}{2}\operatorname{atan2}{(2I_{xy},\,I_{xx}-I_{yy})}
,\quad
\boldsymbol{w}_{\max}
=\cos{\theta}\,\boldsymbol{g}_{x}+\sin{\theta}\,\boldsymbol{g}_{y}
,\quad
\|\boldsymbol{w}_{\max}\|
=\sqrt{\lambda_{\max}}
$$

で与えられる．

[Edge NMS Along the Max Gradient - Shadertoy](https://www.shadertoy.com/view/ffdSzS)

## まとめ

線形写像 $L:\mathbb{R}^{2}\to\mathbb{R}^{N}$ （2 つの係数を渡すと，混ぜ合わせたベクトルを返す写像）を考える．「単位円上の入力 $\boldsymbol{v}$ をどの方向に取ると出力 $L\boldsymbol{v}$ が最も長くなるか」という最適化問題は，対称行列 $A=L^{\top}L$ の固有値問題に帰着する．

最大固有値 $\lambda_{\max}$ は，その混ぜ方をしたときの長さの 2 乗の最大値を与え，対応する固有ベクトル $\boldsymbol{v}_{\max}$ は，そのときの混ぜる比率を与える．

2 本の勾配ベクトル $\boldsymbol{g}_{x},\,\boldsymbol{g}_{y}$ にこの枠組みを適用することで，勾配が最も長くなる方向と，その強さを求めることができる．

## Appendix - 対称行列の固有ベクトルの直交性

本文では，$A$ は $2\times 2$ の対称行列なので，異なる固有値に対する固有ベクトルは直交すると述べた．ここでは，この性質を証明する．

$A$ の異なる固有値 $\lambda_{1},\,\lambda_{2}$ に対応する固有ベクトルを $\boldsymbol{v}_{1},\,\boldsymbol{v}_{2}$ とする．つまり $A\boldsymbol{v}_{1}=\lambda_{1}\boldsymbol{v}_{1},\,A\boldsymbol{v}_{2}=\lambda_{2}\boldsymbol{v}_{2}$ であり，$\lambda_{1}\neq\lambda_{2}$ とする．示したいのは $\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}=0$ である．

まず，$\boldsymbol{v}_{2}$ の固有値方程式 $A\boldsymbol{v}_{2}=\lambda_{2}\boldsymbol{v}_{2}$ の両辺に，左から $\boldsymbol{v}_{1}^{\top}$ を掛けると，

$$
\boldsymbol{v}_{1}^{\top}A\boldsymbol{v}_{2}
=\lambda_{2}\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
$$

ここで，$A=A^{\top}$ なので，$\boldsymbol{v}_{1}$ の固有値方程式 $A\boldsymbol{v}_{1}=\lambda_{1}\boldsymbol{v}_{1}$ を使えば，

$$
\boldsymbol{v}_{1}^{\top}A\boldsymbol{v}_{2}
=\boldsymbol{v}_{1}^{\top}A^{\top}\boldsymbol{v}_{2}
=(A\boldsymbol{v}_{1})^{\top}\boldsymbol{v}_{2}
=(\lambda_{1}\boldsymbol{v}_{1})^{\top}\boldsymbol{v}_{2}
=\lambda_{1}\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
$$

となる．したがって，

$$
\lambda_{1}\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
=\lambda_{2}\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
$$

より，

$$
(\lambda_{1}-\lambda_{2})\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
=0
$$

である．$\lambda_{1}\neq\lambda_{2}$ なので，

$$
\boldsymbol{v}_{1}^{\top}\boldsymbol{v}_{2}
=0
$$

となり，$\boldsymbol{v}_{1}$ と $\boldsymbol{v}_{2}$ は直交する．

## References

- Di Zenzo, S. (1986). A note on the gradient of a multi-image. *Computer Vision, Graphics, and Image Processing*, *33*(1), 116–125. <https://doi.org/10.1016/0734-189X(86)90223-9>
