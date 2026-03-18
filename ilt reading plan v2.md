# 一、总体原则

阅读目标改成“从问题结构出发”，而不是“按学科分块”

更关心的是这条链： **PDE/shape representation → ill-posedness → regularization → numerical optimization → inverse problems → level set / topology control → CTM+/ILT 映射**。
 这不是某本书的原话，而是我按你当前问题抽出来的学习顺序；它和以下外部资料的显式主题是对齐的：

- *Shape and topology optimization*：强调 **Hadamard shape sensitivity** 与 **level set representation/evolution**；
- *Shape Optimization by the Homogenization Method*：强调 **homogenization**、**optimal design**、**numerical algorithms**；
- *Computational Methods for Inverse Problems*：目录明确覆盖 **regularization**、**optimization tools**、**parameter selection**、**TV**、**nonnegativity constraints**；
- *Numerical Optimization*：目录明确覆盖 **line search**、**trust region**、**CG**、**quasi-Newton**、**least-squares**、**penalty / augmented Lagrangian / SQP / interior-point**。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6), [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC)

------

# 二、精简后的主阅读材料（按优先级）

## A. 形状优化 / level set 主线

1. **Shape and topology optimization**（Allaire / Dapogny / Jouve）
    这篇综述明确是一个 introduction，重点强调 **Hadamard** 的 shape sensitivity 和 **level set** 作为 shape representation / evolution 的数值工具，并且会介绍两类实现框架。
2. **J. A. Sethian — Level Set Methods / Level Set Methods and Fast Marching Methods**
    Sethian 官网明确说明 level set / fast marching 用于跟踪会产生 sharp corners、split、merge 的界面；公开可见目录里明确列出了 **The Time-Dependent Level Set formulation**、**Topological Change, Curvature Terms, and Multidimensionality**、**The Stationary Level Set formulation**、**Conversion to Static Hamilton-Jacobi Equation** 等主题。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/2006/book_1999.html), [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html)
3. **Grégoire Allaire — Shape Optimization by the Homogenization Method**
    Springer 页面明确写到，这本书的章节是 **Homogenization**、**The mathematical modeling of composite materials**、**Optimal Design in Conductivity**、**Optimal Design in Elasticity**、**Numerical Algorithms**。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6)
