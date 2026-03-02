---
title: Payorian Cooperation Explained with Kripke Frames
format: html
---

The context is MIRI's twist on [Axelrod's Prisoner's Dilemma tournament](https://doi.org/10.1126/science.7466396).
Axelrod's competitors were programs, facing each other in an iterated Prisoner's Dilemma.
[MIRI's tournament is a one-shot Prisoner's Dilemma, but the programs get to read their opponent's code](https://cdn.aaai.org/ocs/ws/ws1249/8833-38015-1-PB.pdf).
Or, rather, a description of the behavior of the code in Gödel-Löb provability logic, which turns out to be enough to determine their behavior in the setup.

One fun result, right in the beginning of the paper, is about a program, FairBot, whose behavior is specified by "I'll cooperate with you if you (provably) cooperate with me".
Despite the appearance of circularity, FairBot cooperates with itself.
The proof involves Löb's theorem, so we call this Löbian cooperation.

[Andrew Critch has suggested another way of proving self-cooperation](https://www.lesswrong.com/posts/2WpPRrqrFQa6n2x3W/modal-fixpoint-cooperation-without-loeb-s-theorem).
Instead of Löb's theorem, we use what he calls "Payor's lemma".
It suggests a different way of defining a FairBot, something more like "I'll cooperate with you, if that means you will cooperate with me".

This post is my attempt to explain why I think this approach is more promising, or at least why I like it more.

When thinking through these kind of self-reflection problems with modal logic, I use Kripke frames, which are directed graphs of possible worlds.
My fantasy is that when we've figured out this logical decision theory stuff, the reasoning will involve tree-shaped Kripke frames that are intuitively interpretable as something like a game tree.
Like, "if I do this, he'll do that, because he knows I would do the other thing. But if I do that..."

With Löbian cooperation, I got nowhere with this.
But Critch's post was the first glimmer of the fulfilment of my fantasy.
The only way to communicate to you what I see in it, though, is to walk you through a proof of self-cooperation with Kripke frames.

## Löbian FairBot

First, reviewing the Löbian FairBot and Löbian cooperation, without Kripke frames.

We consider these programs as functions that take in another function, and output "True" if they cooperate.
So, $A(B)$ means that $A$ cooperates when the opponent is $B$.

For FairBot, we have

$$F(X) \leftrightarrow \Box X(F)$$

That is, FairBot cooperates if and only if its opponent provably cooperates.

For an example of Löbian cooperation, consider FairBot playing against itself.
Replacing $X$ with $F$:

$$F(F) \leftrightarrow \Box F(F)$$

This is a Henkin sentence, that is, a sentence asserting its own provability:

$$H \leftrightarrow \Box H$$

Such sentences are in fact provable.
This was established by Löb, in the paper that proved what we now call Löb's theorem ([some history from SEP](https://plato.stanford.edu/entries/goedel-incompleteness/#RefPriLbsThe)).
It gets more interesting when considering two FairBots with distinct code, which turn out to also cooperate with each other, but but let's move on to another kind of FairBot.

## Payorian FairBot

Let's say what we were discussing before is a Löbian FairBot, and next, we'll consider a "Payorian FairBot".
I still want to call it a FairBot, since I think the following does express a kind of fairness:

$$F(X) \leftrightarrow \Box (\Box F(X) \rightarrow X(F))$$

Instead of checking whether the opponent cooperates, we check whether FairBot's provable cooperation implies the opponent cooperates.
But be careful about "implies"; it's material implication, and doesn't denote real dependence.
For example, if the opponent unconditionally cooperates, then this implication is trivially true.

The Payorian FairBot looks more complicated.
But reasoning about it is much simpler, at least with Kripke frames.
