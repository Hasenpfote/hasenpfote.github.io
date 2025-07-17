---
title: "剛体シム覚書"
description: "剛体シムの最初の最初"
categories:
  - physics
  - rigidbody
  - python
date: 2021-06-09T09:00:00+09:00
draft: false
---

## 慣性テンソル

慣性テンソル $\boldsymbol{I}$ は次のような対称行列である．

$$
\begin{aligned}
\boldsymbol{I}
&=
\left(
\begin{array}{ccc}
 I_{xx} & I_{xy} & I_{xz}\\
 I_{yx} & I_{yy} & I_{yz}\\
 I_{zx} & I_{zy} & I_{zz}
\end{array}
\right)
=\int \rho d\boldsymbol{V}
\left(
\begin{array}{ccc}
 y^2 + z^2 & -xy & -xz\\
 -xy & z^2 + x^2 & -yz\\
 -xz & -yz & x^2 + y^2
\end{array}
\right)
\end{aligned}
$$

対角項を慣性モーメント，非対角項を慣性乗積という．対称行列なので，ある直交行列 $\boldsymbol{O}$ で対角化をすれば慣性乗積をすべて0にできる．対角化された慣性テンソル $\boldsymbol{I}_{I}$ は

$$
\begin{aligned}
\boldsymbol{I}_{I}
&=\boldsymbol{O}^{-1} \boldsymbol{I} \boldsymbol{O}
=
\left(
\begin{array}{ccc}
 I_{1} & 0 & 0\\
 0 & I_{2} & 0\\
    0 & 0 & I_{3}
\end{array}
\right)
\end{aligned}
$$

対角成分を主慣性モーメント，それが成立する座標系を慣性主軸という．回転対称性のない一般の剛体の場合，慣性主軸を求めることは固有値問題に帰着する．

$$
\boldsymbol{I} \boldsymbol{\omega}
=\lambda \boldsymbol{\omega}
$$

上式を満たす $\boldsymbol{\omega}$ の方向は，行列 $\boldsymbol{I}$ の固有ベクトルの方向と一致する．固有値 $\lambda_{i}$ と固有ベクトル $\boldsymbol{\omega}_{i}$ より

$$
\begin{aligned}
\left(
\begin{array}{c}
 I_{1}\\
 I_{2}\\
 I_{3}
\end{array}
\right)
&=
\left(
\begin{array}{c}
 \lambda_{1}\\
 \lambda_{2}\\
 \lambda_{3}
\end{array}
\right)\\
\\
\boldsymbol{O}
&=
\left(
\begin{array}{ccc}
 \frac{\boldsymbol{\omega}_{0}}{\|\boldsymbol{\omega}_{0}\|} &
 \frac{\boldsymbol{\omega}_{1}}{\|\boldsymbol{\omega}_{1}\|} &
 \frac{\boldsymbol{\omega}_{2}}{\|\boldsymbol{\omega}_{2}\|}\\
\end{array}
\right)
\end{aligned}
$$

主慣性モーメント $I_{i}$ は，それに対応する慣性主軸($I_{i}$ に対応する固有ベクトル $\boldsymbol{\omega}_{i}$ の方向) の周りの慣性モーメントである．

