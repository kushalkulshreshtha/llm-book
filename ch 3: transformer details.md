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
   The way positional encoding works is tokens appearing close to each other would have more simialr embeddings compared to the one appearing far. Some of th initial day transformers used
   absolute positional encoding sucha as 1 for first token 2 for second and so on

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
   Post that these normalized scores are passed through the softmax fucntion that results in attention weights. Individual attention weights are then multiplied and added
   to create a attention head.
   This is how a single attention head is created. In multi attention head instead of just one pair multiple pair of weight matrices are initialized that calculates individual attention heads.
   These attention heads are then concatenated to give final attention head.
   Advantage of multi attention head is it allows to attend different part of the sentences improving the model perfomance

5. **Grouped Attention and Multi Query Attention**: Attention mechanism ovetime has gone various updates and one of them is in the attenion heads. Multi query attention head instead of having one individual key and value pair across different queries shares one Key and Value pair. Although beyond a point these optimizations can be really expensive and hence might not work well, hence we use group attention.
   Group attention finds middle ground between multi attention and mutli query attention, that instead of having just one shared Key and Value vector, it has more number of Key Value pairs that are shared between queries.
   Modern day transformer uses advanced tenchniques like flash attention to further improve the efficiency E.g. Kernal Fusion, where process like masking, embedding and self attention are done on same kernels   


   
     
   
