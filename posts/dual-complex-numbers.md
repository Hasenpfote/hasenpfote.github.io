---
title: "双対複素数"
description: "双対複素数について"
date: 2021-01-05T09:00:00+09:00
categories:
  - math
  - dualcomplexnumbers
  - cpp
draft: false
---

## 概要

二次元の剛体変換に非可換な双対複素数[^1]を導入することを目的とする．複素数[^2]と双対数[^3]についての予備知識だけで概ね事足りる．後者についてはテイラー展開が重要になるので簡単に触れる．

※ 四元数[^4]や双対四元数[^5]にも関連があるのでそれらの知識もあるとなお良い．

双対数は二重数とも訳されるが前者で統一する．

[^1]: <https://en.wikipedia.org/wiki/Dual-complex_number>
[^2]: <https://en.wikipedia.org/wiki/Complex_number>
[^3]: <https://en.wikipedia.org/wiki/Dual_number>
[^4]: <https://en.wikipedia.org/wiki/Quaternion>
[^5]: <https://en.wikipedia.org/wiki/Dual_quaternion>

## 双対数

双対数(Dual numbers)の環は次式で定義される剰余環．

$$
\hat{\mathbb{R}}
:=\mathbb{R}[\epsilon]/(\epsilon^2)
=\{a + b \epsilon \mid a, b \in \mathbb{R}\}
$$

### 加法

$$
(a + b \epsilon) + (c + d \epsilon)
= (a + c) + (b + d) \epsilon
$$

### 乗法

$$
(a + b \epsilon)(c + d \epsilon)
=ac + (ad + bc) \epsilon
$$

### 乗法の逆元

$$
\begin{aligned}
(a + b \epsilon)^{-1}
&=a^{-1}(1 - a^{-1} b \epsilon) \hspace{20pt} (a \ne 0)
\end{aligned}
$$

### 共役

$$
\overline{a + b \epsilon}
=a - b \epsilon
$$

### 絶対値

$$
\begin{aligned}
|a + b \epsilon|^2
&=(a + b \epsilon)(a - b \epsilon)
=a^2\\
\\
|a + b \epsilon|
&=\sqrt{a^2}
=|a|
\end{aligned}
$$

### テイラー展開

$$
\begin{aligned}
f(a + b \epsilon)
&=\sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (b \epsilon)^n\\
&=f^{(0)}(a) + f^{(1)}(a) b \epsilon + \frac{f^{(2)}(a)}{2!} (b \epsilon)^2 + \cdots\\
&=f^{(0)}(a) + f^{(1)}(a) b \epsilon
\end{aligned}
$$

例えば $\sin(a + b \epsilon)$ なら

$$
\sin(a + b \epsilon)
=\sin(a) + \cos(a) b \epsilon
$$

## 双対複素数

双対複素数(Dual Complex Numbers)の環は次式で定義される剰余環．

$$
\hat{\mathbb{C}}
:=\mathbb{C}[\epsilon]/(\epsilon^2)
=\{p_0 + p_1 \epsilon \mid p_0, p_1 \in \mathbb{C}\}
$$

$\hat{\mathbb{C}}$ の乗算について非可換な双対複素数を $\check{\mathbb{C}}$ と定義する．

$\hat{p} \in \check{\mathbb{C}}$ は

$$
\begin{aligned}
\hat{p}
&=p_0 + p_1 \epsilon\\
&=(p_a + p_b \mathbb{i}) + (p_c + p_d \mathbb{i}) \epsilon\\
&=p_a + p_b \mathbb{i} + p_c \epsilon + p_d \mathbb{i} \epsilon
\end{aligned}
$$

ここで $p_a, p_b, p_c, p_d \in \mathbb{R}$

### 加法

$$
\hat{p} + \hat{q} \in \check{\mathbb{C}}
$$

> $$
> \hat{p} + \hat{q}
> =(p_0 + p_1 \epsilon) + (q_0 + q_1 \epsilon)
> =(p_0 + q_0) + (p_1 + q_1) \epsilon
> $$

#### 結合性

$$
\hat{p} + (\hat{q} + \hat{r})
=(\hat{p} + \hat{q}) + \hat{r}
$$

