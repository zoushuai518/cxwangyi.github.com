---
layout: post
title: "Why Asynchronous SGD Works Great?"
description: ""
category:
tags: []
---
{% include JB/setup %}

In this NIPS 2012 paper,
[Large Scale Distributed Deep Networks](http://books.nips.cc/papers/files/nips25/NIPS2012_0598.pdf),
researchers at Google presented their work on distributed learning of
deep neural networks.

One of the most interesting points in this paper is the asynchronous
SGD algorithm, which enables a parallel (distributed) software
architecture that is scalable and can make use of thousands CPUs.

> To apply SGD to large data sets, we introduce Downpour SGD, a
> variant of asynchronous stochastic gradient descent that uses
> multiple replicas of a single DistBelief model. The basic approach
> is as follows: We divide the training data into a number of
> subsets and run a copy of the model on each of these subsets. The
> models communicate updates through a centralized parameter server,
> which keeps the current state of all parameters for the model,
> sharded across many machines (e.g., if we have 10 parameter server
> shards, each shard is responsible for storing and applying updates
> to 1/10th of the model parameters) (Figure 2). This approach is
> asynchronous in two distinct aspects: the model replicas run
> independently of each other, and the parameter server shards also
> run independently of one another.

Intuitively, the asynchronous algorithm looks like a hack, or a
compromise between the effectiveness of the mathematical algorithm and
the scalability of the distributed system.  But to our surprise, the
authors claimed that the asynchronous algorithm works more effective
than synchronous SGD.

Why??

My understand is that traditional gradient-based optimization is like
a bee flying along the direction defined by the current gradient.  In
batch learning, the direction is computed using the whole training
data set.  In SGD, the direction is computed using a randomly selected
mini-batch of the data.

In contrast, the asynchronous parallel SGD works like a swamp of bees,
each flies along a distinct direction.  These directions vary because
they are computed from the asynchronously updated parameters at the
beginning of each mini-batch.  For the same reason, these bees
wouldn't be far away.

The swamp of bees optimize collaboratively and covers a region like
region-based optimization, where the *region* is composed of a set of
points.  This, I think, is the reason that parallel asynchronous SGD
works better than traditional gradient-base optimization algorithms.
