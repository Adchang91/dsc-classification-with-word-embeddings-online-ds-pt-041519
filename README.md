
# Classification with Word Embeddings

## Introduction

In this lesson, you'll look at the practical aspects of how you can use Word Embeddings and Word2Vec models for text classification!


## Objectives

You will be able to:

* Understand and explain the concept of a mean word embedding, and how this can be used to vectorize text at the sentence, paragraph, or document level
* Import and use pretrained word embeddings from popular pretrained models such as GloVe
* Effectively incorporate embedding layers into neural networks using Keras


## Getting Started

In this lesson, you'll get a practical introduction into how to use Word2Vec and Word Embeddings to improve Text Classification models. You'll start by reviewing **_Transfer Learning_** and loading pre-trained word vectors. Then, you'll learn about how to get important word vectors, combine them into **_Mean Word Vectors_**, and streamline this process by writing a custom vectorizer class compatible with scikit-learn pipelines. Finally, you'll end the lesson by examining how to train Deep Neural Networks that include their own Word Embedding Layers, and how you can use Keras to preprocess text data conveniently!


## Using Pretrained Word Vectors With GloVe

Perhaps the single best way to improve performance for text classification is to make use of weights from a Word2Vec model that has been trained for a very long time on a massive amount of text data. With Deep Learning, more data is almost always the single best thing that can improve model performance, and the embedded word vectors created by a Word2Vec model are no exception. For this reason, it's almost always a good idea to load one of the top-tier, industry-standard models that been open sourced for this exact purpose. The most common model to use for this is the **_GloVe_** (short for **_Global Vectors for Word Representation_**) model by the Stanford NLP Group. This model is trained on massive datasets, such as the entirety of wikipedia, for a very long time on server clusters with multiple GPUs. It would be absolutely impossible for us to train a model of similar quality on our own machines&mdash;however, because the model weights are open-source, you don't need to! Instead, you'll simply download the weights and go from there. 

For text classification purposes, loading the weights precludes the need for us to instantiate or train a Word2Vec model entirely--instead, you just:

* Get the total vocabulary in our dataset
* Download and unzip the GloVe file needed from the Stanford NLP Group's website
* Read the GloVe file, and save only the vectors that correspond to the words that appear in the vocabulary of our dataset.

This can be a fairly involved process, so the code for this is provided for you in the next lab. That said, it's important to take some time and examine this code until you have at least general idea of what's going on!


## Mean Word Embeddings

Loading a pretrained model like GloVe may provide you with the most accurate word vectors we could possibly hope, but each vector is still just a single word. This isn't very conducive to classification as is at this stage, because it's highly likely that any Text Classification will be focused on arbitrarily-sized blobs of text, such as sentences or paragraphs. With that, the question is how to get these sentences and paragraphs into a format that can be used for classification, while making use of the word vectors from GloVe?

The answer is to compute a **_Mean Word Embedding_**. The idea behind this is simple. To get the vector representation for any arbitrarily-sized block of text, all you need to do is get the vector for every individual word that appears in that block of text, and average them together! The benefit of this is that no matter how big or small that block of text is, the Mean Word Embedding of that sentence will be the same size as all of the others, because the vectors you're averaging together all have the exact same dimensionality! This makes it a simple matter to get a block of text into a format that we can use with traditional Supervised Learning models such as Support Vector Machines or Gradient Boosted Trees. 


### Working With scikit-learn pipelines

As you'll see in the next lab, it's worth the extra bit of work to build a class that works with the requirements of a scikit-learn `Pipeline` object, so that you can pass the data straight in and generate the mean word embeddings on the fly. This way, you don't need to write the same set of code twice to generate mean word embeddings for both the training and testing set. This is also important if the dataset is too large to fit into your computer's memory; it will allow you to partially train models and load in different chunks of the dataset. By building a Vectorizer class that handles creating the Mean Word Embeddings rather than just writing the code procedurally, you'll save yourself a lot of work in the long run!

The code for the Mean Embedding Vectorizer class is also provided for you in the next lab. As you'll see, the class requires both a `fit()` and a `transform()` method to be compliant with scikit-learn `Pipeline` objects. Take some time to study this code until you understand what it's doing&mdash;it isn't complex, and understanding how to do this yourself will pay dividends in the long run&mdash;after all, writing clean, reusable code always does!


## Deep Learning & Embedding Layers

One problem you may have noticed with the Mean Word Embedding strategy is that by combining all the words, you lose some information that is contained in the sequence of the words. In natural language, the position and phrasing of words in a sentence can often contain information that we pick up on. This is a downside to this approach, and one of the reasons why **_Sequence Models_** tend to outperform all of the 'shallow' algorithms (note: this term just refers to any Machine Learning algorithms that do not fall under the umbrella of Deep Learning&mdash;it doesn't make any judgments about whether they are better or worse, as that is almost always dependent on the situation!). In the next lesson, you'll learn about sequence models including **_Recurrent Neural Networks_** and **_Long Short Term Memory Cells_**. Moreover, in the next lab, you'll also see a preview example of these,so that you can see how to use **_Embedding Layers_** directly within Neural Networks!

An **_Embedding Layer_** is just a layer that learns the word embeddings for our dataset on the fly, right there inside the Neural Network. Essentially, its a way to make use of all the benefits of Word2Vec, without worrying about finding a way to include a separately trained Word2Vec model's output into our Neural Networks (which are probably already complicated enough!). You'll see an example of an **_Embedding Layer_** in the next lab. You should make note of a couple caveats that come with using embedding layers in your Neural Network--namely:

* The Embedding Layer must always be the first layer of the network, meaning that it should immediately follow the `Input()` layer
* All words in the text should be integer-encoded, with each unique word encoded as it's own unique integer. 
* The size of the Embedding Layer must always be greater than the total vocabulary size of the dataset! The first parameter denotes the vocabulary size, while the second denotes the size of the actual word vectors
* The size of the sequences passed in as data must be set when creating the layer (all data will be converted to padded sequences of the same size during the preprocessing step). 

In the next lab, you'll make use of Keras's text preprocessing tools to convert the data from text to a tokenized format. Then, you'll convert the tokenized sentences to sequences. Finally, you'll pad the sequences, so that they're all the same length. During this step, you'll exclusively make use of the preprocessing tools provided by Keras. Don't worry if this all seems a bit complex right now&mdash;as you'll soon see, this is actually the most straightforward part of the next lab!

For a full rundown of how to use Embedding Layers in Keras, see the [Keras Documentation for Embedding Layers](https://keras.io/layers/embeddings/).



## Summary

In this lesson, you focused on the practical and pragmatic elements of using Word2Vec and Word Embeddings for Text Classification. You learned about how to load professional-quality pretrained word vectors with the Stanford NLP Group's open source GloVe data, as well as how to generate mean word embeddings that work with scikit-learn pipelines, and how to add embedding layers into Neural Networks with Keras!