> $$
> \begin{aligned}
> \hat{p} + (\hat{q} + \hat{r})
> &=(p_0 + p_1 \epsilon) + \left((q_0 + q_1 \epsilon) + (r_0 + r_1 \epsilon)\right)\\
> &=(p_0 + p_1 \epsilon) + \left((q_0 + r_0) + (q_1 + r_1) \epsilon\right)\\
> &=(p_0 + q_0 + r_0) + (p_1 + q_1 + r_1) \epsilon\\
> \\
> (\hat{p} + \hat{q}) + \hat{r}
> &=\left((p_0 + q_0) + (p_1 + q_1) \epsilon\right) + (r_0 + r_1 \epsilon)\\
> &=(p_0 + q_0 + r_0) + (p_1 + q_1 + r_1) \epsilon
> \end{aligned}
> $$

#### 可換性

$$
\hat{p} + \hat{q}
=\hat{q} + \hat{p}
$$

> $$
> \begin{aligned}
> \hat{p} + \hat{q}
> &=(p_0 + p_1 \epsilon) + (q_0 + q_1 \epsilon)\\
> &=(p_0 + q_0) + (p_1 + q_1) \epsilon\\
> \\
> \hat{q} + \hat{p}
> &=(q_0 + q_1 \epsilon) + (p_0 + p_1 \epsilon)\\
> &=(q_0 + p_0) + (q_1 + p_1)  \epsilon
> \end{aligned}
> $$

#### 中立元の存在性

$$
\hat{p} + 0
=\hat{p}
$$

> $$
> \begin{aligned}
> \hat{p} + 0
> &=(p_0 + p_1 \epsilon) + (0 + 0 \epsilon)\\
> &=(p_0 + 0) + (p_1 + 0) \epsilon
> =\hat{p}
> \end{aligned}
> $$

#### 反数の存在性

$$
\hat{p} + (- \hat{p})
=0
$$

> $$
> \begin{aligned}
> \hat{p} + (- \hat{p})
> &=(p_0 + p_1 \epsilon) + \left(- (p_0 + p_1 \epsilon)\right)\\
> &=(p_0 - p_0) + (p_1 - p_1) \epsilon
> =0
> \end{aligned}
> $$

### 乗法

$$
\hat{p} \otimes \hat{q} \in \check{\mathbb{C}}
$$

> $$
> \begin{aligned}
> \hat{p} \otimes \hat{q}
> &=(p_0 + p_1 \epsilon) \otimes (q_0 + q_1 \epsilon)\\
> &=p_0 \otimes q_0 + (p_0 \otimes q_1 + p_1 \otimes \overline{q_0}) \epsilon
> \end{aligned}
> $$

#### 乗法のルール

乗積表($\mathbb{i} \epsilon = - \epsilon \mathbb{i}$ に注意)は

$$
\begin{aligned}
\begin{array}{c|cccc}
  L \backslash R & 1 & \mathbb{i} & \epsilon & \mathbb{i} \epsilon\\ \hline
    1 & 1 & \mathbb{i} & \epsilon & \mathbb{i} \epsilon\\
    \mathbb{i} & \mathbb{i} & -1 & \mathbb{i} \epsilon & - \epsilon\\
    \epsilon & \epsilon & - \mathbb{i} \epsilon & 0 & 0\\
    \mathbb{i} \epsilon & \mathbb{i} \epsilon & \epsilon & 0 & 0\\
\end{array}
\end{aligned}
$$

順序に注意し展開

$$
\begin{aligned}
\hat{p} \otimes \hat{q}
&=(p_0 + p_1 \epsilon) \otimes (q_0 + q_1 \epsilon)\\
&=p_0 \cdot q_0
+ p_0 \cdot q_1 \epsilon
+ p_1 \epsilon \cdot q_0
+ p_1 \epsilon \cdot q_1 \epsilon\\
\end{aligned}
$$

第3項は

$$
\begin{aligned}
\epsilon \cdot q_0
&=\epsilon \cdot (q_a + q_b \mathbb{i})\\
&=q_a \epsilon + q_b \epsilon \mathbb{i}\\
&=q_a \epsilon - q_b \mathbb{i} \epsilon\\
&=(q_a - q_b \mathbb{i}) \epsilon
=\overline{q_0} \epsilon
\end{aligned}
$$

第4項は

$$
\begin{aligned}
\epsilon \cdot q_1 \epsilon
&=\epsilon \cdot (q_c \epsilon + q_d \mathbb{i} \epsilon)\\
&=q_c \epsilon \epsilon + q_d \epsilon \mathbb{i} \epsilon\\
&=q_c \epsilon^2 - q_d \mathbb{i} \epsilon^2\\
&=(q_c - q_d \mathbb{i}) \epsilon^2
=\overline{q_1} \epsilon^2
\end{aligned}
$$

