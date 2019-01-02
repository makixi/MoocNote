# out-of-core algorithm

---

### 4.1 外存存储结构与外存算法
#### 4.1.1 随机存取机模型（传统）
![1](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_1.png?raw=true)<br>
当数据量太大时，传统的这种模型失效。<br>
<br>
**标准计算理论模型：**<br>
1.无限内存<br>
2.统一访问代价<br>
3.简单的模型为计算机行业成功的关键<br>

#### 4.1.2 分层存储
![2](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_2.png?raw=true)<br>
**现代计算机有复杂的存储层次：**<br>
1.存储量得到较大提升，但较慢的层次进一步远离CPU<br>
2.以块为单位的数据移动<br>

#### 4.1.3 慢速I/0
![3](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_3.png?raw=true)<br>
磁盘访问比主存访问的速度慢106倍<br>
磁盘系统通过传输大规模连续的数据块来平摊巨大的访问代价（8-16K字节）<br>
重要的是利用块高效存储/访问数据

#### 4.1.4 可扩展性问题
大多数程序在RAM模型上运行，即：<br>
![1](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_1.png?raw=true)<br>
由于操作系统按需访问块<br>
<br>
现代操作系统采用先进的分页和预取策略。<br>
但是，如果程序分散地访问磁盘上的数据，即使是好操作系统也无法利用数据块存取优势<br>
![4](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_4.png?raw=true)<br>

#### 4.1.5 外部存储器模型
![5](https://github.com/makixi/MoocNote/blob/master/BigDataAlgorithms/pic/4_5.png?raw=true)<br>
$$N=\#问题实例数据项个数$$
$$B=\#每个磁盘块中数据项个数$$
$$M=\#内存能容纳的数据项个数$$
$$T=\#输出数据项个数$$
$$I/O：内存和磁盘之间移动的块数$$
$$为了方便，假设：M>B^2$$

**基本界限**：<br>

 /  | 内存算法 | 外存算法
---- | --- | ---
浏览  | $N$ | $\frac{N}{B}$
排序  | $N\log N$ | $\frac{N}{B}\log_{\frac{M}{B}}\frac{N}{B}$
置换  | $N$ | $\min(N,\frac{N}{B}\log_{\frac{M}{B}}\frac{N}{B})$
查找  | $\log_{2}N$ | $log_{B}N$

PS:<br>
线性I/O：$O(\frac{N}{B})$<br>
置换不是线性的<br>
置换和排序范围在所有的实际情况是平等的<br>
$B$时很重要的因素：$\frac{N}{B}<\frac{N}{B}\log_{\frac{M}{B}}\frac{N}{B}<<N$<br>
无法用搜索树优化排序<br>

**队列和堆栈**<br>
队列：维护在主存中的push和pop块=>$O(1/B)$次push/pop操作<br>
堆栈：维护在主存储器push/pop块=>$O(1/B)$次push/pop操作

---


### 4.2 外存算法示例：外存排序算法
$<M/B$个排序列表（队列）可以在 $O(N/B)$ I/Os内合并<br>
未排序的列表（队列）可以使用$<M/B$个分割元素利用$O(N/B)$次I/O实现划分


---


### 4.3 外存数据结构示例：外存查找树
