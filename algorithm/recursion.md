# Recursion


## Deep Dive:

### Personal understanding: TopDown vs BottomUp

- The top down process is the part before the self.recur_func is called in its def
- The bottom up process is the part before the self.recur_func is called in its def

``` python
def recur_func(self,node):
    
    # Top down part: the part before the self.recur_func() is called
    # These code will be executed from root to leaves

    #   Since code in this part is executed before the same part of code is executed on its child
    
    self.recur_func(node.child)
    
    # Bottom up part: the part after the self.recur_func() is called
    # these code will be executed from leave to root
    
    #   Since the parent's code in this part is only executed after the recur_func (the same part code) is fully executed on its child
    
    return something
```