これらから左側から $\epsilon$ を掛けると右側の複素数の複素共役を取り順序が入れ替わる．

#### 結合性

$$
\hat{p} \otimes (\hat{q} \otimes \hat{r})
=(\hat{p} \otimes \hat{q}) \otimes \hat{r}
$$

> $$
> \begin{aligned}
> \hat{p} \otimes (\hat{q} \otimes \hat{r})
> &=(p_0 + p_1 \epsilon) \otimes \left(q_0 r_0 + (q_0 r_1 + q_1 \overline{r_0}) \epsilon\right)\\
> &=p_0 q_0 r_0 + (p_0 q_0 r_1 + p_0 q_1 \overline{r_0} + p_1 \overline{q_0 r_0}) \epsilon\\
> \\
> (\hat{p}\hat{q}) \otimes \hat{r}
> &=\left(p_0 q_0 + (p_0 q_1 + p_1 \overline{q_0}) \epsilon\right) \otimes (r_0 + r_1 \epsilon)\\
> &=p_0 q_0 r_0 + (p_0 q_0 r_1 + p_0 q_1 \overline{r_0} + p_1 \overline{q_0} \overline{r_0}) \epsilon
> \end{aligned}
> $$

#### 可換性

$$
\hat{p} \otimes \hat{q}
\ne \hat{q} \otimes \hat{p}
$$

> $$
> \begin{aligned}
> \hat{p} \otimes \hat{q}
> &=p_0 q_0 + (p_0 q_1 + p_1 \overline{q_0}) \epsilon\\
> \\
> \hat{q} \otimes \hat{p}
> &=q_0 p_0 + (q_0 p_1 + q_1 \overline{p_0}) \epsilon
> \end{aligned}
> $$

#### 中立元の存在

$$
\hat{p} \otimes 1
=\hat{p}
$$

> $$
> \begin{aligned}
> \hat{p} \otimes 1
> &=(p_0 + p_1 \epsilon) \otimes (1 + 0 \epsilon)\\
> &=p_0 + p_1 \epsilon
> =\hat{p}
> \end{aligned}
> $$

### 分配性

$$
\hat{p} \otimes (\hat{q} + \hat{r})
=\hat{p} \otimes \hat{q} + \hat{p} \otimes \hat{r}
$$

> $$
> \begin{aligned}
> \hat{p} \otimes (\hat{q} + \hat{r})
> &=(p_0 + p_1 \epsilon) \otimes \left((q_0 + r_0) + (q_1 + r_1) \epsilon\right)\\
> &=p_0 q_0 + p_0 r_0 + (p_0 q_1 + p_0 r_1 + p_1 \overline{q_0} + p_1 \overline{r_0}) \epsilon\\
> &=\hat{p} \otimes \hat{q} + \hat{p} \otimes \hat{r}
> \end{aligned}
> $$

$$
(\hat{p} + \hat{q}) \otimes \hat{r}
=\hat{p} \otimes \hat{r} + \hat{q} \otimes \hat{r}
$$

> $$
> \begin{aligned}
> (\hat{p} + \hat{q}) \otimes \hat{r}
> &=\left((p_0 + q_0) + (p_1 + q_1) \epsilon\right) \otimes (r_0 + r_1 \epsilon)\\
> &=p_0 r_0 + q_0 r_0 + (p_0 r_1 + q_0 r_1 + p_1\overline{r_0} + q_1 \overline{r_0}) \epsilon\\
> &=\hat{p}\otimes \hat{r} + \hat{q} \otimes \hat{r}
> \end{aligned}
> $$

### 乗法の逆元

$$
\hat{p}^{-1}
=\frac{1}{\hat{p}}
=\frac{\overline{p_0} - p_1 \epsilon}{|p_0|^2}
$$

> $$
> \begin{aligned}
> \frac{1}{p_0 + p_1 \epsilon}
> &=\frac{1}{p_0 + p_1 \epsilon}\frac{\overline{p_0} - p_1 \epsilon}{\overline{p_0} - p_1 \epsilon}
> =\frac{\overline{p_0} - p_1 \epsilon}{p_0 \overline{p_0} + (p_1 p_0 - p_0 p_1) \epsilon}\\
> \end{aligned}
> $$

$$
\hat{p} \otimes \hat{p}^{-1}
=\hat{p}^{-1} \otimes \hat{p}=1
$$

