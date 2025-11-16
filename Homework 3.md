# Homework 3
**Due:** November 18
**Authors:** Zachariah Craven, Dillion Shaffer

---

## Problem 1
Show that the logarithmic approximation ratio found in class for the Set Cover
problem is tight (i.e., show that there are arbitrarily large Set Cover instances such that
\(\text{ALG} \le O(\ln n)\, \text{OPT}\)).

 **Solution:**
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

Optimal is Even and Odd sets. Algorithm selects set 1 as the next greatest set would have been odd with 8 elements. Then sets two, three, and four.

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

Dropping Constants we see $Alg = O(ln(n))$ as n grows.
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

If we have the items depicted above, our algorithm will select all of the small ratio items. Giving $(W-1)\epsilon$. Then it will select item 2 giving $(W-1)\epsilon + 2$ at which point item 1 will not be able to fit and the algorithm will stop. 
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

**Solution:**

The Algorithm $(Alg_1)$ in Problem 2 is optimal if we are able to take fractional amounts of an item because if there was a more optimal approach we would then be taking out an item and replacing with something that weighs more and has less value which can not make a more optimal solution.

$$ Opt \leq Opt_{frac} $$ (Since $Opt_{frac}$ has more options then $Opt$)

$$Alg_2 = max \begin{cases}
v(Alg_1)\\
Single \: Highest \: Valued \: Item \: (v_j)
\end{cases}$$

$$ Opt \leq Opt_{frac} \leq v(Alg_{1})+v_j \qquad $$ Where $v_j$ is the value of the item that is added fractionally.

$$Opt_{frac} = v(Alg_1) + v_{j \: frac} \implies Opt \leq v(Alg_1) + v_j$$

$$Alg_2 = \max{v(Alg_1), v_j} \geq \frac{1}{2} v(Alg_1) + v_j \geq \frac{1}{2} Opt$$

Thus: $$Alg_2 \geq \frac{1}{2}Opt \qquad \alpha = \frac{1}{2}$$

---

## Problem 4
Consider this variation on the classic Set Cover problem:

We have a universe of elements \(U\) and a collection of sets \(S\), where each set
\(S_i \in S\) has a non-negative cost \(c_i\). Each element of the universe appears in at most
\(d\) sets (for a fixed constant \(d\) independent of the element).

The goal is to select a subset of \(S\) of minimum total cost such that every element of \(U\)
is contained in at least one selected set.

**Task:**
- Formulate this problem as an ILP.
- Relax the ILP.
- Perform a deterministic rounding procedure to obtain an approximation algorithm.
- Describe the algorithm and prove that it achieves a \(d\)-approximation ratio.

---

## Problem 5
Suppose we have \(n\) tasks to be completed and \(w\) identical workers who can
complete them. Each task has a time \(t_i\) that it takes a worker to complete.

Let \(A_w\) denote the tasks assigned to worker \(w\). This means the total work time for worker \(w\) is:

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