> 回転対称性のある剛体は慣性テンソルが対角行列になっているので直交行列 $\boldsymbol{O}$ は単位行列．
>
> cf. [色々な物体の慣性モーメント１](http://hooktail.sub.jp/mechanics/inertiaTable1/)

## 座標系

座標系は [慣性主軸] → [ローカル座標] → [ワールド座標] とする．角運動量 $\boldsymbol{L}$ と角速度 $\boldsymbol{\omega}$ の関係式より

- 慣性主軸

  $$
  \begin{aligned}
  \boldsymbol{L}_{I}
  &=\boldsymbol{I}_{I} \boldsymbol{\omega}_{I}\\
  &=(\boldsymbol{O}^{-1} \boldsymbol{I} \boldsymbol{O}) \boldsymbol{\omega}_{I}\\
  \end{aligned}
  $$

- ローカル座標

  $$
  \begin{aligned}
  \boldsymbol{L}_{local}
  &=\boldsymbol{I}_{local} \, \boldsymbol{\omega}_{local}\\
  \boldsymbol{O} \boldsymbol{L}_{I}
  &=\boldsymbol{O} \boldsymbol{I}_{I} \boldsymbol{\omega}_{I}\\
  &=(\boldsymbol{O} \boldsymbol{I}_{I} \boldsymbol{O}^{-1}) \boldsymbol{O} \boldsymbol{\omega}_{I}\\
  &=\boldsymbol{I} \, \boldsymbol{O} \boldsymbol{\omega}_{I}
  \end{aligned}
  $$

- ワールド座標

  $$
  \begin{aligned}
  \boldsymbol{L}_{world}
  &=\boldsymbol{I}_{world} \, \boldsymbol{\omega}_{world}\\
  \boldsymbol{R} \boldsymbol{L}_{local}
  &=\boldsymbol{R} \boldsymbol{I}_{local} \, \boldsymbol{\omega}_{local}\\
  &=(\boldsymbol{R} \boldsymbol{I}_{local} \boldsymbol{R}^{-1}) \, \boldsymbol{R} \boldsymbol{\omega}_{local}\\
  &=(\boldsymbol{R} (\boldsymbol{O} \boldsymbol{I}_{I} \boldsymbol{O}^{-1}) \boldsymbol{R}^{-1}) \, \boldsymbol{R} \boldsymbol{\omega}_{local}\\
  &=(\boldsymbol{R} \boldsymbol{I} \boldsymbol{R}^{-1}) \, \boldsymbol{R} \boldsymbol{\omega}_{local}
  \end{aligned}
  $$

## 力の適用

剛体の重心へ作用し必要に応じて何度でも適用する．

### 並進運動

$$
\begin{aligned}
\begin{array}{l|l}
 \text{パラメータ} & \text{解説}\\ \hline
    \boldsymbol{x} \in \mathbb{R}^{3} & \text{位置}\\
    \boldsymbol{v} \in \mathbb{R}^{3} & \text{速度}\\
    m  \in \mathbb{R} & \text{質量}\\
    \boldsymbol{F} \in \mathbb{R}^{3} & \text{力}\\
    \boldsymbol{J} \in \mathbb{R}^{3} & \text{力積}\\
    \Delta t \in \mathbb{R} & \text{タイムステップ}
\end{array}
\end{aligned}
$$

#### 力を適用する場合

$$
\begin{aligned}
\boldsymbol{v}_{t+1}
&=\boldsymbol{v}_{t} + \frac{\boldsymbol{F}}{m} \Delta t
\end{aligned}
$$

#### 力積を適用する場合

$$
\begin{aligned}
\boldsymbol{v}_{t+1}
&=\boldsymbol{v}_{t} + \frac{\boldsymbol{J}}{m}
\end{aligned}
$$

> 撃力は運動量 $\boldsymbol{p}(t)=m\boldsymbol{v}(t)$ を用いて
>
> $$
> \begin{aligned}
> \int^{t + \Delta t}_{t} \frac{d \boldsymbol{p}(t)}{dt} dt
> &=\int^{t + \Delta t}_{t} \boldsymbol{F}(t) dt\\
> \\
> \left[\boldsymbol{p}(t) \right]^{t + \Delta t}_{t}
> &\simeq \boldsymbol{F}(t) \Delta t\\
> \\
> \underbrace{m \Delta \boldsymbol{v}}_{Change \hspace{2pt} in \hspace{2pt} momentum}
> &\simeq \underbrace{\boldsymbol{F}(t) \Delta t}_{impulse}
> \end{aligned}
> $$

### 回転運動

$$
\begin{aligned}
\begin{array}{l|l}
 \text{パラメータ} & \text{解説}\\ \hline
    \boldsymbol{q}_{r} \in \mathbb{H}_{1} & \text{剛体の回転}\\
    \boldsymbol{I} \in \mathbb{R}^{3\times 3} & \text{慣性テンソル}\\
    \boldsymbol{I}_{world} \in \mathbb{R}^{3\times 3} & \text{慣性テンソル}\\
    \boldsymbol{\omega} \in \mathbb{R}^{3} & \text{角速度}\\
    \boldsymbol{L} \in \mathbb{R}^{3} & \text{角運動量}\\
    \boldsymbol{\tau} \in \mathbb{R}^{3} & \text{トルク}\\
    \boldsymbol{H} \in \mathbb{R}^{3} & \text{力積}\\
    \Delta t \in \mathbb{R} & \text{タイムステップ}
\end{array}
\end{aligned}
$$

#### トルクを適用する場合

$$
\begin{aligned}
\boldsymbol{R}_{t}
&=({\boldsymbol{q}_{r}}_{t}) \text{ as matrix}\\
\\
\boldsymbol{I}_{world}
&=\boldsymbol{R}_{t} \boldsymbol{I} \boldsymbol{R}_{t}^{-1}\\
\\
\boldsymbol{I}^{-1}_{world}
&=\boldsymbol{R}_{t} \boldsymbol{I}^{-1} \boldsymbol{R}_{t}^{-1}\\
\\
\boldsymbol{L}_{t}
&=\boldsymbol{I}_{world} \boldsymbol{\omega}_{t}\\
\\
\boldsymbol{L}_{t+1}
&=\boldsymbol{L}_{t} + \boldsymbol{\tau}_{t} \Delta t\\
\\
\boldsymbol{\omega}_{t+1}
&=\boldsymbol{I}^{-1}_{world} \boldsymbol{L}_{t+1}\\
\end{aligned}
$$

#### 力積を適用する場合

$$
\begin{aligned}
\boldsymbol{R}_{t}
&=({\boldsymbol{q}_{r}}_{t}) \text{ as matrix}\\
\\
\boldsymbol{I}_{world}
&=\boldsymbol{R}_{t} \boldsymbol{I} \boldsymbol{R}_{t}^{-1}\\
\\
\boldsymbol{I}^{-1}_{world}
&=\boldsymbol{R}_{t} \boldsymbol{I}^{-1} \boldsymbol{R}_{t}^{-1}\\
\\
\boldsymbol{L}_{t}
&=\boldsymbol{I}_{world} \boldsymbol{\omega}_{t}\\
\\
\boldsymbol{L}_{t+1}
&=\boldsymbol{L}_{t} + \boldsymbol{H}_{t}\\
\\
\boldsymbol{\omega}_{t+1}
&=\boldsymbol{I}^{-1}_{world} \boldsymbol{L}_{t+1}\\
\end{aligned}
$$

> 撃力は角運動量 $\boldsymbol{L}$ を用いて
>
> $$
> \begin{aligned}
> \int^{t + \Delta t}_{t} \frac{d \boldsymbol{L}(t)}{dt} dt
> &=\int^{t + \Delta t}_{t} \boldsymbol{\tau}(t) dt\\
> \\
> \left[\boldsymbol{L}(t) \right]^{t + \Delta t}_{t}
> &\simeq \boldsymbol{\tau}(t) \Delta t\\
> \\
> \underbrace{\Delta \boldsymbol{L}}_{Change \hspace{2pt} in \hspace{2pt} momentum}
> &\simeq \underbrace{\boldsymbol{\tau}(t) \Delta t}_{impulse}
> \end{aligned}
> $$
>
> または
>
> $$
> \begin{aligned}
> \int^{t + \Delta t}_{t} \frac{d \boldsymbol{L}(t)}{dt} dt
> &=\boldsymbol{r} \times \int^{t + \Delta t}_{t} \boldsymbol{F}(t) dt\\
> \\
> \left[\boldsymbol{L}(t) \right]^{t + \Delta t}_{t}
> &\simeq \boldsymbol{r} \times \boldsymbol{F}(t) \Delta t\\
> \\
> \underbrace{\Delta \boldsymbol{L}}_{Change \hspace{2pt} in \hspace{2pt} momentum}
> &\simeq \underbrace{\boldsymbol{r} \times \boldsymbol{F}(t) \Delta t}_{impulse}
> \end{aligned}
> $$

### 合成運動

剛体上のある位置へ力や撃力を適用する場合について．剛体の中心(重心)から剛体上のある位置 $\boldsymbol{p}$ へのベクトルを $\boldsymbol{r}=\boldsymbol{p}\ - \boldsymbol{x}$ とする．

#### 力を適用する場合

1. 並進運動を行う
2. $\boldsymbol{\tau}=\boldsymbol{r} \times \boldsymbol{F}$ として回転運動を行う

#### 力積を適用する場合

1. 並進運動を行う
2. $\boldsymbol{H}=\boldsymbol{r} \times \boldsymbol{J}$ として回転運動を行う

## 更新

最終的な速度や角速度を用い，位置や姿勢を更新する．

### 並進運動

$$
\begin{aligned}
\boldsymbol{x}_{t+1}
&=\boldsymbol{x}_{t} + \boldsymbol{v}_{t+1} \Delta t
\end{aligned}
$$

### 回転運動

$$
\begin{aligned}
{\boldsymbol{q}_{r}}_{t+1}
&=\frac{{\boldsymbol{q}_{r}}_{t} + \dot{\boldsymbol{q}_{r}}_{t+1} \Delta t}{\|{\boldsymbol{q}_{r}}_{t} + \dot{\boldsymbol{q}_{r}}_{t+1} \Delta t\|}
\end{aligned}
$$

> 四元数の時間微分について
>
> $$
> \begin{aligned}
> \dot{\boldsymbol{q}}(t)
> &=\frac{d}{dt} \boldsymbol{q}(t)\\
> &=\frac{1}{2} \left\{\omega_{x}(t) \boldsymbol{i} + \omega_{y}(t) \boldsymbol{j} + \omega_{z}(t) \boldsymbol{k}\right\} \otimes \boldsymbol{q}(t)
> \end{aligned}
> $$
>
> オイラー法による時間積分
>
> $$
> \begin{aligned}
> \boldsymbol{q}(t+\Delta t)
> &=\frac{\boldsymbol{q}(t) + \dot{\boldsymbol{q}}(t) \Delta t}{\|\boldsymbol{q}(t) + \dot{\boldsymbol{q}}(t) \Delta t\|}
> \end{aligned}
> $$
>
> 大きさが安定しないので正規化が必須．

## 回転運動のパラメータについて

対角化された慣性テンソルに着目する．主慣性モーメントをベクトルとして扱えばストレージサイズが 1/3 となる．それに伴い計算は四元数が主となる．

$$
\begin{aligned}
\begin{array}{c|l}
 \text{パラメータ} & \text{解説}\\ \hline
    \boldsymbol{I}_{d} \in \mathbb{R}^{3} & \text{主慣性モーメント}\\
    \boldsymbol{q}_{o} \in \mathbb{H}_{1} & \boldsymbol{O} \text{ を四元数化}\\
    \boldsymbol{q}_{r} \in \mathbb{H}_{1} & \boldsymbol{R} \text{ を四元数化}\\
\end{array}
\end{aligned}
$$

ただし

$$
\begin{aligned}
\boldsymbol{I}^{-1}_{d}
&\equiv
\left(
\begin{array}{c}
 \frac{1}{{I_{1}}}\\
 \frac{1}{{I_{2}}}\\
 \frac{1}{{I_{3}}}
\end{array}
\right)
\end{aligned}
$$

### トルクを適用する場合

$$
\begin{aligned}
\boldsymbol{q}_{t}
&={\boldsymbol{q}_{r}}_{t} \otimes {\boldsymbol{q}_{o}}_{t}\\
\boldsymbol{q}^{-1}_{t}
&={\boldsymbol{q}_{o}}^{-1}_{t} \otimes {\boldsymbol{q}_{r}}^{-1}_{t}\\
\\
\boldsymbol{L}_{t}
&=\boldsymbol{q}_{t} \diamond (\boldsymbol{I}_{d} \odot (\boldsymbol{q}^{-1}_{t} \diamond \boldsymbol{\omega}_{t}))\\
\boldsymbol{L}_{t+1}
&=\boldsymbol{L}_{t} + \boldsymbol{\tau}_{t} \Delta t\\
\\
\boldsymbol{\omega}_{t+1}
&=\boldsymbol{q}_{t} \diamond \left(\boldsymbol{I}^{-1}_{d} \odot (\boldsymbol{q}^{-1}_{t} \diamond \boldsymbol{L}_{t+1}) \right)
\end{aligned}
$$

### 力積を適用する場合

$$
\begin{aligned}
\boldsymbol{q}_{t}
&={\boldsymbol{q}_{r}}_{t} \otimes {\boldsymbol{q}_{o}}_{t}\\
\boldsymbol{q}^{-1}_{t}
&={\boldsymbol{q}_{o}}^{-1}_{t} \otimes {\boldsymbol{q}_{r}}^{-1}_{t}\\
\\
\boldsymbol{L}_{t}
&=\boldsymbol{q}_{t} \diamond (\boldsymbol{I}_{d} \odot (\boldsymbol{q}^{-1}_{t} \diamond \boldsymbol{\omega}_{t}))\\
\boldsymbol{L}_{t+1}
&=\boldsymbol{L}_{t} + \boldsymbol{H}_{t}\\
\\
\boldsymbol{\omega}_{t+1}
&=\boldsymbol{q}_{t} \diamond \left(\boldsymbol{I}^{-1}_{d} \odot (\boldsymbol{q}^{-1}_{t} \diamond \boldsymbol{L}_{t+1}) \right)
\end{aligned}
$$

## 記号について

$$
\begin{aligned}
\begin{array}{c|l}
 \text{記号} & \text{解説}\\ \hline
    \odot & \text{アダマール積}\\
    \otimes & \text{四元数の積}\\
    \diamond & \text{四元数によるベクトルの回転: } \boldsymbol{p}^{'} = \boldsymbol{q} \boldsymbol{p} \boldsymbol{q}^{-1}\\
\end{array}
\end{aligned}
$$

## Python による実装例

- 基本的な実装例

  [rigid-body-dynamics.ipynb](https://gist.github.com/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-ipynb)

- 「回転運動のパラメータについて」で考察した実装例

  [rigid-body-dynamics-2.ipynb](https://gist.github.com/Hasenpfote/034cc499827da562fed3c8f676b868f0#file-rigid-body-dynamics-2-ipynb)
