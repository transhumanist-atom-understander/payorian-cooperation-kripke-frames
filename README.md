---
title: Payorian Cooperation Explained with Kripke Frames
format: html
---

The context is MIRI's twist on [Axelrod's Prisoner's Dilemma tournament](https://doi.org/10.1126/science.7466396).
Axelrod's competitors were programs, facing each other in an iterated Prisoner's Dilemma.
[MIRI's tournament is a one-shot Prisoner's Dilemma, but the programs get to read their opponent's code](https://cdn.aaai.org/ocs/ws/ws1249/8833-38015-1-PB.pdf).
Or, rather, a description of the behavior of the code in Gödel-Löb provability logic, which turns out to be enough to determine their behavior in the setup.

We consider these programs as functions that take in another function, and output "True" if they cooperate.
So, $A(B)$ means that $A$ cooperates when the opponent is $B$.

One competitor, FairBot ($F$), provides a clean illustration of Löbian cooperation.
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

It gets more interesting when considering two FairBots with distinct code, which turn out to also cooperate with each other.
But let's move on to another kind of FairBot.

It's hard to decide what to call it.
It still expresses the idea of fairness to me, so I still want to call it FairBot.
But, let's say, what we were discussing before is a Löbian FairBot.
Next, we'll consider a "Payorian FairBot", since [Andrew Critch proved they cooperate with each other with "Payor's lemma"](https://www.lesswrong.com/posts/2WpPRrqrFQa6n2x3W/modal-fixpoint-cooperation-without-loeb-s-theorem)

The Payorian FairBot:

$$F(X) \leftrightarrow \Box (\Box F(X) \rightarrow X(F))$$

Instead of checking whether the opponent cooperates, we check whether FairBot's provable cooperation implies the opponent cooperates.
But be careful about "implies"; it's material implication, and doesn't denote real dependence.
For example, if the opponent unconditionally cooperates, then this implication is trivially true.

The Payorian FairBot looks more complicated.
But reasoning about it is much simpler, at least in my experience.
My goal in this post is to share with you the perspective from which Payorian cooperation looks simpler.

That perspective is based on Kripke frames, which are directed graphs of possible worlds.
I guess what is motivating me is I think once we've figured out all of this logical decision theory stuff, we'll have tree-shaped Kripke frames that are intuitively comprehensible as considering possibilities ("if I do this, he'll do that, because he knows I would do the other thing. But if I do that...").
Payorian cooperation feels right, when you think through the Kripke frame, as I'll try to show you in this post.
Whereas Löbian cooperation feels wrong—and this I may never write up in a post, precisely because it's so awkward from the Kripke frame perspective that I don't want to explain it.
