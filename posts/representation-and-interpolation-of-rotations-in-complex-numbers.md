---
title: "複素数における回転の表現と補間"
description: "複素数における回転の表現と補間について"
categories:
  - math
  - complexnumbers
  - cpp
date: 2021-01-12T09:00:00+09:00
draft: false
---

## 概要

複素数における回転の表現と補間について簡単に整理をする．

## 回転の表現

定義は

$$
\mathbb{C}_1 := \{p \in \mathbb{C} \mid |p|=1 \}=\{e^{\theta\mathbb{i}} \mid \theta \in \mathbb{R} \}
$$

$e^{\theta\mathbb{i}}$ はオイラーの公式[^1]で

$$
e^{\theta\mathbb{i}}
=\cos{(\theta)} + \sin{(\theta)} \mathbb{i}
$$

[^1]: <https://en.wikipedia.org/wiki/Euler's_formula>

## 補間の方法

$p=a+b\mathbb{i}, \hspace{3pt} q=c+d\mathbb{i} \in \mathbb{C}_1$ 間を $h \in \mathbb{R}$ で補間する方法．

### 線形補間

定義は

$$
\operatorname{LERP}(p,q;h)
=(1-h)p + hq
$$

特徴は

- 大きさを維持しない
- 速度は不定

### 正規化線形補間

定義は

$$
\operatorname{nLERP}(p,q;h)
=\frac{\operatorname{LERP}(p,q;h)}{|\operatorname{LERP}(p,q;h)|}
$$

特徴は

- 大きさを維持
- 速度は不定

### 球面線形補間

定義は

$$
\operatorname{SLERP}(p,q;h)
=p \cdot (p^{-1} \cdot q)^h
=p \cdot \exp{(h \log{(p^{-1} \cdot q)})}
$$

特徴は

- 大きさを維持
- 速度は一定

### スプライン補間

定義は

$$
\operatorname{SQUAD}(p_{i}, p_{i+1}, s_{i}, s{i+1}; h)
=\operatorname{SLERP}(\operatorname{SLERP}(p_{i}, p_{i+1}; h), \operatorname{SLERP}(s_{i}, s_{i+1}; h), 2h(1-h))
$$

ここで $p_{i}, p_{i+1}, s_{i}, s_{i+1} \in \mathbb{C}_1$ で $s_{i}$ は次式で表される．

$$
s_{i}
=p_{i} \exp{\left(- \frac{\log{(p_{i}^{-1} p_{i-1})} + \log{(p_{i}^{-1} p_{i+1})}}{4}\right)}
$$

## $e^{\theta\mathbb{i}}$ の補間

一般的な回転を表現する複素数での補間は，常に最短円弧を描く補間となる．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/vpaaszozce?embed="></iframe>
<figcaption>Fig. 1. 最短円弧を描く補間</figcaption>
</figure>
</div>

なす角が180度を超える場合に最長円弧を描く補間にするには[^2]

[^2]: 後述の $e^{\frac{\theta}{2}\mathbb{i}}$ の補間での Default 動作

1. 複素数の偏角の主値の範囲を $(-\pi, \pi]$ とし $p, q$ の角度を $[0,\pi)$ で求める

  $$
  \begin{aligned}
  \phi=
  \begin{cases}
    Arg{(p)} + 2\pi & \text{if } Arg{(p)} < 0\\
    Arg{(p)} & \text{otherwise}
  \end{cases}\\
  \\
  \psi=
  \begin{cases}
    Arg{(q)} + 2\pi & \text{if } Arg{(q)} < 0\\
    Arg{(q)} & \text{otherwise}
  \end{cases}
  \end{aligned}
  $$

2. 向きを求める

  $$
  \begin{aligned}
  d=
  \begin{vmatrix}
  p.x & q.x\\
  p.y & q.y
  \end{vmatrix}
  =p.x \cdot q.y - p.y \cdot q.x
  \end{aligned}
  $$

