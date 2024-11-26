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
* Breaks down tokens into individual bytes
* Some tokenizers like GPT Tokenizers use subword + bytes as a fallback mechanism in case they find something they do not know how to represent.

<h2> Different Types of Tokenizers </h2>
* BERT base model (uncased): converts everything into lowercase, then tokenizer. Vocab: around 30k (WordPiece)
* BERT base model (cased): Vocab: around 30k (WordPiece)
* GPT-2 Tokenizer:Vocab: 50,257 (BPE)
* GPT-4 Tokenizer:Vocab: around 100k (BPE)
* Galactica: Science based tokenizer, spl tokens for references
* Phi3 and Llama2: Vocab: 32,000 (BPE)

<h2> Different Types of Tokenization Algorithms </h2>

