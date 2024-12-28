### Transformers

**General Overview of Transformer**

Let's say we give an input prompt to the transformer, it would first go through tokenization and then be processed through various transformer blocks.
Finally transformer uses as LM Head to output the final results.

Few things to note - 
1. Transformer blocks have different roles to play.
2. A LM head is one of may possible blocks attached to the trasformer. Other possible blocks include Sequential Head or token Heads.

Archeticture of transformer invovles various components and various steps till the final output is produced

1. **Tokenization**: Process of breaking the input prompt in indiviudal tokens is called as tokenization. E.g. "I like playing video games", ["I", "like", "play", "ing", "games']

2. **Embedding**: Generated token_id's are then mapped to the respective embedding
   during the trainding phase.

3. **Positional Encoding**: Tokens within the transformer architecture are processed in parallel hence in order to preserve the semenatic relationship and keep the output meanigful.

The way positional encoding works is tokens appearing close to each other would have more simialr embeddings compared to the one appearing far. Some of th initial day transformers used  absolute positional encoding sucha as 1 for first token 2 for second and so on

4. **Multi Head Attention**: Transformer uses multi head attention. This is an important step which allows transformers attend other parts of the input token when a specific token is being processed.

E.g. "I am sitting near a river bank", in the machine translation use case attention layer would ensure the word bank refers to river bank and not bank i.e. financial entity. Now let's understand in more deailed how is this done

After positional encoding is done the input tokens are multiplied with three different weight matrices initalized randomly to form three vectors

Q (Query)= Wq*X
   
K (Key)= Wk*X

V (Value)= Wv*X
   
First step is to find the dot product of query vector with all the other tokens. In the above example we will find the dot product of bank with all other vecto.
   
The obtained results are then divided by sqrt of dimensions of the initialized weight matrices.

This is an important step and called as layer normalization. Layer normalization is the process of normalizing the activation layers across it's feature dimesnions.

It ensures the mean and standard deviation are consitent, stablizing the process and avoiding problems like vanishing or exploding gradient.

Post that these normalized scores are passed through the softmax fucntion that results in attention weights. Individual attention weights are then multiplied and added to create a attention head.
   
This is how a single attention head is created. In multi attention head instead of just one pair multiple pair of weight matrices are initialized that calculates individual attention heads. These attention heads are then concatenated to give final attention head.
   
Advantage of multi attention head is it allows to attend different part of the sentences improving the model perfomance

5. **Grouped Attention and Multi Query Attention**: Attention mechanism ovetime has gone various updates and one of them is in the attenion heads. Multi query attention head instead of having one individual key and value pair across different queries shares one Key and Value pair. Although beyond a point these optimizations can be really expensive and hence might not work well, hence we use group attention.

Group attention finds middle ground between multi attention and mutli query attention, that instead of having just one shared Key and Value vector, it has more number of Key Value pairs that are shared between queries.
   
Modern day transformer uses advanced tenchniques like flash attention to further improve the efficiency E.g. Kernal Fusion, where process like masking, embedding and self attention are done on same kernels   


<h1> Kushal's additions </h1>

* Transformer LLMs are autoregressive in nature, meaning they use their earlier predictions to generate future predictions
* Transformer LLMs have two major components: Transformers and LM-Heads
* There are multiple transformer blocks followed by LM head.
   * The original GPT had 6 transformer blocks, but some LLMs may have up to 100s of them.
  

<h2> Transformer</h2>

* Transformer itself has two different layers: attention and feed-forward neural network
* **Attention**: see multi-head attention above
* **Feed forward neural network:** At a higher level, this helps the model in remembering things without having to access a large database.
   * For eg. when our input is "The shawshank", the model's output is going to be 'Redemption'. But this word is not coming from any database.
   * The model is able to generalize over a large text data, so it would have to be seen this text multiple times.
   * It is not only about memorization though, it is more about the model being able to interpolate between data points and complex patterns to be able to generalize. This property comes from the feed forward neural nets.

<h2> LM Head</h2>

* The LM head, or language model head converts transformers output into probability distribution over the vocabulary.

<h2> Improvements in transformers </h2>

* Multiple improvements have been done in the transformer since their inception in 2017:
   * Local/ Sparse attention: Local attention means the attention block attends to only a few tokens instead of all tokens. Models like GPT-3 use alternate global and local attention blocks to speed up training while maintaining good accuracy.
   * Flash Attention: Algorithm for attention remains the same, but the calculation is optimized from GPU side using some tricks around what comes in and out of GPU memory.
   * 