> $$
> \begin{aligned}
> \hat{p} \otimes \hat{p}^{-1}
> &=(p_0 + p_1 \epsilon) \otimes \frac{\overline{p_0} - p_1 \epsilon}{|p_0|^2}\\
> &=\frac{p_0 \overline{p_0} - p_0 p_1 \epsilon}{|p_0|^2} + \frac{p_1 p_0 \epsilon}{|p_0|^2}
> =\frac{p_0 \overline{p_0}}{|p_0|^2}=1\\
> \\
> \hat{p}^{-1} \otimes \hat{p}
> &=\frac{\overline{p_0} - p_1 \epsilon}{|p_0|^2} \otimes (p_0 + p_1 \epsilon)\\
> &=\frac{\overline{p_0} p_0 - p_1 \overline{p_0} \epsilon}{|p_0|^2} + \frac{\overline{p_0} p_1 \epsilon}{|p_0|^2}
> =\frac{\overline{p_0} p_0}{|p_0|^2}=1
> \end{aligned}
> $$

### 絶対値

$$
\begin{aligned}
|\hat{p}|^2
&=(p_0 + p_1 \epsilon)(\overline{p_0} - p_1 \epsilon)
=|p_0|^2\\
\\
|\hat{p}|
&=|p_0|
\end{aligned}
$$

### 単位双対複素数

単位双対複素数の定義は

$$
\begin{aligned}
\check{\mathbb{C}}_1:&=\{\hat{p} \in \check{\mathbb{C}} \mid |\hat{p}| = 1\}\\
&=\{e^{\theta\mathbb{i}} + p_1 \epsilon \in \check{\mathbb{C}} \mid \theta \in \mathbb{R}, p_1 \in \mathbb{C}\}
\end{aligned}
$$

乗法の逆元は

$$
(e^{\theta\mathbb{i}} + p_1 \epsilon)^{-1}
=e^{-\theta\mathbb{i}} - p_1 \epsilon
$$

> 補足
>
> $$
> \begin{aligned}
> \overline{e^{\theta\mathbb{i}}}
> &=\overline{\cos(\theta) + \sin(\theta)\mathbb{i}}\\
> &=\cos(\theta) - \sin(\theta)\mathbb{i}\\
> &=\cos(-\theta) + \sin(-\theta)\mathbb{i}\\
> &=e^{-\theta\mathbb{i}}\\
> \end{aligned}
> $$

#### 共役による $\check{\mathbb{C}}$ 上の $\check{\mathbb{C}}_1$ の作用

$\mathbb{C}=\mathbb{R}^2$ とし $v \in \mathbb{C}$ を $1 + v \epsilon \in \check{\mathbb{C}}$ と同一視すると $\hat{p} = p_0 + p_1 \epsilon \in  \check{\mathbb{C}}_1$ が剛体変換として作用する．

$$
\begin{aligned}
\hat{p} \diamond (1 + v \epsilon)
&=(p_0 + p_1 \epsilon)(1 + v \epsilon)(\overline{p_0} + p_1 \epsilon)\\
&=(p_0 + p_1 \epsilon)\left(\overline{p_0} + (p_1 + v p_0) \epsilon \right)\\
&=p_0\overline{p_0} + (p_0 p_1 + p_0 v p_0 + p_1 p_0) \epsilon\\
&=|p_0|^2 + (p_0^{2} v + 2 p_0 p_1) \epsilon\\
&=1 + (p_0^{2} v + 2 p_0 p_1) \epsilon
\end{aligned}
$$

+ $p_1 = 0$  
  $v \in \mathbb{C}$ は ${p_0}^{2} v$ に写像される．
  $|p_0| = 1$ なので原点を中心に $2 \arg(p_0)$ 回転をする．
+ $p_0 = 1$  
  $v + 2 p_1$ の変位をする．

##### 回転と変位の表現

回転の表現は $\theta \in \mathbb{R}$ とすると

$$
\hat{p}_{rot}
=e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon
$$

