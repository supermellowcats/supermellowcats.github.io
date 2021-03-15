---
layout: page
title: Hyperbolic Skill Embeddings at the Knowledge Lab
comments: true
categories: Projects
tags:
  - embeddings deep learning knowledge skills sociology
excerpt:
published: false
hidden: false
---

Prof. James Evans at the University of Chicago does some groundbreaking research at the intersection of big data, algorithmic exploration and knowledge creation. As I began a Master's in Computational Social Science at the University of Chicago, I had the opportunity to work with him on an analysis of tens of GB of job advertisement data from the internet.

The idea was to explore the relationships between skills based on how they were talked about in job ads. Some skills are general ('metalworking'), while others are specific ('TIG welding'). Some skills are precursors to others (like 'event organizing' and 'event management'). Some occur frequently with certain other skills ('software development' and 'Java' or 'marketing' and 'public relations') while other combinations are rarer. Can we infer these types of semantic relationships between skills from data? Can we predict the births of new skills, or the deaths of old ones?

Data analysis has the power to capture these multifaceted relationships in elegant ways based on simple first principles. So that is what we set out to do.

## From embeddings to hyperbolic embeddings

The method of our choice was the 'hyperbolic embedding' - a special modification of the idea of an embedding, designed to better represent hierarchical relationships. Let's start with embeddings.

An 'embedding' can be thought of as a numerical representation for something - say, a word. Each word is associated with a vector, a set of numbers that is 'embedded' into a high-dimensional space.

To be more specific - the d-dimensional embeddings for a vocabulary V consisting of n words w_1 through w_n will be a set of n vectors, each of dimension d.

Why are these embeddings useful? Well, because we design them to be. For example, with Mikolov et al's Word2Vec (2013), word vectors were optimized to preserve semantic meaning in the form of probability measurements.

We start with the distributional hypothesis. < incomplete >
