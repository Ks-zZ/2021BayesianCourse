# 27 Bayesian Decision Theory



> 吴喜之老师编著的《贝叶斯数据分析——基于R与Python的实现》一书的第二章中，对决策的基本概念做出如下的定义与解释：



##  贝叶斯决策的基本概念

​	考虑一个统计模型，其观测值$x=(x_1,x_2,\dots,x_n)$的分布依赖参数 $\vartheta\in\Theta$，这里 $\Theta$ 为状态空间，而 $\vartheta$ 代表“自然状态”。令A为可能的行动空间，对于行动 $a\in A$ 和参数 $\vartheta\in\Theta$，定义一个损失函数$l=(\vartheta,a)$（也可以采用与损失函数相反的效用函数）.例如在估计参数的某个函数 $q(\vartheta)$ 中，行动 $a$ 就代表估计量。常见的损失函数有**平方损失函数** $l(\vartheta,a)=[q(\vartheta)-a]^2$、**绝对损失函数** $l(\vartheta,a)=|q(\vartheta)-a|$ 等，在检验 $H_0:\vartheta\in \Theta_0$ 对 $H_1:\vartheta\in \Theta_1$ 中可以用 **0-1 损失函数**，如果 $\vartheta\in \Theta_a$（判断正确），则 $l(\vartheta,a)=0$，否则为1。

​	用 $\vartheta$ 表示基于数据 $x$ 的决策，则**风险函数**定义为：
$$
R(\vartheta,\delta)=E\{l[\vartheta,\delta(x)|\vartheta]\}=\int l(\vartheta,\delta(x))p(x,\vartheta)dx
$$
对于先验分布$p(\vartheta)$，贝叶斯风险定义为：
$$
r(\var)=E[R(\vartheta,\var)]=\int R(\vartheta,\var)p(\vartheta)d\vartheta.
$$
对于后验分布$p(\vartheta|x)$，后验风险定义为：
$$
r(\vartheta,x)=E(l[\vartheta,\var(X)]|X=x)=\int l(\vartheta,\var(x))p(\vartheta|x)d\vartheta.
$$
*使贝叶斯风险最小的决策称为**贝叶斯决策**（在估计问题中称为贝叶斯估计）。

对于估计$q(\vartheta)$的问题：如果采用平方损失函数，则贝叶斯估计为后验均值 $E[q(\vartheta)|x]$；如果采用绝对损失函数，则贝叶斯估计为后验中位数 $med(q(\vartheta)|x)$.



> 虽然定义与过程结论非常直观全面，但也许对于初学者来说，各种各样参数、损失函数、风险函数已经足够让人晕头转向了。
>
> 让我们带着这些概念，通过 James O. Berger 所著的《Statistic Decision Theory and Bayesian Analysis》一书第一章中所举的众多例子，来将这些他们一一弄清楚。



## 一、背景介绍



### 决策理论

**决策理论**，正如字面意思所展示的一样，是用来进行决策的理论；而**统计决策理论**是在统计知识存在的情况下进行决策的理论，它揭示了决策问题中涉及的一些不确定性。我们将这些**不确定性假定为未知的数值**，并将它们用 $\vartheta$（也许是个向量或者矩阵）来表示。

比如一家药品公司现在正在决定是否要生产一种镇痛药，影响这项决策的众多因素之二有：这款药能在多大比例的人群中证明它的有效性$(\vartheta_1)$；这款药将会占领多大的市场份额$(\vartheta_2)$。 $\vartheta_1$ 和 $\vartheta_2$ 通常都是未知的，但可以通过传统的实验方法获得它们的一些统计信息。这个问题是决策理论的一个问题，因为最终目的是决定是否销售药物，销售多少，怎样定价，等等。

**经典统计学**的方向是**使用样本信息(统计调查中产生的数据)来推断 $\vartheta$**。这些经典的推论，在大多数情况下，是不考虑它们的用途的。另一方面，在**决策理论**中，试图**将样本信息与问题的其他相关方面结合起来，以做出最佳决策。**

除了示例信息之外，另外两种类型的信息通常是相关的。

> 笔者把这两类信息总结为：“瞻前顾后”

1. <font color=#00008B>**决策可能产生的后果**</font>。

   通常，这种后果可以通过**每个可能的决定和各种可能的 $\vartheta$ 值下的损失来量化**。

> 统计学家似乎是悲观的生物，他们从损失的角度思考问题。相反，经济学和商业中的决策理论家谈论的是收益(效用)。作为我们主要是统计方向,我们将使用术语的损失函数。请注意，获得只是负损失，所以这两种方法之间没有真正的区别。

*亚伯拉罕·瓦尔德(Abraham Wald)首先对将损失函数纳入统计分析进行了广泛的研究；Wald(1950)也回顾了决策理论的早期工作。*

在上述药物的例子中,**是否将药物推向市场取决于许多$\vartheta_1$、$\vartheta_2$ 和许多其他因素的复杂作用。**一个比较简单的情况是，在广告活动中估计使用 $\vartheta_1$。**低估 $\vartheta_1$ 的损失来自于使产品看起来比实际情况更糟(对销售产生不利影响)**，而**高估 $\vartheta_1$ 的损失则是基于误导广告可能受到惩罚的风险。**

2. <font color=#00008B>**先验信息**</font>。

   这是**从统计调查以外的来源获得的关于 $\vartheta_1$ 的信息。**一般来说，先验信息来自于过去涉及类似的 $\vartheta_1$ 的经验。例如，在药物的例子中，从不同但相似的止痛药中可能有大量关于 $\vartheta_1$和 $\vartheta_2$ 的信息。



