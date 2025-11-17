# Homework 3
**Due:** November 18
**Authors:** Zachariah Craven, Dillon Shaffer

---

## Problem 1
Show that the logarithmic approximation ratio found in class for the Set Cover
problem is tight (i.e., show that there are arbitrarily large Set Cover instances such that
$\text{ALG} \le O(\ln n)\, \text{OPT}$).

### Solution
Given sets:



$$
{1,2,3,4,5,6,7,8}
$$

$$
{9,10,11,12}
$$

$$
{13,14}
$$

$$
{15}
$$

$$
Even \hspace{1cm} Odd
$$

Optimal is Even and Odd sets. Algorithm selects set 1 as the next greatest set is odd with 8 elements. Then sets two, three, and four.

\[
Alg = k
\]

\[
Opt = 2
\]

\[
n = 8 + 4 + 2 + 1
\]

\[
n = 2^{k-1} + 2^{k-2} ...+ 2^{k-k} = 2^k-1
\]

\[
\ln(n) = k \ln(2) \qquad (k \to \infty)
\]

\[
k = \frac{ln(n)}{ln(2)}
\]

Dropping Constants we see $Alg = O(ln n)$ as n grows.
$$
\frac{Alg}{Opt} = \frac{ln(n)}{2ln(2)} = O(ln(n))
$$

\[
Alg \leq \mathcal{O}(ln(n)) * Opt
\]

---

## Problem 2
Consider the following algorithm for the Knapsack problem discussed in class:

For each item, calculate its value-to-weight ratio and greedily fill the knapsack with the
highest-valued item at each iteration.

**Question:**
What is the approximation ratio for this algorithm? Explain why that is the case.

**Solution:**

There is no constant approximation ratio. As the ratio will be $\frac{W}{2}$. Which can be infinitely bad.

| Item | Value | Weight | Ratio |
| ---- | ----- | ------ | ----- |
| 1    | W     | W      | 1     |
| 2    | 2     | 1      | 2     |
| 3    | ε     | 1      | ε     |
| …    | …     | …      | …     |
| W+1  | ε     | 1      | ε     |

If we have the items depicted above, our algorithm will select all of the small ratio items. Given the value of items, $(W-1)\epsilon$,  it will select item 2. At this point, item 1 will not be able to fit in the knapsack and the algorithm will halt. 
$$
Alg = 2 + (W-1)\epsilon = 2 \qquad as \qquad (\epsilon \to 0)
$$

$$
Opt = W
$$

$$
Alg \geq \frac{1}{\alpha}Opt
$$


$$
\frac{Opt}{Alg} \leq \alpha
$$

$$
Thus: \alpha = \frac{W}{2}
$$



## Problem 3
Consider the following algorithm for the Knapsack problem discussed in class: Select the best of:

1. The output from the algorithm in **Problem 2**.
2. The single highest-valued item (whose weight is below the threshold).

**Question:**
What is the approximation ratio for this algorithm? Explain why that is the case.

*Hint:* Consider what would happen if we were allowed to take fractional amounts of the items.

---

##  Problem 4
Consider this variation on the classic Set Cover problem:

We have a universe of elements $U$ and a collection of sets $S$, where each set
$S_i \in S$ has a non-negative cost $c_i$. Each element of the universe appears in at most
$d$ sets (for a fixed constant $d$ independent of the element).

The goal is to select a subset of $S$ of minimum total cost such that every element of $$U
is contained in at least one selected set.

**Task:**

- Formulate this problem as an ILP.
- Relax the ILP.
- Perform a deterministic rounding procedure to obtain an approximation algorithm.
- Describe the algorithm and prove that it achieves a $d$-approximation ratio.



### Solution

#### First, an ILP

Our goal is: $min \sum_{i < |S|} c_i \cdot x_i$

We have two types of variables:

- $x_i, \forall i < |S|$, where each $x_i$ represents the selection of the corresponding $S_i$
- $c_i, \forall i < |S|$, where each $c_i$ represents $cost(S_i)$

And our rules:

