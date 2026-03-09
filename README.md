---
title: Payorian Cooperation is easy with Kripke frames
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

We consider a program that takes another program as input, and outputs True to cooperate with it or False to defect.
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

## Kripke frames

In fact, it's easy enough that I think it's a suitable first exercise in using Kripke frames.
So I hope you'll keep reading even if you've never used them before, and this post can serve as a tutorial.
To sell them a bit, I think of them as a tree data structure for tracking quotation levels.
In the prisoner's dilemma tournament, these are base reality, you imagining your opponent, you imagining your opponent imagining you, and so on.
They end up at different depths of the tree.

The way they come up in the prisoner's dilemma tournament is that we resolve matches using provability logic.
But instead of writing proofs, we're going to use the semantics.
If you don't know what I mean by semantics, I hope it helps to say that it's analogous to resolving questions in propositional logic using truth tables.
The semantics of propositional logic are that a sentence can be satisfied or not by a row in a truth table.
Or, put another way, a row in a truth table can be a model for a sentence in propositional logic.
So what does a model for a sentence of modal logic look like?
(Really we'll be applying this to a collection of sentences, but they can be combined to a single sentence by conjunction).

We'll start with a Kripke frame, which is a set of possible worlds and a relation on that set.
I had always heard this called an "accessibility" relation, but [the Arbital page](https://www.lesswrong.com/w/kripke-model) calls it a "visibility" relation, so I'll stick with that.

In the special case of provability logic, the visibility relation must be transitive.
When I draw a Kripke frame like $w_0 \rightarrow w_1 \rightarrow w_2$, I'm not going to draw an arrow from $w_0$ to $w_2$, but $w_2$ is visible from $w_0$.
So the diagrams will be directed graphs, and visibility is reachability following the arrows.
The directed graph is I suppose not necessarily a tree, but it's only in the case of a tree that I have some intuition, where a possible world deeper in the tree is more deeply nested in whatever kind of quotation we're considering.

A Kripke model is a Kripke frame, plus an assignment of which sentences are true at each possible world.
Our strategy for disproving a sentence of provability logic will be to show that it cannot be satisfied by any Kripke model.
(And to prove a sentence, we'll disprove the negation.)

The sentences that we assign to the possible worlds are not just "elementary" sentences of propositional logic, but can include modal sentences (those including $\Diamond$ or $\Box$).
Modal sentences at one world have implications for what sentences must be true at other worlds.
A claim of possibility $\Diamond P$ requires that $P$ be true in a visible world; in a tree, $\Diamond P$ at a node requires $P$ at some descendant node (not necessarily a direct child).
A claim of necessity $\Box P$ requires that $P$ be true in all visible worlds; in a tree, $\Box P$ at a node requires $P$ in all descendants.

Worlds are not necessarily visible from themselves, because this is provability logic so we can't in general go from $\Box P$ to $P$.

Well, that's not the best beginner's guide to Kripke frames.
If you want a more complete explanation, check out [the Arbital page](https://www.lesswrong.com/w/kripke-model) or [the SEP](https://plato.stanford.edu/entries/logic-modal/#PosWorSem).
But at least I've repeated the facts that I'm going to actually use, so let's move on to applying these ideas to the Payorian FairBot.

## Proving cooperation with Kripke frames, and CooperateBot

Our strategy for proving cooperation is to assume FairBot defects, write down what conditions this puts on a Kripke model, and look for a contradiction.
A contradiction means no Kripke model can satisfy the premises, so FairBot must cooperate.

To assume FairBot defects means to assume

$$\lnot \Box (\Box P \rightarrow Q)$$

Or equivalently,

$$\Diamond (\Box P \land \lnot Q)$$

A statement of possibility.
We'll call the world in which this statement is true $w_0$, and write what this implies about other possible worlds as so:

$$
\begin{array}{ccl}
  w_0 & \Vdash & \Diamond (\Box P \land \lnot Q)\\
  \downarrow & & \\
  w_1 & \Vdash & \Box P, \lnot Q
\end{array}
$$

We satisfied the proposition that this stuff is possible at the root world by making it true in a visible world.

Anyway, now we can prove that FairBot cooperates with CooperateBot.
CooperateBot always cooperates, meaning that $Q$ is true.
The assumption we will add, therefore, is $\Box Q$, that is, that $Q$ is _provable_.
FairBot's behavior depends upon what it can prove about its opponent, so we will always wrap the description of the opponent's behavior like this.

$\Box Q$ at $w_0$ implies $Q$ in all downstream worlds, which contradicts with $\lnot Q$ at $w_1$.
So there cannot be a Kripke model that simultaneously satisfies FairBot's non-cooperation and $\Box Q$, so FairBot must cooperate.

This behavior justifies the name "FairBot".
It plays fair by cooperating with CooperateBot, rather than playing to win by exploiting it.

Writing this out more carefully.
Let's put these propositions at the root.
First, the behavior of our FairBot:

$$w_0 \Vdash P \leftrightarrow \Box (\Box P \rightarrow Q)$$

Next, the behavior of its opponent CooperateBot, but wrapped as a statement of provability:

$$w_0 \Vdash \Box Q$$

Finally, our assumption that FairBot defects:

$$w_0 \Vdash \lnot P$$

I hope you'll agree that the first statement and the third together are equivalent to what we were putting at the root before.

So to model these statements, we would need a Kripke model where they're all true at the root:

$$
\begin{array}{ccl}
w_0 &\Vdash & P \leftrightarrow \Box (\Box P \rightarrow Q)\\
&  & \Box Q\\
&  & \lnot P
\end{array}
$$

Since this implies the possibility statement from before, we put in the implied visible world:

$$
\begin{array}{ccl}
w_0 &\Vdash & P \leftrightarrow \Box (\Box P \rightarrow Q)\\
&  & \Box Q\\
&  & \lnot P\\
\downarrow \\
w_1 &\Vdash & \Box P, \lnot Q
\end{array}
$$

Since we have $\Box Q$ at the root, that implies $Q$ downstream:

$$
\begin{array}{ccl}
w_0 &\Vdash & P \leftrightarrow \Box (\Box P \rightarrow Q)\\
&  & \Box Q\\
&  & \lnot P\\
\downarrow \\
w_1 &\Vdash & \Box P, \lnot Q, Q
\end{array}
$$

There's our contradiction.
No Kripke model can satisfy the requirements.

## One Payorian FairBot will cooperate with another

Now, let's assume both players are FairBots.
Before we play them against each other, let's just write down what their non-cooperation implies for a Kripke model, as before.
Two separate Kripke frames, side by side:

$$
\begin{array}{c|c}
\begin{array}{ccl}
w_0 & \Vdash & P \leftrightarrow \Box(\Box P \rightarrow Q)\\
    &        & \lnot P\\
\downarrow & & \\
w_1 & \Vdash & \Box P,\ \lnot Q
\end{array}
&
\begin{array}{ccl}
w_0' & \Vdash & Q \leftrightarrow \Box(\Box Q \rightarrow P)\\
     &        & \lnot Q\\
\downarrow & & \\
w_1' & \Vdash & \Box Q,\ \lnot P
\end{array}
\end{array}
$$

Now, considering a FairBot whose opponent is a FairBot.
It's going to look like grafting the second Kripke frame above onto the first.
To set it up, as in the previous section, we put three premises at the root world: the behavior of our FairBot, the behavior of its opponent (wrapped in a provability modality), and the assumption that our FairBot defects.

$$
\begin{array}{ccl}
w_0        & \Vdash & P \leftrightarrow \Box (\Box P \rightarrow Q)\\
           &        & \Box (Q \leftrightarrow \Box(\Box Q \rightarrow P))\\
           &        & \lnot P\\
\downarrow &        & \\
w_1        & \Vdash & \Box P, \lnot Q\\
           &        & Q \leftrightarrow \Box(\Box Q \rightarrow P) \\
\downarrow &        & \\
w_2        & \Vdash & \Box Q, \lnot P, P
\end{array}
$$

Contradiction at $w_2$.
That's our proof that a FairBot cooperates with another FairBot.

But let's slow down and look at what happened.
At $w_1$, we "unwrapped" the description of the other FairBot.
Because we also have $\lnot Q$ at $w_1$, this world now looks like our $w_0'$ from before.
So at $w_2$, we have everything from our previous $w_1'$.
But because we had $\Box P$ at $w_1$, we unwrap that at $w_2$, resulting in a contradiction.

## Why this procedure feels right to me

I think of $w_1$ as a world our FairBot is simulating.
It's asking, "if I provably cooperate, what will my opponent do?"
When the opponent is itself a FairBot, $w_1$ is familiar: it looks just like [when we considered FairBot vs CooperateBot](https://www.lesswrong.com/posts/LaCP6WyNzX8kiZn3w/payorian-cooperation-is-easy-with-kripke-frames#Proving_cooperation_with_Kripke_frames__and_CooperateBot), but with $P$ and $Q$ switched.

I imagine $w_2$ as a simulation done by the simulated opponent FairBot.
We avoid an infinite regress, because we have assumed that in this simulation, the simulated opponent finds that the first FairBot cooperates.

This feels like the right depth to me.
I imagine you imagining me, and we're done.
That feels like what I actually do when I'm thinking through Newcomb-like problems.
In Newcomb's problem, I think about Omega's simulation of me, which I suppose is in $w_2$, if my mental "simulation" of Omega is in $w_1$.

## The sense in which this is simpler than Löbian cooperation

But I feel I haven't really indicated the appeal until I contrast this to the complexity I encountered with Löbian cooperation.
Before I knew about Payorian cooperation, I tried putting these premises at the root world:

$$
\begin{gather}
H \leftrightarrow \Box H\\
\Box (H \leftrightarrow \Box H)\\
\Box (L \rightarrow (\Box L \rightarrow H))\\
\lnot H
\end{gather}
$$

I didn't like having to introduce the Löb sentence $L$.
I felt like I was putting it in by hand, and I wanted everything to be automatic.
But with these premises, I could just mechanically hang requirements on a Kripke frame until I found a contradiction—in $w_4$.

In [Critch's post](https://www.lesswrong.com/posts/2WpPRrqrFQa6n2x3W/modal-fixpoint-cooperation-without-loeb-s-theorem), he also notes that a Payorian approach avoids the auxiliary sentence.
And he argues that it is simpler, by counting lines of his proof in a "natural deduction"-type system.

The Kripke frame approach suggests a different way of quantifying complexity: how deep in the Kripke frame do you have to go, before you rule out a consistent Kripke model?
For Payorian cooperation, you go two worlds deep, which feels right.
For Löbian cooperation, you go four worlds deep, which feels wrong.

I titled this post "Payorian Cooperation is easy with Kripke frames".
You could also say that proving cooperation with Kripke frames is easy, once you use Payorian cooperation.
On a personal level, that's the significance.
When I first started doing these kinds of proofs a few years ago, I was optimistic about defining a logical decision theory this way, with the Kripke frame structure tracing out the decision algorithm.
I thought proving FairBot cooperates with itself would be the tutorial level, but it felt more like a final boss.
Stumbling across Critch's post has restored my confidence in the potential of this approach.