### 三个实验

> *L. J. Savage(1961)给出了一个关于先验信息可能重要性的令人信服的例子。他考虑了以下三个统计实验:*

1. 一位往茶里加牛奶的女士声称，她能分辨出是将茶和牛奶中的哪一种先倒进杯子里。在进行的十项试验中，她都能正确地判断出哪一种先倒。
2. 一位音乐专家声称能够区分一页海顿乐谱和一页莫扎特乐谱。在进行的十次试验中，他每次都做出正确的判断。
3. 个喝醉酒的朋友说他能预测掷硬币的结果。在十次测试中，他每次都是正确的。

在三种情况下，未知量 $\vartheta$ 是人们正确回答的可能性，对各种情况的经典显著性检验将考虑零假设($\H_0$)，即 $\vartheta$ = 0.5 (即故事的主人公是在猜测) 。在所有三种情况下，这一假设都将以 $2^{-10}$ 的(单侧)显著性水平被拒绝。因此，**经典统计为上述实验提供了强有力的证据，证明各种说法都是有效的。**

在实验2中，我们没有理由怀疑这个结论。(根据我们之前的知识，这个结果是相当可信的) 然而，在实验3中，我们先前认为这个预测是不可能的（除非相信超感官知觉的存在），这往往会导致我们忽视实验证据，认为这是一种幸运。在实验1中，我们无法判定，因为不同的人根据他们之前对这一主张的可信性的信任程度会得出不同的结论。在这三种相同的统计情况下，先验信息显然不能被忽略。

**正式寻求利用先验信息的统计学方法被称为贝叶斯分析**(以贝叶斯(1763)命名)。贝叶斯分析和决策理论很自然地结合在一起，部分原因是它们都有**利用非实验信息来源的共同目标**，部分原因是**它们之间有着深刻的理论联系**；因此，我们将在书中强调贝叶斯决策理论。

> 然而，在这里需要注意的是，现实中也存在着广泛发展的非贝叶斯决策理论和广泛发展的非决策理论贝叶斯观点。



## 二、基本要素



### 自然状态（参数$\theta$) 与行动（决策a）

**影响决策过程的未知数 $\vartheta$ 通常被称为自然状态**。在做决定时，考虑自然的可能状态显然是重要的。**符号 $\Theta$ 将被用来表示自然界所有可能状态的集合**。通常，当进行实验以获得关于 $\vartheta$ 的信息时，实验是这样设计的，即观测结果是按照某种概率分布分布的，该概率分布以 $\vartheta$ 作为未知参数。在这种情况下，**$\vartheta$ 被称为参数，$\Theta$ 被称为参数空间**。

**决策在文献中更常被称为行动**。**特定的行动用 $a$ 表示**，所有**可能行动集合用 $A$ 表示**。



### 损失函数

如引言中所述，决策理论的一个关键元素是**损失函数**。如果采取某一特定行动 $a_1$，而 $\vartheta_1$ 是真实的自然状态，则会产生损失 $L(\vartheta_1,a_1)$ 。因此，我们假设损失函数$L(\vartheta,a)$对所有 $(\vartheta,a)\in\Theta\times A$ 都有定义。

