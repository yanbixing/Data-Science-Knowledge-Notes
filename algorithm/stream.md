# Stream

Assume there is a stream (indexed from 1 to N), i.e. $\mathrm{stream} = [x_1,\dots, x_N ]$

## 1. Stream statistics

### 1.1. Average

#### 1.1.1. Deduction

Average is calculated via sum:

$\mathrm{avg}(i) = \frac{1}{t} \sum^i_{t=1} x_t $
$\begin{aligned}
    \mathrm{avg}(i+1) =& \frac{1}{i+1} \sum^{i+1}_{t=1} x_t = \frac{1}{i+1} \left( \sum^{i}_{t=1} x_t + x_{i+1} \right) \\\
    =&  \frac{i}{i+1} \left( \frac{1}{i} \sum^{i}_{t=1} x_t \right) + \frac{1}{i+1}x_{i+1} \\
    =& \frac{i}{i+1} \mathrm{avg}(i)  + \frac{1}{i+1}x_{i+1}\\
    =& \mathrm{avg}(i) - \frac{1}{i+1} \mathrm{avg}(i)  + \frac{1}{i+1}x_{i+1} 
\end{aligned} $

#### 1.1.2. Implementation

**recursion algo:**

$\mathrm{avg}(i) = \mathrm{avg}(i-1) - \frac{1}{i} \mathrm{avg}(i-1)  + \frac{1}{i}x_{i} $

Initialization:

$\mathrm{avg}(0) = 0 \Leftarrow \mathrm{avg}(1) = x_1$

**Code:**

```python
def get_cur_avg(prev_avg,idx,val):
    """Note: idx starts from 1"""
    cur_avg = prev_avg - (1/idx)*prev_avg + (1/idx)*val
    return cur_avg

def generate_avg(stream):
    
    N = len(stream)
    prev_avg = 0
    idx = 1
    
    while idx <= N:
        # Note: i-th item in stream is stream[i-1]
        cur_avg =  get_cur_avg(prev_avg,idx,stream[idx-1])
        yield cur_avg
        
        idx+=1
        prev_avg = cur_avg

# test
stream = [1,2,3,4,5]
for i,avg in enumerate(generate_avg(stream)):
    print(i,avg)
```

Verification code:
```python
# verify res
import numpy as np

stream = []

for i in range(1,6):
  stream.append(i)
  print( np.mean(stream) )
```

