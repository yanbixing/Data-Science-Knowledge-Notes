# Transformer

Ref: 
- [Github-blog](http://jalammar.github.io/illustrated-transformer/)
- [Wechat](https://mp.weixin.qq.com/s?__biz=MzI1NjQ0Mzc1Mw==&mid=2247496823&idx=1&sn=7517e2b8c225b79b2bb54ae6a5c40cf7&source=41#wechat_redirect)


## Interview Qs

- Transformer characteristics
  - Self-attention, capture the word-word (long-term) dependency
  - Position embedding, encoding the info of word position. So that different order give different output.

- Advantage of CNN(TBD)
  - Smaller number of parameters
  - Faster
  - Better performance
  - Ref: [CSDN](https://blog.csdn.net/qq_41558173/article/details/105602022)
  

- Transformer vs RNN/LSTM 
  - Transformer is parallel, non-sequential; 
    - RNN is sequential, slower
  - Transformer uses self-attention, the long-term dependency is kept (depdency are not affected by distance.)
    - Naive-RNN only have hidden state, LSTM use cell, slightly better, but both of them will lose very long term dependency
  - Transformer have positional embedding to denote order information. So words with different order have different output. No problem.
    - RNN run sequentially, inherently have an order. 
  - Ref: [StackExhcange](https://ai.stackexchange.com/questions/20075/why-does-the-transformer-do-better-than-rnn-and-lstm-in-long-range-context-depen/20084)

- Transformer vs CNN
  - Transformer can capture dependency over the whole sentence
    - CNN only capture the dependency within the kernel size.
  - Ref: [StackExhcange](https://ai.stackexchange.com/questions/20075/why-does-the-transformer-do-better-than-rnn-and-lstm-in-long-range-context-depen/20084)
  

- My unsolved Qs
  - Attention 为了解决什么: long term dependency problem; sequential computing slow problem;
  - Attention之前模型是什么: LSTM? CNN for Text?



## 0. Basics

### 0.0 Notation:

- $\boldsymbol{x}_i$: the input (embedding) of $i$-th position
- Attention:
  - $\boldsymbol{k}_i$: the key vector of i-th position. 
    - The "reference key" of i-th position. Used when referred by other positions.
  - $\boldsymbol{q}_i$: the query vector of i-th position.
    - The "identity" of i-th position. Only used when generate i-th position's output.
  - $\boldsymbol{v}_i$: the value vector of i-th position. The value to be weighted pooled.
    - If not use self-attention, usually is the word embedding.

### 0.1. Attention Mechanism

The purpose of attention is to allow the representation of a word able to change with different context.

#### 0.1.1. Attention

Attention is like a weighted sum of info (value $\boldsymbol{v}$) from different position in the sequence. The output of position $i$ equals:

$$\begin{cases}
    \boldsymbol{z}_i = \sum_{\color{red}{j}} w_{i{\color{red}{j}}} \boldsymbol{v}_{\color{red}{j}}
    \\
    w_{i{\color{red}{j}}} =  \boldsymbol{k}_{\color{red}{j}} \cdot \boldsymbol{q}_i
\end{cases}$$

or 

$$ \boldsymbol{z}_i = \sum_{\color{red}{j}} ( \boldsymbol{k}_{\color{red}{j}} \cdot \boldsymbol{q}_i) \boldsymbol{v}_{\color{red}{j}} $$ 

Note: usually we will apply softmax function on weight to normalize-it/suppress-extreme-value, i.e. $\boldsymbol{z}_i = \sum_{\color{red}{j}} softmax( w_{i{\color{red}{j}}}) \boldsymbol{v}_{\color{red}{j}}$
#### 0.1.2 Self-Attention

Self-attention, the $\boldsymbol{k}_i,\boldsymbol{q}_i,\boldsymbol{v}_i$ are all included in the embedding $\boldsymbol{x}_i$. The embedding itself contain all the info, that is why it is called "self"-attention. E.g. In BERT:
$$\begin{cases}
    \boldsymbol{k}_i = \boldsymbol{W^K} \boldsymbol{x}_i\\
    \boldsymbol{q}_i = \boldsymbol{W^Q} \boldsymbol{x}_i\\
    \boldsymbol{v}_i = \boldsymbol{W^V} \boldsymbol{x}_i
\end{cases}$$

where, $\boldsymbol{W^K}$ $\boldsymbol{W^Q}$ $\boldsymbol{W^V}$ denotes the matrix to generate $\boldsymbol{k}_i,\boldsymbol{q}_i,\boldsymbol{v}_i$ from $\boldsymbol{x}_i$, they are also trained during during training process.

#### 0.1.3. Self-Attention in Bert

**The self-attention part in BERT:**

Ref: [Github blog](http://jalammar.github.io/illustrated-transformer/)


<div  align="center"><img src=http://jalammar.github.io/images/t/encoder_with_tensors.png
 style = "zoom:40%"></div> 


**Calculation of k,q,v:**

<div  align="center"><img src=http://jalammar.github.io/images/t/transformer_self_attention_vectors.png
 style = "zoom:50%"></div> 

 **Generate output:**

 <div  align="center"><img src=http://jalammar.github.io/images/t/self-attention-output.png
 style = "zoom:50%"></div> 


### 0.2. Positional Encoding

#### 0.2.1 Motivation

In RNN, we will
- input word1 ($x_1$) into the RNN, generate $h_1$; 
- input word2 ($x_2$) and $h_1$ into RNN, generate $h_2$;
- ...Finally, during the process, we will generate $y_1, y_2$, or use $y_{-1}$ or use $h_{-1}$
So, the order of the words matter. The output is affected by order of words.

However, Transformer, we will input the whole sentence together, the attentions of each words are calculated parallelly, then how to know position of word? Ref:  we have a matrix representation of our sequence. [Medium-blog: "However, these representations are not encoding the fact that words appear in different positions." ](https://medium.com/dissecting-bert/dissecting-bert-part-1-d3c3d495cdb3)

- Personal Understanding: 
  - Since transformer is parallelly input, it is faster than RNN (Under same model complexity/size. But, BERT is usually larger...)
  - CNN is also parallelly input. The way CNN differentiate pixel at different position is kernel: still process info locally (several pixels) not globally (the whole picture/doc). 
    - However, BERT process the whole doc (globally) at same time, thus, we need position encoding to differentiate words at different loc.

Positional encoding is brought to solve the problem. For each embedding, we will add a positional embedding (vector) to its current embedding. Then, for same words at different position, the embedding will be different, and thus, we can bring positional info to the transformer.



#### 0.2.2 Details

The positional embedding is a embedding only depends on position. (It can be either predefined or learnable. Ref: [Arxiv](https://arxiv.org/abs/2010.04903))

**Purpose:** Add position info to the word embedding. Same word at different position will have different embedding, different info.

**Note:** The position embedding have a same size with word embedding. It is added to word embedding in element-wise way (embedding dimension NOT change. NOT concatenation).

- Why? seems no formal answer yet. Ref: [Personal blog](https://kazemnejad.com/blog/transformer_architecture_positional_encoding/)
  - Intuitive understanding is 
    - Summation could save memory.
    - I.e. you want the embedding could change with position, not change the size of embedding. 
      - Let's say your word embedding is dim 128. So, if you concatenate, the size become 256, you will also want another layer to transform the 256 to a 128 dim vector that contain both position and word info. 
    - I.e. In practice, they found summation could do the job well, just not need to use concatenation.


<!-- Ref:
- [Personal blog](https://kazemnejad.com/blog/transformer_architecture_positional_encoding/) -->

 <div  align="center"><img src=https://d33wubrfki0l68.cloudfront.net/a76be57763d942798f8081b77edf8f078720cd45/bd31a/img/transformer_architecture_positional_encoding/model_arc.jpg
 style = "zoom:40%"></div> 





### 0.3 Multi-Head

Multi-head means you create multiple  versions of attention, each of them will generate output $z_i$ independently. These **output will be concatenated** together as the final output. E.g. 8 versions in BERT:

$$\boldsymbol{z}_i = [\boldsymbol{z}_{(1,Att1)},\dots, \boldsymbol{z}_{(1,Att8)}] $$

Note: the num_col of final $\boldsymbol{z}$ is 8 times of an individual attention head. The weight has 8 times of num_row to the single-head cases. 

 <div  align="center"><img src=http://jalammar.github.io/images/t/transformer_multi-headed_self-attention-recap.png
 style = "zoom:40%"></div> 


## 1. General transformer introduction

### 1.1.  structure

The transformer is actually an encoder-decoder system. 

<div  align="center"><img src=http://jalammar.github.io/images/t/The_transformer_encoders_decoders.png
 style = "zoom:50%"></div> 

 Each encoder is a sequential stack of multiple encoder unis. (The same for decoder part.)

<div  align="center"><img src=http://jalammar.github.io/images/t/The_transformer_encoder_decoder_stack.png
 style = "zoom:50%"></div> 

Each encoder units is composed from a self-attention layer and a feed forward layer:

<div  align="center"><img src=http://jalammar.github.io/images/t/Transformer_encoder.png
 style = "zoom:50%"></div> 

Decoder is similar. But decoder also receive attention info from the encoder to make decision, realized by one additional "encoder-decoder attention layer":

<div  align="center"><img src=http://jalammar.github.io/images/t/Transformer_decoder.png style = "zoom:50%"></div> 




 ### 1.2 How transformer works (TBD)

**How the whole sentence is put into a BERT model?**

1. The whole input sentence is fed to the encoder word by word first.
    <div  align="center"><img src=http://jalammar.github.io/images/t/transformer_decoding_1.gif style = "zoom:40%"></div> 

2. Then, the decoder start to work, generate words step by step. And, the output of each step is fed to the bottom decoder in the next time step. 
   - I.e. The decoder not only receive info from encoder, like RNN the output of previous step will also feed to next step.

  <div  align="center"><img src=http://jalammar.github.io/images/t/transformer_decoding_2.gif style = "zoom:40%"></div> 

Note (TBD): different words will go through **same** attention and feed forward layer. This is reasonable, like recurrent neural network, the network will not change with time/position, or in CV, the kernel will not change with position. Don't be confused by the figure below:

Note: actually, in BERT, the info (embedding) from encoder is fed to the "encoder-decoder" layers of the decoder.

<div  align="center"><img src=http://jalammar.github.io/images/t/transformer_resideual_layer_norm_3.png style = "zoom:40%"></div> 


## 2. BERT Specifics

### 2.1. Training

- Training method: Masked Language modeling
  - Mask some positions in the document(replace the word with a [mask] token), and predict the possibility of different token with the context words.
  - Ref: [TowardsDataScience](https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270)
  <div  align="center"><img src=https://miro.medium.com/max/1400/0*ViwaI3Vvbnd-CJSQ.png style = "zoom:60%"></div> 
- Loss: **cross entropy** on the masked position, other positions are ignored.
  - Here, CrossEntropy = $1\cdot \log \hat{p}(\tau_{masked}) $
    - $H(p^*(\tau),\hat{p}(\tau)) = \sum_{\tau}p^*(\tau)\log \hat{p}(\tau)$
      - $p^*(\tau)$: the one-hot encoding of the masked token:at the position of the ground true token $\tau_masked$ the value is 1, at position of other tokens the value is 0.
      - $\hat{p}(\tau)$: prob distribution of different possible tokens
      - CrossEntropy ref: [entropy_related_statistics.md](../../general_machine_learning/math_topics/entropy_related_statistics.md),[Wiki-CrossEntropy](https://en.wikipedia.org/wiki/Cross_entropy)
  - Ref: , [TowardsDataScience](https://towardsdatascience.com/masked-language-modelling-with-bert-7d49793e5d2c),[Huggingface](https://discuss.huggingface.co/t/bertformaskedlm-s-loss-and-scores-how-the-loss-is-computed/607)

#### 2.2. Embedding
  
- Word embedding:
  - concatenation or element-wise sum of embedding from different layers (usually last four layers.)
- Sentence embedding:
  - element-wise superposition (sum) and mean of
  - embedding for all tokens from all layers
- Ref: [Blog](https://mccormickml.com/2019/05/14/BERT-word-embeddings-tutorial/#33-creating-word-and-sentence-vectors-from-hidden-states)

#### 2.3 Out-of-vocabulary word processing

BERT use character level token, so usually is able to handle OOV words.


## Other Basics

- Types of attention depending on range:
  - Soft/Global attention: consider **all other keys**
  - Hard attention: only consider **one key**
  - Local attention: locate one key with hard attention first, consider the keys within a **window** of that hard attention key.
  - Ref: [CSDN](https://blog.csdn.net/qq_41558173/article/details/105602022)