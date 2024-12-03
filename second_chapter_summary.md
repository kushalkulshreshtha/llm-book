Chapter talks about tokenization and embedding.

<h1> Tokenization </h1>

* The process of breaking text into chunks called tokens is called tokenization.
* Different LLMs have different tokenizers, and the output of that tokenizer is fed to the model for training, so model can only use the tokenizer it was trained for.
* There are 3 major algorithms to train tokenizers: Byte Pair Encoding (BPE) (most popular, used in GPT models), WordPiece (used in BERT), SentencePiece
* Tokenization can be at character level, word level, subword level and byte level.
* Main parameters to train tokenizers are: vocabulory size (hyperparameter), training dataset, special tokens (to handle class, unknown word, mask, <|assistant|>, <|end-of-text|>  etc)

<h2> Different Types of Tokens </h2>
<h3> Character Level</h3>

* Each character (alphabet) has its own token
* **Advantages** : Less vocab size
* **Disadvantages** : Character carries less information than a word (like the letter 'd' will have less info than deer), will need higher context window to generate output

<h3> Word Level</h3>

* Each word has its own token
* **Advantages** : Each word carries a meaning
* **Disadvantages** : High vocab size, no context (for example, the word bank may mean money bank or river bank, but the token will stay the same), words will similar meaning will be different tokens (for example apology, apologize, apologetic etc will end up having different tokens but are very similar in meaning) 

<h3> Sub-word Level</h3>

* Best of both worlds
* **Advantages** : Can handle similar words which can be broken down into multiple (like apologize = apolog+ize, apologetic = apolog+etic). This keeps the main word common
* **Disadvantages** : 

<h3> Byte Level</h3>

* Breaks down tokens into individual bytes (1s and 0s)
* Some tokenizers like GPT Tokenizers use subword + bytes as a fallback mechanism in case they find something they do not know how to represent.

<h2> Different Types of Tokenizers </h2>

* BERT base model (uncased): converts everything into lowercase, then tokenizer. Vocab: around 30k (WordPiece)
* BERT base model (cased): Vocab: around 30k (WordPiece)
* GPT-2 Tokenizer:Vocab: 50,257 (BPE)
* GPT-4 Tokenizer:Vocab: around 100k (BPE)
* Galactica: Science based tokenizer, spl tokens for references
* Phi3 and Llama2: Vocab: 32,000 (BPE)

<h2> Different Types of Tokenization Algorithms </h2>

* Source: https://huggingface.co/docs/transformers/tokenizer_summary
<h3> Byte Pair Encoding (BPE) <h3>
* In the first step, we compute the frequency of each word in the text. for eg ['bun':10, 'pun': 15, 'pug': 10, 'hug': 20, 'hugs':10]
* Now, the vocab is broken into singular characters: [('b' 'u' 'n', 10), ('p' 'u' 'n', 15), ('p' 'u' 'g', 10), ('h' 'u' 'g', 20), ('h' 'u' 'g' 's', 10)]
* Individual characters are combined based on their frequency: 'bu' occurs 10 times in 'bun' and 20 times in 'bug', so total frequency of 'bu' is 30. Similarly the possible combinations are: {'bu': 30, 'un': 25, 'pu': 25, 'ug': 40, 'hu': 30, 'gs': 10}. In this example, we now combine 'ug' into single token. 
* Now the vocab is: [('b' 'u' 'n', 10), ('p' 'u' 'n', 15), ('p' 'ug', 10), ('h' 'ug', 20), ('h' 'ug' 's', 10)]
* Repeat this process again:  {'bu': 30, 'un': 25, 'pu': 25, 'pug': 10, 'hug': 30, 'ugs': 10}. Now combine 'bu' (or 'hug') to form a token.
* Repeat until the desired vocabulary length is reached.

<h3> Word Piece <h3>
* It is very similar to BPE: first breaks down text into individual tokens.
* However, the merging trick is differente from BPE. It merges on the based on probability of the data, not on the frequency.
