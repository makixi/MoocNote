# Sublinear Algorithm Examples

---

### 3.1 数据流中频繁元素

#### 3.1.1 大数据的数据流模型
数据只能顺序扫描1次或几次<br>
能够使用的内存是有限的<br>
希望通过维护一个内存结果（概要）来给出相关性质的一个有效估计<br>
数据流模型适用于大数据（顺序扫描数据仅一次/内存亚线性）

---

#### 3.1.2 数据流模型
来自某个域中的元素序列$<x_1,x_2,x_3,x_4,\cdots>$<br>
有限的内存：内存<<数据的规模  通常$O(\log_{k}{n})$或$O(n^\alpha)$ for $a<1$ <br>
快速处理每个元素<br>

---

#### 3.1.3 频繁元素计算算法（misra gries）
处理元素x<br>
if 已经为$x$分配计算器，增加之<br>
else if 没有相应计数器，但计数器个数小于$k$，为$x$分配计数器，并设为1<br>
else 所有计数器减1，删除值为0的计数器<br>
<br>
<br>
**一个计数器x减少了几次？<=>有几个减少计算器的步骤<br>**
整个数据的权重（计数器的和）记作$m'$<br>
整个数据流的权重（全部元素的数量）是$m$<br>
每一个计数器降低的步骤减少从$k$个计数，但是并未计输入元素的此次出现，即$k+1$次未计入的元素出现。<br>
>>最多有$\frac{m-m'}{k+1}$个减少步骤<br>
>>估计值与真实值相差最多$\frac{m-m'}{k+1}$<br>
>>当数据流中元素的总数>>$\frac{m-m'}{k+1}$时，得到x的一个好的估计<br>
>>错误的界限和k成反比

---

### 3.2 最小生成树
输入：无向有权联通图$G=(V,E)$，其顶点的度最大为$D$，边上的权来自整数集合{$1,\cdots,W$}<br>
<br>
输出：图$G$的最小生成树的权重。<br>
<br>

#### 3.2.1 精确解
贪心法（prime/kruskal）<br>
时间复杂性：$O(m\log n)$<br>
超过线性

#### 3.2.2 亚线性算法的假设
图组织成邻接表的形式（可以直接访问每个结点的邻居）<br>
可以随即均匀地选择结点<br>

#### 3.2.3 时间亚线性算法的思想
利用特定子图连通分量的数量估计最小生成树的权重<br>
假设所有边的权重都是1或者2，最小生成树的权重<br>
$=\#N_1+\#N_2$($\#N_i$为最小生成树中权重至少为$i$的边的数量)<br>
$=n-1+\#N_2$(最小生成树有$n-1$
条边)<br>
$=n-1+权重为1边构成的导出子图的联通分量数-1$<br>

#### 3.2.4 最小生成树和连通分量的关系
**一般的情况**<br>
1.$G_i$:$G$中包含所有权重小于$i$的边的子图<br>
2.$C_i$:$G_i$中的连通分量数<br>
3.最小生成树权重大于$i$的边数为$C_i-1$<br>
<br>
$W_{MST}(G)=n-w+\sum_{i=1}^{w-1}C_i$<br>
证明:<br>
令$\beta_i$为最小生成树中权重大于i的边的个数<br>
每一条MST边对WMST基础贡献为1，每个权重大于1的边额外贡献了1，每条权重大于2的边贡献的更多，因此：
$$W_{MST}(G)=\sum_{i=0}^{w-1}\beta_i=\sum_{i=0}^{w-1}(C_i-1)=-w+\sum_{i=0}^{w-1}C_i=n-w+\sum_{i=1}^{w-1}C_i$$


#### 3.2.5 基础算法：连通分量个数的估计
输入：图G=(V,E)，有n个顶点，表示为邻接矩阵，结点最大度为d。<br>
<br>
输出：连通分量的个数<br>
<br>
精确解时间复杂性：$O(dn)$<br>
<br>
利用随机化方法<br>
<br>
估计联通分量个数$\#CC$<br>
$\#CC±\epsilon的概率≥2/3$<br>
运行时间和$n$无关<br>

---

$C$:连通分量的个数<br>
对于每个结点$u,n_u$:u所在连通分量的结点数<br>
对于每个连通分量：$\sum_{u∈A}\frac{1}{n_u}=1$<br>
故：$\sum_{u∈V}\frac{1}{n_u}=C$<br>
<br>
通过估计抽样顶点的$n_u$来估计这个和<br>
如果u所在的分量很小，其规模可以通过BFS估计<br>
如果u所在的连通分量很大，$1/n_u$很小，对和的贡献很小<br>
<br>
**每个u所在连通分量结点数的估计**<br>
令$\hat n_u=\min{(n_u,2/\epsilon)}$<br>
在这种情况下，对C的估计$$\hat C=\sum \frac{1}{\hat n_u}$$
则$$|\hat C-C|=|\sum(\frac{1}{\hat n_u}-\frac{1}{n_u})|≤\frac{\epsilon n}{2}$$<br>

#### 3.2.6 连通分量数估计算法
$CC(G,d,\epsilon)$<br>
1.$for\ i=1\ to\ s=\theta(\frac{1}{\epsilon^2})\ do$<br>
2.$\ \ \ \ 随机选择点u$<br>
3.$\ \ \ \ 从u开始BFS，将访问到的顶点存到排序序列L中，访问完连通分量或L=2/\epsilon时停止，\hat n_u=|L|$<br>
4.$\ \ \ \ N=N+\hat n_u$<br>
5.$返回\hat C=s/N*n$<br>
<br>
运行时间:$O(\frac{d}{\epsilon^3}\log \frac{1}{\epsilon})$

---

**连通分量近似计数的分析**<br>
分析的目的：$$Pr[|\widetilde C-\hat C|>\frac{\epsilon n}{2}]\leq \frac{1}{3}$$
估计值和真实值相差过大的概率很小<br>
<br>
对于采样中的第i个结点u，令$$Y_i=\frac{1}{\hat n_u}$$
<br>
$$Y=\sum_{i=1}^{s}Y_i=\frac{s \widetilde C}{n}$$
<br>
$$E[Y]=\sum_{i=1}^{s}E[Y_i]=sE[Y_1]=s \cdot \frac{1}{n} \sum_{u∈V} \frac{1}{\hat n_v}=\frac{s \widetilde C}{n}$$
$$Pr[|\widetilde C-\hat C|>\frac{\epsilon n}{2}]=Pr[|\frac{n}{s}Y-\frac{n}{s}E[Y]|>\frac{\epsilon n}{2}]=Pr[|Y-E[Y]|>\frac{\epsilon s}{2}]$$
<br>
**Hoeffding界**：$Y_1,\cdots,Y_s$为[0,1]区间内独立同分布的随机变量，令$Y=\sum_{i=1}^{s}Y_i$，则$Pr[|Y-E[Y]|≥\delta \leq 2e^{-2\delta^2/s}]$
$$Pr[|\widetilde C-\hat C|>\frac{\epsilon n}{2}]=Pr[|Y-E[Y]|>\frac{\epsilon s}{2}] \leq 2e^{-\frac{\epsilon^2 s}{2}}$$
$$s=\theta(\frac{1}{\epsilon^2})=>Pr[|\widetilde C-\hat C|>\frac{\epsilon \epsilon n}{2}] \leq \frac{1}{3}$$
<br>
得出：$$Pr[|\widetilde C-\hat C|>\frac{\epsilon n}{2}]\leq \frac{1}{3}$$
$$|\widetilde C-C|\leq \frac{\epsilon n}{2}$$
因此，下列事件发生的概率大于2/3：$$|\widetilde C-C|\leq |\widetilde C-\hat C|+|\hat C-C| \leq \frac{\epsilon n}{2}+\frac{\epsilon n}{2}=\epsilon n$$
<br>
综上所述，有n个顶点的途中，若其顶点的度至多为d，则其连通分量的数量估计误差最多为$+\epsilon n$

#### 3.2.7 最小生成树近似算法
$for\ i=1\ to\ w-1\ do$<br>
$\ \ \ \ \hat C_i=CC(G_i,d,\frac{\epsilon}{w})$<br>
$return\ \widetilde w_{MST}=n-w+\sum_{i=1}^{w-1}\widetilde C_i$<br>
<br>
假设$C_i$的所有估计都是正确的，$|\widetilde C_i-C_i|\leq \frac{\epsilon}{w}n$，则$|\widetilde w_{MST}-w_{MST}|=|\sum_{i=1}^{w-1}(\widetilde C_i-C_i)|\leq w \cdot \frac{\epsilon}{w}n=\epsilon n$<br>
$Pr[所有w-1次估计都正确]≥(2/3)^{w-1}$<br>




---
### 3.3 序列有序的判定
输入：n个数的数组<br>
<br>
输出：这个数组是否有序<br>
<br>
近似版本：这个数组是有序的还是$epsilon$远离有序的<br>
<br>
**$\epsilon$远离**：必须删除大于$\epsilon n$个元素才能保证剩下的元素有序<br>

---

**亚线性算法**<br>
$for\ k=1\ to\ w/\epsilon\ do$<br>
$\ \ \ \ 选择数组中第i个元素x_i$<br>
$\ \ \ \ 用x_i在数组中做二分查找$<br>
$\ \ \ \ if\ 发现i<j\ 但是x_i>x_j\ then$<br>
$\ \ \ \ \ \ \ \ return\ false$<br>
$return\ true$<br>
<br>
算法的时间复杂性$O(\frac1\epsilon \log n)$<br>
<br>
输入数组有序，则总返回True<br>
下面证明：当输入数列$\epsilon$远离有序时，算法返回false的概率大于2/3<br>
首先证明：如果输入$\epsilon$远离有序，则存在大于$\epsilon n$个坏索引<br>
证明其逆否命题，即如果坏索引的个数小于$\epsilon n$，则其存在一个长度大于$\epsilon n$的单调递增子序列<br>

