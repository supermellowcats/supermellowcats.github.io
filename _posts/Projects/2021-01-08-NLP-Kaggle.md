---
layout: page
title: Winning a Natural Language Processing Kaggle competition
comments: true
categories: Projects
tags:
  - deep learning classification nlp
excerpt:
hidden: false
---

## Quick summary:

- What I did:
    - Won an [NLP Kaggle competition](https://www.kaggle.com/c/ttic-31190-paraphrase-hard/leaderboard) to build a sentence paraphrase classifier
    - Coded from scratch, tuned hyperparameters and trained a complex BERT-based LSTM architecture on GPUs
- Things I learnt:
    - How to successfully use intuition to improve deep learning model performance
    - How to diligently keep track of hyperparameter tuning and other meta-processes in machine learning
    - How to run experiments in NLP
- When was this?
    - September 2021 through December 2021

Courses at the Toyota Technical Institute at Chicago are always a challenge, and a fast-paced one at that.

With Prof. Kevin Gimpel's NLP, we started out with simple count-based word vectors. For context, TF-IDF normalization was the 'try this' challenge at the end of week 1.

We started using deep learning only at week 5 of a 10-week quarter. Thankfully I had a grasp of the concepts (from Prof. David McAllester's Deep Learning at TTIC, as well as the unexcludable Andrew Ng) - but figuring out the implementation was gritty.

Our final assignment was a [Kaggle competition](https://www.kaggle.com/c/ttic-31190-paraphrase-hard/leaderboard). The task was to build a binary classifier for sentence paraphrase detection - given two sentences, predict `True` if they paraphrase each other and `False` if they do not.

32 graduate students (many earning their PhDs) were given millions of training examples of sentence paraphrase pairs (such as "I am going to the hardware store" and "I plan to go to Hardware Depot"), and told to unleash the world of NLP models on the task.

There were two test datasets - easy and hard. The hard set had trickier examples such as "Kevin taught a course Aabir was in" and "Aabir taught a course Kevin was in", which are of course **not** a paraphrase pair despite having the same set of words.

I implemented a unique BERT-based recurrent neural network for the task. I tokenized each input sentence to get BERT vectors that were fed into the same 'paraphraser' LSTM. I then concatenated the final hidden state for each input sentence and squeezed that through a fully connected layer (with dropout to mitigate overfitting).

It turns out that mine was the winning architecture, scoring 83.1% accuracy on the hard dataset. I had never won a Kaggle competition before, and I must say it felt pretty good!

The intuition I used was that the LSTM adds a unique ability to respect word order while using BERT embeddings, something that mere fine-tuning of BERT would not be able to do. This explains why my model did well.
