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
It suggests a different way of defining a FairBot, something more like "If, when I hypothetically cooperate with you, you would cooperate with me, then I really will cooperate."

This post is my attempt to explain why I think this approach is more promising, or at least why I like it more.

When thinking through these kinds of reflection problems with modal logic, I use Kripke frames, which are directed graphs of possible worlds.
My fantasy is that when we've figured out this logical decision theory stuff, the reasoning will involve tree-shaped Kripke frames that are intuitively interpretable as something like a game tree.
Like, "if I do this, he'll do that, because he knows I would do the other thing. But if I do that..."

With Löbian cooperation, I got nowhere with this.
But Critch's post was the first glimmer of the fulfillment of my fantasy.
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
It gets more interesting when considering two FairBots with distinct code, which turn out to also cooperate with each other, but let's move on to another kind of FairBot.

## Payorian FairBot

Let's call what we were discussing before a "Löbian FairBot", and next, we'll consider a "Payorian FairBot".
I still want to call it a "FairBot", since I think the following does express a kind of fairness:

$$F(X) \leftrightarrow \Box (\Box F(X) \rightarrow X(F))$$

Actually, I'd rather drop all these parentheses, and say that $P$ means the FairBot cooperates, and $Q$ means its opponent cooperates.
If you like, $P \equiv F(X)$ and $Q \equiv X(F)$.
Then the condition is:

$$P \leftrightarrow \Box (\Box P \rightarrow Q)$$

Instead of checking whether the opponent cooperates, we check whether FairBot's provable cooperation implies the opponent cooperates.
But be careful about "implies"; it's material implication, and doesn't denote real dependence.
For example, if the opponent unconditionally cooperates, then this implication is trivially true—we'll cover this case in more detail later.

The Payorian FairBot looks more complicated.
But reasoning about it is much simpler, at least with Kripke frames.

## Proving cooperation with Kripke frames, and CooperateBot

Here's how we will prove cooperation, under whatever assumptions.
We will assume FairBot defects.
Then, we will write down conditions on a Kripke model for these propositions, in the hopes of finding a contradition, thus proving cooperation.

To assume FairBot defects means to assume

$$\lnot \Box (\Box P \rightarrow Q)$$

Or equivalently,

$$\Diamond (\Box P \land \lnot Q)$$

We'll call the world in which this is true $w_0$, and write what this implies about other possible worlds as so:

$$
\begin{aligned}
  &w_0 \\
  &\downarrow \\
  &w_1 : \Box P, \lnot Q
\end{aligned}
$$

The arrow denotes that world $w_1$ is "accessible" or "visible" from the root world $w_0$.
That's how we satisfy the proposition that this stuff is possible at the root world: it's true in a visible world.

Don't forget that whatever propositions we're trying to model are true at the root world, even though I won't write them.

Anyway, now we can prove that FairBot cooperates with CooperateBot.
CooperateBot always cooperates, meaning that $Q$ is true.
The assumption we will add, therefore, is $\Box Q$, that is, that $Q$ is _provable_.
FairBot's behavior depends upon what it can prove about is opponent, so we will always wrap the description of the opponent's behavior like this.

$\Box Q$ at $w_0$ implies $Q$ in all downstream worlds, which contradicts with $\lnot Q$ at $w_1$.
So there cannot be a Kripke model that simultaneously satisfies FairBot's non-cooperation and $\Box Q$, so FairBot must cooperate.

This behavior justifies the name "FairBot".
It plays fair by cooperating with CooperateBot, rather than playing to win by exploiting it.
