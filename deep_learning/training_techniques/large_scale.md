# Large Scale

Ref: [CSDN](https://blog.csdn.net/qq_32690999/article/details/78642838), [Jianshu-Blog](https://www.jianshu.com/p/ccce391c9e45)

## 1. Map-reduce

 - Map: Distributed data to different machines, calculated statistical value for each sample/batch.
 - Reduce: combine those results together to update model.

Gradient descent can be map-reduced. (we can calculate gradient for each batch.)

## 2. Batch-gradient descent

- GD: use all data (not possible for large scale), also called "full-batch GD".
- Batch GD: use part of data (batch) each time. Also called "mini-batch GD"
- Stochastic GD: use one sample each time.
  - A kind of online learning method, also called "Online GD"
  - Ref: [StackExchange](https://stats.stackexchange.com/questions/167088/stochastic-gradient-descent-vs-online-gradient-descent), [StackExchange](https://stats.stackexchange.com/questions/110078/full-batch-vs-online-learning-vs-mini-batch), [batch_normalization.md](./batch_normalization.md)