当进行统计调查以获得关于 $a$ 的信息时，结果(一个随机变量)将被记为 $X$。$X$ 通常是一个向量， $X = (X_1, X_2，…，X_n)$， $X$是独立于共同分布的观测值。$X$的特定实现将被记为 $x$。可能结果的集合是样本空间，将被记为 X (通常 X 将是$R_n$ , $n-$维欧氏空间的子集。

当然，**$X$ 的概率分布取决于未知的自然状态 $a$。**当 $\vartheta$ 是自然的真实状态。设$P_\vartheta(a)$或$P_\vartheta(X\in A)$表示事件 $A$ ($A\subset$ X ) 发生的概率。或者简单地说，假设 $X$ 是一个连续的或离散的随机变量，有密度函数$f(x|\vartheta)$。如果 $X$ 是连续的（即有密度函数），那么：

> 密度函数积分得到概率分布函数

$$
P_\vartheta(A)=\int f(x|\vartheta)dx
$$

如果 $X$ 是离散的那么：
$$
P_\vartheta(A)=\sum_{x\in A} f(x|\vartheta)dx
$$
对于给定值为 $\vartheta$ 的函数 $h(x)$ 的期望(在$x$上)定义为：
$$
E_\vartheta[h(X)]=
	\begin{cases}
	\int h(x)f(x|\vartheta)\mathrm{d}x &(连续)\\
	\sum h(x)f(x|\vartheta)&(离散)
	\end{cases}
$$
要分别处理这两个不同的表达式 $E_\vartheta[h(X)]$ 是很麻烦的，因此，我们定义：
$$
E_\vartheta[h(X)]=
	\int h(x)\mathrm{d}F^X(x|\vartheta)
$$
右边的表达式是用来解释前面的表达式 $E_\vartheta[h(X)]$ 的。（当然，这个积分可以被认为是黎曼积分，其中 $F^X$ 是 $X$ 的累积分布函数，不熟悉这些术语的读者可以把积分当作一种符号。）用同样的方法，我们可以写出：
$$
P_\vartheta[A]=
	\int_A \mathrm{d}F^X(x|\vartheta)
$$
通常，有必要说明期望或概率是基于哪个随机变量产生的。$E$ 或 $P$ 的上下标起到这个作用。（上下标可以是随机变量、它的密度、它的分布函数或它的概率测度）在这里 $E$ 的下标表示期望的参数值 $\vartheta$ 。

在上述部分提及的第三类信息是**关于 $\vartheta$ 的先验信息**。谈论先验信息的一个有用的方法是根据 $\Theta$ 上的概率分布(关于 $\vartheta$ 的先验信息很少是非常精确的，因此，用一个存在的各种可能值的概率来陈述先验信息是自然而然的。)**符号 $\pi_\vartheta$ 将被用来表示 $\vartheta$ 的先验密度**(同样适用于连续或离散情况)。综上，如果 $A\subset \Theta$ :
$$
P(\vartheta\in A)=\int_A \mathrm{d}F^\pi(\vartheta)=
	\begin{cases}
	\int \pi(\vartheta)\mathrm{d}\vartheta &(连续)\\
	\sum \pi(\vartheta)&(离散)
	\end{cases}
$$
未来会讨论先验概率分布的构造与 $\vartheta$ 概率的含义。毕竟，**在大多数情况下 $\vartheta$ 并不是“随机的”。**一个典型的例子是，当 $\vartheta$ 是一个未知的需要确定的固定物理常数（比如光速），其基本思想是，关于 $\vartheta$ 的概率定义被解释为“个人概率”，反映了个人对所给陈述的可能性的相信程度。



### 举例说明

> 下面列举出使用上述术语的三个例子。

------

**例1.** 在介绍的药物例子中，假设需要估计 $\vartheta_2$。 由于 $\vartheta_2$ 表示比例，可以明显得出$\Theta =\{\vartheta_2:0\leq\vartheta_2\leq1\}=[0,1]$。由于目标是估计 $\vartheta_2$ ，我们所采取的行动即为：选择一个数字作为 $\vartheta_2$ 的估计值。 $A=[0,1]$ （在估计问题中，$A$ 通常等于 $\Theta$ ）。公司可以确定损失函数：
$$
P(\vartheta_2, A)=
	\begin{cases}
	\vartheta_2-A &if\ \vartheta_2-A\geq0\\
	2(A-\vartheta_2)&if\ \vartheta_2-A\leq0
	\end{cases}
$$
从式子中我们可以看出，高估需求(从而导致药物生产过剩)的代价是低估需求的两倍。

为了获得 $\vartheta_2$ 的样本信息，一个合理的实验是进行抽样调查。例如，假设采访了 $n$ 个人，有 $X$ 个人会买这种药。可以合理地假设 $X$ 服从二项分布 $B(n,\theta_2)$，在这种情况下，样本的密度函数为：
$$
f(x|\theta)={n\choose k}\theta_{2}^x{(1-\theta_2)}^{n-x}
$$
可能有相当多的关于 $\theta_2$ 的信息来自于以前将新的类似药物引入市场产生的反响。假设,在过去,新的药物往往占领 $\frac{1}{10}$ 到 $\frac{1}{5}$ 的市场,并且取两者之间的所有值都是等可能性的。这种先验信息可以通过给 $\theta_2$ 一个服从均匀分布 $U(0.1,0.2)$ 的先验密度，如：
$$
\pi(\theta_2)=10I_{(0.1,0.2)}(\theta_2)
$$
以上对 $I$ 、$f$ 和 $\pi$ 的取值相当粗糙，通常需要更详细的结构才能获得满意的结果。

------

**例2.** 一家无线电公司收到了一批晶体管。单独检查每个晶体管的性能花费的成本太贵了，所以使用抽样计划来检查整个装运。从发货的晶体管中随机选择n个样品并进行测试。根据样品中有缺陷的晶体管数量 $X$，我们将接受或拒绝发货。因此有两种可能的行动：$a_1$——接受货物，$a_2$——拒绝货物。如果 $n$ 比总货量小，可以假设 $X$ 服从 $B(n,\vartheta)$ 分布，其中 $\vartheta$ 是总货量中有缺陷的晶体管所占的比例。

公司确定他们的损失函数为：$l(\vartheta,a_1)=10\vartheta,l(\vartheta,a_2)=1$ 。当 $a_2$ 被确定(即该批次被明确拒绝)时，损失为不变值1，这反映了由于更换货物的不便、延迟和测试而产生的成本。当 $a_1$ 被确定(即该批次被接受)时，损失与 $\vartheta$ 成比例，因为 $\vartheta$ 反映出生产的有缺陷收音机的比例，设置常数因子10表示这两种误差所涉及的相对成本。

这家无线电公司过去曾多次收到同一供货公司寄来的晶体管。因此，他们有大量关于过去发货的值 $\vartheta$ 的数据存储。 事实上过去的统计调查数据显示, $\vartheta$ 是分布式服从$Be(0.05,1)$分布。因此：
$$
\pi=(0.05)\Theta^{-0.95}I_{[0,1]}(\vartheta)
$$

------

**例3.** 投资者决定是否购买风险较高的债券。如果投资者购买债券，到期时可以赎回净收益500美元。然而，债券可能会违约，在这种情况下，最初的1000美元投资将会损失。如果投资者把他的钱放在一个“安全”的投资中，他将保证在同一时期获得300美元的净收益。投资者估计违约概率为0.1。

这里$A=\{a_1,a_2\}$ ,$a_1$表示买债券，$a_2$表示不买。同样，$\Theta=\{\theta_1,\theta_2\}$，其中 $\theta_1$ 表示自然状态“没有默认发生”， $\theta_2$ 表示状态“有默认发生”。在收益用负损失表示的情况下，损失函数如下表所示。

|            | $a_1$ | $a_2$ |
| ---------- | ----- | ----- |
| $\theta_1$ | -500  | -300  |
| $\theta_2$ | 1000  | -300  |

当 $\Theta$ 和 $A$ 都是有限的，损失函数可以很容易地用这样一张表来表示，称为损失矩阵。行通常放置在表的顶部，$\theta$ 值放置在侧边。先验信息可以写成 $\pi(\theta_1)=0.9$ 和 $\pi(\theta_2)=0.1$ 。

> 注意，在本例中没有来自相关统计实验的样本信息。这样的问题称为**无数据问题**。



### 考虑损失和先验信息的三个原因

从上面的例子中，**不是每个问题都有明确定义的损失函数和明确的先验信息。在许多问题中，这些量是非常模糊的，甚至是非唯一的。**这方面最重要的例子是统计推理问题。在统计推断中，**目标不是立即做出决定**，而是**提供统计证据的“摘要”**，以便各种未来的“用户”能够很容易地将这些证据纳入他们自己的决策过程中。因此，像上文提及的光速问题中，一个测量光速的物理学家不可能知道他的结果的使用者将会有多少损失。

由于这一点，许多统计学家使用“统计推断”作为盾牌，以避免考虑损失和事先信息。这是一个错误，原因有几个。首先，**应该(理想地)构建来自统计推断的报告，以便在个人决策中很容易地加以利用。**我们将看到许多经典的推论在这方面是失败的。

在推理中考虑损失和先验信息的第二个原因是，调查人员很可能掌握这些信息；**他通常会非常清楚他的推论可能被用于何处，并且可能对情况有相当多的先验知识。**因此，在他的分析中提出这些资料几乎是必要的，尽管应当小心地将“主观”和“客观”信息清楚地分开。

在推理中涉及损失和先验信息的最后一个原因是，**推理的选择**(不仅仅是数据总结)**可以被看作是一个决策问题，**在这个问题中，行动空间是所有可能的潜在语句的集合，并且使用了一个反映知识传递成功的损失函数。这种“推论损失”后续讨论。同样，可以构造“推理先验”，并用于推理中令人信服的优势。

虽然上述原因证明了将损失函数和先验信息具体纳入推断的合理性，但决策理论即使在这种纳入被禁止的情况下也是有用的。这是因为对于某些形式的损失函数，许多标准推理标准可以正式地再现为决策理论标准。我们将会遇到许多例子来说明这一点，以及使用决策理论机制来解决推理问题的价值。



##  三、预期损失、决策规则和风险

正如前文中提到的，我们将在存在不确定性的情况下进行决策。因此，实际发生的损失 $L(\theta, a)$ 在决策时将永远无法确定。面对这种不确定性，一种自然的方法是**考虑做出决策的“预期”损失**，然后**根据这一预期损失选择一个“最优”决策**。在本节中，我们考虑几种预期损失的标准类型。



### 贝叶斯预期损失

从直观的角度来看，最自然的预期损失是涉及 $\theta$ 中的不确定性的损失，因为 $\theta$ 在做决定时是未知的。我们已经提到，可以**将 $\theta$ 视为一个具有概率分布的随机量，并且根据这个概率分布考虑预期损失**是非常明智的.

定义 如果 $\pi^*(\theta)$ 为决策时 $\theta$ 的可信概率分布，则行动 $a$ 的贝叶斯期望损失为：
$$
\rho(\pi^*,a)=E^{\pi^*}L(\theta,a)=
	\int L(\theta,a)\mathrm{d}F^{\pi^*}(\theta)
$$
**例 镇痛药的销售（续）**.假设没有获得任何数据，因此 $\theta_2$ 的置信分布简单为：
$$
\begin{flalign*}
\rho(\pi,a)&=\int_0^1 L(\theta_2,a)\pi(\theta_2)\mathrm{d}\theta_2\\
&=\int_0^a 2(a-\theta_2)10I_{(0.1,0.2)}(\theta_2)\mathrm{d}\theta_2\ +\int_a^1 2(\theta_2-a)10I_{(0.1,0.2)}(\theta_2)\mathrm{d}\theta_2 \\
&=\begin{cases}
	0.15-a &if\ a\leq0.1\\
	15a^2-4a+0.3&if\ 0.1\leq a\leq 0.2\\
	2a-0.3 &ifa\geq0.2\\
  \end{cases}
\end{flalign*}
$$
**例 债券购买（续）**.
$$
\begin{align*}
\rho(\pi,a_1)
& =E^\pi L(\theta,a_1)\\
& =L(\theta_1,a_1)\pi(\theta_1)+L(\theta_2,a_1)\pi(\theta_2)\\
&=(-500)(0.9)+(1000)(0.1)=-350\\
\rho(\pi,a_2)
&=E^\pi L(\theta,a_2)\\
&=L(\theta_1,a_2)\pi(\theta_1)+L(\theta_2,a_2)\pi(\theta_2)\\
&=-300
\end{align*}
$$
我们在定义1中使用 $\pi^*$ 而不是 $\pi$，因为 $\pi$ 通常指的是 $\theta$ 的初始分布，而 $\pi^*$ 通常指得到数据后 $\theta$ 的后验分布。注意，这里隐含假设 $a$ 的选择不会影响 $\theta$ 的分布。当行动确实有效果时，可以用 $\pi_a^{*}(\theta)$ 代替 $\pi^*(\theta)$ ，但仍然考虑预期损失。



### 频率学派的风险

**非贝叶斯决策理论学派**，今后将被称为**频率学派**或**古典学派**，采用了一种完全不同的、基于随机变量 $X $ 上均值的预期损失。定义这种预期损失的第一步，是定义一个决策规则(或决策程序)。

**定义** 决策规则$\delta(x)$ 是一个从X到A的映射（函数）。（我们假定引入的函数是适合测量的）如果 $X=x$ 是样本信息中可观测值，那么$\delta(x)$ 则是对应采取的行为。（对一个没有数据的问题，一个决定通常就是一个行为）两个决策规则 $\delta_1$ 和 $\delta_2$ 是视为相等的，当对于所有的 $\theta$ 来说， $P_\theta(\delta_1(X)=\delta_2(X))=1$。

**例 镇痛药的销售（续）**.对于该问题的情况，$\delta(x)=\frac{x}{n}$ 是估计  的标准决策规则。（在估计问题中，一个决策规则通常被称作估计量。）这个估计量并没有用到题目中给的损失函数或者先验信息。

**例 晶体管的检查（续）**决策规则:
$$
\delta(x)=
\begin{cases}
a_1 &if\ x/n\leq0.5\\
a_2 &if\ x/n>0.5
\end{cases}
$$
是这个问题的标准规则类型。

频率主义者的决策理论家试图评估，对于每个$\theta$，如果他在问题中用不同的 $X$ 来反复使用 $\delta(X)$，他“期望”的损失是多少。



## 四、随机决策规则

在某些决策情况下，随机采取行动是必要的。当遇到一个聪明的对手时，这种情况最常见。举个例子，如下面这个叫做“匹配硬币”的游戏。

**例 (匹配硬币)**。你和你的对手要同时揭开一枚硬币。如果两个硬币匹配(即都是正面或反面)，你从对手那里赢得1美元。如果硬币不匹配，你的对手将从你那里赢得1美元。提供给你的行动 $a_1$ ——选择正面,或 $a_2$ ——选择反面。可能的自然状态是 $\theta_1$ ——对手的硬币是正面， $\theta_2$ ——对手的硬币是反面。这个博弈的损失矩阵是:

|            | $a_1$ | $a_2$ |
| ---------- | ----- | ----- |
| $\theta_1$ | -1    | 1     |
| $\theta_2$ | 1     | -1    |

 $a_1$ 和 $a_2$ 都是允许的行为。然而，如果游戏要玩很多次，那么决定只使用 $a_1$ 或 $a_2$ 显然是一个非常糟糕的主意。你聪明的对手会很快意识到你的策略，并选择自己的行动来确保胜利。同样地，对于 $a_1$ 和 $a_2$ 的任何模式选择都能被聪明的对手识别出来，并据此制定出获胜策略。因此，防止终极def的唯一确定方法，是通过某种随机机制选择 $a_1$ 和 $a_2$ 。一个很自然的方法就是选择 $a_1$ 和 $a_2$ ，概率分别是 $p$ 和 $1-p$ 。这种随机决策规则的正式定义如下：

**定义** 随机决策规则 $\delta^*(x,·)$ 是对每个 $x$ ,一个在A上可能的概率分布,如果x被观测到，$\delta^*(x,A)$ 是一个在 $A$ 中行为被选中概率。

一个随机决策规则 $\delta^*(x,·)$ 是，对于每一个 $x$ , A 的概率分布，其解释是，如果 $x$ 被观察到，$\delta^*(x,A)$ 是 $A$ (A的子集)中的一个行动被选择的概率。在无数据问题中，随机决策规则，也称为随机行动，将简单地注意到 $\delta^*(·)$ 也是A上的概率分布。带有h的非随机决策规则被认为是随机规则的一种特殊情况，因为它们对应于随机规则，对于每个x，选择一个概率为1的特定行动。

非随机决策规则将被视为随机规则的一种特殊情况，因为它们与随机规则相对应，对于每个 $x$，选择一个概率为1的特定行动。确实，如果 $\delta(x)$ 是一个非随机决策规则，设 $\langle\delta\rangle$ 表示等价的随机规则:
$$
\langle\delta\rangle(x,A)I_A(\delta(x))=
\begin{cases}
    1&if\ \delta(x)\in A\\
	0 &if\ \delta(x)\notin A\\
  \end{cases}
$$
**例 (匹配硬币)(续)**。在这个例子中讨论的随机行动是由$\delta^*(a_1)=p$ 和$\delta^*(a_2)=1-p$ 定义的。使用前面定义的符号来表示这一点的简便方法是：
$$
\delta=p\langle a_1\rangle+(1-p)\langle a_2\rangle
$$
**定义** 随机规则 $\delta^*$ 的损失函数为 $L(\theta,\delta^*(x,·))$：
$$
L(\theta,\delta^*)=E^{\delta^*(x,·)}[(\theta,\delta^*(x,·))],
$$
期望用 $a$ 表示。 $\delta^*$ 的风险函数为：
$$
R(\theta,\delta^*)=E_\theta^XL[(\theta,\delta^*(x,·))].
$$
**例 (匹配硬币)(续)**由于这是一个没有数据的问题，风险就是损失。那么很明显：
$$
\begin{flalign*}
L(\theta,\delta^*)&=E^{\delta^*}[L(\theta,a)]
=\delta^*(a_1)L(\theta,a_1)+\delta^*(a_2)L(\theta,a_2)\\
&=\rho L(\theta,a_1)+(1-\rho) L(\theta,a_2) \\
&=\begin{cases}
	-\rho +(1-\rho)=1-2\rho &if\ \theta=\theta_1,\\
	\rho -(1-\rho)=2\rho -1 &if\ \theta=\theta_2.\\
  \end{cases}
\end{flalign*}
$$
可以注意到，如果$p=\frac{1}{2}$不管对手怎么做，损失是0。随机规则 $\delta^*$ 与 $\rho=\frac{1}{2}$ 保证了预期损失为零。



## 五、决策原则

> 接下来简要介绍**实际做出决策或选择决策规则**的主要方法。

### 条件贝叶斯决策原理

当我们可以对每个 $a$ 确定贝叶斯预期损失 $\rho(\pi^*,a)$ 时，通常很容易选择最优行动。（做决策时，$\pi ^*$ 是 $\theta$ 的概率分布）

选择一个使 $\rho(\pi^*,a)$ 达到最小的 $a\in A$ ，这样的行动将被称为贝叶斯行动，表示为 $a^{\pi^*}$



**例 镇痛药的销售（续）**.在之前我们推导过：
$$
\begin{flalign*}
\rho(\pi,a)&=\begin{cases}
	0.15-a &if\ a\leq0.1\\
	15a^2-4a+0.3&if\ 0.1\leq a\leq 0.2\\
	2a-0.3 &ifa\geq0.2\\
  \end{cases}
\end{flalign*}
$$
微积分证明了这个函数在 $a$ 上的最小值是 $\frac{1}{3}$ ，此时 $a^\pi=\frac{2}{15}$ 。这个可以成为 $\theta _2$ （新药的市场份额）的估计。（在没有任何可以获取到的数据的情况下）

**例 债券购买（续）**.

在之前我们推导过：$\rho(\pi,a_1)=-350$，$\rho(\pi,a_2)=-300$ 显然 $a_1$ 的贝叶斯预期损失更小，因此是贝叶斯决策。因此，应该购买这些高风险债券。（根据条件贝叶斯原理）



### 频率论的决策原则

我们在前文中提到，使用风险函数来选择决策规则是困难的，因为通常有许多可接受的决策规则(即不能在风险方面占主导地位的决策规则)。为了选择特定的规则使用，必须引入**附加的原则**。在经典统计学中，有许多这样的原则用于发展统计过程:最大似然性、无偏性、最小方差和最小二乘原则等等。在决策理论中，也有一些可能的原则可以使用；最重要三条的是**贝叶斯风险原理，极大极小原理和不变性原理**。本节将阐述这三个原则的基本目标。

#### 贝叶斯风险原理

可以看出，另一种包含先验分布 $\pi$ 的方法，是去看贝叶斯风险 $r(\pi,\delta)=E^\pi R(\theta,\delta)$。由于这是一个数字，我们可以简单地寻找一个决策规则，使它最小化。

**贝叶斯风险原理** 当出现下列情况时，相比于决策 $\delta_2$ ,我们会优先选择决策 $\delta_1$ ：
$$
r(\pi,\delta_1)<r(\pi,\delta_2)
$$
使 $r(\pi,\delta)$最小的决策规则是最优的，这也被称为一个贝叶斯准则，用 $\delta^*$ 表示。参数 $r(\pi)=r(\pi，\delta^\pi)$ 也被叫做作 $\pi$ 的贝叶斯风险。

**例 债券购买（续）**。因为这是一个没有数据的问题，所以决策规则就是简单的动作，而风险函数就是简单的损失函数。因此贝叶斯风险就是贝叶斯期望损失，由此我们解决了问题。

#### 极大极小原理

使用极大极小原理来完全分析问题通常需要考虑随机决策规则。因此让* E沪随机规则,并考虑参数：
$$
sup\ R(\theta,\delta^*)
$$
这个代表，在规则 $\delta^*$ 被使用下，可能发生的最坏情况。如果想要让它不受可能出现的最坏的自然状态的影响，那么就需要：

**极大极小原理** 当出现下列情况时，相比于决策 $\delta_2$ ,我们会优先选择决策 $\delta_1$ ：
$$
sup\ R(\theta,\delta_1^*)<sup\ R(\theta,\delta_1^*
$$
**定义** 决策 $\delta^{*^M}$ 是一个极大极小决策原则，当在所有随机规则（$D^*$）中，它使 $sup\ R(\theta,\delta^*)$ 最小，即：
$$
sup\ R(\theta,\delta^{*^M})=\ inf\ sup\ R(\theta,\delta^*)
$$
上面表达式右边的量称为问题的极大极小值。(将 “$inf$” 替换为 “$min$”，将 “$sup$” 替换为 “$max$” ，这就是 “$minimax$” 这个名字的由来。)对于无数据问题，$minimax$ 决策规则将被简单地称为 $minimax$ 动作。

有时，根据极大极小原则确定最佳非随机规则是很有趣的。如果存在这种最佳规则，则称其为极大极小非随机规则（在无数据问题中被称为极小极大非随机行动)。

**例 债券购买（续）**我们可以很容易得到：
$$
sup\ L(\theta,a_1)=max\{-500,1000\}=1000\\
sup\ L(\theta,a_2)=max\{-300,-300\}=-300
$$
因此 $a_2$ 是极大极小非随机决策。

#### 不变性原理

不变性原理是说，如果两个问题具有相同的形式结构（即具有相同的样本空间、参数空间、密度和损失函数），那么每个问题应采用相同的判定规则。对于一个给定的问题，这一原理是通过考虑问题的变换（例如，度量单位的尺度变化），从而导致结构相同的变换问题。禁止原始问题和转换问题中的决策规则是相同的，这导致了对所谓“不变”决策规则的限制。这类规则通常足够小，因此将存在“最佳不变”决策规则。



## 六、实际应用

> Allen Downey B.在所著的《Think Bayes-O'Reilly Media (2013)》一书中，使用Python代码对贝叶斯决策理论进行了应用。应用基于美国的一档名为 The Price is Right problem 的综艺节目。



### 故事背景

2007年11月1日，两位名叫 $Letia$ 和 $Nathaniel$ 的选手出现在美国游戏节目《猜价格》上。他们参加了一个名为“展柜”的游戏，游戏的目标是猜测展示奖品的价格。最接近展示品实际价格的选手赢得奖品。

 $Nathaniel$ 先出价。他的展柜包括一台洗碗机、一个酒柜、一台笔记本电脑和一辆汽车。他出价26000美元。 $Letia$ 的展柜包括一个弹球机，一款电子街机游戏，一个台球桌和巴哈马的游轮。她出价21500美元。

 $Nathaniel$ 橱窗的实际价格是25347美元。他的出价太高，所以他输了。 $Letia$ 的陈列柜的实际价格是21578美元。她只差了78美元，所以她开始了她的展示，因为她的出价差了不到250美元，她还赢得了 $Nathaniel$ 的展示。

**对于一个“贝叶斯思考者”来说，这个场景暗示了几个问题:**

1. 在看到奖品前，参赛者对橱窗里展品的价格应该有什么先验的想法?

2. 看完奖品后，参赛者应该如何更新自己的想法?

3. 根据后验分布，参赛者应该出价多少?

第三个问题展示了贝叶斯分析的一个常见用途：决策分析。给定一个后验分布，我们可以选择使参赛者的期望收益最大化的报价。



### 先验信息

为了得到一个预先的价格分布，我们需要利用以前的数据。幸运的是，该剧的粉丝们有详细的记录，包括2011和2012赛季的每一个展柜的价格以及参赛选手的出价。

```
/*通过下列Python代码可绘制展柜1、2的价格分布图*/
prices = ReadData()
pdf = thinkbayes.EstimatedPdf(prices)
low, high = 0, 75000
n = 101
xs = numpy.linspace(low, high, n)#返回一个数组，其中包含n个在low和high之间等距的元素
pmf = pdf.MakePmf(xs)

class EstimatedPdf(Pdf):
	def __init__(self, sample):
		self.kde = scipy.stats.gaussian_kde(sample)
	def Density(self, x):
		return self.kde.evaluate(x)
		
class GaussianPdf(Pdf):
	def __init__(self, mu, sigma):
		self.mu = mu
		self.sigma = sigma
	def Density(self, x):
		return scipy.stats.norm.pdf(x, self.mu, self.sigma)

class Pdf(object):
	def Density(self, x):
		raise UnimplementedMethodException()
	def MakePmf(self, xs):
		pmf = Pmf()
		for x in xs:
			pmf.Set(x, self.Density(x))
			pmf.Normalize()
			return pmf
```



<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="C:\Users\86155\AppData\Roaming\Typora\typora-user-images\image-20211018112615892.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图1 2011和2012赛季的展柜价格分布</div>

可以看出，这两个展柜的普遍价格都在2.8万美元左右，但第一个展柜在5万美元处有小型集中分布，第二个展柜的价格在7万美元处有小型集中分布。

这些分布基于实际数据，但已被高斯核密度估计(KDE)平滑。

> 核密度估计(KDE)是一种算法——可以找到一个适当的平滑的概率密度函数（PDF）来拟合样本数据。



### 对选手建模

图1中的概率密度估计了可能的价格分布。如果你是这个节目的选手，你可以用这个分布来量化你之前对每个展柜价格的先验信息(在你看到奖品之前)。

为了更新这些先验信息，我们必须回答以下问题:

1. 我们应该考虑哪些数据？我们应该如何量化这些数据?

2. 我们能不能计算一个似然函数，也就是说，对于每个假设的价格，我们能计算出它们出现的条件可能性吗?

为了回答这些问题，我们把选手作为一个具有已知误差特征的**价格猜测工具**进行建模。换句话说，当选手看到展柜中奖品时，他会逐一猜测每个奖品的价格，然后把价格加起来，我们把这个叫做总猜测。



- **“如果实际价格是price，那么参赛者的估计是guess的可能性有多大?”**

定义：$error = price - guess$

- “**参赛者的估计出错的可能性有多大?**”

 定义：$diff = price - bid$



当 $diff$ 为负时，表示出价太高。顺便提一下，我们可以使用这个分布来计算选手出价过高的概率：第一个选手出价过高的概率为25%；第二名选手出价过高的概率为29%。

> 我们还可以看到竞价是有偏见的;也就是说，它们更有可能太低而不是太高。考虑到游戏规则（后出价的选手给出的价格不得高于前面的人），这是有道理的。



最后，我们可以利用这个分布来估计选手猜测的可靠性。

这一步有点棘手，因为我们并不知道参赛者的猜测（$guess$）；我们只知道他们出价（$bid$）是多少。所以我们要做出假设：误差（$error$）服从是高斯分布，均值为0，方差与 $diff$ 相同。

<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="C:\Users\86155\AppData\Roaming\Typora\typora-user-images\image-20211018113400085.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图2 选手的报价（bid）与实际价格（price）之间的差额（diff）的累积分布（CDF）</div>

我们用 $diff$ 的方差来估计 $error$ 的方差。

> 这个估计并不完美，因为竞争者的报价有时是战略性的；例如，如果玩家2认为玩家1出价过高，玩家2可能会出很低的价。在这种情况下，$diff$ 不能反映 $error$ 。如果这种情况经常发生，$diff$ 的方差可能会过高估计 $error$ 的方差。尽管如此，在一定误差的范围内，作者认为这是一个合理的建模决策。

另一种方法是，准备上节目的人可以通过观看往期节目，记录下他们的猜测（$guess$）和实际价格（$price$）进而估计他们自己的误差（$error$）分布



### 各种误差（结果）的可能性

现在我们可以写出似然函数了。

```
class Price(thinkbayes.Suite):
	#pmf代表先验分布，player属于之前定义的Player对象
	def __init__(self, pmf, player):
		thinkbayes.Suite.__init__(self, pmf)
		self.player = player
		
def Likelihood(self, data, hypo):
	price = hypo
	guess = data
	error = price - guess
	#hypo是橱窗的假设价格。data是选手对价格的最佳猜测。error是差异，like是假设下的data的可能性。
	
	like = self.player.ErrorDensity(error)
	#ErrorDensity的工作原理是在给定的error值上计算它的概率密度分布
	
	return like
```

结果（$like$）是一个概率密度值，不是一个概率值。但这样做不会对结果产生任何不良影响，因为它是跟概率成比例的量，当我们对后验分布进行归一化时，它被约去了。

因此，我们可以说，概率密度是一种很好的可能性。



### 更新决策信息

```
#Player提供了一种方法，取参赛者的猜测(guess)并计算后验分布:
class Player
	def MakeBeliefs(self, guess):
		pmf = self.PmfPrice()
		#PmfPrice生成一个离散的近似的价格的PDF，我们使用它来构建先验。
		self.prior = Price(pmf, self)
		self.posterior = self.prior.Copy()
		self.posterior.Update(guess)
		#Update为每个假设调用Likelihood，将先验乘以可能性，并重新标准化。
		
# class Player
	n = 101
	price_xs = numpy.linspace(0, 75000, n)
	#PmfPrice使用MakePmf(), 它按一系列值计算pdf_price:
	def PmfPrice(self):
		return self.pdf_price.MakePmf(self.price_xs)		
```

图3显示了关于人们对实际价格的先验和后验信念。

> 后验分布向左偏移因为人们的猜测是在先前范围的底端。

在某种程度上，这个结果是有道理的。最可能的先验价值是27750美元，最好的猜测是20000美元，而后验的均值介于两者之间：25096美元。

在另一个层面上，你可能会发现这个结果很奇怪，因为它表明，如果你**认为**价格是2万美元，那么你应该**相信**价格是2.4万美元。

> 要解决这个明显的矛盾，请记住，您是在结合两种信息来源，关于过去展览的历史数据和对您看到的奖品的猜测。

<center>   
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="C:\Users\86155\AppData\Roaming\Typora\typora-user-images\image-20211019174208936.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图3 玩家1的先验后验分布</div>
</center>

我们**将历史数据视为先验数据，并根据您的猜测更新它，但我们也可以同样地将您的猜测作为先验数据，并根据历史数据更新它。**如果你这样想，也许最可能的后验价格就不是你最初的猜测就不那么令人惊讶了。



### 最优报价

对计算最优报价生成一个类：GainCalculator 

```
class GainCalculator(object):
	def __init__(self, player, opponent):
		self.player = player
		self.opponent = opponent
		
	#定义EXpectedGains计算每一种报价以及每一个报价的预期收益
	def ExpectedGains(self, low=0, high=75000, n=101):
		bids = numpy.linspace(low, high, n)
		#报价在区间(low,high)中取值，个数为n
		
		gains = [self.ExpectedGain(bid) for bid in bids]
		return bids, gains	
	
    #定义EXpectedGain计算给定报价的预期收益
	def ExpectedGain(self, bid):
		suite = self.player.posterior
		total = 0
		for price, prob in sorted(suite.Items()):
			gain = self.Gain(bid, price)
			total += prob * gain
		return total
```



<center>
    <img style="border-radius: 0.3125em;
    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);" 
    src="C:\Users\86155\AppData\Roaming\Typora\typora-user-images\image-20211019180840168.png">
    <br>
    <div style="color:orange; border-bottom: 1px solid #d9d9d9;
    display: inline-block;
    color: #999;
    padding: 2px;">图4 玩家1的最佳猜测是2万美元；玩家2的最佳猜测是4万美元。</div>

