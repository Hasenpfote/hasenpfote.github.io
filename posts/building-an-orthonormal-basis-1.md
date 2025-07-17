---
title: "正規直交基底の構築 (1)"
description: "Frisvad の方法について考察をする"
date: 2025-01-13T09:00:00+09:00
categories:
  - computerscience
  - onb
  - orthonormal
  - basis
draft: false
---

## Frisvad の方法

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/3d/xl9ffacnhf"></iframe>
</figure>
</div>

Frisvad [2012] では，接空間の $z$ 軸から任意の単位ベクトル $\vec{n}$ への回転に四元数を利用する．これは，サンプリングされた方向 ${}_{\perp}\vec{\omega}$ に適用して $\vec{\omega}$ を得る回転である．以下の式で，ベクトル $\boldsymbol{s}$ から $\boldsymbol{t}$ への回転を指定する単位四元数 $\hat{q}$ を見つける(導出は Appendix 1).

$$
\hat{q}
=(q_{v}, q_{w})
=\left(\frac{\boldsymbol{s} \times \boldsymbol{t}}{\sqrt{2(1 + \langle \boldsymbol{s}, \boldsymbol{t} \rangle)}}, \frac{\sqrt{2(1 + \langle \boldsymbol{s}, \boldsymbol{t} \rangle)}}{2} \right)
\tag{1}
$$

$z$ 軸の方向 $(0,0,1)^{\mathsf{T}}$ から $\vec{n}=(n_{x}, n_{y}, n_{z})^{\mathsf{T}}$ に回転する場合，式 $(1)$ は次のように簡略化される.

$$
\hat{q}
=\left(\frac{(-n_{y}, n_{x}, 0)^{\mathsf{T}}}{\sqrt{2(1 + n_{z})}}, \frac{\sqrt{2(1 + n_{z})}}{2} \right)
\tag{2}
$$

代数的な操作の後，$\vec{n}$ が単位ベクトルであることを利用すると，次の結果が得られる(正規性と直交性の証明は Frisvad[2012] の Appendix B).

$$
\begin{align*}
\vec{\omega}
&=(\hat{q} ({}_{\perp}\vec{\omega}, 0) \hat{q}^{*})_{v}\\
&=
\begin{pmatrix}
x
\begin{pmatrix}
1 - \frac{n^{2}_{x}}{1 + n_{z}}\\
- \frac{n_{x} n_{y}}{1 + n_{z}}\\
- n_{x}
\end{pmatrix}
+
y
\begin{pmatrix}
- \frac{n_{x} n_{y}}{1 + n_{z}}\\
1 - \frac{n^{2}_{y}}{1 + n_{z}}\\
- n_{y}
\end{pmatrix}
+
z
\begin{pmatrix}
n_{x}\\
n_{y}\\
n_{z}
\end{pmatrix}
\end{pmatrix}
\tag{3}
\end{align*}
$$

この方程式は，$1 + n_{z}=0$ の場合に特異点を持つが，これは $\vec{n}=(0, 0, -1)^{\mathsf{T}}$ の場合にのみ起こる．この場合は，$\vec{\omega}=(-y, -x , -z)^{\mathsf{T}}$ と設定することで対処できる．

$z$ 軸周りにサンプリングした方向を任意の方向 $\vec{n}$ まわりにサンプリングした方向に回転させる式 $(3)$ を見ると，3 つのベクトルがある.

$$
\begin{align*}
\vec{b}_{1}
&=
\left(
1 - \frac{n^{2}_{x}}{1 + n_{z}}, - \frac{n_{x} n_{y}}{1 + n_{z}}, - n_{x}
\right)^{\mathsf{T}}
\tag{4}\\
\vec{b}_{2}
&=
\left(
- \frac{n_{x} n_{y}}{1 + n_{z}}, 1 - \frac{n^{2}_{y}}{1 + n_{z}}, - n_{y}
\right)^{\mathsf{T}}
\tag{5}\\
\vec{n}
&=
\left(
n_{x}, n_{y}, n_{z}
\right)^{\mathsf{T}}
\tag{6}\\
\end{align*}
$$

