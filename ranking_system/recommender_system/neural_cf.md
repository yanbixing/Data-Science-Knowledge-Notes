# Neural collaborative filtering



- Ref: 
  - [Zhihu](https://zhuanlan.zhihu.com/p/31122667), [Blog](https://www.cxyzjd.com/article/zhuhongming123/106665628): mechanism
  - [1p3a](https://www.1point3acres.com/bbs/thread-520357-1-1.html), [1p3a](https://www.1point3acres.com/bbs/forum.php?mod=viewthread&tid=519657&ctid=232594): interview question

<div  align="center"><img src= https://pic2.zhimg.com/v2-f2325c6555f1f578c7f888529cbbc727_1440w.jpg?source=172ae18b style = "zoom:100%"> </div>

- With the **one-hot encoding** of user m and item n as the input, with the score $r_{mn}$ as output label, train a network.
  - There will a weight matrix $W_{M \times V_u}$ to project the $M$-dim user one-hot encoding to a $V_u$-dim **user embedding**.
  - There will be a weight matrix $W_{N \times V_i}$ to project the $N$-dim user one-hot encoding to a $V_i$-dim **item embedding**.
  - The user and item embedding will be concatenated together as the input of the neural network.