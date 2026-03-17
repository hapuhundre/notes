

# 总体阅读目标（8 周后你应该达到什么程度）

8 周结束后，目标不是“把所有书读完”，而是形成这 4 个层次的完整认知链：

1. **Shape / topology optimization 的大图景**：知道 shape optimization、topology optimization、homogenization、level set 各自解决什么问题。 
2. **Level set 数学与数值主线**：清楚 zero level set、signed distance、法向速度、Hamilton–Jacobi 演化、reinitialization 为什么重要。 
3. **Shape derivative / Hadamard 视角**：能把“目标函数对边界变化的灵敏度”与实际优化更新联系起来。 
4. **和你当前 CTM+/ILT 的映射**：知道哪些思想适合直接迁移，哪些会在 runtime、solver、拓扑锁定、MRC regularization 上出问题。20240712_CTM+_RoshniBiswas.pptx 已经明确提到 Hamilton–Jacobi 数值积分在 full-chip OPC 上会很慢；2209.10814v2.pdf 的参考文献也已经覆盖了 convex optimization / Sobolev spaces 这些支撑材料。

------

# 核心书单 / 资料清单（按优先级）

### A 级：一定要看

- **Shape and topology optimization**（Allaire / Dapogny / Jouve 的综述）——它明确是一个 introduction，重点强调 **Hadamard shape sensitivity** 与 **level set method**，而且会介绍两类实现框架。
- **Level Set Methods and Fast Marching Methods**（Sethian）或 **Level Set Methods and Dynamic Implicit Surfaces**（Osher & Fedkiw）——MIT 讲义明确把这两本列为 “two good introductory books”。
- **Shape Optimization by the Homogenization Method**（Allaire）——Springer 页面明确说它是一个 self-contained account，重点是 homogenization 及其在 optimal design / shape & topology optimization 中的应用。

### B 级：最好同步补

- **Topology Optimization: Theory, Methods, and Applications**（Bendsøe & Sigmund）——Springer 页面明确说它是该领域广泛阅读的经典，也可以作为 introduction。
- **Sobolev Spaces**、**Lectures on Convex Optimization**、**Numerical Optimization**——这些题目已经出现在你自己的 2209.10814v2.pdf 参考文献里，说明它们和你现在的优化主线是相容的。

# 8 周阅读计划

## 第 1 周：先搭建大图景——shape / topology / level set 各自解决什么

**主读：**

- *Shape and topology optimization* 综述。它的摘要已经明确说：会介绍 shape / topology optimization，强调 Hadamard 方法与 level set 表示，并讨论两类实现框架。
- 同时快速翻一下 *Topology Optimization: Theory, Methods, and Applications* 的目录感，Springer 页面显示它覆盖 topology optimization、extensions and applications、anisotropic materials、truss topology design 等。

**这一周的目标：**
 先不要陷进公式细节，而是回答 3 个问题（这是**我建议你在读的时候带着的问题**）：

1. shape optimization 和 topology optimization 的边界在哪里；
2. level set 是“表示工具”还是“优化方法”；
3. 固定网格、显式边界、材料分布法各自的优缺点。
    这些问题与综述和两本书的主题直接相关；具体问题是我给你的阅读建议。

**建议产出：**
 写 1 页笔记，把 “boundary variation / Hadamard / level set / homogenization / topology change” 之间的关系画成一张图。这个产出形式是我的建议。

------

## 第 2 周：把 level set 基础彻底理顺

**主读：**

- Sethian 或 Osher & Fedkiw 任一本作为主书。MIT 讲义明确给出了这些 level set 的核心概念：implicit geometry、signed distance function、法向、曲率、以及 level set equation $\phi_t + F|\nabla\phi| = 0$。

**这一周的目标：**
 你需要把下面这些概念从“看过”变成“能自己解释清楚”：

- zero level set 是什么；
- signed distance 为什么重要；
- 为什么需要 reinitialization；
- 法向速度 $F$ 和边界演化之间的关系；
- 为什么 level set 天然能处理 topology changes。
   这些点都直接出现在 MIT 讲义内容里。

**建议产出：**
 用你自己的语言写一份 1~2 页说明：
 “如果把 CTM+ 里的 phi image 看成 level set function，那么 $\phi=0$ 边界、$|\nabla\phi|=1$、reinit 分别对应什么工程含义？”
 这是我的建议，不是来源原文。

------

## 第 3 周：把 shape derivative / Hadamard 视角接上

**主读：**

- *Shape and topology optimization* 综述中关于 Hadamard / shape sensitivity 的部分。摘要明确说它特别强调 “the method of Hadamard for appraising the sensitivity of quantities of interest with respect to the domain”。
- 同时参考 *Structural Optimization by the Level Set Method* 这份报告/讲义，它直接把 shape differentiation、front propagation、algorithm and numerical results 放在同一条线里。

**这一周的目标：**
 回答两个问题：

1. 目标函数对“域/边界变化”的导数，和普通变量梯度有什么本质不同；
2. 为什么很多 level set shape optimization 方法实际上还是靠 shape gradient / boundary sensitivity 来驱动。
    第一个问题来自综述摘要的主题，第二个问题在讲义里也有同样主线；这两个问题本身是我整理给你的阅读目标。

**建议产出：**
 写一个小节笔记：

- 连续问题中的 shape derivative 是什么；
- 到离散网格 / phi image 上通常怎么近似；
- 对你的 CTM+/ILT，最自然的“形状变量”到底是 contour、phi 还是 pixel。
   这部分属于我的建议性整理。

------

## 第 4 周：进入 Allaire 的 homogenization 主线

**主读：**

