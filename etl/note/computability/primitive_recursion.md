# 原始递归函数

## 定义
原始递归函数接受一个或多个自然数作为输入，返回一个自然数作为输出。原始递归函数由如下公理给出：

1. 基本情况
   1. **常值函数**：0元常数函数 $0$ 是原始递归的
   2. **后继函数**：1元后继函数 $S$ ，它接受一个参数并返回[皮亚诺公理](https://zh.wikipedia.org/zh-cn/%E7%9A%AE%E4%BA%9A%E8%AF%BA%E5%85%AC%E7%90%86)给出的后继数，是原始递归的。
   3. **投影函数**：对于所有 $n\ge1$ 和每个 $1\le i\le n$ 的 $i$ ， $n$ 元投影函数 $P_i^n$ 接受 $n$ 个参数并返回第 $i$ 个参数，是原始递归的。
2. 运算
   1. **复合**：给定 $k$ 元原始递归函数 $f$ ，和 $k$ 个 $m$ 元原始递归函数 $g_1,\cdots,g_k$ ，则 $m$ 元函数 $h$ 定义为 $h(x_1,\cdots,x_m)=f(g_1(x_1,\cdots,x_m),\cdots,g_k(x_1,\cdots,x_m))$ ，是原始递归的。
   2. **原始递归**：给定 $n$ 元原始递归函数 $f$ 和 $n+2$ 元原始递归函数 $g$ ，则 $n+1$ 元函数 $h$ 定义为 $h(x_1,\cdots,x_n,y)=\begin{cases}f(x_1,\cdots,x_n)&\text{if }y=0\\g(x_1,\cdots,x_n,y-1,h(x_1,\cdots,x_n,y-1))&\text{if }y>0\end{cases}$ ，是原始递归的。

一个函数是原始递归的，当且仅当它是基本情况中的函数，或可以从基本情况通过有限次复合和原始递归得到。

## 以阶乘为例

```lisp
(define (factorial n)
  (if (= n 0)
      1
      (* n (factorial (- n 1)))))
```

令 $f: ()\to 1$ ， $g: (x, y)\to (x + 1) * y$

可证明 ~~（略）~~ $f$ 和 $g$ 都是原始递归的。

发现 $factorial$ 满足 $factorial(0)=f()$ ，且 $factorial(S(n))=g(n, factorial(n))$

因此 $factorial$ 是原始递归的。

## 参考资料

[Wikipedia: 原始递归函数](https://zh.wikipedia.org/zh-cn/%E5%8E%9F%E5%A7%8B%E9%80%92%E5%BD%92%E5%87%BD%E6%95%B0)

## 阅读材料

[递归函数的通俗解释](https://blog.sciencenet.cn/blog-320682-974114.html)
