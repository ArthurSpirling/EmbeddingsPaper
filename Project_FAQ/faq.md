# The Embeddings FAQ
## (A Work in Progress)


## 1.  What are word embeddings?
Literally, a word embedding is a densely valued vector that represents a word. Less literally, scholars have shown that, by comparing these vectors and the distances between them, we can learn something about how 'humans' represent the meaning of words. Thus, if we have a corpus in which the embedding vector for "tax" is closer to "socialist" than it is to "conservative", we could update that "tax" is closer conceptually to the former rather than to the latter.  

## 2. I've not heard much about them. Is it a new idea?
No. The idea is quite old, and goes back at least to the 1950s. What's new are fast, scalable ways to *obtain* embeddings.

## 3. Fine. So what's the "old" idea?
Firth (1957) has a quote which everybody cites:

> You shall know a word by the company it keeps

In a nutshell this captures what is known as the "distributional hypothesis". Literally, the idea is that words that appear in similar *contexts* probably mean similar things.  If "coffee" and "tea" are always close to "cup", then we might infer that "coffee" and "tea" are similar.

Models that use this insight are sometimes called "distributional semantic models" (DSMs).

## 4. What, literally, is a "context" in this case?
In the embeddings literature, it's typically a local and symmetric window around a word. So, suppose our sentence was 

> "We then heard some nice, relaxing music that gently worked to a crescendo."

Here a two word symmetric window around "music" would be ("nice, relaxing") and ("that gently").  A three word window would be ("some nice, relaxing") and ("that gently worked").  Note that there are some models that use *asymmetric* windows, but they are not as common.

## 5. So do all DSMs use local windows?
No. Distributional semantic models (DSMs) include things like Latent Dirichlet Allocation (LDA), which political science commonly uses for "topic models". But those don't typically use local windows.

## 6. I see! So embeddings models don't make the "bag of words" assumption?
Well, it depends what you mean. Clearly, a local window is helping to take word order into account in some sense. But *within* the window, the models often treat things as bags of words (i.e. unordered).

## 7. Are embddings vectors related to the "vector space" model I've learned in my text-as-data course?
Yes and no. In the typical vector space model, each document is a real valued vector (often a count). So you might have "dog eat dog world" represented as `[2, 1, 1]`, with the first element representing "dog", the second representing "eat", third representing "world" and so on.  In word embeddings, each *word* has its own vector, and these are things to be learned by a model. It is related in that words are represented as vectors in a multidimensional space.

## 8.  Sounds interesting. But why is having word embeddings helpful?
It turns out representing words this way is helpful for a many 'downstream' NLP and machine learning tasks. For example, parts-of-speech tagging: embeddings can help us distinguish the 'sense' in which a word is being used in a given context. More generally, knowing relationships between concepts can be useful: if we know the word "umbrella" is closer to "raincoat" than "sunscreen" in our corpus, we might want to advertise umbrellas for those searching for raincoats, but not those searching for sunscreen. A natural application in political science might be building dictionaries: if "republican" is near "conservative" and "neocon" in the embedding space, that might tell us we should be counting all of these as examples of right-wing ideology in US politics.

All this leads to the classic example of analogies, that every exposition of embeddings includes. Suppose we have an embedding vector for the following words: "king", "queen", "man", "woman". For some specifications, it turns out that, approximately: "king"-"man"+"woman"="queen". That is, "king" is analogous to "queen", as "man" is to "woman".

## 9. You've convinced me. So how do I get these embeddings?
You need a model. And there are lots and lots of options, starting with the "neural" models of the early-2000s.

## 10. Wow, "neural" sounds very involved, no?
No, not really. Those models have been around for a long time too (at least since the late-1990s/early-2000s). To reiterate, they got fast and scalable recently.

