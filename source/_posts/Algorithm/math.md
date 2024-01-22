---
title: 数学知识
date: 2023-2-10 13:33:33
description: 数学知识整理
tags: [Algorithm]
categories: Algorithm
---
# 数学知识
## 基础知识
### 整除
整除：如果一个整数a除以另一个整数b，商为整数，余数为0，则称a能被b整除，b能整除a，a是b的倍数，b是a的因数，记作b|a。
一个正整数n的因数最多有O($\sqrt{n}$)个，更准确的说法是，n的因数的个数不超过2$\sqrt{n}$。

### 最大公约数
最大公约数（Greatest Common Divisor，GCD）：两个或多个整数公有的约数中最大的一个，记作gcd(a,b)。
### 最小公倍数
最小公倍数（Least Common Multiple，LCM）：两个或多个整数公有的倍数中最小的一个，记作lcm(a,b)。
### 最大公约数与最小公倍数的关系
gcd(a,b) * lcm(a,b) = a * b
gcd(k * a, k * b) = k * gcd(a,b)
gcd(a,b) = gcd(a-b,b) = gcd(a,b-a)
lcm(k * a, k * b) = k * lcm(a,b)
gcd(k,a,b) = gcd(gcd(a,b),k)
lcm(k,a,b) = lcm(lcm(a,b),k)
k|a && k|b <=> k|gcd(a,b)
a|k && b|k <=> lcm(a,b)|k
### 贝祖定理
贝祖定理（Bézout's identity）：对于任意整数a、b，存在整数x、y，使得ax+by=c，当且仅当gcd(a,b)|c时，方程有解。
### 素数与合数
素数（Prime Number）：大于1的整数，除了1和它本身以外，不能被其他正整数整除的数。
合数（Composite Number）：大于1的整数，除了1和它本身以外，还能被其他正整数整除的数。
1既不是素数也不是合数。
如果素数p满足p|ab，则p|a或p|b。
### 算数基本定理
算数基本定理（Fundamental Theorem of Arithmetic）：任何一个大于1的整数，要么本身就是一个素数，要么可以写成一系列素数的乘积，而且这些素数按大小排列之后，写法仅有一种方式。
### 素数定理
素数定理（Prime Number Theorem）：对于不超过x的素数个数π(x)，有π(x) ~ $\frac{x}{ln(x)}$。
### 模与同余
模（Modulus）：如果a除以b，商为整数，余数为c，则称a对b取模为c，记作a mod b = c。
同余（Congruence）：如果a mod m = b mod m，则称a与b对模m同余，记作a ≡ b (mod m)。
同余关系具有如下性质：
a ≡ b (mod m) => a+c ≡ b+c (mod m), a-c ≡ b-c (mod m), ac ≡ bc (mod m)
a ≡ b (mod m) && c ≡ d (mod m) => a+c ≡ b+d (mod m), a-c ≡ b-d (mod m), ac ≡ bd (mod m)
费马小定理（Fermat's little theorem）：如果p是素数，a是任意整数，则 **$a^p$ ≡ a (mod p)**，即 **$a^{p-1}$ ≡ 1 (mod p)**。
威尔逊定理（Wilson's theorem）：如果p是素数，则 **(p-1)! ≡ -1 (mod p)**。
逆元：在模p(p≥2)意义下，如果a与p互质，则存在整数b，使得ab ≡ 1 (mod p)，则称b是a的逆元，记作 **b = a<sup>-1</sup>**。
若在模p意义下n存在逆元n<sup>-1</sup>，定义 **$\frac{m}{n}$ ≡ mn<sup>-1</sup> (mod p)**。
