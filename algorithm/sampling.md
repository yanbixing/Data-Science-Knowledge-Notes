# Sampling

## Bernoulli sampling:

Run a bernoulli trial on each sample, accept if trial give true else recject.

[Wiki-Bernoulli_sampling]:(https://en.wikipedia.org/wiki/Bernoulli_sampling)



## Reservoir sampling:

Algorithm: 
```
K denotes the size of the memory (reservoir). (1-indexed)
i denotes the idx (order) of item in the stream. (i starts from 1)
N denotes the total number of items in the steam.

When i<=K:
    just put value into mem[i]

When i>K, 
    Generate a random number j among $[1,i]$
    If j<=k:
        Replace the j-th sample in the reservoir with the i-th sample
        i.e mem[j] = num[i]
```
					
**Proof**:

From the algorithm, we have the following facts:

- When i<=k, we don't need to exclude items, each item have equal prob to be included in the reservoir.
- When i>k, according to the algo:
  - the prob i-th element to be include in reservoir is k/i
    - Because when random sample $j \in [1,i]$, $\Pr(j\leq K) = \frac{K}{i}$
  - the prob that $x$-th element in the reservoir is replaced by the i-th element is $\frac{1}{i}$
    - Because only when $j=x$, the x-th item is replaced
    - random sample $j\in[1,i]$, $\Pr(j=x) = \frac{1}{i}$

After the stream ends, "i-th item exists in mem" need to satisfy:  

- in the i-th round, when i-th item is passed in, it is included in the mem
  - i.e. the random number j in the i-th round $\leq K$
  - i.e. the probability is $\frac{K}{i}$
  - Assume  $j = x$

- in the following rounds, it is not replaced by the following items. 
  - i.e. in each following round, it is not replaced by the income item.
  - i.e. in $r$-th round, the probability of the $x$-th item is not replaced is $\frac{r-1}{r}$, i.e. as long as random number $j \neq x$ ($j\in[1,r]$), mem[x] will not be replaced.

So, the prob that "i-th item exist in mem after stream ends" is:
$$\begin{aligned}
&\Pr({\text{ i-item in mem}})\\
=&\Pr({\text{ i-item is included in mem in the i-th round}})\\
\cdot &\Pr(\text{the i-th item is not replaced in the (i+1)-th round})\\
\cdot &\Pr(\text{the i-th item is not replaced in the (i+2)-th round})\\
\vdots\\
\cdot &\Pr(\text{the i-th item is not replaced in the (N)-th round})\\
=&(K/i)*[1-1/(i+1)][1-1/(i+2)]……[1-1/N]\\
=&(K/i)*[i/(i+1)][(i+1)/(i+2)]……[(N-1)/N]\\
=&K/N
\end{aligned}$$

From the above proof, we can see that each element have equal prob to be included in the reservoir.

Interview Qs Ref:
    - [A-AS-Intern](https://www.1point3acres.com/bbs/thread-701069-1-1.html): check 2021-InterviewPrep
    - [L-MLE-VO]()


### Similar: Randomization

