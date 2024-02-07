# LLM model based on Whatsapp Chats

Wrote a Language Model from scratch for my own Whatsapp Chats!

The references are taken from this legendary Attention Paper: [link](https://arxiv.org/pdf/1706.03762.pdf)

This uses just the decoder part of the transformer as the focus was on Language Modelling, generating the future text based on the previous inputs. It also uses words with look ahead masks for faster processing of a single sentence.

# Preprocessing
- Invalid data is completely removed. Data with links, code, and many sentences are removed. Even words with shorter lengths like 1 are not considered.
- This is not a bigram model, but uses words to predict the next words, so the main focus was to create tokens with the correct meaning
- Used the Longest Common Subsequence algorithm between the sorted lengths of all the tokens in the database to try to generate the proper spellings of the chats miswritten by people by typing. There are lots of other techniques to do this too - for example using the subword-based tokenization used in BERT for improving the quality of sentences. Still, I have preferred to use it similarly for now. The LCS algorithm does have some conditions which force some patterns and tries to generate the correct word.
- The remaining sentences are lemmetized using NLTK.

- Embeddings are generated using Word2Vec - this can be improved as the embedding layer is a better option, but due to lack of computational resources, I found it much better to use.
- I have used 128-sized embeddings. This can be increased, as I was using single group data, which was huge too, I had around 119 tokens, so 128 was fine to use.
- Positional Embeddings are generated using the formula mentioned in the research paper.
- Look Ahead Masks are used to make faster processing for predicting single sentence
- Model uses 8 heads of MultiHeadAttention, with a dropout of 20%, which amounts to around 

Training LLMs is not easy, there are lots of parameters, and loads of data are needed, methods of training vary a lot too. Though, not a perfect solution, I have divided whole sentences into 4 tokens and then asked the model to predict them one by one taking previous sentences using look ahead masks.

# Using the model
- The "data/" folder contains chat exported from WhatsApp in .txt format (Chat can be easily exported by navigating to the menu of chats)
- Try to run the code after installing dependencies

# Scope for Improvements:
- I couldn't experiment much due to lack of resources and it takes a lot of time to make a decent model.
- Dropouts, regularization layers, and using more data (most important) might give better results
- Also using <start> and <end> tokens, using just sentences from the main dataset, and not dividing the data into 4 tokens, may give better training to the model using query masks.

# Result:
After 20 epochs over 5311 unique tokens, taking 8 hours to train, I was able to provide quite good results. The group I used to train the model, had generally all technical people, and it was able to detect some important keywords. For example, generating words from input "code" outputted words with "github", "error", "deployment" and "web", though they were not properly ordered due to lack of data.



