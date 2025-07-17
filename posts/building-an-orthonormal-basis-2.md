---
title: "正規直交基底の構築 (2)"
description: "Pixar の方法について考察をする"
categories:
  - computerscience
  - onb
  - orthonormal
  - basis
date: 2025-01-13T21:00:00+09:00
draft: false
---

## Pixar の方法

<div class="quarto-figure quarto-figure-center">
<figure class="figure">
<iframe src="https://www.desmos.com/3d/0kr0tuvllz"></iframe>
</figure>
</div>

Duff et al. [2017] では，Frisvad の方法の問題点と改善案を提示している．特異点付近で大きな誤差が生じることから，行列式が負になることもあり，誤った座標フレームが使用される．$n_{z}$ が $-1$ に近くない場合は適切に動作することから，$n_{z}\lt 0$ の場合は $-n$ のフレームを計算し，その結果を反転することで対処している．  

Frisvad の方法では，基底は次のように定義される.

$$
\begin{align*}
\vec{b}_{1}
&=
\left(
1 - \frac{n^{2}_{x}}{1 + n_{z}}, - \frac{n_{x} n_{y}}{1 + n_{z}}, - n_{x}
\right)^{\mathsf{T}}\\
\vec{b}_{2}
&=
\left(
- \frac{n_{x} n_{y}}{1 + n_{z}}, 1 - \frac{n^{2}_{y}}{1 + n_{z}}, - n_{y}
\right)^{\mathsf{T}}\\
\vec{n}
&=
\left(
n_{x}, n_{y}, n_{z}
\right)^{\mathsf{T}}
\end{align*}
$$

$n_{z}\lt 0$ の場合は，$\vec{b}_{1}, \vec{b}_{2}$ に $-\vec{n}$ を代入し，$\vec{b}_{2}$ を反転すると次のようになる.

$$
\begin{align*}
\vec{b}_{1}
&=
\left(
1 - \frac{n^{2}_{x}}{1 - n_{z}}, - \frac{n_{x} n_{y}}{1 - n_{z}}, n_{x}
\right)^{\mathsf{T}}\\
\vec{b}_{2}
&=
\left(
\frac{n_{x} n_{y}}{1 - n_{z}}, \frac{n^{2}_{y}}{1 - n_{z}} - 1, - n_{y}
\right)^{\mathsf{T}}\\
\vec{n}
&=
\left(
n_{x}, n_{y}, n_{z}
\right)^{\mathsf{T}}
\end{align*}
$$

::: {.callout-note}
四元数を基礎として得られた接ベクトルの方向の一貫性を失う.
:::

実装は次のようになる.

```cpp
// Listing 2. The code of Listing 1, revised to avoid catastrophic cancellation.
void revisedONB(const Vec3f &n, Vec3f &b1, Vec3f &b2)
{
    if(n.z<0.){
        const float a = 1.0f / (1.0f - n.z);
        const float b = n.x * n.y * a;
        b1 = Vec3f(1.0f - n.x * n.x * a, -b, n.x);
        b2 = Vec3f(b, n.y * n.y*a - 1.0f, -n.y);
    }
    else{
        const float a = 1.0f / (1.0f + n.z);
        const float b = -n.x * n.y * a;
        b1 = Vec3f(1.0f - n.x * n.x * a, b, -n.x);
        b2 = Vec3f(b, 1.0f - n.y * n.y * a, -n.y);
    }
}
```

```cpp
// Listing 3. A branchless version of Listing 2, using copysignf to eliminate the test.
void branchlessONB(const Vec3f &n, Vec3f &b1, Vec3f &b2)
{
    float sign = copysignf(1.0f, n.z); // n.z>=0.0f ? 1.0f : -1.0f
    const float a = -1.0f / (sign + n.z);
    const float b = n.x * n.y * a;
    b1 = Vec3f(1.0f + sign * n.x * n.x * a, sign * b, -sign * n.x);
    b2 = Vec3f(b, sign + n.y * n.y * a, -n.y);
}
```

[Comparing ONB Accuracy](https://www.shadertoy.com/view/M3KcR3) で違いを確認するには，次の箇所を変更しコンパイルする.

```glsl
/**
 * 0: frisvad vs max
 * 1: frisvad vs pixar
 * 2: max vs pixar
 */
#define COMPARISON_MODE 1
```

### Pros and Cons

- Pros
  - Frisvad の方法と遜色のない速度を維持する
  - 特異点がなく平均誤差も小さい
- Cons
  - 四元数を基礎として得られた接ベクトルの方向の一貫性を失う  
    接空間の $z$ 軸に対称な分布に適用するような場合は問題ない

## References

- Duff, T., Burgess, J., Christensen, P., Hery, C., Kensler, A., Liani, M., & Villemin, R. (2017). Building an orthonormal basis, revisited. *Journal of Computer Graphics Techniques (JCGT)*, 6(1), 1–8. <http://jcgt.org/published/0006/01/01/>