4. **Martin P. Bendsøe / Ole Sigmund — Topology Optimization: Theory, Methods, and Applications**
    公开目录明确列到 **Topology optimization by distribution of isotropic material**、**Extensions and applications**、**Design with anisotropic materials**、**Topology design of truss structures**。更细一点的目录还明确给出 **problem formulation**、**optimality criteria**、**sensitivity analysis**、**checkerboard problem**、**multiple loads**、**alternative approaches** 等条目。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-662-05086-6), [[external.d...ndelon.com\]](https://external.dandelon.com/download/attachments/dandelon/ids/AT00121E61D9ED9E8A924C1257B940023E7A3.pdf)

------

## B. inverse problems + 数值方法主线

1. **Curtis R. Vogel — Computational Methods for Inverse Problems**
    目录明确写到：
   - Chapter 1: **Introduction**（含 filtering、variational regularization、iterative regularization）
   - Chapter 2: **Analytical Tools**（ill-posedness、SVD、pseudo-inverse、generalized Tikhonov）
   - Chapter 3: **Numerical Optimization Tools**（steepest descent、CG、Newton、trust region、BFGS、line search）
   - Chapter 6: **Parameter Identification**（gradient、adjoint / costate、Hessian、Gauss-Newton）
   - Chapter 7: **Regularization Parameter Selection Methods**（UPRE、GCV、discrepancy principle、L-curve）
   - Chapter 8: **Total Variation Regularization**
   - Chapter 9: **Nonnegativity Constraints**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[catalog-te...hicago.edu\]](https://catalog-test.lib.uchicago.edu/vufind/Record/12577278/TOC)
2. **Andreas Kirsch — An Introduction to the Mathematical Theory of Inverse Problems**
    这本书的定位是：介绍 inverse problems 的基本概念、ill-posedness、regularization，并讨论若干非线性 inverse problems。公开目录里明确列到：
   - **Introduction and Basic Concepts**
   - **Regularization Theory for Equations of the First Kind**（Tikhonov、Landweber、Morozov discrepancy principle、CG）
   - **Regularization by Discretization**（projection、Galerkin、least squares、collocation、Backus–Gilbert）
   - 以及具体 inverse problems 章节。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-030-63343-1), [[books.google.com\]](https://books.google.com/books/about/An_Introduction_to_the_Mathematical_Theo.html?id=RTo9ZFaSSugC), [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf)
3. **Jorge Nocedal / Stephen J. Wright — Numerical Optimization**
    目录明确列到：
    **Line search methods**、**Trust-region methods**、**Conjugate gradient methods**、**Quasi-Newton methods**、**Least-squares problems**、**Penalty and augmented Lagrangian methods**、**Sequential quadratic programming**、**Interior-point methods for nonlinear programming**。 [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC), [[Numerical...University\]](http://users.iems.northwestern.edu/~nocedal/book/toc.html)
4. **Stephen Boyd / Lieven Vandenberghe — Convex Optimization**
    目录明确列到：
   - Chapter 1: **Least-squares and linear programming**
   - Chapter 4: **Convex optimization problems**
   - Chapter 5: **Duality**
   - Chapter 6: **Approximation and fitting**（含 regularized approximation、robust approximation）
   - 第三部分还有 **Unconstrained minimization**。 [[seas.ucla.edu\]](https://www.seas.ucla.edu/~vandenbe/cvxbook/bv_cvxbook.pdf), [[unina2.on-line.it\]](https://unina2.on-line.it/sebina/repository/catalogazione/documenti/Boyd, Vandenberghe - Convex optimization.pdf)

------

# 三、8 周阅读计划

------

## 第 1 周：搭建统一问题框架

### 主读材料

- *Shape and topology optimization*：先建立 shape / topology / level set / Hadamard 的总图景。
- *An Introduction to the Mathematical Theory of Inverse Problems*：看 **Introduction and Basic Concepts**。公开目录明确包含 **Examples of Inverse Problems**、**Ill-Posed Problems**、**The Worst-Case Error**。 [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf), [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-030-63343-1)

### 这一周重点看什么

- **shape optimization vs topology optimization**：它们差异在哪里，什么时候拓扑变化是核心。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-662-05086-6)
- **inverse problem 的基本语言**：正问题 / 逆问题、ill-posedness、稳定性、误差传播。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-030-63343-1), [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf)
- **level set 在这个大框架里的角色**：它是 shape representation / evolution 工具，而不是自动等同于优化器。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/2006/book_1999.html)

### 建议产出

写一页图：
 **“shape optimization / topology optimization / inverse problems / regularization / level set / numerical optimization 的关系图”**。
 这张图是我的建议性产出。

------

## 第 2 周：level set 基础与 Hamilton–Jacobi 主线

### 主读材料

- Sethian：优先看公开目录中的这些主题：
   **The Time-Dependent Level Set formulation**、**Topological Change, Curvature Terms, and Multidimensionality**、**The Stationary Level Set formulation**、**Conversion to Static Hamilton-Jacobi Equation**。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html)
- Sethian 书页还明确说明：level set / fast marching 可以处理 **sharp corners**、**change topology**，并覆盖 theory、implementations、applications。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/2006/book_1999.html)

### 这一周重点看什么

- 零水平集、signed distance、法向速度、曲率项。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/2006/book_1999.html), [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Explanations/level_set_explain.html)
- 为什么 **time-dependent** 和 **stationary** 两种 formulation 都重要。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html)
- 为什么 reinitialization / static Hamilton–Jacobi 视角和数值稳定性相关。这里“与数值稳定性相关”是我对这些主题的阅读建议；目录里明确存在这些章节主题。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html)

### 建议产出

用你自己的话写一页：
 **“如果把 CTM+ 里的 phi image 看成 level set function，$\phi=0$、$|\nabla\phi| \approx 1$、stationary formulation 各自对应什么工程意义？”**
 这同样是我的建议性产出。

------

## 第 3 周：shape derivative / level-set shape optimization

### 主读材料