> $$
> \begin{aligned}
> \hat{p}_{rot} \diamond (1 + v \epsilon)
> &=\left(e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon\right) \otimes (1 + v \epsilon) \otimes \left(\overline{e^{\frac{\theta}{2}\mathbb{i}}} + 0 \epsilon\right)\\
> &=\left(e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon\right) \otimes (1 + v \epsilon) \otimes \left(e^{-\frac{\theta}{2}\mathbb{i}} + 0 \epsilon\right)\\
> &=1 + e^{\theta\mathbb{i}} v \epsilon
> \end{aligned}
> $$
>
> 複素数の半角形式(スピノル表現)での回転について
>
> $$
> e^{\theta\mathbb{i}} v
> =\left(e^{\frac{\theta}{2}\mathbb{i}}\right)^2 v
> =e^{\frac{\theta}{2}\mathbb{i}} v e^{\frac{\theta}{2}\mathbb{i}}
> $$
>
> また**符号を反転しても同じ回転を表現**する
>
> $$
> \left(- e^{\frac{\theta}{2}\mathbb{i}}\right)^2
> =e^{\theta\mathbb{i}}
> $$
>
> <div class="quarto-figure quarto-figure-center">
> <figure class="figure">
> <iframe src="https://www.desmos.com/calculator/usdbcrfr30?embed="></iframe>
> <figcaption>Fig. 1. 回転と補間</figcaption>
> </figure>
> </div>

変位の表現は $d \in \mathbb{C}$ とすると

$$
\hat{p}_{disp}
=1 + \frac{d}{2} \epsilon
$$

> $$
> \begin{aligned}
> \hat{p}_{disp} \diamond (1 + v \epsilon)
> &=\left(1 + \frac{d}{2} \epsilon\right) \otimes (1 + v \epsilon) \otimes \left(1 + \frac{d}{2} \epsilon\right)\\
> &=1 + (v + d) \epsilon
> \end{aligned}
> $$

例

+ 原点を中心に $\theta \in \mathbb{R}$ 回転後に $d \in \mathbb{C}$ の変位を表現する変換  
  $\hat{p} = \hat{p}_{disp} \otimes \hat{p}_{rot} \in \check{\mathbb{C}}_1$

  <div class="quarto-figure quarto-figure-center">
  <figure class="figure">
  <iframe src="https://www.desmos.com/calculator/2cliapnche?embed="></iframe>
  <figcaption>Fig. 2. 原点を中心に回転後に変位</figcaption>
  </figure>
  </div>

+ $d \in \mathbb{C}$ の変位後に原点を中心に $\theta \in \mathbb{R}$ 回転を表現する変換  
  $\hat{p} = \hat{p}_{rot} \otimes \hat{p}_{disp} \in \check{\mathbb{C}}_1$

  <div class="quarto-figure quarto-figure-center">
  <figure class="figure">
  <iframe src="https://www.desmos.com/calculator/8rg6yvogcl?embed="></iframe>
  <figcaption>Fig. 3. 変位後に原点を中心に回転</figcaption>
  </figure>
  </div>

+ 階層構造  
  $\hat{p} = \hat{P}_{disp} \otimes \hat{P}_{rot} \otimes \hat{C}_{disp} \otimes \hat{C}_{rot} \in \check{\mathbb{C}}_1$

  <div class="quarto-figure quarto-figure-center">
  <figure class="figure">
  <iframe src="https://www.desmos.com/calculator/f6wgqv6brr?embed="></iframe>
  <figcaption>Fig. 3. 変位後に原点を中心に回転</figcaption>
  </figure>
  </div>

##### 性質

符号を反転した変換は符号を反転した回転による変換と同じになる．

$$
\begin{aligned}
- (\hat{p}_{disp} \otimes \hat{p}_{rot})
&=\hat{p}_{disp} \otimes \hat{p}_{-rot}
\end{aligned}
$$

> $$
> \begin{aligned}
> - \hat{p}_{disp} \otimes \hat{p}_{rot}
> &=- \left(1 + \frac{d}{2} \epsilon\right)\otimes (e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon)\\
> &=- \left(e^{\frac{\theta}{2}\mathbb{i}} + \frac{d}{2} e^{- \frac{\theta}{2}\mathbb{i}} \epsilon\right)\\
> \\
> \hat{p}_{disp} \otimes \hat{p}_{-rot}
> &=\left(1 + \frac{d}{2} \epsilon\right) \otimes (- e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon)\\
> &=-e^{\frac{\theta}{2}\mathbb{i}} - \frac{d}{2} e^{- \frac{\theta}{2}\mathbb{i}} \epsilon\\
> &=- \left(e^{\frac{\theta}{2}\mathbb{i}} + \frac{d}{2} e^{- \frac{\theta}{2}\mathbb{i}} \epsilon\right)
> \end{aligned}
> $$

$$
\begin{aligned}
- (\hat{p}_{rot} \otimes \hat{p}_{disp})
&=\hat{p}_{-rot} \otimes \hat{p}_{disp}
\end{aligned}
$$