これらの式は，$\vec{n}$ を $z$ 軸とする基底である.

::: {.callout-note title="特異点への対処について"}
$1 + n_{z}=0$ の場合は，$\vec{b}_{1}=(0, -1, 0)^{\mathsf{T}}$ および $\vec{b}_{2}=(-1, 0, 0)^{\mathsf{T}}$ とする.

$$
\vec{\omega}
=(\vec{b}_{1}, \vec{b}_{2}, \vec{n}) {}_{\perp}\vec{\omega}
=
\begin{pmatrix}
0 & -1 & 0\\
-1 & 0 & 0\\
0 & 0 & -1
\end{pmatrix}
\begin{pmatrix}
x\\
y\\
z
\end{pmatrix}
=
\begin{pmatrix}
-y\\
-x\\
-z
\end{pmatrix}
$$
:::

実装は次のようになる.

```cpp
// Listing 2. New way of finding an orthonormal basis from a unit 3D vector.
void frisvad(const Vec3f& n, Vec3f& b1, Vec3f& b2)
{
  if(n.z < -0.9999999f) // Handle the singularity
  {
    b1 = Vec3f ( 0.0f, -1.0f, 0.0f);
    b2 = Vec3f (-1.0f , 0.0f, 0.0f);
    return;
  }
  const float a = 1.0f/(1.0f + n.z);
  const float b = -n.x*n.y*a;
  b1 = Vec3f (1.0f - n.x*n.x*a, b, -n.x);
  b2 = Vec3f (b, 1.0f - n.y*n.y*a, -n.y);
}
```

### Issues

Max [2017] によると，単精度浮動小数点数の場合に特異点付近で許容できない誤差が発生することが報告されている．また，最大誤差を最小化することにも触れており,

> As t increases away from -1, the maximum errors for vectors n with n.z ≥ t will decrease, and those errors for n.z < t will increase, so there should be an optimum trade-off value of t where they are equal. When considering only the normality conditions, the maximum normality error was minimized at 0.00222844 when t = -0.9999805689, in which case the maximum orthogonality error was 0.00622611. When considering only the orthogonality conditions, the maximum error was minimized to the value 0.004901 when t = -0.99998796, in which case the maximum normality condition error was 0.00180161. This same value of t minimized the maximum errors when both the orthogonality and normality conditions were considered together. So if these errors of about a half of one percent are acceptable, this threshold of -0.99998796 would work in Frisvad’s method.

閾値を次のように変更する.

```cpp
  ...
  if(n.z < -0.99998796f) // Handle the singularity
  ...
```

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.shadertoy.com/embed/M3KcR3?gui=true&t=10&paused=true&muted=false" allowfullscreen></iframe>
<figcaption>確度の比較</figcaption>
</figure>
</div>

### Pros and Cons

- Pros
  - 外積と正規化がないため，既存の方法を凌駕する実行速度を得る
  - 実質的に分岐がない
  - 四元数を基礎としているため，接ベクトルの方向に一貫性がある (Frisvad [2014])
- Cons
  - 単精度浮動小数点数の場合に，特異点付近で許容できない誤差が発生する

## Appendix 1: 最短円弧の四元数について

Melax [2000] を参考に最短円弧の四元数を導出する.

### 安定した式の導出

回転を表す単位四元数は，次のように表される.

$$
q
=e^{\frac{\theta}{2} \hat{\boldsymbol{u}}}
=\cos{\left(\frac{\theta}{2}\right)} + \sin{\left(\frac{\theta}{2}\right)} \hat{\boldsymbol{u}}
$$

2 つのベクトル $\hat{\boldsymbol{a}}, \hat{\boldsymbol{b}} \in \mathbb{R}^{3}$ かつ $\|\hat{\boldsymbol{a}}\|=\|\hat{\boldsymbol{b}}\|=1$ について，内積 $d=\langle \hat{\boldsymbol{a}}, \hat{\boldsymbol{b}} \rangle$ と外積 $\boldsymbol{c}=\hat{\boldsymbol{a}}\times \hat{\boldsymbol{b}}$ を定義する．四元数 $q$ のベクトル成分 $q_{v}=(q_{x}, q_{y}, q_{z})$ の長さは，角度 $\theta$ の半分に対する正弦 $\sin{\left(\frac{\theta}{2} \right)}$ に等しい．また，外積 $\boldsymbol{c}$ の長さは，角度 $\theta$ に対する正弦 $\sin{(\theta)}$ に等しい．これらの関係を元に，四元数の成分を求める.

