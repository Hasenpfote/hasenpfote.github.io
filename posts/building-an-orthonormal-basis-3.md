---
title: "正規直交基底の構築 (3)"
description: "鏡映を利用する方法について考察をする"
date: 2025-01-15T09:00:00+09:00
categories:
  - computerscience
  - onb
  - orthonormal
  - basis
draft: false
---

::: {.callout-warning}
この方法は基底を構築しない．
:::

## 鏡映を利用する方法

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/3d/8bmpqx0ubv"></iframe>
</figure>
</div>

[Comparing ONB Accuracy](https://www.shadertoy.com/view/M3KcR3) で助言を頂いたので整理をする．

> [mla](https://www.shadertoy.com/user/mla), 2025-01-12
> Interesting. Simpler to derive Frisvad's equation with a 3D reflection rather than using
> quaternions (given unit vectors p,q, reflection in plane with normal p-q exchanges p and q).

ベクトルの鏡映は次のように定義される．

$$
\boldsymbol{r}
=\boldsymbol{v} - 2 (\boldsymbol{v}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{n}}
$$

一般的な場合から考える．

$$
\begin{align*}
\boldsymbol{q}
&=\boldsymbol{p} - 2 (\boldsymbol{p}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{n}}\\
&=\boldsymbol{p} - 2 \left(\boldsymbol{p}\cdot\frac{\boldsymbol{p} - \boldsymbol{q}}{\|\boldsymbol{p} - \boldsymbol{q}\|}\right) \frac{\boldsymbol{p} - \boldsymbol{q}}{\|\boldsymbol{p} - \boldsymbol{q}\|}\\
&=\boldsymbol{p} - 2 \left(\frac{\|\boldsymbol{p}\|^{2} - \boldsymbol{p}\cdot\boldsymbol{q}}{\|\boldsymbol{p}\|^{2} + \|\boldsymbol{q}\|^{2} - 2 \boldsymbol{p}\cdot\boldsymbol{q}}\right) (\boldsymbol{p} - \boldsymbol{q})
\end{align*}
$$

ここで $\boldsymbol{p}, \boldsymbol{q}$ を単位ベクトルとすると，次のように $\boldsymbol{p}$ と $\boldsymbol{q}$ を交換する式になっていることがわかる．

$$
\boldsymbol{q}
=\boldsymbol{p} - \left(\frac{1 - \boldsymbol{p}\cdot\boldsymbol{q}}{1 - \boldsymbol{p}\cdot\boldsymbol{q}}\right) (\boldsymbol{p} - \boldsymbol{q})
=\boldsymbol{q}
$$

特異点は $\boldsymbol{p}=\boldsymbol{q}$ となるが，交換は不要ということに他ならない．単位ベクトルであることを強調すると次のようになる．

$$
\hat{\boldsymbol{q}}
=\hat{\boldsymbol{p}} - 2 (\hat{\boldsymbol{p}}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{n}}
=\hat{\boldsymbol{p}} - \left(\hat{\boldsymbol{p}}\cdot\frac{\hat{\boldsymbol{p}} - \hat{\boldsymbol{q}}}{1 - \hat{\boldsymbol{p}}\cdot\hat{\boldsymbol{q}}}\right) (\hat{\boldsymbol{p}} - \hat{\boldsymbol{q}})
=\hat{\boldsymbol{q}}
$$

この交換式を使って $\hat{\boldsymbol{s}}=(0,0,1)^{\mathsf{T}}$ から $\hat{\boldsymbol{t}}$ への回転とするならば，

$$
\begin{align*}
\boldsymbol{\omega}
&={}_{\perp}\boldsymbol{\omega} - 2 ({}_{\perp}\boldsymbol{\omega}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{n}}\\
&={}_{\perp}\boldsymbol{\omega} - 2 \left({}_{\perp}\boldsymbol{\omega}\cdot\frac{\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}}}{\|\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}}\|}\right) \frac{\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}}}{\|\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}}\|}\\
&={}_{\perp}\boldsymbol{\omega} - \left(\frac{{}_{\perp}\boldsymbol{\omega}\cdot(\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}})}{1 -  \hat{t}_{z}}\right) (\hat{\boldsymbol{s}} - \hat{\boldsymbol{t}})\\
\end{align*}
$$

$\hat{t}_{z}=1$ のときに特異点となるが，これは $\hat{\boldsymbol{s}}=\hat{\boldsymbol{t}}$ なので回転は不要ということに他ならない．

### Issues

接空間の $z$ 軸に対称な分布に適用するような場合は問題なさそうに見えるが，Frisvad の方法に合わせるなら対応が必要になるのではないだろうか？  

見通しをよくするため 2 次元で考える．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/3vxfwl42st?embed="></iframe>
<figcaption>2次元で動作を確認する</figcaption>
</figure>
</div>

$\hat{\boldsymbol{s}}$ に対する ${}_{\perp}\boldsymbol{\omega}$ と $\hat{\boldsymbol{t}}$ に対する $\boldsymbol{\omega}$ は直感に反している．図とベクトルの鏡映式を眺めると，${}_{\perp}\boldsymbol{\omega}$ をある平面に対して反転させる必要があるようだ．  

ある平面の法線ベクトル $\hat{\boldsymbol{m}}$ は次のようになる．

$$
\begin{align*}
\boldsymbol{m}
&=\hat{\boldsymbol{n}} - (\hat{\boldsymbol{s}}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{s}}\\
\hat{\boldsymbol{m}}
&=
\begin{cases}
\frac{\boldsymbol{m}}{\|\boldsymbol{m}\|} & |\hat{\boldsymbol{s}}\cdot\hat{\boldsymbol{n}}|\lt 1\\
\boldsymbol{0} & \text{otherwise}
\end{cases}
\end{align*}
$$

その平面に対して反転すると次のようになる．

$$
{}_{\perp}\boldsymbol{\omega}^{'}
=
\begin{cases}
{}_{\perp}\boldsymbol{\omega} - 2 (\hat{\boldsymbol{m}}\cdot{}_{\perp}\boldsymbol{\omega}) \hat{\boldsymbol{m}} & |\hat{\boldsymbol{s}}\cdot\hat{\boldsymbol{t}}|\lt 1\\
{}_{\perp}\boldsymbol{\omega} & \text{otherwise}
\end{cases}
$$

ベクトルの鏡映式に代入すると，直感に従った結果を得る．

$$
\boldsymbol{\omega}^{'}
={}_{\perp}\boldsymbol{\omega}^{'} - 2 ({}_{\perp}\boldsymbol{\omega}^{'}\cdot\hat{\boldsymbol{n}}) \hat{\boldsymbol{n}}
$$

## Acknowledgements

鏡映を利用する方法を共有して頂いた mla 氏に感謝いたします．

## References

- Wikipedia contributors. (2024, November 5). _Reflection (mathematics)_. Wikipedia. <https://en.wikipedia.org/w/index.php?title=Reflection_(mathematics)&oldid=1255601321>