3. なす角を決定する

  $$
  \begin{aligned}
  \alpha=
  \begin{cases}
  \begin{cases}
    \cos^{-1}{(\langle p, q \rangle)} & \text{if } d < 0\\
    2 \pi - \cos^{-1}{(\langle p, q \rangle)} & \text{otherwise}
  \end{cases}
  & \text{if } \psi - \phi < 0\\
  \begin{cases}
    2 \pi - \cos^{-1}{(\langle p, q \rangle)} & \text{if } d < 0\\
    \cos^{-1}{(\langle p, q \rangle)} & \text{otherwise}
  \end{cases}
  & \text{otherwise}
  \end{cases}
  \end{aligned}
  $$

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/z9ucokttw8?embed="></iframe>
<figcaption>Fig. 2. 最長円弧を描く補間</figcaption>
</figure>
</div>

## $e^{\frac{\theta}{2}\mathbb{i}}$ の補間

半角形式の補間は，なす角が180度を超えると最長円弧を描く補間となる．半角形式は半円で360度を表現し残りは繰り返しとなるので  $\pm e^{\frac{\theta}{2}\mathbb{i}}$ は同じ回転を表現する．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/usdbcrfr30?embed="></iframe>
<figcaption>Fig. 3. 半角形式の補間</figcaption>
</figure>
</div>

なす角が180度を超える場合に最短円弧を描く補間にするには[^3]

[^3]: 前述の $e^{\theta\mathbb{i}}$ の補間での Default 動作

- なす角が180度を超える場合に片方の符号を反転し補間を行う

  **符号を反転しても同じ回転を表現することを利用**

  $$
  \begin{aligned}
  r=
  \begin{cases}
   -q & \text{if } \langle p, q \rangle \lt 0\\
   +q & \text{otherwise}
  \end{cases}
  \end{aligned}
  $$

## 球面線形補間の別形式