上图显示了两个玩家的结果，基于玩家1的最佳猜测是2万美元，玩家2的最佳猜测是4万美元。

- 玩家1的最优出价是21000美元，预期回报率接近16700美元。

在这种情况下(结果是不寻常的)，最优出价实际上比竞争者的最佳猜测要高。

- 玩家2的最优出价是31500美元，预期回报率接近19400美元。

这是最优出价低于最佳猜测的典型情况。



### 对此次建模的讨论

贝叶斯估计的特征之一是其结果以后验分布的形式出现。经典估计通常生成一个单点估计或置信区间，如果估计是过程的最后一步，这就足够了，但如果您想使用一个估计作为后续分析的输入，点估计和区间通常没有多大帮助。

在这个例子中，我们使用后验分布来计算最优报价。给定出价的回报是不对称的和不连续的(如果你出价过高，你就会损失)，所以很难用分析的方法解决这个问题。但是计算起来相对简单。

尝试贝叶斯的新手常常试图通过计算均值或最大似然估计来总结后验分布。这些总结可能很有用，但如果你只需要这些，那么可能一开始你就不需要贝叶斯方法。

贝叶斯方法是最有用的，你可以**将后验分布带入分析的下一个步骤来执行一些决策分析**，就像上述我们对《猜价格》节目所做的分析。

