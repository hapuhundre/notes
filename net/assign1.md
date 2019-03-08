#### 1. 报文交换时长计算
**A->C**  
其中报文大小$M_1$=2Mbits，链路带宽$R_1$、$R_2$、$R_3$分别为10Mbps，20Mbps，10Mbps

$$t_{AC} = M_1/R_1+M_1/R_2+M_1/R_3$$
$$=0.5s$$

**B->D**  
这里有个问题，因为A->C是早于B->D 0.1秒发货，会在左侧路由处排队等候，因此发送时间为

$$t_{BD} = M_2/R_1+(M_1+M_2)/R_2+M_2/R_3$$
$$=0.35s$$

#### 2. 分组交换时长计算
$$T = M/R_1+nL/R_2$$

L = 1000bit
n=2

$$t_{AC} = $$
$$t_{BD} = +0.1s  = $$
#### 3. 交换方式比较