指数・対数関数を用いない形式について．$z_0=a+b\mathbb{i}, \hspace{3pt} z_1=c+d\mathbb{i} \in \mathbb{C}_1$ のなす角を $\alpha$ とし $h \in \mathbb{R}$ で補間した $z_h \in \mathbb{C}_1$ を求める．この時 $z_1$ を元に $z_0$ に垂直な軸を $z^{'}_1$ とすると

$$
z_h
=\cos{(\alpha h)} z_0 + \sin{(\alpha h)} z^{'}_1
$$

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/6mnn9miiyf?embed="></iframe>
<figcaption>Fig. 4. 球面線形補間</figcaption>
</figure>
</div>

$z^{'}_1$ は二次元の場合は90度回転として求められる．

$$
z^{'}_1
=- b + a \mathbb{i}
$$

より一般的には

$$
z^{'}_1
=\frac{z_1 - \langle z_0, z_1 \rangle z_0}{|z_1 - \langle z_0, z_1 \rangle z_0|}
$$

または簡潔に

$$
z^{'}_1
=\frac{z_1 - \cos{(\alpha)} z_0}{\sin{(\alpha)}}
$$

> $s=\langle z_0, z_1 \rangle=\cos{(\alpha)}$ とすると分母は
>
> $$
> \begin{aligned}
> |z_1 - \langle z_0, z_1 \rangle z_0|
> &=|z_1 - s z_0|\\
> &=\sqrt{(c-sa)^2 + (d-sb)^2}\\
> &=\sqrt{s^2 |z_0|^2 - 2s \langle z_0,z_1 \rangle + |z_1|^2}\\
> &=\sqrt{s^2 - 2s^2 + 1}\\
> &=\sqrt{1 - s^2}\\
> &=\sqrt{1 - \cos^{2}{(\alpha)}}\\
> &=\sqrt{\sin^{2}{(\alpha)}}
> =\sin{(\alpha)}
> \end{aligned}
> $$

$z^{'}_1$ を $z_h$ に代入すると

$$
\begin{aligned}
z_{h}
&=\cos(\alpha h) z_0 + \sin(\alpha h) z^{'}_1\\
&=\cos(\alpha h) z_0 + \sin(\alpha h) \frac{z_1 - \cos(\alpha) z_0}{\sin(\alpha)}\\
&=\cos(\alpha h)z_{0} + \frac{\sin(\alpha h)}{\sin(\alpha)}z_{1} - \frac{\sin(\alpha h)\cos(\alpha)}{\sin(\alpha)}z_{0}\\
&=\frac{\sin(\alpha)\cos(\alpha h) - \cos(\alpha)\sin(\alpha h)}{\sin(\alpha)}z_{0} + \frac{\sin(\alpha h)}{\sin(\alpha)}z_{1}\\
&=\frac{\sin(\alpha(1 - h))}{\sin(\alpha)}z_{0} + \frac{\sin(\alpha h)}{\sin(\alpha)}z_{1}
\end{aligned}
$$

**途中，三角関数の加法定理を利用した．**

このままでは分母に $\sin{(\alpha)}$ があるため $\alpha \to 0$ で発散するので，$1 - |\langle z_0, z_1 \rangle| \le \epsilon$ ならば線形補間に切り替え回避をする．

## スプライン補間の補足

未知数 $s_{i}$ の導出方法．

- $\operatorname{SLERP}$ の微分

  $$
  \begin{aligned}
  \frac{d}{dh} \operatorname{SLERP}(p,q;h)
  &=\frac{d}{dh} p \cdot (\overline{p} \cdot q)^{h}\\
  &=p \cdot \frac{d}{dh} \exp{(h \log{(\overline{p} \cdot q)})}\\
  &=p \cdot \exp{(h \log{(\overline{p} \cdot q)})} \cdot \log{(\overline{p} \cdot q)}\\
  &=\operatorname{SLERP}(p,q;h) \cdot \log{(\overline{p} \cdot q)}
  \end{aligned}
  $$

- $\operatorname{Squad}(p_{i-1}, p_{i}, s_{i-1}, s_{i}, 1) = \operatorname{Squad}(p_{i}, p_{i+1}, s_{i}, s_{i+1}, 0)$

  $$
  \begin{aligned}
  \operatorname{Squad}(p_{i-1}, p_{i}, s_{i-1}, s_{i}; 1)
  &=\operatorname{SLERP}(\operatorname{SLERP}(p_{i-1}, p_{i}; 1), \operatorname{SLERP}(s_{i-1}, s_{i}; 1); 0)\\
  &=\operatorname{SLERP}(p_{i}, s_{i}; 0)\\
  &=p_{i}\\
  \\
  \operatorname{Squad}(p_{i}, p_{i+1}, s_{i}, s_{i+1}; 0)
  &=\operatorname{SLERP}(\operatorname{SLERP}(p_{i}, p_{i+1}; 0), \operatorname{SLERP}(s_{i}, s_{i+1}; 0), 0)\\
  &=\operatorname{SLERP}(p_{i}, s_{i}; 0)\\
  &=p_{i}
  \end{aligned}
  $$

- $\operatorname{SQUAD}$ の微分

  $g_{i}(h) = \overline{\operatorname{SLERP}(p_{i}, p_{i+1}; h)} \cdot \operatorname{SLERP}(s_{i}, s_{i+1}; h)$ とすると

  $$
  \begin{aligned}
  \frac{d}{dh} \operatorname{Squad}(p_{i}, p_{i+1}, s_{i}, s_{i+1}; h)
  &=\frac{d}{dh} \operatorname{SLERP}(\operatorname{SLERP}(p_{i}, p_{i+1}; h), \operatorname{SLERP}(s_{i}, s_{i+1}; h), 2h(1-h))\\
  &=\frac{d}{dh} \operatorname{SLERP}(p_{i}, p_{i+1}; h) \cdot g_{i}(h)^{2h(1-h)}\\
  &=\left(\frac{d}{dh} \operatorname{SLERP}(p_{i}, p_{i+1}; h)\right) \cdot g_{i}(h)^{2h(1-h)}
  - \operatorname{SLERP}(p_{i}, p_{i+1}; h) \cdot \left(\frac{d}{dh} g_{i}(h)^{2h(1-h)}\right)\\
  &=\operatorname{SLERP}(p_{i}, p_{i+1}; h) \cdot \log{(\overline{p_{i}} \cdot p_{i+1})} \cdot g_{i}(h)^{2h(1-h)}
  - \operatorname{SLERP}(p_{i}, p_{i+1}; h) \cdot \left(\frac{d}{dh} g_{i}(h)^{2h(1-h)}\right)
  \end{aligned}
  $$

- $g_{i}(h)^{2h(1-h)}$ の微分

  $g_{i}(h)=\cos{(\theta_{g_{i}(h)})} + \sin{(\theta_{g_{i}(h)})} \mathbb{i}, \hspace{5pt} f(h)=2h(1-h)$ と置いて

  $$
  \begin{aligned}
  \frac{d}{dh} g_{i}(h)^{2h(1-h)}
  &=\frac{d}{dh} \exp{(f(h) \cdot \log{(g_{i}(h))})}\\
  &=\frac{d}{dh} \exp{(f(h) \cdot \theta_{g_{i}(h)} \mathbb{i})}\\
  &=\frac{d}{dh} \left(\cos{(f(h) \cdot \theta_{g_{i}(h)})} + \sin{(f(h) \cdot \theta_{g_{i}(h)})} \mathbb{i}\right)\\
  &=-\sin{(f(h) \cdot \theta_{g_{i}(h)})} \cdot \left(\frac{d}{dh} f(h) \cdot \theta_{g_{i}(h)}\right)
  - \cos{(f(h) \cdot \theta_{g_{i}(h)})} \cdot \left(\frac{d}{dh} f(h) \cdot \theta_{g_{i}(h)}\right) \mathbb{i}\\
  &=-\sin{(f(h) \cdot \theta_{g_{i}(h)})} \cdot \left(\left(\frac{d}{dh} f(h)\right) \cdot \theta_{g_{i}(h)} + f(h) \cdot \left(\frac{d}{dh} \theta_{g_{i}(h)}\right)\right)
  \\&\hspace{11pt}
  - \cos{(f(h) \cdot \theta_{g_{i}(h)})} \cdot \left(\left(\frac{d}{dh} f(h)\right) \cdot \theta_{g_{i}(h)} + f(h) \cdot \left(\frac{d}{dh} \theta_{g_{i}(h)}\right)\right) \mathbb{i}
  \end{aligned}
  $$

- $\frac{d}{dh} \operatorname{Squad}(p_{i-1}, p_{i}, s_{i-1}, s_{i}, 1) =  \frac{d}{dh} \operatorname{Squad}(p_{i}, p_{i+1}, s_{i}, s_{i+1}, 0)$

  $$
  \begin{aligned}
  \frac{d}{dh} \operatorname{Squad}(p_{i-1}, p_{i}, s_{i-1}, s_{i}, 1)
  &=\operatorname{SLERP}(p_{i-1}, p_{i}; 1) \cdot \log{(\overline{p_{i-1}} \cdot p_{i})}
  - \operatorname{SLERP}(p_{i-1}, p_{i}; 1) \cdot \left(\frac{d}{dh} g_{i-1}(h)^{2h(1-h)}\right)(1)\\
  &=p_{i} \cdot \log{(\overline{p_{i-1}} \cdot p_{i})}
  - 2 p_{i} \cdot \theta_{g_{i-1}(1)} \mathbb{i}\\
  &=p_{i} \cdot \left(\log{(\overline{p_{i-1}} \cdot p_{i})}
  - 2 \log(g_{i-1}(1))\right)\\
  &=p_{i} \cdot \left(\log{(\overline{p_{i-1}} \cdot p_{i})}
  - 2 \log(\overline{p_{i}} \cdot s_{i})\right)\\
  \\
  \frac{d}{dh} \operatorname{Squad}(p_{i}, p_{i+1}, s_{i}, s_{i+1}, 0)
  &=\operatorname{SLERP}(p_{i}, p_{i+1}; 0) \cdot \log{(\overline{p_{i}} \cdot p_{i+1})}
  - \operatorname{SLERP}(p_{i}, p_{i+1}; 0) \cdot \left(\frac{d}{dh} g_{i}(h)^{2h(1-h)}\right)(0)\\
  &=p_{i} \cdot \log{(\overline{p_{i}} \cdot p_{i+1})}
  - 2 p_{i} \cdot \theta_{g_{i}(0)} \mathbb{i}\\
  &=p_{i} \cdot \left(\log{(\overline{p_{i}} \cdot p_{i+1})}
  - 2 \log{(g_{i}(0))}\right)\\
  &=p_{i} \cdot \left(\log{(\overline{p_{i}} \cdot p_{i+1})}
  - 2 \log{(\overline{p_{i}} \cdot s_{i})}\right)
  \end{aligned}
  $$

- 上式より

  $$
  \begin{aligned}
  p_{i} \cdot \left(\log{(\overline{p_{i-1}} \cdot p_{i})}
  - 2 \log(\overline{p_{i}} \cdot s_{i})\right)
  &=p_{i} \cdot \left(\log{(\overline{p_{i}} \cdot p_{i+1})}
  - 2 \log{(\overline{p_{i}} \cdot s_{i})}\right)\\
  \\
  \log{(\overline{p_{i-1}} \cdot p_{i})}
  - 2 \log(\overline{p_{i}} \cdot s_{i})
  &=\log{(\overline{p_{i}} \cdot p_{i+1})}
  - 2 \log{(\overline{p_{i}} \cdot s_{i})}\\
  4 \log{(\overline{p_{i}} \cdot s_{i})}
  &=\log{(\overline{p_{i-1}} \cdot p_{i})} - \log{(\overline{p_{i}} \cdot p_{i+1})}\\
  \\
  \overline{p_{i}} \cdot s_{i}
  &=\exp{\left(\frac{\log{(\overline{p_{i-1}} \cdot p_{i})} - \log{(\overline{p_{i}} \cdot p_{i+1})}}{4}\right)}\\
  \\
  s_{i}
  &=p_{i} \cdot \exp{\left(\frac{\log{(\overline{p_{i-1}} \cdot p_{i})} - \log{(\overline{p_{i}} \cdot p_{i+1})}}{4}\right)}
  \end{aligned}
  $$

  $\overline{p_1 \cdot p_2}=\overline{p_1} \cdot \overline{p_2}, \hspace{3pt} \log(\overline{p})=-\log(p), \hspace{3pt} \overline{p}=p^{-1}$ を利用し

  $$
  \begin{aligned}
  s_{i}
  &=p_{i} \cdot \exp{\left(\frac{\log{(\overline{p_{i-1}} \cdot p_{i})} - \log{(\overline{p_{i}} \cdot p_{i+1})}}{4}\right)}\\
  &=p_{i} \cdot \exp{\left(\frac{\log{(\overline{p_{i-1} \cdot \overline{p_{i}}})} - \log{(\overline{p_{i}} \cdot p_{i+1})}}{4}\right)}\\
  &=p_{i} \cdot \exp{\left(- \frac{\log{(\overline{p_{i}} \cdot p_{i-1})} - \log{(\overline{p_{i}} \cdot p_{i+1})}}{4}\right)}\\
  &=p_{i} \cdot \exp{\left(- \frac{\log{(p_{i}^{-1} \cdot p_{i-1})} - \log{(p_{i}^{-1} \cdot p_{i+1})}}{4}\right)}
  \end{aligned}
  $$

  **四元数 $p_{i} \in {H}_1$ なら $\overline{p_1 \otimes p_2}=\overline{p_2} \otimes \overline{p_1}$**

四元数のスプライン補間[^4]と同様となる．

[^4]: 参考文献 [2] を参照．

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/calculator/kmrffw5ldr?embed="></iframe>
<figcaption>Fig. 5. スプライン補間</figcaption>
</figure>
</div>

## C++ での実装例

[GitHub Gist](https://gist.github.com/Hasenpfote/566a04b7a5f9563921dff7fe1c1b5a01)

## 参考文献

1. [Jim Van Verth 2012 Understanding Rotations](https://www.essentialmath.com/GDC2012/GDC2012_JMV_Rotations.pdf)
2. [Erik B. Dam, Martin Koch, Martin Lillholm 1998 Quaternions, Interpolation and Animation](https://web.mit.edu/2.998/www/QuaternionReport1.pdf)