Playground: [MyColab](https://colab.research.google.com/drive/1f1fXyEhhpHOp_XCX7qOMrtR-kxXvtl0F?authuser=1#scrollTo=43O5DlYGwg8X&line=6&uniqifier=1)

- Ref: 
  - [Github-blog](https://gist.github.com/pfmiles/2641160), [StackExchange](https://math.stackexchange.com/questions/445076/continuously-averaging-a-stream-of-numbers)
  - [CSDN](https://blog.csdn.net/yutianzuijin/article/details/74276897): Similar case, stream with window
  - [1p3a](https://www.1point3acres.com/bbs/thread-710489-1-1.html): interview Q of L


### 1.2. Variance

#### 1.2.1. Deduction

Denote: $\mu_i := \mathrm{avg}(i) = \sum^{i}_{t=1} x_t$

$\mathrm{Var}(i) = \frac{1}{i} \sum^i_{t=1} (x_t - \mu_i)^2$
$\begin{aligned}
    \mathrm{Var}(i+1) =& \frac{1}{i+1} \sum^{i+1}_{t=1} (x_t - \mu_{i+1})^2\\
    =& \frac{1}{i+1} \sum^{i}_{t=1} (x_t - \mu_{i+1})^2 + \frac{1}{i+1}(x_{i+1} - \mu_{i+1})\\
    =& \frac{1}{i+1} \sum^{i}_{t=1}[(x_t - \mu_i) + (\mu_i - \mu_{i+1})]^2+ \frac{1}{i+1}(x_{i+1} - \mu_{i+1}) \\
    =& \frac{1}{i+1} \sum^{i}_{t=1}(x_t - \mu_i)^2 + \frac{1}{i+1} \sum^{i}_{t=1} 2(x_t - \mu_i)(\mu_i - \mu_{i+1}) \\
    & + \frac{1}{i+1} \sum^{i}_{t=1}(\mu_i - \mu_{i+1})^2+ \frac{1}{i+1}(x_{i+1} - \mu_{i+1})
\end{aligned}$

Analyze the first terms one by one

- Frist term:
  - $\frac{1}{i+1} \sum^{i}_{t=1}(x_t - \mu_i)^2 = \frac{i}{i+1} \frac{1}{i}\sum^{i}_{t=1}(x_t - \mu_i)^2 = \frac{i}{i+1} \mathrm{Var}(i)$
- Note:
  - $\mu_i, \mu_{i+1}$ is constant (independent) to variable $t$
  - $\mu_i := \frac{1}{i}\sum^i_{t=1} x_t \Rightarrow \sum^i_{t=1} x_t = i\mu_i \Rightarrow \sum^i_{t=1} (x_t-\mu_i) = 0$
- So, we have second term as:
  - $\frac{1}{i+1}\sum^{i}_{t=1} 2(x_t - \mu_i)(\mu_i - \mu_{i+1}) = \frac{1}{i+1} 2(\mu_i - \mu_{i+1}) \sum^{i}_{t=1} (x_t - \mu_i)= 0$
- And the tird term: 
  - $\frac{1}{i+1} \sum^{i}_{t=1}(\mu_i - \mu_{i+1})^2 = \frac{i}{i+1} (\mu_i - \mu_{i+1})^2$

Thus:

$\mathrm{Var}(i+1) = \frac{i}{i+1} \mathrm{Var}(i) + \frac{i}{i+1} (\mu_i - \mu_{i+1})^2 + \frac{1}{i+1}(x_{i+1} - \mu_{i+1})$



#### 1.2.2. Implementation

**recursion relation:**

$\mathrm{Var}(i) = \frac{i-1}{i} \mathrm{Var}(i-1) + \frac{i-1}{i} [\mathrm{avg}(i-1) - \mathrm{avg}(i)]^2 + \frac{1}{i}[x_{i} - \mathrm{avg}(i)]^2$

Initialization:

$\mathrm{Var}(0) = 0 \Leftarrow \mathrm{Var}(1) = (x_1 - \mu_1)^2$

**Code:**


```python
def get_cur_var(prev_var, prev_avg, cur_avg, idx, val):
    """Note: idx starts from 1"""
    term1 = (idx-1)/idx * prev_var
    term2 = (idx-1)/idx * (prev_avg - cur_avg)**2
    term3 = 1/idx * (val-cur_avg)**2
    cur_avg = term1+term2+term3
    return cur_avg

def generate_var(stream):
    
    N = len(stream)
    prev_avg = 0
    prev_var = 0
    idx = 1
    
    while idx <= N:
        # Note: i-th item in stream is stream[i-1]
        cur_avg = get_cur_avg(prev_avg,idx,stream[idx-1])
        cur_var = get_cur_var(prev_var,prev_avg,cur_avg,idx,stream[idx-1])
        yield cur_var
        
        idx+=1
        prev_avg = cur_avg
        prev_var = cur_var

# test
stream = [1,2,3,4,5]
for i,var in enumerate(generate_var(stream)):
    print(i,var)
```

Verification code:
```python
# verify res
import numpy as np

stream = []

for i in range(1,6):
  stream.append(i)
  print( np.var(stream) )
```

- Ref:
  - [StackExchange](https://math.stackexchange.com/questions/20593/calculate-variance-from-a-stream-of-sample-values): not sure correct or not.

### 1.3. Median

Rely on min_heap + max_heap, logN

Ref:
    [LC]()
    [LC]()

## 2. Stream sampling

### 2.1. Reservoir sampling

See [sampling.md](./sampling.md)