- *Shape and topology optimization*：重点看 **Hadamard shape sensitivity** 与 level set 更新框架。
- 可配合看 *A level-set method for shape optimization*：摘要明确说它把 **Osher/Sethian 的 level-set algorithm** 和 **classical shape gradient** 结合起来，并且在 fixed Eulerian mesh 上处理 topology changes。

### 这一周重点看什么

- 什么是 **shape derivative / shape gradient**。
- 为什么很多 “level set shape optimization” 本质上依赖 boundary sensitivity，而不是单纯几何 PDE。
- fixed mesh / Eulerian capture 的好处与局限。

### 建议产出

写一页笔记：
 **“连续的 shape derivative，离散到 phi image / 网格之后最可能在哪里失真？”**
 这是我的建议性问题。

------

## 第 4 周：homogenization 与 topology optimization 经典路线

### 主读材料

- *Shape Optimization by the Homogenization Method*：重点放在
   **Homogenization**、**Optimal Design in Conductivity**、**Optimal Design in Elasticity**、**Numerical Algorithms**。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6)
- *Topology Optimization: Theory, Methods, and Applications*：重点放在 Chapter 1 的
   **Problem formulation and parametrization of design**、**Solution methods**、**Sensitivity analysis**、**checkerboard problem**、**non-uniqueness / local minima / dependence on data**。 [[external.d...ndelon.com\]](https://external.dandelon.com/download/attachments/dandelon/ids/AT00121E61D9ED9E8A924C1257B940023E7A3.pdf)

### 这一周重点看什么

- 为什么很多 shape/topology optimization 问题是 **ill-posed**。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6), [[SHAPE OPTI...ION METHOD\]](http://gillesfrancfort.com/assets/files/abjf copy.pdf)
- 为什么 relaxed / material distribution / homogenization 会让问题更可算。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6), [[SHAPE OPTI...ION METHOD\]](http://gillesfrancfort.com/assets/files/abjf copy.pdf)
- checkerboard、mesh dependence、local minima 这些“数值假象/数值病态”怎么出现。 [[external.d...ndelon.com\]](https://external.dandelon.com/download/attachments/dandelon/ids/AT00121E61D9ED9E8A924C1257B940023E7A3.pdf)

### 建议产出

整理一张表：
 **boundary variation / level set / homogenization / material distribution** 四条路线各自的表示方式、优点、局限。
 这是我的建议性产出。

------

## 第 5 周：inverse problems 的 regularization 主线

### 主读材料

- Kirsch：重点看
   **Regularization Theory for Equations of the First Kind**、**Tikhonov Regularization**、**Landweber Iteration**、**Morozov discrepancy principle**、**The Conjugate Gradient Method**、**Regularization by Discretization**。 [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf)
- Vogel：重点看
   Chapter 1 的 **Regularization by Filtering**、**Variational Regularization Methods**、**Iterative Regularization Methods**，以及 Chapter 2 的 **SVD / pseudo-inverse / generalized Tikhonov**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[catalog-te...hicago.edu\]](https://catalog-test.lib.uchicago.edu/vufind/Record/12577278/TOC)

### 这一周重点看什么

- “逆问题病态”到底体现在哪里：不唯一、不稳定、噪声放大、离散化误差。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-3-030-63343-1), [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF)
- Tikhonov、filtering、iterative regularization 之间的关系。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf)
- regularization parameter 为什么是方法成败关键。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF)

### 建议产出

写一页：
 **“把 CTM+/ILT 看成逆问题时，EPE cost、MRC regularization、topology barrier 分别更像 data term、prior 还是 constraint？”**
 这是我的建议性问题。

------

## 第 6 周：inverse problems 里的数值优化器

### 主读材料

- *Numerical Optimization*：重点看
   **Fundamentals of unconstrained optimization**、**Line search methods**、**Trust-region methods**、**Conjugate gradient methods**、**Quasi-Newton methods**、**Least-squares problems**。 [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC), [[Numerical...University\]](http://users.iems.northwestern.edu/~nocedal/book/toc.html)
- Vogel 的 Chapter 3：
   **Steepest Descent**、**CG**、**Newton**、**Trust Region Globalization**、**BFGS**、**Inexact Line Search**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF)

### 这一周重点看什么

