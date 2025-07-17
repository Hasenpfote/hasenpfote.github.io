---
title: "プリュッカー座標の考察"
description: ""
date: 2021-02-19T09:00:00+09:00
categories:
  - math
  - plückercoordinates
  - cpp
draft: false
---

## 概要

[プリュッカー座標](https://en.wikipedia.org/wiki/Pl%C3%BCcker_coordinates)についての備忘録．

大昔に [John Carmack](https://en.wikipedia.org/wiki/John_Carmack) が [Quake](https://en.wikipedia.org/wiki/Quake_(video_game)) の衝突判定に利用した記憶がある．

## 用途

例えば

- 衝突判定
- [レイトレーシング](https://en.wikipedia.org/wiki/Ray_tracing_(graphics))
- [双対四元数](https://en.wikipedia.org/wiki/Dual_quaternion)
- [スクリュー理論](https://en.wikipedia.org/wiki/Screw_theory)の土台
- [ロボティクス](https://en.wikipedia.org/wiki/Robotics)

## 考察

追記予定．

### 光線と三角形の交差判定

**reciprocal product** は

$$
\{l_{1}:m_{1}\}*\{l_{2}:m_{2}\}
\equiv l_{1} \cdot m_{2} + l_{2} \cdot m_{1}
$$

三角形内に交点があるかを見る程度なら **reciprocal product** を最大三回実行すればよい．また直線の正規化なども不要．デモでは光線のプリュッカー座標と三角形(CCWを表面とした)の交差判定を行っている．$P$ または $Q$ をドラッグし光線の方向を変えることで挙動が確認できる．

[geogebra demo](https://www.geogebra.org/m/et7ukcbq)

## A C++ Library

- [https://github.com/Hasenpfote/plucker](https://github.com/Hasenpfote/plucker)

  **Shoemake** をベースに **Mason** で実装．

## 参考文献

1. [Ken Shoemake 1998 Plücker Coordinate Tutorial](http://www.realtimerendering.com/resources/RTNews/html/rtnv11n1.html#art3)
2. [Matthew T. Mason - Mechanics of Manipulation](http://www.cs.cmu.edu/afs/cs/academic/class/16741-s07/www/index.html)
3. [Yan-Bin Jia 2020 Plücker Coordinates for Lines in the Space∗](http://web.cs.iastate.edu/~cs577/handouts/plucker-coordinates.pdf)
4. [Bartholomew Randall 2013 Plücker Coordinate of a Line in 3-Space](https://slideplayer.com/slide/6904983/)