> $$
> \begin{aligned}
> - (\hat{p}_{rot} \otimes \hat{p}_{disp})
> &=- (e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon) \otimes \left(1 + \frac{d}{2} \epsilon\right)\\
> &=- \left(e^{\frac{\theta}{2}\mathbb{i}} + e^{\frac{\theta}{2}\mathbb{i}} \frac{d}{2} \epsilon\right)\\
> \\
> \hat{p}_{-rot} \otimes \hat{p}_{disp}
> &=(- e^{\frac{\theta}{2}\mathbb{i}} + 0 \epsilon) \otimes \left(1 + \frac{d}{2} \epsilon\right)\\
> &=- e^{\frac{\theta}{2}\mathbb{i}} -  e^{\frac{\theta}{2}\mathbb{i}} \frac{d}{2} \epsilon\\
> &=- \left(e^{\frac{\theta}{2}\mathbb{i}} + e^{\frac{\theta}{2}\mathbb{i}} \frac{d}{2} \epsilon\right)
> \end{aligned}
> $$

よって**符号を反転した変換は，元と同じ変換を表現**する．

### 補間

#### Dual complex number Linear Blending

$\hat{p}_i \in \check{\mathbb{C}}_1, \hspace{5pt} w_i \in \mathbb{R}$ とした補間を次のように定義．

$$
\operatorname{DLB}{(\hat{p}_1,\cdots ,\hat{p}_n; w_1,\cdots ,w_n)}
=\frac{w_1 \hat{p}_1 + \cdots + w_n \hat{p}_n}{|w_1 \hat{p}_1 + \cdots + w_n \hat{p}_n|}
$$

加重平均の正規化なので分母が $0$ の場合は定義ができない．

##### 正規化

$$
\frac{\hat{p}}{|\hat{p}|}
=\frac{p_0 + p_1 \epsilon}{|p_0|}
$$

> $$
> \left|\frac{\hat{p}}{|\hat{p}|}\right|
> =\left|\frac{p_0}{|p_0|}\right|
> =1
> $$

#### Spherical Linear interpolation

$\hat{p} \in \check{\mathbb{C}}_1$ から $\hat{q} \in \check{\mathbb{C}}_1$ への $h \in \mathbb{R}$ での補間を次のように定義．

$$
\begin{aligned}
\operatorname{SLERP}(\hat{p}, \hat{q}; h)
&=\hat{p} \otimes (\hat{p}^{-1} \otimes \hat{q})^{h}\\
&=\hat{p} \otimes \exp\left(h \log(\hat{p}^{-1} \otimes \hat{q})\right)
\end{aligned}
$$

##### 指数関数と対数関数

指数関数

$$
\begin{aligned}
\exp{(\hat{p})}
&=\exp{(p_0 + p_1 \epsilon)}\\
&=\exp{(p_0)} + \exp{(p_0)} p_1 \epsilon\\
&=\exp{(p_0)}\left(1 + p_1 \epsilon\right)
\end{aligned}
$$

対数関数

$$
\begin{aligned}
\log{(\hat{p})}
&=\log{(p_0 + p_1 \epsilon)}\\
&=\log{(p_0)} + \frac{p_1}{p_0} \epsilon
\end{aligned}
$$

性質

$$
\log{(\exp{(\hat{p})})}
=\exp{(\log{(\hat{p})})}
=\hat{p}
$$

> $$
> \begin{aligned}
> \log{(\exp{(\hat{p})})}
> &=\log{(\exp{(p_0)})} + \frac{\exp{(p_0)} p_1 \epsilon}{\exp{(p_0)}}\\
> &=p_0 + p_1 \epsilon
> =\hat{p}\\
> \\
> \exp{(\log{(\hat{p})})}
> &=\exp{\left(\log{(p_0)}\right)} \otimes \left(1 + \frac{p_1}{p_0} \epsilon\right)\\
> &=p_0 \otimes \left(1 + \frac{p_1}{p_0} \epsilon\right)\\
> &=p_0 + p_1 \epsilon
> =\hat{p}
> \end{aligned}
> $$

##### 性質

回転と変位の表現で合成された変換を $\hat{p}, \hat{q} \in \check{\mathbb{C}}_1$ とする．

+ なす角が180度を超えても最短円弧で補間をしない．  
  $\langle p_0, q_0 \rangle \lt 0$ の時に一方の符号を反転 ($\operatorname{SLERP}(\hat{p}, -\hat{q}; h)$) すると最短円弧での補間となる．

