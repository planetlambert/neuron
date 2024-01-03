# Guide

This guide and codebase is a companion resource for those reading [A Logical Calculus of the Ideas Immanent in Nervous Activity](https://www.cs.cmu.edu/~./epxing/Class/10715/reading/McCulloch.and.Pitts.pdf) by [Warren McCulloch](https://en.wikipedia.org/wiki/Warren_Sturgis_McCulloch) and [Walter Pitts](https://en.wikipedia.org/wiki/Walter_Pitts).

The guide annotates the paper section-by-section. I often quote directly from the paper (especially in the [introduction](./GUIDE.md#1-introduction)). When possible I bias towards a fully working implementation in code that the reader can use themselves.

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

> The nervous system is a net of neurons, each having a soma and
an axon.

Simple enough. Although for some reason [dendrites](https://en.wikipedia.org/wiki/Dendrite) aren't included in these assumptions.

> Their adjunctions, or synapses, are always between the axon of one
neuron and the soma of another.

So the axon always connects to the soma.

> At any instant a neuron has some threshold, which excitation must exceed to initiate an impulse.

So the neurons have state (a threshold).

> This, except for the fact and the time of its occurence, is determined by the neuron, not by the excitation.

Here "This" is referring to the impulse. I think McCulloch and Pitts are trying to say that excitation may cause an impulse, but it is the neuron's threshold and other state that is responsible for determining if an impulse should occur or not.

> From the point of excitation the impulse is propagated to all parts of
the neuron. The velocity along the axon varies directly with its diameter, from <1 ms<sup>-1</sup> in thin axons, which are usually short, to >150 ms<sup>-1</sup> in thick axons, which are usually long. 

So the impulse travels through the whole neuron, and travels relatively fast through the axon (the ms<sup>-1</sup> is meters per second). Apparently impulses travel faster in thicker axons because there is [less resistance facing the ion flow](https://www.quora.com/Why-does-an-axon-diameter-increase-speed).

> The time for axonal conduction is consequently of little
importance in determining the time of arrival of impulses at points unequally remote from the same source.

Here the authors are saying that the time impulses take traveling through axons/neurons is small relative to the amount of time it takes to travel across a synapse (which they will touch upon later).

> Excitation across synapses occurs predominantly from axonal terminations to somata.

Impulses generally travel from axon -> soma, and not in the reverse direction.

> It is still a moot point whether this depends upon irreciprocity of individual synapses or merely upon prevalent anatomical configurations.

This might be because synapses fundamentally evolved to be unidirectional, or just a property of the way things are physically configured. We now know that the former is correct, with exceptions.

> To suppose the latter requires no hypothesis *ad hoc* and explains known exceptions, but any assumption as to cause is compatible with the calculus to come.

Either way our logical calculus deals with synaptic unidirectionality, whichever assumption we make.

### Temporal summation and other excitation details

> No case is known in which excitation through a single synapse has elicited a nervous impulse in any neuron, whereas any neuron may be excited by impulses arriving at a sufficient number of neighboring synapses within the period of latent addition, which lasts ~0.25 ms.

All this is saying is that excitation occurs when we receive synaptic impulses across many synapses within a period of time. We haven't observed a single synapse causing excitation (although theoretically if the threshold is low enough, etc. it should be possible).

Take note of the approximate "latent addition" period (~0.25 milliseconds), which we will use later. It is important to understand that latent addition means a period of time where synaptic pulses contribute together towards the neuron's threshold, with each neuron's contribution fading after the latent addition period. McCulloch and Pitts also use the term "temporal summation" for the same concept.

> Observed temporal summation of impulses at greater intervals is impossible for single neurons and empirically depends upon structural
properties of the net.

So the time period can vary depending on the neural net but in general for each neuron is about the same.

> Between the arrival of impulses upon a neuron and its own propagated impulse there is a synaptic delay of >0.5 ms.

It takes >0.5 milliseconds to travel across a synapse.

> During the first part of the nervous impulse the neuron is absolutely refractory to any stimulation. Thereafter its excitability returns rapidly, in some cases reaching a value above normal from which it sinks again to a subnormal value, whence it returns slowly to normal.

Self-explanatory - some nice graphs can be found on [Wikipedia's action potential article](./https://en.wikipedia.org/wiki/Action_potential).

> Frequent activity augments this subnormality.

Here McCulloch and Pitts may be referring to [Spike-timing-dependent plasticity (STDP)](https://en.wikipedia.org/wiki/Spike-timing-dependent_plasticity) and [Hebbian learning](https://en.wikipedia.org/wiki/Hebbian_theory), neither of which were known or formulated at the time.

> Such specificity as is possessed by nervous impulses depends solely upon their time and place and not on any other specificity of nervous energies. Of late only inhibition has been seriously adduced to contravene this thesis. 

McCulloch and Pitts are trying to say that all impulses are essentially the same. This assumption might be fine for this paper, but I'm not sure their [statement](https://en.wikipedia.org/wiki/Neurotransmitter) [holds](https://en.wikipedia.org/wiki/Neuromodulation) today. The exception to their statement is inhibition, described below. 

### Inhibition

> Inhibition is the termination or prevention of the activity of one group of neurons by concurrent or antecedent activity of a second group.

So one group of "anti-impulses" blocking a group of impulses that come afterwards.

> Until recently this could be explained on the supposition that previous activity of neurons of the second group might so raise the thresholds of internuncial neurons that they could no longer be excited by neurons of the first group, whereas the impulses of the first group must sum with the impulses of these internuncials to excite the now inhibited neurons. 

Previously inhibation was explained by network trickery (gaming the thresholds).

> Today, some inhibitions have been shown to consume <1 ms. This excludes internuncials and requires synapses through which impulses inhibit that neuron which is being stimulated by impulses through other synapses. 

Now it is known that some [synapses are inhibitory by design](https://en.wikipedia.org/wiki/Inhibitory_postsynaptic_potential).

> As yet experiment has not shown whether the refractoriness is relative or absolute. We will assume the latter and demonstrate that the
difference is immaterial to our argument.

By "relative or absolute" McCulloch and Pitts are saying that it is not known whether these inhibitory synapses simply turn off a neuron (temporarily) all-together, or if they just contribute negatively towards excitation.

They now explain why it doesn't matter.

> Either variety of refractoriness can be accounted for in either of two ways. The “inhibitory synapse” may be of such a kind as to produce a substance which raises the threshold of the neuron, or it may be so placed that the local disturbance produced by its excitation opposes the alteration induced by the otherwise excitatory synapses. Inasmuch as position is already known to have such effects in the cases of electrical
stimulation, the first hypothesis is to be excluded unless and until it be
subtantiated, for the second involves no new hypothesis. We have, then, two
explanations of inhibition based on the same general premises, differing only in the assumed nervous nets and, consequently, in the time required for inhibition. Hereafter we shall refer to such nervous nets as *equivalent in the extended sense*. Since we are concerned with properties of nets which are invarient under equivalence, we may make the physical assumptions which are most convenient for the calculus.

We don't know whether inhibitory synapses increase the threshold, or block other synapses. They are functionally the same except for the time it takes to inhibit, so we will just wave our hands, use the most convenient assumption, and move on.

### Inspiration

This next paragraph is one of the most enjoyable in the paper. It kicks off as follows:

> Many years ago one of us, by considerations impertinent to this argument,
was led to ...

Was it McCulloch or Pitts who had the original inspiration for the paper?! 

Either way, they go on about how neural nets can be represented by propositional logic (as was hinted in the abstract), and it is all very readable. Note that by disjunction they mean [OR](https://en.wikipedia.org/wiki/Logical_disjunction) and by conjunction they mean [AND](https://en.wikipedia.org/wiki/Logical_conjunction) (in general they use the language as described in [this article](https://en.wikipedia.org/wiki/Logical_connective)).

Finally we get to two "difficulties":

> The first concerns facilitation and extinction, in which antecedent activity temporarily alters responsiveness to subsequent stimulation of one and the same part of the net.

I believe by this they are referring to the [creation of new neurons](https://en.wikipedia.org/wiki/Neurogenesis) and the [death of others](https://en.wikipedia.org/wiki/Neurodegenerative_disease).

> The second concerns learning, in which activities concurrent at some previous time have altered the net permanently, so that a stimulus which would previously have been inadequate is now adequate.

I think this is a rather nice definition for learning. 

> But for nets undergoing both alterations, we can substitute equivalent fictitious nets composed of neurons whose connections and thresholds are unaltered.

We will learn towards the end of [section 2](./GUIDE.md#2-the-theory-nets-without-circles) that there will be a formal equivalence of sorts for neuron creation, learning, etc. within the calculus. The following few sentences are some of the most interesting in the paper:

> But one point must be made clear: neither of us conceives the formal equivalence to be a factual explanation. *Per contra!* - we regard facilitation and extinction as dependent upon continuous changes in threshold related to electrical and chemical variables, such as after-potentials and ionic concentrations; and learning as an enduring change which can survive sleep, anaesthesia, convulsions and coma. The importance of the formal equivalence lies in this: that the alterations actually underlying facilitation, extinction and learning in no way affect the conclusions which follow from the formal treatment of the activity of nervous nets, and the relations of the corresponding propositions remain those of the logic of propositions.

McCulloch and Pitts are saying a few subtle things. First, they are (enthusiastically) not claiming to have an explanation for dynamic neuron facilitation (creation), extinction (death), and learning in their mathematics. Their main point about these "alterations" is that they don't affect the mathematics they are about to propose (presumably because the authors believe they are less fundamental than the excitation mechanics, etc. they describe above).

This area around the foundations of AI and inspiration from biological neural networks is always fascinating to read about.

The final paragraph briefly introduces the idea of neural nets containing circles, which will be treated in full in [section 3](./GUIDE.md#3-the-theory-nets-with-circles).

## 2. The Theory: Nets without Circles

### Final Assumptions

Section 1 started with the sentence,

> Theoretical neurophysiology rests on certain cardinal
assumptions.

and section 2 begins with,

> We shall make the following physical assumptions for our calculus.

which essentially summarizes our assumptions from section 1.

As [noted by C.J. Wilson](https://marlin.life.utsa.edu/mcculloch-and-pitts.html) the major thing to take note of in these assumptions, is that in combination they allow McCulloch and Pitts to work in discrete time periods, specifically that of the synaptic delay.

### Notation Hell

Ok, now to the hardest part of the paper - understanding the notation. The notation in the paper is drawn from two primary sources: *[Principia Mathematica](https://lesharmoniesdelesprit.files.wordpress.com/2015/11/whiteheadrussell-principiamathematicavolumei.pdf)* by [Alfred North Whitehead](https://en.wikipedia.org/wiki/Alfred_North_Whitehead) and [Bertrand Russell](https://en.wikipedia.org/wiki/Bertrand_Russell), and *[Logical Syntax of Language](https://altexploit.files.wordpress.com/2017/09/rudolf-carnap-logical-syntax-of-language-international-library-of-philosophy-2001.pdf)* by [Rudolf Carnap](https://en.wikipedia.org/wiki/Rudolf_Carnap). [We know these influences come from Pitts](https://en.wikipedia.org/wiki/Walter_Pitts#Academic_career).

At this point I will point out some links that were invaluable in figuring out what is going on notation-wise here (alongside the source material already linked above):
- [Stanford Encyclopedia of Philosophy's article on Principia Mathematica Notation](https://plato.stanford.edu/entries/pm-notation)
- [Stanford Encyclopedia of Philosophy's article on Logical Syntax of Language](https://plato.stanford.edu/entries/carnap/syntax.html)

TODO

## 3. The Theory: Nets with Circles

## 4. Consequences

# Overall Thoughts
1. There is so much inspiration from biological neural nets in this paper. Many experts today are [more hesitant to take inspiration from the human brain](https://youtu.be/cdiD-9MMpb0?si=DGZ2MZUZj27fl0b2&t=362). To me, whether or not to anthropomorphize is one of the more interesting open questions today.