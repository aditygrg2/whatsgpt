# LLM model based on Whatsapp Chats

Wrote a Language Model from scratch for my own Whatsapp Chats!

The references are taken from this legendary Attention Paper: [link](https://arxiv.org/pdf/1706.03762.pdf)

This uses just the decoder part of the transformer as the focus was on Language Modelling, generating the future text based on the previous inputs. It also uses words with look ahead masks for faster processing of a single sentence.

# Preprocessing
- Invalid data is completely removed. Data with links, code, and having many sentences are removed. Even the words with shorter length like 1 are not considered.
- This is not a bigram model, but uses words to predict the next words, so main focus was to create tokens with correct meaning
- Used Longest Common Subsequence algorithm between sorted length of all the tokens in the database to try to generate the proper spellings of the chats written by people incorrectly by typing. There are lots of other techniques to do this too - for example using the subword-based tokenization used in BERT for improving the quality of sentences, but I have preferred to use it similarly for now. The LCS algorithm do have some conditions which force some patterns, and tries to generate the correct word.
- The remaining sentences are lemmetized using NLTK.

- Embeddings are generated using Word2Vec - this can be improved as embedding layer is a better option, but due to lack of computational resource, I found it much better to use.
- I have used 128 sized embeddings. This can be increased, as I was just using single group data, which was really huge too, I had around 119 tokens, so 128 was fine to use.
- Positional Embeddings are generated using the formula mentioned in the research paper.
- Look Ahead Masks are used to make faster processing for predicting single sentence

Training LLM's is not really easy, there are lots of parameters, and loads of data is needed, methods of training varies a lot too. Though, not a perfect solution, I have dividied whole sentences to 4 tokens, and then asked the model to predict them one by one taking previous sentences using look ahead masks.

# Using the model
- "data/" folder contains chat exported from whatsapp in .txt format (Chat can be easily exported by navigating to menu of chats)
- Try to run the code after installing dependencies

# Scope for Improvements:
- I couldnt experiment much due to lack of resources and it takes a lot of time to make a decent model.
- Dropouts, regularization layers, using more data (most important) might give more better results
- Also using <start> and <end> tokens, using just sentences from the main dataset, not dividing the data into 4 tokens, may give better training to the model using query masks.