### 変換の行列表現

#### 積の行列表現

$\hat{p} = p_0 + p_1 \epsilon, \hspace{5pt} \hat{q} = q_0 + q_1 \epsilon \in \check{\mathbb{C}}$ の積は

$$
\begin{aligned}
\hat{p} \otimes \hat{q}
&=(p_0 + p_1 \epsilon) \otimes (q_0 + q_1 \epsilon)\\
&=p_0 \otimes q_0 + (p_0 \otimes q_1 + p_1 \otimes \overline{q_0}) \epsilon\\
&=(p_a q_a - p_b q_b)\\
&\hspace{13pt}
+(p_a q_b + p_b q_a) \mathbb{i}\\
&\hspace{13pt}
+(p_a q_c - p_b q_d + p_c q_a + p_d q_b) \epsilon\\
&\hspace{13pt}
+(p_a q_d + p_b q_c - p_c q_b + p_d q_a) \mathbb{i}\epsilon
\end{aligned}
$$

成分計算は $\hat{p} = (p_a + p_b \mathbb{i}) + (p_c + p_d \mathbb{i}) \epsilon$ とした．

$\hat{q}$ に対し左から $\hat{p}$ をかける行列は

$$
\begin{aligned}
P_L q
&=
\left(
\begin{array}{cccc}
  p_a & - p_b & 0 & 0\\
  p_b & p_a & 0 & 0\\
  p_c & p_d & p_a & - p_b\\
  p_d & - p_c & p_b & p_a
\end{array}
\right)
\left(
\begin{array}{c}
  q_a\\
  q_b\\
  q_c\\
  q_d
\end{array}
\right)
\end{aligned}
$$

$\hat{p}$ に対し右から $\hat{q}$ を掛ける行列は

$$
\begin{aligned}
Q_R p
&=
\left(
\begin{array}{cccc}
  q_a & - q_b & 0 & 0\\
  q_b & q_a & 0 & 0\\
  q_c & - q_d & q_a & q_b\\
  q_d & q_c & - q_b & q_a
\end{array}
\right)
\left(
\begin{array}{c}
  p_a\\
  p_b\\
  p_c\\
  p_d
\end{array}
\right)
\end{aligned}
$$

#### 変換の行列表現

$\hat{p} \in \check{\mathbb{C}}_1$ での変換に対する次のような行列を考える．

$$
\begin{aligned}
\hat{p} \diamond (1 + v \epsilon)
&=(p_0 + p_1 \epsilon) \otimes (1 + v \epsilon) \otimes (\overline{p_0} + p_1 \epsilon)\\
&=M v\\
&=M_L M_R v
\end{aligned}
$$

$$
\begin{aligned}
M
&=M_L M_R\\
&=
\left(
\begin{array}{cccc}
  p_a & - p_b & 0 & 0\\
  p_b & p_a & 0 & 0\\
  p_c & p_d & p_a & - p_b\\
  p_d & - p_c & p_b & p_a
\end{array}
\right)
\left(
\begin{array}{cccc}
  p_a & p_b & 0 & 0\\
  - p_b & p_a & 0 & 0\\
  p_c & - p_d & p_a & - p_b\\
  p_d & p_c & p_b & p_a
\end{array}
\right)\\
&=
\left(
\begin{array}{cccc}
  p_a p_a + p_b p_b & p_a p_b - p_b p_a & 0 & 0\\
  p_b p_a - p_a p_b & p_b p_b + p_a p_a & 0 & 0\\
  p_c p_a - p_d p_b + p_a p_c - p_b p_d &
  p_c p_b + p_d p_a - p_a p_d - p_b p_c &
  p_a p_a - p_b p_b &
  - p_a p_b - p_b p_a\\
  p_d p_a + p_c p_b + p_b p_c + p_a p_d &
  p_d p_b - p_c p_a - p_b p_d + p_a p_c &
  p_b p_a + p_a p_b &
  - p_b p_b + p_a p_a
\end{array}
\right)\\
&=
\left(
\begin{array}{cccc}
  {p_a}^2 + {p_b}^2 & 0 & 0 & 0\\
  0 & {p_a}^2 + {p_b}^2 & 0 & 0\\
  2(p_a p_c - p_b p_d) & 0 & {p_a}^2 - {p_b}^2 & - 2 p_a p_b\\
  2(p_a p_d + p_b p_c) & 0 & 2 p_a p_b & {p_a}^2 - {p_b}^2
\end{array}
\right)\\
&=
\left(
\begin{array}{cccc}
  1 & 0 & 0 & 0\\
  0 & 1 & 0 & 0\\
  2(p_a p_c - p_b p_d) & 0 & {p_a}^2 - {p_b}^2 & - 2 p_a p_b\\
  2(p_a p_d + p_b p_c) & 0 & 2 p_a p_b & {p_a}^2 - {p_b}^2
\end{array}
\right)\\
&=
\left(
\begin{array}{cccc}
  1 & 0 & 0 & 0\\
  0 & 1 & 0 & 0\\
  \mathfrak{Re}(2 p_0 p_1) & 0 & \mathfrak{Re}({p_0}^2) & -  \mathfrak{Im}({p_0}^2)\\
  \mathfrak{Im}(2 p_0 p_1) & 0 & \mathfrak{Im}({p_0}^2) & \mathfrak{Re}({p_0}^2)\\
\end{array}
\right)
\end{aligned}
$$