- $(\sum_{S_i \in \text{sets-that-contain}(u)} x_i) \ge 1, \forall u \in U$, which represents that for each element, there must be at least one set selected that contains it.
- $(x_i \in [0, 1]), \forall i < |S|$, which represents that each set can either be selected or not, no in-between

#### Second, A Relaxed ILP

We, unfortunately, cannot completely relax our ILP. However, we jump a head a little to explain why there is a work around.

We have to modify the second rule into this: $(x_i \in \{0, 1\}), \forall i < |S|$ to be a real-valued LP.

#### Third, A "Deterministic Rounding Procedure"

In order to reduce the complexity of our ILP to polynomial time, we relaxed it into an LP. This, however, discarded the property of $x_i \in [0, 1]$ in favor of $x_i \in {0, 1}$. This is okay if we can define what it means for an $x_i$ to be 0.5, or 0.7, or any other real-value. We can use the property of this problem, that each element is in at most $d$ sets. We utilize it by rounding up any value greater-than-or-equal-to $\frac{1}{d}$. This value is special because it is the minimum value of an $x_i$ that signifies that the corresponding $S_i$$ must be in the Set-Cover.

We can logically prove this to be true by thinking about what happens when a set is not selected because each of its elements in $U$ are selected by other, lower-cost sets. Since each element requires that the sum of the selection values of each of its sets must be greater-than-or-equal-to 1, it means that a set can be partially selected but only truly in the set-cover if it passes that $1/d$ threshold. This is because one set with higher cost but many more elements can be partially selected with another set that has a low cost but only one element that overlaps. In that case, the larger set will be picked (despite its higher cost) because the value of that set is important to many clauses for the different elements it contains (assuming there is no larger or cheaper cost set). Smaller values ($< 1/d$) may be attributed to other sets that do provide an element but are not essential to the cover given the min-cost. This is because they are required to have a non-zero value by the clause for each of their universe-elements but not in a significant way to pass the $1/d$ threshold.

#### Fourth,  the d-ratio

To prove that this is a $d$-approximation algorithm, we need to compute the minimum cost and show the difference from the optimal algorithm.
$$
cost(S) = \sum_{i:x_i\ge1/d} c_i
$$
We can then prove that $ALG \ge d \cdot OPT$ by showing that $1 \le \frac{x_i}{d}$ for each element in the sum. This fact let's us show that $\sum c_i \le d \sum c_i x_i$. In other words, the optimal cost is less-than-or-equal-to the approximated cost times $d$.

---

## Problem 5
Suppose we have $n$ tasks to be completed and $w$ identical workers who can
complete them. Each task has a time $t_i$ that it takes a worker to complete.

Let $A_w$ denote the tasks assigned to worker $w$. This means the total work time for worker $w$ is:

\[
T_w = \sum_{i \in A_w} t_i.
\]

The goal is to assign all tasks to workers in such a way as to minimize the maximum work time:
$$
\min \max_w T_w
$$
Consider the following algorithm:
 For each task (in arbitrary order), assign the task to the worker who currently has the smallest work time.

Show that this algorithm is a 2-approximation algorithm.

*Hint:* First show that $\frac{1}{w}\sum_i t_i \le \text{OPT}$.
 Then consider the last task assigned to the worker responsible for the algorithm’s cost.
 Why was that task assigned to that worker?

------

## **Problem 6**

The **Minimum Dominating Set** problem is:
 Given a graph, find the smallest subset of the vertices such that each remaining vertex has a neighbor in the selected subset.

**Task:**
 Find an approximation algorithm for the Minimum Dominating Set problem.
 (Hint: Draw it out and consider whether it is similar to a problem studied in class.)

------

## **Problem 7**

The **Max Cut** problem is as follows:

Given an undirected graph $G = (V, E)$ with non-negative edge weights, find a cut $(A, B)$ such that the sum of the edge weights crossing the cut is maximized.

Consider the following randomized algorithm:
 Assign vertex $v$ to set $A$ with probability $\tfrac{1}{2}$.
 Let $B = V \setminus A$.

**Task:**
 Show that this algorithm is a $\tfrac{1}{2}$-approximation algorithm.

---
