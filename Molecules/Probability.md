---
#Probability
---

# Probability

## (What is) Probability

Define _probbability_ in terms of a sample space $S$, which is a set whole elements are called **elementary events**. An **event** is a subset of sample space $S$. 

_probability distribution_ $P()$ on sample space $S$ is a **mapping** from events of $S$ to a real number. Here are three axioms on $P()$:

1. $P(A) \ge 0$ for any event A.
2. $P(S) = 1$.
3. $P(A \cup B) = P(A) + P(B)$ if events A and B are mutually exclusive events.

Now, we can define _probability_ of event(s) A as $P(A)$, which is in terms of _probability distribution (function)_ $P()$.

## Conditional probability and independence

Some time we have prior knowledge about the outcome of a experiment. 

The **conditional probability** of event A given event B occurs is defined as:

$$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

whenever, $P(B) \neq 0$ . 

Two events are **independent** $P(A \cap B) = P(A)P(B)$. 

## Bayes Rule

$$P(A|B) = \frac{P(B|A)P(A)}{P(B)}$$

The proof:

Commutative law: $P(AB) = P(BA)$, then

$$P(A|B)P(B) = P(B|A)P(A)$$

## Discrete Random Variable

A _discrete random variable_ $X$ is a _function_ from a finite or countable infinite sample space $S$ to the real numbers. 

So the insight is that random variable is a NOT a probability, it is a function; even $f(x)$ is NOT a probability. However there is a relationship between random variable and probability:

For a random variable $X$ and a real number $x$, we define the event X = x to be ${s \in S : X(s) = x}$; thus:

$$P(X = x) = \sum_{s \in S: X(s)=x}P(s)$$

_probability density function_ : $f(x)$ 

$$f(x) = P(X = x)$$

_joint probability density function_ of $X$ and $Y$:

$$f(x, y) = P(X=x \space and \space Y=y)$$

For $X$ and $Y$ are **independent**: $P(X = x \space and \space Y=y) = P(X=x)P(Y=y)$.

_expected value_:

$$E[X] = \sum_x x P(X=x) = \sum_x x f(x)$$

_Jensen's inequality_, when we apply a **convex** function $f(x)$ to a random variable $X$:

$$E[f(X)] \ge f(E[X])$$

_variance_ of random variable $X$:

$$Var[X] = E[(X - E[X])^2] = E[X^2] - E^2[X]$$