## 11. What model should I use?
Up to you, but the most popular ones are:
* `Word2Vec` from Mikolov et al, 2013 ([paper](https://arxiv.org/pdf/1310.4546.pdf), [paper](https://arxiv.org/pdf/1301.3781.pdf), [code](https://code.google.com/archive/p/word2vec/)) 
* `GloVe` from Pennington et al, 2014 ([website](https://nlp.stanford.edu/projects/glove/))

## 12. What's `Word2Vec`?
It's a specific way of estimating word embeddings, and comes in two varities (known as "architectures"): 
1. Continuous Bag of Words (CBOW). This assumes you want to predict the target word ("music" above) given the context words (from the local  window).
2. Skip-gram. This assumes you want to predict the context words (the stuff in the local window around "music") given a particular word ("music" in our case above).

## 13.  How is `Word2Vec` fit to the data?
It goes word-to-word in the data and tries to predict either the target word, or the context, depending on the architecture desired. Ultimately, it uses a neural network model to do this. 

## 14.  Cool, so deep learning?
No. The neural network only has one layer. So it's not really "deep learning". It's "shallow".

## 15.  What's `GloVe`?
It's a specific way of producing word embeddings, and is literally **Glo**bal **Ve**ctors for word representation.

## 16. How is `GloVe` fit to the data?
It uses 'global' (aggregate) co-occurance counts.  Note that `Word2Vec` doesn't do this: it goes word-by-word and never looks at the corpus as a whole.

## 17. Oh, so `GloVe` isn't deep learning?
No, but neither is `Word2Vec` (see above).

## 17. Which is better?  `GloVe` or `Word2Vec`?
Neither. For one thing, they are fundamentally similar in terms of  what they do (ultimately). There is some evidence that `GloVe` is a little more stable and perhaps performs better on some tasks.

## 18. I have a corpus, how do I use these models?
You can download the software (for either/both), and fit the models to your data. That will produce embeddings, once you've made some decisions about hyperparameters and a few other technical things like how the model will iterate through fitting steps.  

## 19. Hmm. Sounds complicated. Are there any other options?
You could use *pre-trained* embeddings. These are embeddings for, say, words that appeared in (English) Wikipedia in 2014. You can get them from the same place(s) you get the code. If your corpus is "like" Wikipedia, these will probably do a good job of capturing meaning for you.  

## 20. What if my corpus is nothing like Wikipedia?
You can take the models above and fit them *locally* to your data. But note that embeddings tend to be better (for the tasks we mentioned above) when they are fitted to more data. So you probably want quite a bit of data: not 50 manifestos, for example.

## 21. OK, so local v pre-trained. What other decisions do I need to make?
Going back to FAQ 18, the key parameters you need to make a decision about are:
* window-size: literally, how big the (symmetric) window around your words. Examples might be 2, 4, 6, 10 etc.
* embedding vector length: how large you want the vectors that represent your words to be. Examples might 50, 100, 300, 450 etc.

Note that you need to make these decisions even if you use *pretrained* embeddings, because you'll be downloading different embeddings datasets depending on what you want.

## 22. What do we know about optimal window-size?
For social science? Not much.  
In general? We know that larger windows (>2) capture more topical relations (e.g. "Obama-President") while smaller ones (<2) capture syntactic ones (e.g. "jumps-jumping").

## 23. What do we know about optimal embedding vector length?
For social science? Not much.  
In general? We know that bigger vectors capture more senses of the word. But it may not be optimal to have them too large, since this leads to redundancy (i.e. there are lots of dimensions that aren't doing much, and perhaps just capturing noise). But too small is bad too: in the limit, one may not be able to distinguish words at all.

## 24.  OK, what else do I need to (not) know?
Embeddings aren't *stable*. If you are locally fitting to your corpus, you'll get different results each time. This is due to the nature of the models (e.g. a model's word vectors are randomly initialized at the start of estimation), and is basically a fact of life. One way to mitigate this is to estimate the same model several times and aggregate over the desired distance metric.

## 25.  Anything else?
Yes, there's a bunch of hyperparameters to do with technical model fitting that you can choose (e.g. convergence threshold, number of iterations).  But we won't get into those---many of these have appropriate defaults.

## 26. Right. So what should I do for my political science case?
Glad you asked. Our paper takes on these issues for several corpora in political science which we think are somewhat representative of what researchers use in practice.  We assess how "performance" varies as we move between hyperparamter choices, and how that affects stability.

## 27. How do we assess "performance" for embeddings in political science?
We use four tools:
1. technical criteria: essentially a version of "fit" (as a function of window size), plus computation time.
2. stability: how correlated different runs of the same model are, in terms of the nearest neighbors of a set of key words.
3. similarity in query search ranking: how similar the nearest neighbors of query words are *across* two different models.
4. Turing test: ask a human to compare the nearest neighbors produced by the embedding model with those produced by a (different, independent) human coder.

## 28. What are nearest neighbors in this context? And what are the query words?
Embedding models, of whatever specification, give you a vector for any word in your vocabulary. For a given word---a given vector---you can ask for the closest `n` words to it (e.g. largest cosine similarity). These are its nearest neighbors, and *should* be similar in some semantic way. 

Obviously, we are not going to check every single word in the vocabulary, but rather some that are specific to political science. For us, those are connected to: 
- "big" issues: `democracy`, `freedom`, `equality`, `justice`. 
- policy debates in America: `immigration`, `abortion`, `welfare`, `taxes`. 
- parties: `republican`, `democrat`

We could have picked others, but we felt these would be a good start.

## 29. What is our corpus?
Our focus corpus is the *Congressional Record* transcripts for the 102nd-111th Congresses. It's American English, and about 1.4M documents long. But we have others, in various languages, too (see below).

## 30. What parameter combinations do we try?
- Window-size: 1, 6, 12, 24, 48
- embedding dimensions: 50, 100, 200, 300, 450

## 31.  What model do we use?
We use `GloVe` only (not `Word2Vec`, but we anticipate our results would be very similar if we did).

## 32. How does the Turing test work in practice?
We give human coders a set of query or prompt words (from FAQ 28). They are told to come up with (ten) words that are close to those words, similar in meaning, and which would likely appear "near them" in a corpus etc. From the "machine" side we grab the ten nearest neighbors that the model suggests.

Finally, we ask (different, independent) human coders to compare the "human" nearest neighbors to the "machine" nearest neighbors (we don't tell them which is which). They just have to tell us which they think is a better fit for the prompt word in question.

This is a "Turing test" in the sense that we are trying to find out if machines (models) can do a good job of mimicking humans. It is not a [Turing test](https://en.wikipedia.org/wiki/Turing_test) in the sense that we are not trying to "fool" the humans.

## 33. What are the results based on technical criteria?
First, notice that to assess this, we are referring to `GloVe` fit *locally* to our *Congressional Record* corpus.

Bigger windows and more embedding dimensions always improve model fit---but with decreasing returns for both. Unequivocally, using very small embeddings vectors (<100) is bad for fit.  Unsurprisingly, bigger models---bigger windows, longer vectors---are more time consuming to fit. But even the biggest models aren't that bad: it took somewhat over 3 hours to fit the largest.

In terms of trading off fit vs time, the 'standard' option of 6-300 (i.e. window-size =6, embedding dimensions = 300) is a reasonable option.

## 34. What are the stability results?
Models with bigger windows (ceteris paribus) produce more stable results, up to a point. When the window size gets too large, especially in models with big dimension numbers, we see some performance degradation. Models with fewer dimensions (ceteris paribus) also seem to be less stable. A small number of dimensions with small window sizes produce particularly high variance results.

## 35. What about query ranking correlations?
First and foremost, whether we look at our key politics words or just random words, all pre-trained models show high correlations (typically >0.6). That is, there really isn't much "substantive" difference between them. What about pre-trained and local? Here too, the correlations are quite high (>0.5), esp for our politics words.  

This suggests that using off-the-shelf pre-trained embeddings won't generate conclusions radically different to those from a *locally* fit model.  

## 36. What do humans prefer: humans or machines?
Suprisingly, there is no unqualified winner (for our set of politics queries). Locally-trained models tend to get to around 70% of human performance (very approximately, this means that on average, humans are indifferent 70% of the time between a machine and another human). This is higher for the pre-trained embeddings: they achieve up to 87% human performance.

## 37. Are the findings generalizable? Or do they just refer to this corpus?
We did the same experiments (without the Turing test) on several more corpora, and our results are broadly similar to the above.  That is, we don't think there is something about the *Congressional Record* that caused these results.

The other corpora we looked at are:
- Hansard (UK English), 1935--2013 (~4.5M documents)
- State of the Union (US English), 1790--2018 (small corpus)
- Cortes Generales (Spanish), 1993--2018 (~1.3M documents)
- Deutscher Bundestag (German), 1998--2018 (~1.2M documents)

## 38.  Bottom line: what should a researcher do in applied settings?
For politics, *pre-trained* "standard" 6-300 `GloVe` models (window-size=6, dimensions=300) work about as well as anything else. These are pretty highly correlated with other things you do. Perhaps more importantly, humans like the results---certainly relative to locally trained models, and its even close to human generated results. Obviously, it's cheaper to use pre-trained, but our experience was that this probably isn't a clincher on its own, because the models are quite quick for corpora of ~1M documents (which we assume is fairly typical). 

You may make some gains substantively by fitting locally, but it isn't obvious in terms of our human validation what those are. But there may be some criteria you are optimizing if you do this which we are not aware of. If you *must* locally fit, then avoid small windows and small dimension numbers, especially in combination.    

## 39. Any final thoughts?
We didn't look at whether embeddings are *per se* "good" at representing things relative to completely different models---including standard "bag of word" stuff. Put crudely, just because embeddings models are all the rage, doesn't mean they should replace topic models or whatever else you are doing.  

This is a very fast-moving area. More complex, "better performing" versions of embeddings will (have) emerge(d). What we consider our key contribution then is the Turing test arrangement: we conjecture that supervised problems will remain relatively rare in political science, so what scholars should do is make sure their model representations of documents and words actually accord with something useful.