- *Shape Optimization by the Homogenization Method*。Springer 页面明确给了目录级别信息：包括 **Homogenization**、**composite materials modeling**、**Optimal Design in Conductivity**、**Optimal Design in Elasticity**、**Numerical Algorithms**。

**这一周的目标：**
 你不用纠结每个证明，而是要抓住：

- 为什么 shape / topology optimization 常常会 ill-posed；
- 为什么 homogenization / relaxation 能让问题更“好做”；
- 为什么固定网格方法在数值上常常更方便。
   这些目标是我从书的定位和你当前问题里抽出来的阅读重点；书页明确支持它讲 homogenization、optimal design、numerical algorithms。 

**建议产出：**
 把你现在在 CTM+/ILT 里看到的“局部最小、拓扑锁定、初始化敏感、grid/threshold 依赖”，试着和“ill-posedness / relaxation”做个一一对应。这个映射是我的建议。

------

## 第 5 周：补 topology optimization 经典范式

**主读：**

- *Topology Optimization: Theory, Methods, and Applications*。Springer 页面明确显示它覆盖 topology optimization、extensions、anisotropic materials、truss structures 等章节。

**这一周的目标：**
 不是为了以后照搬结构优化算法，而是为了建立一个“**长度尺度控制 / 约束 / regularization / 可制造性**”的标准视角。这个角度对你现在思考 CTM+ 中的 minWidth / minSpace / minArea / topology barrier 很有帮助；这句是我的建议性解释，而不是书页原话。相关书页明确说明这本书是 theory / methods / applications 的统一介绍。

**建议产出：**
 整理一张对照表（建议项）：

- density-based / homogenization / level set / parametric shape
- 表示方式
- 是否天然支持拓扑变化
- 长度尺度控制在哪里实现
- 计算代价 / 对初始化的敏感度
   这个表格内容是我建议你自己做。

------

## 第 6 周：回到你的主战场——CTM+ / curvilinear optimization 映射

**主读：**

- 复习内部资料 20240712_CTM+_RoshniBiswas.pptx，其中明确讲到了 **Propagation of Surfaces under Curvature**、Hamilton–Jacobi 数值积分，以及对 CTM+ curvilinear optimization 的映射；同时它也明确指出 full-chip OPC 上这种数值积分“too slow”。
- 结合 2209.10814v2.pdf 参考文献中列到的优化和函数空间书籍。

**这一周的目标：**
 你要做的是把前 5 周的内容映射回你现在的问题：

- 如果把 CTM+ 的 phi image 当成 level set，哪部分是 shape evolution，哪部分是离散 optimizer；
- 为什么直接 PDE 式演化在 full-chip 上不可取；
- 哪些思想更适合以 regularization / proximal / splitting 的形式迁移，而不是照抄 level-set PDE。
   “PDE 式演化 full-chip 太慢”这个事实来自内部 PPT；“更适合作为 regularization / splitting 迁移”是我对你当前方向的建议。

**建议产出：**
 写一页 memo：
 “如果把 topology-preserving level set 引入 CTM+，哪些是 solver 层问题，哪些是 regularization 层问题，哪些是 extraction / separation 层问题？”
 这是你前面已经在问的问题，适合作为这一周的总结。

------

## 第 7 周：补优化器和正则化的数学支撑

**主读：**

- ADMM ILT paper 中出现的 **Lectures on Convex Optimization**、**Numerical Optimization**、以及与 split Bregman 相关的文献。 

**这一周的目标：**
 重点不是把 convex analysis 全学完，而是把下面这些“对你当前工作最实用”的问题理清楚：

- 一阶法、拟牛顿、分裂法各自适合怎样的目标项；
- 什么时候把 regularization 写成 penalty，什么时候更适合 splitting / proximal；
- 当你把 topology barrier、MRC regularization、EPE cost 拼在一起时，数值上最可能的冲突在哪里。

**建议产出：**
 做一个公式级别的分类表（建议项）：

- 光滑可微项
- 非光滑项
- 约束项
- 拓扑保护项
- 哪类项适合梯度下降 / L-BFGS / ADMM / proximal
   这个是我的建议，不是来源直述。

------

## 第 8 周：综合输出——形成你自己的“技术路线文档”

**主读：**

- 回看前面所有笔记，不新增主书。
- 如果你愿意补一点实践向资料，可以看 Allaire 的 level set code 页面，它明确提供 2-D Scilab routines，并说明基于 level set method 与相关论文工作，还包含 topological gradient 生成新 hole 的思路。

**这一周的目标：**
 把前 7 周内容压缩成一份 **你自己的技术判断**，建议结构如下（这是我的建议）：

1. 形状优化 / 拓扑优化 / level set 的理论地图
2. 为什么 CTM+ 不适合直接用 full PDE-style level set evolution
3. 哪些思想可以迁移：
   - shape derivative 视角
   - signed distance / reinit 思想
   - topology barrier 作为额外 regularization
   - splitting / proximal 处理复杂正则
4. 哪些地方必须谨慎：
   - runtime
   - main–SRAF separation
   - MRC / iMRC / minArea 冲突
   - 初始化拓扑锁定

其中第 2 点可由内部 CTM+ PPT 对 HJ 数值积分 runtime 的说明支撑；第 4 点和你最近连续讨论的主题高度一致。 

------

# 每周时间分配建议（更适合你这种工程研究型阅读）

这是**我的建议**，不是来源原文：

- **周内 4 天 × 45~60 分钟**：主阅读
- **周末 1 次 × 2 小时**：做笔记 / 公式整理 / 和自己项目映射
- 每周尽量输出一个小产物：
  - 1 页 summary
  - 1 张关系图
  - 1 份公式分类表
  - 1 份对 CTM+/ILT 的映射 memo
