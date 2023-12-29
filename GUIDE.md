# Guide

This guide and codebase is a companion resource for those reading [A Logical Calculus of the Ideas Immanent in Nervous Activity](https://www.cs.cmu.edu/~./epxing/Class/10715/reading/McCulloch.and.Pitts.pdf) by [Warren McCulloch](https://en.wikipedia.org/wiki/Warren_Sturgis_McCulloch) and [Walter Pitts](https://en.wikipedia.org/wiki/Walter_Pitts).

The guide annotates the paper section-by-section. I only quote directly from the paper when there is something to call out explicitly. When possible I bias towards a fully working implementation in code that the reader can use themselves.

## Abstract

To dive right in we are first presented with a short abstract, the first sentence of which is the most important:

> Because of the “all-or-none” character of nervous activity, neural events and the relations among them can be treated by means of propositional logic.

Basically, neural impulses (firings) can be interpreted as true or false (firing or not firing), which inspires us to model networks of neurons as [propositional logic](https://en.wikipedia.org/wiki/Propositional_calculus). So we will be using formal logic to model neuron behavior.

> It is found that the behavior of every net can be described in these terms, with the addition of more complicated logical means for nets containing circles;

McCulloch and Pitts are stating that their calculus encompasses the behavior of "every net" (which I assume means a physical human neural network). This is quite an audacious statement.

They go on to say that things get more complicated for "nets containing circles". These circles of course mean that the graph representing neural network can be cyclic.

> and that for any logical expression satisfying certain conditions, one can find a net behaving in the fashion it describes.

So we can describe neural networks formally, and derive them from our formal expressions.

> It is shown that many particular choices among possible neurophysiological assumptions are equivalent, in the sense that for every net behaving under one assumption, there exists another net which behaves under the other and gives the same results, although perhaps not in the same time.

This just means that there are many way to translate the functionality of real neural networks into our mathematical model, and they are all more-or-less equivalent (although some are faster).

> Various applications of the calculus are
discussed.

How much was known about neuroscience was known at the time of publication (1943)? The wikipedia article on the [history of neuroscience](https://en.wikipedia.org/wiki/History_of_neuroscience) gives a great overview (although at the time of writing McCulloch and Pitts are unexpectedly absent). McCulloch and Pitts released this paper in the same year the [Goldman equation](https://en.wikipedia.org/wiki/Goldman_equation) was discovered (which influenced electrical models of neurons like the [Hodgkin-Huxley model](https://en.wikipedia.org/wiki/Hodgkin%E2%80%93Huxley_model)). At this time much was already known about the [structure and behavior](https://en.wikipedia.org/wiki/Neuron_doctrine) of the nervous system, but this was the first time an attempt had been made to make a mathematical model.

## 1. Introduction

> Theoretical neurophysiology rests on certain cardinal
assumptions.

The first (long) paragraph of the introduction is simply a list of assumptions McCulloch and Pitts make for their paper. They refer to many aspects of the [Neuron Doctrine](https://en.wikipedia.org/wiki/Neuron_doctrine) (there is great overlap), but also make many specific points about excitation.

Let's go over all of the assumptions McCulloch and Pitts will be making.

### The Basics

## 2. The Theory: Nets without Circles

## 3. The Theory: Nets with Circles

## 4. Consequences

# Overall Thoughts