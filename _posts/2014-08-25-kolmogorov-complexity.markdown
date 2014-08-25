---
layout: post
title: "Kolmogorov复杂性"
date: 2014-08-25 22:38:55 +1200
comments: true
categories: [computation theory, Godel]
---
原文-[wiki](http://en.wikipedia.org/wiki/Kolmogorov_complexity)

看Kolmogorov复杂性看到云里雾里，于是干脆把wiki上的翻译了一下。<!--more-->

[toc]

Chaitin complexity, algorithmic entropy, program-size complexity

---

# 定义
Kolmogorov 复杂性可被定义到任意数学对象，为简化本文的范围，限制到字符串。必须首先为字符串指定一个描述语言。这种描述语言可以基于任意计算机编程语言。如果$P$是一个程序，输出字符串$x$，则$P$是$x$的一个 **描述** 。描述的长度只是$P$作为字母串的长度，再乘与每个字母的bit数。

另外，我们可以选择一个图灵机编码，这里 __编码__ 指一个把每个图灵机$M$关联到一个bit串$&lt;M&gt;$的函数。假如$M$是一个图灵机，对于输入$w$，输出字符串$x$，则字符串的联合$&lt;M&gt;w$则为$x$的一个 **描述** 。为了进行理论上的分析，这个方法更适合于构造详细的正式证明，且通常被研究性文本所采用。本文中，我们讨论一种非正式的方法。

任意串$s$至少有一个 **描述** ，即程序:

```groovy
function GenerateFixedString()
    return s
```

如果$s$的描述，$d(s)$，长度最小，即使用最少的bit数,即称 $d(s)$ 为 $s$ 的 __最小描述__ 。因此，$d(s)$ 的长度(即，描述中的bit数)即为 $s$ 的 __Kolmogorov__ 复杂性，记为 $K(s)$ 。用符号表示，

$
K(s)=|d(s)|
$


最小描述的长度取决于所选择的描述语言；但是改变描述语言的效果是有范围的(一个结果称之为 _invariance_ 定理)。

---

# Invariance 定理

## 非正式方法

从下面这些方面讲，存在一些描述语言是最优的：用某个描述语言得到的对某个对象的描述，可以将其连同一个固定的头信息放到最优描述语言中。头信息只取决于所用的语言，不受对象的描述影响，或者对象本身。
下面是最优描述语言的例子。一个描述由两个部分组成：

* 描述另一个描述语言的部分
* 用这种语言描述的对象部分

更技术性的术语中，第一个部分是一个计算机程序，第二个部分是能让程序输出此对象的输入。
Invariance定理如下：任意描述语言$L$,连同头信息，最优描述语言至少等于$L$的效率。


__证明：__ 

通过先把 $L$ 表述成一个计算机程序 $P$ (第一个部分)，然后用原描述D作为程序的输入(第二个部分)，可以把$L$中的任意一个描述$D$转换到最优语言。新的描述$D\prime$的长度为(近似地)：

$\|D\prime\|=\|P\| + \|D\|$

$P$的长度固定且不取决于$D$。因此，最多有一个固定的开销，且与被描述的对象无关。因此，最优语言一般最多是附加的固定开销。

## 更正式些的方法

__定理__： 

如果 $K1$ 和 $K2$ 是对于图灵完备的描述语言 $L1$ 和 $L2$ 的复杂性函数,那么有一个常量 $c$ -- $c$ 只取决于选择 $L1$ 还是 $L2$ -- 使得

$ \forall s. - c \leq K1(s) - K2(s) \leq c. $

__证明：__

根据对称性，有某个常量$c$使得对于所有串$s$

$K1(s) \leq K2(s) + c$

假设有$L1$语言的程序作为$L2$的解释器：

```java
function InterpretLanguage(string p)
```

$p$ 为 $L2$ 语言的程序。解释器的特征由以下属性决定:

* 对输入 $p$ 运行 InterpreLanguage 返回 $p$ 的执行结果

因此，如果 $P$ 是一个 $L2$ 程序，是 $s$ 的最小描述，那么 $InterpreteLanguage(P)$ 返回串 $s$ 。$s$ 的描述的长度是：

1. InterpreLanguage程序的长度，即常量 $c$；

2. $P$ 的长度，即 $K2(s) $

以上二者之和。复杂性的上限得到证明。


---

# 历史与环境

算法信息理论是计算机科学的一个领域，研究对于字符串（或其他数据结构)的Kolmogorov复杂性和其他复杂性度量。

Kolmogorov复杂性的概念和理论基于一个关键的定理，此定理首先被Ray Solomonoff发现，于1960年发表，在_"A Preliminary Report on a General Theory of Inductive Inference"_ 中，作为他发明的 *算法概率* 的一部分得到描述。在1964年出版的 *"A Formal Theory of Inductive Inference"* 中，他给出了一个更完整的描述，在 *Information And Control* 的第一部分和第二部分。

Andery Kolmogorov后来在1965，*Proglems Inform. Transmission* 上独立发表了这个定理。Gregory Chaitin也在 *J.ACM* 上提出了他的定理 - Chaitin的论文于1966年10月提交并修改于1968年12月，引用了Solomonoff和Kolmogorov的论文。

定理表述，把字符串从其描述(编码)解码的算法当中，存在一个最优的算法。此算法，对于所有字符串，它可以使得编码可以与其他算法允许的编码一样短，到一个附加的取决于算法但并不取决于字符串自身的常量。Solomonoff用这个算法和其允许的编码长度，定义了一个字符串的_"普遍概率"_，字符串的后续数字的归纳推理可以基于此概率。Kolmogorov使用这个定理定义了几个字符串函数，包括，复杂性，随机性和信息。