- 为什么 inverse problems 里常见的数值核心，最后又回到 **least-squares / Gauss-Newton / CG / quasi-Newton**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC)
- **line search** 和 **trust region** 的差异。 [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC), [[Numerical...University\]](http://users.iems.northwestern.edu/~nocedal/book/toc.html)
- 什么情况下用 BFGS / Gauss-Newton / LM 更自然。`Numerical Optimization` 和 Vogel 都把 least-squares、Newton、BFGS、trust-region 列成核心内容。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC)

### 建议产出

做一张公式/方法对照表：

- steepest descent
- CG
- Gauss-Newton
- BFGS / L-BFGS
- trust region
- line search
- 它们各自适合的目标结构
   这是我的建议性产出。

------

## 第 7 周：convex / variational / constrained inverse problems

### 主读材料

- *Convex Optimization*：重点看
   **Least-squares and linear programming**、**Convex optimization problems**、**Duality**、**Regularized approximation**、**Robust approximation**、以及第三部分的 **Unconstrained minimization**。 [[seas.ucla.edu\]](https://www.seas.ucla.edu/~vandenbe/cvxbook/bv_cvxbook.pdf), [[unina2.on-line.it\]](https://unina2.on-line.it/sebina/repository/catalogazione/documenti/Boyd, Vandenberghe - Convex optimization.pdf)
- Vogel：重点看
   **Regularization Parameter Selection Methods**、**Total Variation Regularization**、**Nonnegativity Constraints**、**Parameter Identification**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[catalog-te...hicago.edu\]](https://catalog-test.lib.uchicago.edu/vufind/Record/12577278/TOC)
- 如果你时间够，再补 *Numerical Optimization* 的
   **Penalty and augmented Lagrangian methods**、**Sequential quadratic programming**、**Interior-point methods for nonlinear programming**。 [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC)

### 这一周重点看什么

- 为什么很多 inverse problems / regularization 可以写成 **variational optimization**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[seas.ucla.edu\]](https://www.seas.ucla.edu/~vandenbe/cvxbook/bv_cvxbook.pdf)
- duality、regularized approximation、robust approximation 对你后面处理多项正则冲突很有帮助。 [[unina2.on-line.it\]](https://unina2.on-line.it/sebina/repository/catalogazione/documenti/Boyd, Vandenberghe - Convex optimization.pdf)
- TV、nonnegativity、parameter-selection（UPRE/GCV/L-curve/discrepancy principle）这些内容，对“多正则项 + 参数调优”最直接。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[catalog-te...hicago.edu\]](https://catalog-test.lib.uchicago.edu/vufind/Record/12577278/TOC)

### 建议产出

写一页：
 **“对于 ILT/CTM+，哪些 regularization 更适合 penalty，哪些更适合 splitting / proximal / augmented Lagrangian？”**
 这个问题是我的建议性问题。

------

## 第 8 周：综合成你自己的技术路线；内部材料只做“映射”

### 主读材料

这一周**不新增外部主书**，只回看前 7 周笔记。

- A LEVELSET APPROACH FOR INVERSE PROBLEMS INVOLVING OBSTACLES：它明确是关于 **level set approach for inverse problems involving obstacles**，而且文中还明确提到 **Gauss-Newton algorithm**、以及 inverse problem ill-conditioning 不会因为 level set representation 自动消失，还建议用 **curve shortening / curvature flow** 一类正则化思路。
- 提醒自己，PDE-style Hamilton–Jacobi 数值积分在 full-chip OPC 上 runtime 会很重。 
- ADMM ILT paper：只用来对照你当前 ILT 背景里已经引用过的 **ADMM / split Bregman / TV / convex optimization / numerical optimization / Sobolev spaces**。

### 这一周重点看什么

- 把 shape optimization、inverse problems、regularization、numerical optimization 的语言体系统一起来。
- 明确哪些思想你准备迁移到 CTM+/ILT：
  1. shape derivative / level set 表示；
  2. inverse problem regularization；
  3. parameter selection；
  4. least-squares / Gauss-Newton / BFGS / splitting 的适用边界。
      这些 4 点是我给你的综合框架，不是来源原话；但每一点都能在前面列的外部材料目录中找到对应主题。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC), [[seas.ucla.edu\]](https://www.seas.ucla.edu/~vandenbe/cvxbook/bv_cvxbook.pdf)

### 最终产出（强烈建议）

整理一份 2~4 页的技术 memo：
 **《从 shape optimization / inverse problems 视角重构 CTM+/ILT 数值框架》**。
 建议大纲（这是我的建议）：

1. 目标函数结构：data term / regularization / hard constraints
2. 表示方式：pixel / phi / contour
3. 优化器：L-BFGS / Gauss-Newton / trust-region / splitting
4. regularization：TV / topology barrier / MRC / minArea / nonnegativity
5. 参数选择：手调、discrepancy、GCV/L-curve 等
6. 为什么 full PDE-style level set evolution 不适合直接全量搬进 CTM+

------

# 四、把“重点关注内容/章节”再压成一张速查表

## 你最应该优先啃的外部章节

- **Allaire / Dapogny / Jouve**：
   **Hadamard shape sensitivity**、**level set representation/evolution**、两类实现框架。
- **Sethian**：
   **The Time-Dependent Level Set formulation**、**Topological Change, Curvature Terms, and Multidimensionality**、**The Stationary Level Set formulation**、**Conversion to Static Hamilton-Jacobi Equation**。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html)
- **Allaire (Homogenization book)**：
   **Homogenization**、**Optimal Design in Conductivity**、**Optimal Design in Elasticity**、**Numerical Algorithms**。 [[link.springer.com\]](https://link.springer.com/book/10.1007/978-1-4684-9286-6)
- **Bendsøe & Sigmund**：
   **Problem formulation**、**optimality criteria**、**sensitivity analysis**、**checkerboard problem**、**multiple loads**。 [[external.d...ndelon.com\]](https://external.dandelon.com/download/attachments/dandelon/ids/AT00121E61D9ED9E8A924C1257B940023E7A3.pdf)
- **Kirsch**：
   **Ill-posed Problems**、**Tikhonov**、**Landweber**、**discrepancy principle**、**CG**、**regularization by discretization**。 [[scispace.com\]](https://scispace.com/pdf/an-introduction-to-the-mathematical-theory-of-inverse-2xl23oduti.pdf)
- **Vogel**：
   **Regularization by Filtering**、**Variational Regularization**、**Iterative Regularization**、**SVD / pseudo-inverse**、**Numerical Optimization Tools**、**Parameter Identification**、**Regularization Parameter Selection**、**TV**、**Nonnegativity Constraints**。 [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[catalog-te...hicago.edu\]](https://catalog-test.lib.uchicago.edu/vufind/Record/12577278/TOC)
- **Nocedal & Wright**：
   **Line search**、**trust region**、**CG**、**quasi-Newton**、**least-squares**、**penalty / augmented Lagrangian**、**SQP**、**interior-point**。 [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC), [[Numerical...University\]](http://users.iems.northwestern.edu/~nocedal/book/toc.html)
- **Boyd & Vandenberghe**：
   **Least-squares and linear programming**、**Convex optimization problems**、**Duality**、**Regularized approximation**、**Robust approximation**。 [[seas.ucla.edu\]](https://www.seas.ucla.edu/~vandenbe/cvxbook/bv_cvxbook.pdf), [[unina2.on-line.it\]](https://unina2.on-line.it/sebina/repository/catalogazione/documenti/Boyd, Vandenberghe - Convex optimization.pdf)

------

# 五、我对你这个版本的最终建议

如果你只打算投入**有限时间**，那最值得优先保证的是这 4 段：

1. **Allaire / Dapogny / Jouve** 综述
2. **Sethian** 的 level set 主线
3. **Vogel** 的 inverse problems + numerical methods
4. **Nocedal & Wright** 的优化器主线

因为这 4 段组合起来，正好对应你现在最缺的统一语言：
 **形状表示 → 病态性 → 正则化 → 优化器**。
 这个“四段优先级”是我的建议，不是来源原话；但每本材料的章节覆盖范围都能支持这条组合。 [[math.berkeley.edu\]](https://math.berkeley.edu/~sethian/2006/Publications/Book/1996/description_of_book.html), [[gbv.de\]](https://www.gbv.de/dms/ilmenau/toc/34711654X.PDF), [[libcat.can...bury.ac.nz\]](https://libcat.canterbury.ac.nz/Record/1059549/TOC)
