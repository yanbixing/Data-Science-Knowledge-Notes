# Join operation

## 1. Reduce-side Join vs Map-side Join

### 1.1. Reduce-side Join

**Naming:** The join operation is performed by reducer. For each reduce task, reducer receive all the join-values for a same join key and perform the join operation.


Reduce-side join is also called

  - **sort-merge join**
    - Ref: [Quora](https://www.quora.com/What-is-map-side-join-and-reduce-side-join-Which-one-is-better)
  - It is the normal/default join method
    - Ref:[Edureka](https://www.edureka.co/blog/map-side-join-vs-join/)

The workflow of reduce join is like:

<div  align="center"><img src=https://www.edureka.co/blog/wp-content/uploads/2013/11/joins1.png style = "zoom:100%"></div> 


- **Map stage**: mapper read table and return (join key, join value) pairs to an intermediate file
    ```
    Table1 -> (k,v) = (Id, ColA)
        E.g [(1,A1),(1,A3),….(2,A11)(2,A9)(2,A10)…]
    Table2 -> (k,v) = (Id, ColB)
        E.g [(1,B2),(1,B4),….(2,B99)(2,B3)…]
    ```
- **Shuffle stage**: the intermediate file is sorted and merged/aggregated/group by the "join key", as a list of values corresponding to each key.
    ```
    id: [ColA_values, ColB_values] = 
    [
        {1: [ ColA:[A1,A3], ColB: [B2,B4]}
        {2: [ ColA: [A11, A9, A10], ColB:[B99,B3]]
        {3:….}
    ]
    ```
- **Reduce stage**: operate the join operation within the group of each key. And output the final table.
    ```
    id:[ (colA,ColB) pairs ] = 
    [
        {1: [(A1,B2), (A1,B4), (A3,B2), (A3,B4)] }
        {2: [ (A11,B99),(A11,B3), (A9,B99), 
        (A9,B3), (A10,B99), (A10,B3)]}
        {3:….}
    ]
    => (id, ColA, ColB)
    ```

Alternative illustration

<div  align="center"><img src=https://www.edureka.co/blog/wp-content/uploads/2016/11/Reduce-Side-Join-Edureka-1.png style = "zoom:60%"></div> 

Example illustration:

<div  align="center"><img src=https://www.guru99.com/images/Big_Data/061114_1003_Introductio1.png style = "zoom:80%"></div> 


Ref:
- [PersonalBlog](https://www.guru99.com/introduction-to-counters-joins-in-map-reduce.html): Detailed example and corresponding illustration.
- [Edureka](https://www.edureka.co/blog/map-side-join-vs-join/): illusration and overall description
- [Edureka](https://www.edureka.co/blog/mapreduce-example-reduce-side-join/): alternative illustration and overall description
- [DataFlair](https://data-flair.training/forums/topic/explain-reduce-side-joins/): overall description.


### 1.2. Map side Join

**Naming:** The join operation is performed by **mapper**, there is no reducer during the process. 

Specifically, The smaller table will be **broadcasted** to all worker nodes, and join operation is just performed on mapper, between: "small table (copy)" <-> "partition of large table". That is why map-side join is a lso called "**broadcast join**." (TBD, Ref?)


The workflow or map-side join is like:

<div  align="center"><img src=https://www.edureka.co/blog/wp-content/uploads/2013/11/joins2.png style = "zoom:80%"></div> 




Ref:


- [Edureka](https://www.edureka.co/blog/map-side-join-vs-join/): illusration and overall description of map side join
- [Medium](https://medium.com/datakaresolutions/optimize-spark-sql-joins-c81b4e3ed7da): descriptioin of boradcast join.


### 1.3. Reduce-side join vs Map-side join

- Table size
  - **Map-side join** is usually used when **one data set is large and the other data set is small**. 
  - Whereas the Reduce side join can join both the large data sets.
- Speed (when one table is much smaller than another, or we cannot use map-side join when both table are large.)
  - Map-side join is faster: NO need to wait for all mappers to complete 
  - Reduce side join is slower: need to wait for all mappers to complete


Ref: [ClouderaCommunity](https://community.cloudera.com/t5/Support-Questions/Differentiate-between-Map-Side-join-and-Reduce-side-Join-in/td-p/237174#:~:text=Map%20side%20join%20is%20usually,reduce%20side%20join%20is%20slower.)