Kolmogorov知道Solomonoff的工作时，他知道了Solomonoff 优先。几年来，比起在西方世界，Solomonoff的工作在苏联更多人知道。然而，科学界的一般共识是把这类复杂性归功于Kolmogorov，他关注序列的随机性，而算法概率归功于Solomonoff，Solomonoff 专注于用他发明的普遍优先概率分布去做预测。在更广泛的包含描述复杂性和概率的领域中，这个领域则通常称为Kolmogorov复杂性。

还有几个其他Kolmogorov复杂性或者算法信息的变体。应用最广泛的一个基于self-delimiting program，主要归功于 Leonid Levin。

对Kolmogorov复杂性的一个不证自明的方法基于 Blum 公理，由Mark Burgin在Andrey Kolmogorov发表的论文中引入。（TODO:??)

---

# 基本结论

下面的讨论中，$K(s)$是字符串$s$的复杂性。

不难看出，字符串的最小描述不能比它自身更长 -- 输出s的程序GenerateFixedString是一个固定的大于s的数量。

__定理：__

存在一个常量$c$，使得

$\forall s. K(s) \leq \|s\| + c$

##  Kolmogorov复杂性的不可计算

__定理：__

存在有任意大Kolmogorov复杂性的字符串。形式化表述：对于每个属于自然数的数$n$, 有一个字符串 $s$，$K(s) \geq n$。

__证明：__ 

否则所有无限多的可能的字符串可以由有限多的复杂性低于$n$ bit的程序产生。

__定理：__ 

K不是一个可计算函数。换句话说，没有程序可以接受字符串$s$作为输入，产生一个整数$K(s)$作为输出。

下面的非直接的证明使用了类似 _Pascal_ 的语言表示程序；为简化证明，假设其描述(即解释器)长度为1,400,000bit。为得到矛盾，假设有这样一个程序，

```java
function KolmogorovComplexity(string s)
```

接受$s$作为输入，返回$K(s)$；为简化证明，假设长度为7,000,000,000bit。思考长度为1,288bit的程序：

```java
function GenerateComplexString()
    for i = 1 to infinity:
        for each string s of length exactly i
            if KolmogorovComplexity(s) >= 8000000000
                return s
```

使用KolmogorovComplexity作为子程序，尝试每个字符串，由最短的开始，直到返回一个字符串，其Kolmogorov复杂性至少是8,000,000,000bit，即，字符串不能由任意短于8,000,000,000bit的程序产生。但是，上面产生$s$的程序长度仅仅是7,001,401,288 bit，得到矛盾。(如果KolmogorovComplexity的代码更短一些，矛盾仍然存在。更长一些的话，则GenerateComplexString的常量可以任意改变。)

上面的证明用了和Berry悖论一样的矛盾："The smallest positive integer that can not be defined in fewer than twenty English words"。通过从停机问题 $H$归约，同样可以用来显示$K$的不可计算性，因为$K$和$H$是图灵等价的。

有一个引理，在编程语言社区叫做"全雇佣定理"，说的是没有完美的对长度进行优化的编译器。

## Kolmogorov复杂性的链式法则

Klmogorov复杂性的链式法则：

$
K(X,Y) = K(X)+K(Y|X) + O(log(K(X,Y)))
$

表述产生$X$和$Y$的最短程序，比产生$X$的程序加上对给定的$X$产生$Y$的程序，不会多于对它所取的对数。

---

# 压缩

计算K(s)的上限是很直接的 - 只要用某个方法压缩字符串$s$,用特定语言实现相应的解压缩程序，然后把解压缩程序与压缩后的字符串连接起来，再测量其长度即可。

$c$ 是一个整数，如果对字符串 $s$，存在一个描述，其长度不超过  $ \|s\|-c $ bit，则 $s$ 对于 $c$ 是 **可压缩** 的。否则，$s$ 对于 $c$ 是不可压缩的。不能被1压缩的字符串简单地称为不可压缩 - 根据鸽巢原理，鸽巢原理所以适用是因为每个被压缩串只映射到一个未压缩串，不可压缩串一定存在，因为长度为$n$的，$2^n$bit的串只有$2^n-1$个更短的串存在，即，长度（0,1,...,n-1)小于$n$的串。

同样原因，从不能对他们高度压缩这个方面看来，很多串是复杂的 -- 比起|s|，他们的$K(s)$不是很小。要更精确的的话，则固定一个n的值。有长度为$n$的$2^n$个bit串。此空间上的bit串的一致概率分布准确地分派相等的权重$2^{-n}$到每个长度为$n$的字符串。

__定理:__

长度为n的bit串的空间上的一致概率分布，使得字符串对于$c$不可压缩的概率至少是$1-2^{-c+1}+2^{-m}$。

要证明这个定理，注意长度不超过$n-c$的描述的数量由以下几何级数指定：

$
1+2+2^n+ ... +2^{n-c}=2^{n-c+1}-1
$

有至少$2^n-2^{n-c+1}+1$个长度为n的bit串对于$c$是不可压缩的。要得到此概率，则用它除与$2^n$.

---

# Chaitin的不完备定理
【略】

---

# 最小消息长度
【略】

---

# Kolmogorov随机性
【略】

---

# 与熵的关系
【略】