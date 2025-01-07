## What is AI ?
The term is often used to describe the process of machine being able to autonomusly take decision and perform tasks close to the human intelligence level
### Laungage AI
Langague AI is the subfield of AI which focuses on technologies focused on understanding, processing and generating human laguages.
The term can be intercahanglably used with NLP but the scope expands further where the LLM technologies encompasses things that might not directly be langugage models but that can have significant impact on the field </br>

### History of Langague Models
1. Bag of Words
2. Word2Vec
3. Attention
4. BERT
5. GPT
5. GPT 2 
6. Roberta
and many more including ChatGPT and Gemini

### Model Elobrated Understanding </br>
1. Bag of Words: Contains below steps
   A. Tokenization: Breaks the statements in tokens (E.g. "Cat sat on a mat" corresponding tokens will be ['Cat','sat','on','a','mat']
   B. Create a dictionary from the individual tokens based on the entire document
   C. Represent the original sentence based on the numerical representation depending presence of the word in sentence (1 if the word exists, 0 if not)

**Limitation**
A. Final representation is just a bag of word without any sementaic meaning E.g. "Tiger ate rabbit" and "Rabbit ate tiger" would essentially have similar representation
B. Introduces high dimensional (dimension = dicrtionary size) sparse vectors.
C. Does not capture semantics (cosine similarity between 2 word vectors will always be 0).
D. Similar words are treated differently (tiger and tigers are similar but will be totally different in representation)

2. **Word2Vec**: It is a NN based model that represents tokens in the high multidimensional space in the form of vector. Words having similar meaning or that appear together in similar context usually are closer to each other compared to the ones 
   that have very different meaning or don't appear toghther often
   The way model works is as below
   1. Words are intially embedded and represented in the form of vectors, and the initial values are randomized
   2. Eventually as the training data is passed to the model, the model tries to optimize similar words (with meaning/context) closer to each other
   3. End output is the vector representation of the words in the high dimensional space

**Advantages**
Preserves some degree of semantic meaning

**Limitation**
The end output is a static representation of the words. However, if the same word appears in a very different context tha was not present in the training data the results are not tha accurate </br>
E.g. "I am standing at a bank". Here the word "bank" can have various meanings depending on the context which can't be adjusted for in the static represenation

**3. # Encoder-Decoder with Attention

This is an RNN-based architecture that uses Recurrent Neural Networks (RNNs) on both the encoder and decoder sides. Let’s use an example of machine translation, where we want to translate the English sentence *"I love India"* into Hindi.

## Encoder Side

1. **Tokenization:**  
   The input sentence is broken down into individual tokens: `['I', 'love', 'India']`. These tokens are sequentially represented as:  
x1 -> 'I'
x2 -> 'love'
x3 -> 'India'


Each token is then passed to the encoder one by one. The encoder is an RNN-based neural network that processes these tokens sequentially.

2. **Processing Tokens:**  
At each time step, the encoder has two inputs:  
- The current token (e.g., `x1` = `'I'`).  
- The previous hidden state (e.g., `h0` for the initial step).  

**At time `t = 1`:**  
- Input token: `x1 = "I"`.  
- Previous hidden state: `h0 = 0` (since it’s the first step).  
- New hidden state: `h1 = F(h0, x1)` (calculated using the RNN function `F`).  

**At time `t = 2`:**  
- Input token: `x2 = "love"`.  
- Previous hidden state: `h1`.  
- New hidden state: `h2 = F(h1, x2)`.  

This process continues until all tokens are processed, resulting in the final hidden state (`h3`) for the last token (`x3 = 'India'`).

3. **Final Hidden State:**  
The final hidden state (`h3`) is a summary of the input sentence and contains both:  
- The vector representation of the tokens processed so far.  
- Information about the sequence order.

## Decoder Side

The decoder is also an RNN-based architecture and operates **after the encoder has processed the entire input sequence**. The timeline of the decoder is independent of the encoder.

1. **Initialization:**  
At time `t = 0`, the decoder’s initial hidden state (`s0`) is set to the encoder’s final hidden state (`h3`).  

- Hidden state: `s0 = h3`.  
- Input token: `y0` (a special start-of-sentence token added by the decoder).  
- Context vector: `c0 = h3` (initialized to the encoder’s final hidden state).

2. **Generating Output:**  
At each time step, the decoder generates the next token based on:  
- The previous hidden state.  
- The previous output token.  
- The context vector from the attention mechanism.

For example:  
s1 = DecoderRNN(s0, y0, c0) y1 = softmax(Linear(s1))

## Attention Mechanism

The attention mechanism ensures that the decoder focuses on relevant parts of the input sentence while generating each output token. This solves issues where the encoder’s final hidden state alone may not fully capture long or complex sentences.

1. **Context Vector (`c1`):**  
Instead of using just the encoder’s final hidden state (`h3`), the context vector is computed as a weighted sum of all encoder hidden states (`h1, h2, h3`):  


c1 = α11h1 + α12h2 + α13h3

- `αij` represents the attention weight for the decoder’s hidden state at time `t=i` with respect to the encoder’s hidden state at position `j`.

2. **Calculating Attention Weights:**  
- A similarity score is computed between the current decoder hidden state (`s1`) and each encoder hidden state (`hi`):  
  ```
  score = dot_product(s1, hi)
  ```
- The scores are passed through a softmax function to normalize them:  
  ```
  αij = softmax(score)
  ```
  This ensures that the attention weights sum up to 1.

3. **Using the Context Vector:**  
The context vector is combined with the decoder hidden state to generate the next output:  
s2 = DecoderRNN(s1, y1, c1) <br>
y2 = softmax(Linear(s2))


## Example for Context:

Consider the sentence: *"The animal couldn't cross the road because it was injured."*  
Here, the word *"it"* can refer to either *"animal"* or *"road."* The attention mechanism helps resolve such ambiguities by assigning higher weights to the relevant parts of the input sequence, ensuring that the model generates contextually accurate translations.

## Summary:
The encoder-decoder architecture with attention dynamically adjusts focus based on the input sentence, enabling more accurate translations. By combining the power of sequential processing (RNNs) with a context-aware attention mechanism, this architecture significantly improves the quality of machine translation and similar tasks.

Transformer Architecture

# Transformer Architecture for Translation: "I love India" (English to Hindi)

This explanation breaks down the steps of a Transformer model for translating *"I love India"* into Hindi.

---

## **Encoder Side**

1. **Tokenization and Embedding:**  
   - The input sentence is tokenized: `["I", "love", "my", "country"]`.  
   - Each token is passed through an embedding layer, converting them into vector representations.  

2. **Positional Encoding:**  
   - Since Transformer processes tokens in parallel, positional encoding is added to the embeddings.  
   - Positional encoding provides information about the order of tokens relative to each other.

3. **Parallel Processing:**  
   - The embedded tokens, now combined with positional encodings, are processed simultaneously by the encoder.  
   - This results in a series of hidden states:  
     ```
     H = [h1, h2, h3, ..., hn]
     ```

---

### **Self-Attention Mechanism**

Self-attention helps determine how much focus each word should have on other words in the sentence.  

#### **How It Works:**
1. **Key Terms:**
   - **Query (Q):** Represents the word in focus.  
   - **Key (K):** Represents other words to compare against.  
   - **Value (V):** The actual information associated with each word.  

   These are derived using learned weight matrices (`Wq`, `Wk`, and `Wv`):  
Q = X * Wq
K = X * Wk
V = X * Wv


2. **Analogy:**  
Think of searching for a book in a library:  
- **Query:** Your topic of interest.  
- **Key:** The titles of the books.  
- **Value:** The content in the books.  

3. **Attention Score Calculation:**
- Compute the similarity score:  
  ```
  score = dot_product(Q, K)
  ```
- Scale the score:  
  ```
  score_scaled = score / sqrt(d_k) where d_k is dimension of weight matrix
  ```
- Normalize using softmax to get attention weights.  

4. **Weighted Sum:**  
Multiply attention weights with the Value vectors and sum them to obtain the final representation.

---

### **Multi-Head Attention**
- Instead of a single set of `Wq`, `Wk`, and `Wv`, multiple sets are used, called attention heads.
- Each head focuses on different aspects of the input, allowing the model to capture more nuanced relationships.
- Outputs from all heads are concatenated and transformed back to the original dimension.

---

### **Feed Forward Network**
- The output of the attention mechanism is passed through a feed-forward network.  
- Residual connections are added to stabilize gradients and prevent sudden losses.  

---

## **Decoder Side**

1. **Masked Attention:**  
- Prevents the decoder from accessing future tokens during training.  
- For example, while translating `"I"`, the model cannot see `"love"`, `"my"`, or `"country"`.  

2. **Cross-Attention:**  
- In this step, the decoder attends to the encoder’s hidden states.  
- The decoder takes the encoder outputs (`H`) and the previously generated tokens as input to calculate attention scores.

3. **Output Generation:**  
- Tokens are generated one at a time until the `<EOS>` (End of Sentence) token is reached.

---

### **Summary**
The Transformer architecture excels in handling parallel processing with mechanisms like self-attention and multi-head attention. By combining encoder and decoder layers, it achieves state-of-the-art performance in tasks like machine translation, efficiently generating contextually accurate translations.