$$
q_{v}
=\sin{\left(\frac{\theta}{2} \right)} \hat{\boldsymbol{u}}
=\frac{\sin{\left(\frac{\theta}{2} \right)}}{\sin{\theta}} \boldsymbol{c}
$$

$\frac{\sin{\left(\frac{\theta}{2} \right)}}{\sin{\theta}}$ 項の安定した式を決定する必要がある．$\sin{\left(\frac{\theta}{2} \right)}$ は，三角関数の半角公式から次のように表される.

$$
\sin{\left(\frac{\theta}{2} \right)}
=\sqrt{\frac{1 - \cos{\theta}}{2}}
$$

また，次の三角関数の恒等式を用いる.

$$
\sin^{2}{\theta} + \cos^{2}{\theta}=1
$$

すると，次のようになる.

$$
\frac{\sin{\left(\frac{\theta}{2} \right)}}{\sin{\theta}}
=\frac{\sqrt{\frac{1 - \cos{\theta}}{2}}}{\sqrt{1 - \cos^{2}{\theta}}}
$$

$d$ を用いて簡略化すると,

$$
\frac{\sqrt{\frac{1 - d}{2}}}{\sqrt{1 - d^{2}}}
=\sqrt{\frac{1 - d}{2(1 - d^{2})}}
=\sqrt{\frac{1 - d}{2(1 + d)(1 - d)}}
=\frac{1}{\sqrt{2(1 + d)}}
$$

それらを代入するとベクトル部は次のようになる.

$$
q_{v}
=\frac{\boldsymbol{c}}{\sqrt{2(1 + d)}}
$$

スカラー部は，三角関数の半角公式を用いると簡単になる.

$$
q_{s}
=\cos{\left(\frac{\theta}{2} \right)}
=\sqrt{\frac{1 + \cos{\theta}}{2}}
=\sqrt{\frac{1 + d}{2}}
$$

平方根(コード上の sqrt)の呼び出しを減らすため，平方根内に $\frac{2}{2}$ を掛けると次のように変形することができる.

$$
q_{s}
=\sqrt{\frac{1 + d}{2}}
=\sqrt{\frac{2}{2} \frac{1 + d}{2}}
=\frac{\sqrt{2(1 + d)}}{2}
$$

### 残存する不安定性条件

もし $\hat{\boldsymbol{a}}=-\hat{\boldsymbol{b}}$ なら回転軸は無限に存在し，一意に決定することが出来なくなる．この場合，内積 $d=-1$ になるのでゼロ除算が発生し，数値的不安定性が生じる (※ Melax [2000] では対象オブジェクトをターゲット方向に向ける目的で利用されるため，このケースに遭遇することは少なく，その回避処理は省略されている).

### 実装例

```glsl
vec4 rotation_arc(in vec3 a, in vec3 b){
    vec3 c = cross(a, b);
    float d = dot(a, b);
    float s = sqrt(2. * (1. + d));
    
    return vec4(c / s, s / 2.);
}
```

## References

- Frisvad, J. R. (2012). Building an Orthonormal Basis from a 3D Unit Vector Without Normalization. Journal of Graphics Tools, 16(3), 151–159. <https://doi.org/10.1080/2165347X.2012.689606>
- Frisvad, J. R. (2014). *Combing hair and sampling directions* [Presentation slides]. Extra slide added October 2018. <https://people.compute.dtu.dk/jerf/code/hairy/HairAndDirections.pdf>
- Max, N. (2017). Improved accuracy when building an orthonormal basis. *Journal of Computer Graphics Techniques (JCGT)*, 6(1), 9–16. <http://jcgt.org/published/0006/01/02/>
- Melax, S. (2000). The shortest arc quaternion. In M. Deloura (Ed.), *Game programming gems* (pp. 214–218). Charles River Media.