ここで ${p_a}^2 + {p_b}^2 = |p_0|^2 = 1$ を利用した．

また $\mathfrak{Re}, \mathfrak{Im}$ は実部と虚部とする．

> $$
> \begin{aligned}
> M v
> &=
> \left(
> \begin{array}{cccc}
>  1 & 0 & 0 & 0\\
>  0 & 1 & 0 & 0\\
>  \mathfrak{Re}(2 p_0 p_1) & 0 & \mathfrak{Re}({p_0}^2) & -  \mathfrak{Im}({p_0}^2)\\
>  \mathfrak{Im}(2 p_0 p_1) & 0 & \mathfrak{Im}({p_0}^2) & \mathfrak{Re}({p_0}^2)
> \end{array}
> \right)
> \left(
> \begin{array}{c}
>  1\\
>  0\\
>  x\\
>  y
> \end{array}
> \right)\\
> &=
> \left(
> \begin{array}{c}
>  1\\
>  0\\
>  \mathfrak{Re}(2 p_0 p_1) + \mathfrak{Re}({p_0}^2) x - \mathfrak{Im}({p_0}^2) y\\
>  \mathfrak{Im}(2 p_0 p_1) + \mathfrak{Im}({p_0}^2) x +  \mathfrak{Re}({p_0}^2) y
> \end{array}
> \right)
> \end{aligned}
> $$

#### 同次変換行列

4次正方行列では都合が良くない場合もあるので2次元の同次変換行列にする．

$$
\begin{aligned}
M
&=
\left(
\begin{array}{ccc}
  \mathfrak{Re}({p_0}^2) & - \mathfrak{Im}({p_0}^2) & \mathfrak{Re}(2 p_0 p_1)\\
  \mathfrak{Im}({p_0}^2) & \mathfrak{Re}({p_0}^2) &  \mathfrak{Im}(2 p_0 p_1)\\
  0 & 0 & 1
\end{array}
\right)
\end{aligned}
$$

> $$
> \begin{aligned}
> M v
> &=
> \left(
> \begin{array}{ccc}
>  \mathfrak{Re}({p_0}^2) & - \mathfrak{Im}({p_0}^2) &  \mathfrak{Re}(2 p_0 p_1)\\
>  \mathfrak{Im}({p_0}^2) & \mathfrak{Re}({p_0}^2) &  \mathfrak{Im}(2 p_0 p_1)\\
>  0 & 0 & 1
> \end{array}
> \right)
> \left(
> \begin{array}{c}
>  x\\
>  y\\
>  1
> \end{array}
> \right)\\
> &=
> \left(
> \begin{array}{c}
>  \mathfrak{Re}({p_0}^2) x - \mathfrak{Im}({p_0}^2) y +  \mathfrak{Re}(2 p_0 p_1)\\
>  \mathfrak{Im}({p_0}^2) x + \mathfrak{Re}({p_0}^2) y +  \mathfrak{Im}(2 p_0 p_1)\\
>  1
> \end{array}
> \right)
> \end{aligned}
> $$

## A C++ Library

+ [https://github.com/Hasenpfote/dualcomplex](https://github.com/Hasenpfote/dualcomplex)

## 参考文献

1. [GENKI MATSUDA, SHIZUO KAJI, HIROYUKI OCHIAI 2016 ANTI-COMMUTATIVE DUAL COMPLEX NUMBERS AND 2D RIGID TRANSFORMATION](https://arxiv.org/abs/1601.01754v1)
