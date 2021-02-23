---
title: Chapter 2. 타원곡선
date: 2021-02-22 01:00:00 +07:00
tags: [python, bitcoin, blockchain]
description: Studying bitcoin
---

### 타원곡선의 정의

타원곡선은 다음과 같은 식으로 나타낸다.

&nbsp;&nbsp;&nbsp; y<sup>2</sup> = x<sup>3</sup> + ax + b

타원곡선은 y<sup>2</sup> 항으로 인해 x축에 대칭이고 우리가 아는 3차곡선보다 기울기가 완만한 형태가 된다.

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="연속 타원곡선">
<figcaption>[그림 2.4] 연속 타원곡선</figcaption>
</figure>

아래와 같이 계숫값에 따라 곡선이 하나로 이어지지 않고 분리되기도 한다.

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="이어지지 않는 타원곡선">
<figcaption>[그림 2.5] 이어지지 않는 타원곡선</figcaption>
</figure>

우리가 배울 비트코인에서 사용되는 타원곡선은 **secp256k1**이라고 하며 아래와 같이 계숫값이 a = 0, b = 7인 곡선으로 정의된다.

&nbsp;&nbsp;&nbsp; y<sup>2</sup> = x<sup>3</sup> + 7

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="secp256k1">
<figcaption>[그림 2.9] secp256k1</figcaption>
</figure>

**secp256k1이란?**

secp256k1 란 이름은, sec(Standard for Efficient Cryptography) + p(Parameter p over Fp) + 256(Field Size p 의 bit수) + k(Koblitz curve 변형) + 1(sequence number) 로 구성되어 있다고 한다. 비트코인 시스템 외 다른 분야에선 secp256r1 처럼, k 대신 r(Random Parameter)를 사용하는 Curve를 사용한다고 한다.

### 두 점의 덧셈

점 덧셈은 곡선 위의 두 점에 대해 어떤 연산을 거쳐 곡선에 존재하는 제 3의 점을 얻는 과정이다. [그림 2.14]와 같이 두 점 A와 B를 지나는 직선이 타원과 만나는 교점을 x축으로 대칭시킨 점을 A+B로 정의한다.

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="두 점의 덧셈">
<figcaption>[그림 2.14] 두 점의 덧셈</figcaption>
</figure>

[그림 2.14]에서 확인할 수 있듯이 타원곡선 위의 두 점 A, B에 대해 A+B를 다음과 같이 구한다.

- 두 점 A와 B를 지나가는 직선이 타원곡선과 새롭게 만나는 점 C를 찾는다.
- 그 점과 x축에 대해 대칭인 점이 덧셈의 결과 A+B이다.

점 덧셈의 결과를 쉽게 예측할 수 없다는 것이 앞으로 사용할 중요한 성질 중 하나이다.

### 점 덧셈 성질

점 덧셈은 일반 덧셈 연산과 유사한 몇 가지 성질을 만족한다.

1. 항등원 존재
2. 교환법칙 성립
3. 결합법칙 성립
4. 역원 존재

여기서 항등원은 대수의 0과 같은 의미의 점이 존재한다는 뜻이다. 즉 곡선 위의 I라는 점이 존재하여 어떤 점 A와 더한 결과 역시 점 A가 된다.

&nbsp;&nbsp;&nbsp; I + A = A

여기서 점 I를 **무한원점**이라고 한다. 무한원점은 덧셈에 대한 역원과 관련이 있는데, 어떤 점 A에 대해 -A가 존재하고 그 합이 항등원이 된다는 것이다.

&nbsp;&nbsp;&nbsp; A + (-A) = I

위 그래프에서 x축과 수직인 직선이 타원곡선과 두 점에서만 만나므로(한 점에서 접하는 경우 제외) 세 번째 점이 영원히 만나지 않는 것을 확인할 수 있다. 따라서 무한대에 있다고 상상할 수 있는 것이다.

교환법칙은 A + B = B + A를 만족한다는 것인데 타원곡선의 점 덧셈에서 교환법칙이 성립하는 것은 자명하다. 그래프에서도 확인할 수 있듯 A와 B를 지나는 직선은 순서를 바꾸어도 곡선과 동일한 위치에서 만나기 때문이다.

결합법칙 또한 성립하는데 [그림 2.16]과 [그림 2.17]을 통해 직관적으로 확인하여 보자.

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="결합법칙 1">
<figcaption>(A + B) + C</figcaption>
</figure>

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="결합법칙 2">
<figcaption>A + (B + C)</figcaption>
</figure>

### x<sub>1</sub> ≠ x<sub>2</sub>인 경우의 점 덧셈

방금까지는 x축에 수직인 직선을 다루었고 이제는 두 점의 x좌표가 다른 경우를 생각해보자. 

&nbsp;&nbsp;&nbsp; <em>P</em><sub>1</sub> = (x<sub>1</sub>, y<sub>1</sub>), <em>P</em><sub>2</sub> = (x<sub>2</sub>, y<sub>2</sub>), <em>P</em><sub>3</sub> = (x<sub>3</sub>, y<sub>3</sub>)<br>
&nbsp;&nbsp;&nbsp; <em>P</em><sub>1</sub> + <em>P</em><sub>2</sub> = <em>P</em><sub>3</sub><br>
&nbsp;&nbsp;&nbsp; s = (y<sub>2</sub> - y<sub>1</sub>) / (x<sub>2</sub> - x<sub>1</sub>) (s는 기울기)

점 <em>P</em><sub>1</sub>, <em>P</em><sub>2</sub>, <em>P</em><sub>3</sub>에 대해 계산을 해보면 다음과 같다. (점 덧셈 공식 유도 과정은 생략)

&nbsp;&nbsp;&nbsp; x<sub>3</sub> = s<sup>2</sup> - x<sub>1</sub> - x<sub>2</sub><br>
&nbsp;&nbsp;&nbsp; y<sub>3</sub> = s(x<sub>1</sub> - x<sub>3</sub>) - y<sub>1</sub> (y<sub>3</sub>값은 x축에 대칭되어 유도된 값과 부호가 반대가 됨)

### P<sub>1</sub> = P<sub>2</sub>인 경우의 점 덧셈

P<sub>1</sub> = P<sub>2</sub>인 경우(x축에 수직인 경우 제외) 해당 점에서 점 덧셈을 하기 위해 선을 그으면 그 직선은 타원곡선에 대한 접선이 된다. 이 접선이 타원곡선의 다른 부분에서 만나는 교점을 계산하면 다음과 같다.

&nbsp;&nbsp;&nbsp; x<sub>3</sub> = s<sup>2</sup> - 2x<sub>1</sub><br>
&nbsp;&nbsp;&nbsp; y<sub>3</sub> = s(x<sub>1</sub> - x<sub>3</sub>) - y<sub>1</sub>

### P<sub>1</sub> = P<sub>2</sub>이면서 y 좌표가 0인 경우의 점 덧셈

<figure>
<img src="/programming-bitcoin-2/~~~.png" alt="타원곡선의 접선이면서 x축에 수직인 직선">
<figcaption>타원곡선의 접선이면서 x축에 수직인 직선</figcaption>
</figure>

이 경우는 두 점의 y 좌표가 0이기 때문에 기울기 계산 공식을 사용하면 분모가 0이므로 계산 오류가 발생한다. 위 그림에서 확인할 수 있듯이 이 경우에서는 결과값이 무한원점이 된다.