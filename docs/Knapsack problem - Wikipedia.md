---
description:
title: Knapsack problem - Wikipedia
categories: 
 - 알고리즘
aliases: 
 - knapsack problem
tags: [" algo/graph  ", algo/graph]
source: https://en.wikipedia.org/wiki/Knapsack_problem
author: Contributors to Wikimedia projects
created: 2023-02-12T12:31:26
updated: 2023-07-11T15:21:08
---

parent link: [[0011 Algorithms ♾️|algorithms]]

# Knapsack problem - Wikipedia

> ## Excerpt
> 

---
[![](https://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/Knapsack.svg/250px-Knapsack.svg.png)](https://en.wikipedia.org/wiki/File:Knapsack.svg)

Example of a one-dimensional (constraint) knapsack problem: which boxes should be chosen to maximize the amount of money while still keeping the overall weight under or equal to 15 kg? A [multiple constrained problem](https://en.wikipedia.org/wiki/List_of_knapsack_problems#Multiple_constraints "List of knapsack problems") could consider both the weight and volume of the boxes.  
(Solution: if any number of each box is available, then three yellow boxes and three grey boxes; if only the shown boxes are available, then all except for the green box.)

The **knapsack problem** is the following problem in [combinatorial optimization](https://en.wikipedia.org/wiki/Combinatorial_optimization "Combinatorial optimization"):

_Given a set of items, each with a weight and a value, determine which items to include in the collection so that the total weight is less than or equal to a given limit and the total value is as large as possible._

It derives its name from the problem faced by someone who is constrained by a fixed-size [knapsack](https://en.wikipedia.org/wiki/Knapsack "Knapsack") and must fill it with the most valuable items. The problem often arises in [resource allocation](https://en.wikipedia.org/wiki/Resource_allocation "Resource allocation") where the decision-makers have to choose from a set of non-divisible projects or tasks under a fixed budget or time constraint, respectively.

The knapsack problem has been studied for more than a century, with early works dating as far back as 1897.<sup id="cite_ref-1"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-1">[1]</a></sup> The name "knapsack problem" dates back to the early works of the mathematician [Tobias Dantzig](https://en.wikipedia.org/wiki/Tobias_Dantzig "Tobias Dantzig") (1884–1956),<sup id="cite_ref-2"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-2">[2]</a></sup> and refers to the commonplace problem of packing the most valuable or useful items without overloading the luggage.

## Applications\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=1 "Edit section: Applications")\]

Knapsack problems appear in real-world decision-making processes in a wide variety of fields, such as finding the least wasteful way to cut raw materials,<sup id="cite_ref-3"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-3">[3]</a></sup> selection of [investments](https://en.wikipedia.org/wiki/Investment "Investment") and [portfolios](https://en.wikipedia.org/wiki/Portfolio_(finance) "Portfolio (finance)"),<sup id="cite_ref-4"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-4">[4]</a></sup> selection of assets for [asset-backed securitization](https://en.wikipedia.org/wiki/Securitization "Securitization"),<sup id="cite_ref-5"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-5">[5]</a></sup> and generating keys for the [Merkle–Hellman](https://en.wikipedia.org/wiki/Merkle%E2%80%93Hellman_knapsack_cryptosystem "Merkle–Hellman knapsack cryptosystem")<sup id="cite_ref-6"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-6">[6]</a></sup> and other [knapsack cryptosystems](https://en.wikipedia.org/wiki/Knapsack_cryptosystems "Knapsack cryptosystems").

One early application of knapsack algorithms was in the construction and scoring of tests in which the test-takers have a choice as to which questions they answer. For small examples, it is a fairly simple process to provide the test-takers with such a choice. For example, if an exam contains 12 questions each worth 10 points, the test-taker need only answer 10 questions to achieve a maximum possible score of 100 points. However, on tests with a heterogeneous distribution of point values, it is more difficult to provide choices. Feuerman and Weiss proposed a system in which students are given a heterogeneous test with a total of 125 possible points. The students are asked to answer all of the questions to the best of their abilities. Of the possible subsets of problems whose total point values add up to 100, a knapsack algorithm would determine which subset gives each student the highest possible score.<sup id="cite_ref-7"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-7">[7]</a></sup>

A 1999 study of the Stony Brook University Algorithm Repository showed that, out of 75 algorithmic problems, the knapsack problem was the 19th most popular and the third most needed after [suffix trees](https://en.wikipedia.org/wiki/Suffix_tree "Suffix tree") and the [bin packing problem](https://en.wikipedia.org/wiki/Bin_packing_problem "Bin packing problem").<sup id="cite_ref-8"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-8">[8]</a></sup>

## Definition\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=2 "Edit section: Definition")\]

The most common problem being solved is the **0-1 knapsack problem**, which restricts the number _![x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e87000dd6142b81d041896a30fe58f0c3acb2158)_ of copies of each kind of item to zero or one. Given a set of _![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)_ items numbered from 1 up to _![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b)_, each with a weight _![w_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fe22f0329d3ecb2e1880d44d191aba0e5475db68)_ and a value _![v_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7dffe5726650f6daac54829972a94f38eb8ec127)_, along with a maximum weight capacity _![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7)_,

maximize ![\sum _{i=1}^{n}v_{i}x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/85620037d368d2136fb3361702df6a489416931b)

subject to ![\sum _{i=1}^{n}w_{i}x_{i}\leq W](https://wikimedia.org/api/rest_v1/media/math/render/svg/dd6e7c9bca4397980976ea6d19237500ce3b8176) and ![x_{i}\in \{0,1\}](https://wikimedia.org/api/rest_v1/media/math/render/svg/07dda71da2a630762c7b21b51ea54f86f422f951).

Here _![x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e87000dd6142b81d041896a30fe58f0c3acb2158)_ represents the number of instances of item _![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)_ to include in the knapsack. Informally, the problem is to maximize the sum of the values of the items in the knapsack so that the sum of the weights is less than or equal to the knapsack's capacity.

The **bounded knapsack problem** (**BKP**) removes the restriction that there is only one of each item, but restricts the number ![x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e87000dd6142b81d041896a30fe58f0c3acb2158) of copies of each kind of item to a maximum non-negative integer value ![c](https://wikimedia.org/api/rest_v1/media/math/render/svg/86a67b81c2de995bd608d5b2df50cd8cd7d92455):

maximize ![\sum _{i=1}^{n}v_{i}x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/85620037d368d2136fb3361702df6a489416931b)

subject to ![\sum _{i=1}^{n}w_{i}x_{i}\leq W](https://wikimedia.org/api/rest_v1/media/math/render/svg/dd6e7c9bca4397980976ea6d19237500ce3b8176) and ![{\displaystyle x_{i}\in \{0,1,2,\dots ,c\}.}](https://wikimedia.org/api/rest_v1/media/math/render/svg/67d086ac9e491ea240621847e94bdc3b2a1d2b7f)

The **unbounded knapsack problem** (**UKP**) places no upper bound on the number of copies of each kind of item and can be formulated as above except that the only restriction on ![x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e87000dd6142b81d041896a30fe58f0c3acb2158) is that it is a non-negative integer.

maximize ![\sum _{i=1}^{n}v_{i}x_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/85620037d368d2136fb3361702df6a489416931b)

subject to ![\sum _{i=1}^{n}w_{i}x_{i}\leq W](https://wikimedia.org/api/rest_v1/media/math/render/svg/dd6e7c9bca4397980976ea6d19237500ce3b8176) and ![{\displaystyle x_{i}\in \mathbb {Z} ,\ x_{i}\geq 0.}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9d937551941f492c5970aaed67f02c9796c80e28)

One example of the unbounded knapsack problem is given using the figure shown at the beginning of this article and the text "if any number of each box is available" in the caption of that figure.

## Computational complexity\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=3 "Edit section: Computational complexity")\]

The knapsack problem is interesting from the perspective of computer science for many reasons:

-   The [decision problem](https://en.wikipedia.org/wiki/Decision_problem "Decision problem") form of the knapsack problem (_Can a value of at least_ V _be achieved without exceeding the weight_ W_?_) is [NP-complete](https://en.wikipedia.org/wiki/NP-complete "NP-complete"), thus there is no known algorithm that is both correct and fast (polynomial-time) in all cases.
-   While the decision problem is NP-complete, the optimization problem is not, its resolution is at least as difficult as the decision problem, and there is no known polynomial algorithm which can tell, given a solution, whether it is optimal (which would mean that there is no solution with a larger _V_, thus solving the NP-complete decision problem).
-   There is a [pseudo-polynomial time](https://en.wikipedia.org/wiki/Pseudo-polynomial_time "Pseudo-polynomial time") algorithm using [dynamic programming](https://en.wikipedia.org/wiki/Dynamic_programming "Dynamic programming").
-   There is a [fully polynomial-time approximation scheme](https://en.wikipedia.org/wiki/Polynomial-time_approximation_scheme "Polynomial-time approximation scheme"), which uses the pseudo-polynomial time algorithm as a subroutine, described below.
-   Many cases that arise in practice, and "random instances" from some distributions, can nonetheless be solved exactly.

There is a link between the "decision" and "optimization" problems in that if there exists a polynomial algorithm that solves the "decision" problem, then one can find the maximum value for the optimization problem in polynomial time by applying this algorithm iteratively while increasing the value of k. On the other hand, if an algorithm finds the optimal value of the optimization problem in polynomial time, then the decision problem can be solved in polynomial time by comparing the value of the solution output by this algorithm with the value of k. Thus, both versions of the problem are of similar difficulty.

One theme in research literature is to identify what the "hard" instances of the knapsack problem look like,<sup id="cite_ref-pisinger200308_9-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-pisinger200308-9">[9]</a></sup><sup id="cite_ref-Cacetta2001_10-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-Cacetta2001-10">[10]</a></sup> or viewed another way, to identify what properties of instances in practice might make them more amenable than their worst-case NP-complete behaviour suggests.<sup id="cite_ref-poirriez_et_all_2009_11-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-poirriez_et_all_2009-11">[11]</a></sup> The goal in finding these "hard" instances is for their use in [public key cryptography](https://en.wikipedia.org/wiki/Public_key_cryptography "Public key cryptography") systems, such as the [Merkle-Hellman knapsack cryptosystem](https://en.wikipedia.org/wiki/Merkle-Hellman_knapsack_cryptosystem "Merkle-Hellman knapsack cryptosystem").

Furthermore, notable is the fact that the hardness of the knapsack problem depends on the form of the input. If the weights and profits are given as integers, it is [weakly NP-complete](https://en.wikipedia.org/wiki/Weakly_NP-complete "Weakly NP-complete"), while it is [strongly NP-complete](https://en.wikipedia.org/wiki/Strongly_NP-complete "Strongly NP-complete") if the weights and profits are given as rational numbers.<sup id="cite_ref-Wojtczak18_12-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-Wojtczak18-12">[12]</a></sup> However, in the case of rational weights and profits it still admits a [fully polynomial-time approximation scheme](https://en.wikipedia.org/wiki/Polynomial-time_approximation_scheme "Polynomial-time approximation scheme").

## Solving\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=4 "Edit section: Solving")\]

Several algorithms are available to solve knapsack problems, based on the dynamic programming approach,<sup id="cite_ref-eduk2000_13-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-eduk2000-13">[13]</a></sup> the [branch and bound](https://en.wikipedia.org/wiki/Branch_and_bound "Branch and bound") approach<sup id="cite_ref-martellototh90_14-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-martellototh90-14">[14]</a></sup> or [hybridizations](https://en.wikipedia.org/wiki/Hybrid_algorithm "Hybrid algorithm") of both approaches.<sup id="cite_ref-poirriez_et_all_2009_11-1"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-poirriez_et_all_2009-11">[11]</a></sup><sup id="cite_ref-martellopisingertoth99a_15-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-martellopisingertoth99a-15">[15]</a></sup><sup id="cite_ref-plateau85_16-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-plateau85-16">[16]</a></sup><sup id="cite_ref-martellotothe99_17-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-martellotothe99-17">[17]</a></sup>

### Dynamic programming in-advance algorithm\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=5 "Edit section: Dynamic programming in-advance algorithm")\]

The **unbounded knapsack problem** (**UKP**) places no restriction on the number of copies of each kind of item. Besides, here we assume that ![x_i > 0](https://wikimedia.org/api/rest_v1/media/math/render/svg/d7ca010976c6eb10a9e66a4ec495766452bfe828)

![{\displaystyle m[w']=\max \left(\sum _{i=1}^{n}v_{i}x_{i}\right)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/0fb212a278752e5096827e4169957b9f97deeed7)

subject to ![{\displaystyle \sum _{i=1}^{n}w_{i}x_{i}\leq w'}](https://wikimedia.org/api/rest_v1/media/math/render/svg/71537a75e5abf64e1e6f19be93db861ef4e0222a) and ![x_i > 0](https://wikimedia.org/api/rest_v1/media/math/render/svg/d7ca010976c6eb10a9e66a4ec495766452bfe828)

Observe that ![m[w]](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f989722f439db408fa230d5a4a0e92841c6f088) has the following properties:

1\. ![m[0]=0\,\!](https://wikimedia.org/api/rest_v1/media/math/render/svg/fcb874d3e0ee718ca8002fc877af1b94a1e46c2e) (the sum of zero items, i.e., the summation of the empty set).

2\. ![{\displaystyle m[w]=\max(v_{1}+m[w-w_{1}],v_{2}+m[w-w_{2}],...,v_{n}+m[w-w_{n}])}](https://wikimedia.org/api/rest_v1/media/math/render/svg/762ebe580061e8b68803f8527f719650733a3ea2) , ![{\displaystyle w_{i}\leq w}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9918f9014b516f236f28c3dc9dda17e946033d5b), where ![v_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7dffe5726650f6daac54829972a94f38eb8ec127) is the value of the ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th kind of item.

The second property needs to be explained in detail. During the process of the running of this method, how do we get the weight ![w](https://wikimedia.org/api/rest_v1/media/math/render/svg/88b1e0c8e1be5ebe69d18a8010676fa42d7961e6)? There are only ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) ways and the previous weights are ![{\displaystyle w-w_{1},w-w_{2},...,w-w_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/93973016e85b824be2d07387f55829705a4a86f2) where there are total ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) kinds of different item (by saying different, we mean that the weight and the value are not completely the same). If we know each value of these ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) items and the related maximum value previously, we just compare them to each other and get the maximum value ultimately and we are done.

Here the maximum of the empty set is taken to be zero. Tabulating the results from ![m[0]](https://wikimedia.org/api/rest_v1/media/math/render/svg/420ffeb7cf30d5b19e19175a4ac404b6253bf780) up through ![m[W]](https://wikimedia.org/api/rest_v1/media/math/render/svg/00874bc364aa5a8b086b929be26181924d5a6e20) gives the solution. Since the calculation of each ![m[w]](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f989722f439db408fa230d5a4a0e92841c6f088) involves examining at most ![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b) items, and there are at most ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7) values of ![m[w]](https://wikimedia.org/api/rest_v1/media/math/render/svg/4f989722f439db408fa230d5a4a0e92841c6f088) to calculate, the running time of the dynamic programming solution is [![O(nW)](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e748dd6992ec4d942b90c5d6edb8fdd63b93688)](https://en.wikipedia.org/wiki/Big_O_notation "Big O notation"). Dividing ![w_{1},\,w_{2},\,\ldots ,\,w_{n},\,W](https://wikimedia.org/api/rest_v1/media/math/render/svg/696c0fc9828c2cdee9ad6352fb0e99b87b6c63ff) by their [greatest common divisor](https://en.wikipedia.org/wiki/Greatest_common_divisor "Greatest common divisor") is a way to improve the running time.

Even if [P≠NP](https://en.wikipedia.org/wiki/P_versus_NP_problem "P versus NP problem"), the ![O(nW)](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e748dd6992ec4d942b90c5d6edb8fdd63b93688) complexity does not contradict the fact that the knapsack problem is [NP-complete](https://en.wikipedia.org/wiki/NP-complete "NP-complete"), since ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7), unlike ![n](https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b), is not polynomial in the length of the input to the problem. The length of the ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7) input to the problem is proportional to the number of bits in ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7), ![\log W](https://wikimedia.org/api/rest_v1/media/math/render/svg/c9b1e72cccb9d6e0bb4f2c4402754265e3e1a553), not to ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7) itself. However, since this runtime is [pseudopolynomial](https://en.wikipedia.org/wiki/Pseudopolynomial "Pseudopolynomial"), this makes the (decision version of the) knapsack problem a [weakly NP-complete problem](https://en.wikipedia.org/wiki/Weak_NP-completeness "Weak NP-completeness").

#### 0-1 knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=6 "Edit section: 0-1 knapsack problem")\]

[![A demonstration of the dynamic programming approach.](https://upload.wikimedia.org/wikipedia/commons/thumb/d/dc/Knapsack_problem_dynamic_programming.gif/220px-Knapsack_problem_dynamic_programming.gif)](https://en.wikipedia.org/wiki/File:Knapsack_problem_dynamic_programming.gif)

A demonstration of the dynamic programming approach.

A similar dynamic programming solution for the 0-1 knapsack problem also runs in pseudo-polynomial time. Assume ![w_{1},\,w_{2},\,\ldots ,\,w_{n},\,W](https://wikimedia.org/api/rest_v1/media/math/render/svg/696c0fc9828c2cdee9ad6352fb0e99b87b6c63ff) are strictly positive integers. Define ![m[i,w]](https://wikimedia.org/api/rest_v1/media/math/render/svg/8508d64cc83cae95ff73fa5a6e1a2bf4bfa43ca0) to be the maximum value that can be attained with weight less than or equal to ![w](https://wikimedia.org/api/rest_v1/media/math/render/svg/88b1e0c8e1be5ebe69d18a8010676fa42d7961e6) using items up to ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) (first ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) items).

We can define ![m[i,w]](https://wikimedia.org/api/rest_v1/media/math/render/svg/8508d64cc83cae95ff73fa5a6e1a2bf4bfa43ca0) recursively as follows: **(Definition A)**

The solution can then be found by calculating ![m[n,W]](https://wikimedia.org/api/rest_v1/media/math/render/svg/f7f1046af8319a84c25c874bde6bcaf24bee4489). To do this efficiently, we can use a table to store previous computations.

The following is pseudocode for the dynamic program:

```
// Input:
// Values (stored in array v)
// Weights (stored in array w)
// Number of distinct items (n)
// Knapsack capacity (W)
// NOTE: The array "v" and array "w" are assumed to store all relevant values starting at index 1.

array m[0..n, 0..W];
for j from 0 to W do:
    m[0, j] := 0
for i from 1 to n do:
    m[i, 0] := 0

for i from 1 to n do:
    for j from 0 to W do:
        if w[i] > j then:
            m[i, j] := m[i-1, j]
        else:
            m[i, j] := max(m[i-1, j], m[i-1, j-w[i]] + v[i])

```

This solution will therefore run in ![O(nW)](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e748dd6992ec4d942b90c5d6edb8fdd63b93688) time and ![O(nW)](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e748dd6992ec4d942b90c5d6edb8fdd63b93688) space. (If we only need the value m\[n,W\], we can modify the code so that the amount of memory required is O(W) which stores the recent two lines of the array "m".)

However, if we take it a step or two further, we should know that the method will run in the time between ![O(nW)](https://wikimedia.org/api/rest_v1/media/math/render/svg/6e748dd6992ec4d942b90c5d6edb8fdd63b93688) and ![O(2^{n})](https://wikimedia.org/api/rest_v1/media/math/render/svg/d4b1a4ff0bc4f81ebf79f28260c6fb54ee08ff8d). From **Definition A**, we know that there is no need to compute all the weights when the number of items and the items themselves that we chose are fixed. That is to say, the program above computes more than necessary because the weight changes from 0 to W often. From this perspective, we can program this method so that it runs recursively.

```
// Input:
// Values (stored in array v)
// Weights (stored in array w)
// Number of distinct items (n)
// Knapsack capacity (W)
// NOTE: The array "v" and array "w" are assumed to store all relevant values starting at index 1.

Define value[n, W]

Initialize all value[i, j] = -1

Define m:=(i,j)         // Define function m so that it represents the maximum value we can get under the condition: use first i items, total weight limit is j
{
    if i == 0 or j <= 0 then:
        value[i, j] = 0
        return

    if (value[i-1, j] == -1) then:     // m[i-1, j] has not been calculated, we have to call function m
        m(i-1, j)

    if w[i] > j then:                      // item cannot fit in the bag
        value[i, j] = value[i-1, j]
    else: 
        if (value[i-1, j-w[i]] == -1) then:     // m[i-1,j-w[i]] has not been calculated, we have to call function m
            m(i-1, j-w[i])
        value[i, j] = max(value[i-1,j], value[i-1, j-w[i]] + v[i])
}

Run m(n, W)

```

For example, there are 10 different items and the weight limit is 67. So,

![{\displaystyle {\begin{aligned}&w[1]=23,w[2]=26,w[3]=20,w[4]=18,w[5]=32,w[6]=27,w[7]=29,w[8]=26,w[9]=30,w[10]=27\\&v[1]=505,v[2]=352,v[3]=458,v[4]=220,v[5]=354,v[6]=414,v[7]=498,v[8]=545,v[9]=473,v[10]=543\\\end{aligned}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/52b4c8149a5b2e88869f5c4bbf61c4e287abc79c)

If you use above method to compute for ![{\displaystyle m(10,67)}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6b26bc3ec3190ee9808e126b791c1ead2b310487), you will get this, excluding calls that produce ![{\displaystyle m(i,j)=0}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bc8c37b1fea7c10f5489833873189f9130d798fd):

![{\displaystyle {\begin{aligned}&m(10,67)=1270\\&m(9,67)=1270,m(9,40)=678\\&m(8,67)=1270,m(8,40)=678,m(8,37)=545\\&m(7,67)=1183,m(7,41)=725,m(7,40)=678,m(7,37)=505\\&m(6,67)=1183,m(6,41)=725,m(6,40)=678,m(6,38)=678,m(6,37)=505\\&m(5,67)=1183,m(5,41)=725,m(5,40)=678,m(5,38)=678,m(5,37)=505\\&m(4,67)=1183,m(4,41)=725,m(4,40)=678,m(4,38)=678,m(4,37)=505,m(4,35)=505\\&m(3,67)=963,m(3,49)=963,m(3,41)=505,m(3,40)=505,m(3,38)=505,m(3,37)=505,m(3,35)=505,m(3,23)=505,m(3,22)=458,m(3,20)=458\\&m(2,67)=857,m(2,49)=857,m(2,47)=505,m(2,41)=505,m(2,40)=505,m(2,38)=505,m(2,37)=505,m(2,35)=505,m(2,29)=505,m(2,23)=505\\&m(1,67)=505,m(1,49)=505,m(1,47)=505,m(1,41)=505,m(1,40)=505,m(1,38)=505,m(1,37)=505,m(1,35)=505,m(1,29)=505,m(1,23)=505\\\end{aligned}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/33576ad0db77298eb7d7116472294383b556e745)

Besides, we can break the recursion and convert it into a tree. Then we can cut some leaves and use parallel computing to expedite the running of this method.

To find the actual subset of items, rather than just their total value, we can run this after running the function above:

```
/**
 * Returns the indices of the items of the optimal knapsack.
 * i: We can include items 1 through i in the knapsack
 * j: maximum weight of the knapsack
 */
function knapsack(i: int, j: int): Set<int> {
    if i == 0 then:
        return {}
    if m[i, j] > m[i-1, j] then:
        return {i} ∪ knapsack(i-1, j-w[i])
    else:
        return knapsack(i-1, j)
}

knapsack(n, W)

```

### Meet-in-the-middle\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=7 "Edit section: Meet-in-the-middle")\]

Another algorithm for 0-1 knapsack, discovered in 1974<sup id="cite_ref-18"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-18">[18]</a></sup> and sometimes called "meet-in-the-middle" due to parallels to [a similarly named algorithm in cryptography](https://en.wikipedia.org/wiki/Meet-in-the-middle_attack "Meet-in-the-middle attack"), is exponential in the number of different items but may be preferable to the DP algorithm when ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7) is large compared to _n_. In particular, if the ![w_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fe22f0329d3ecb2e1880d44d191aba0e5475db68) are nonnegative but not integers, we could still use the dynamic programming algorithm by scaling and rounding (i.e. using [fixed-point arithmetic](https://en.wikipedia.org/wiki/Fixed-point_arithmetic "Fixed-point arithmetic")), but if the problem requires ![d](https://wikimedia.org/api/rest_v1/media/math/render/svg/e85ff03cbe0c7341af6b982e47e9f90d235c66ab) fractional digits of precision to arrive at the correct answer, ![W](https://wikimedia.org/api/rest_v1/media/math/render/svg/54a9c4c547f4d6111f81946cad242b18298d70b7) will need to be scaled by ![10^{d}](https://wikimedia.org/api/rest_v1/media/math/render/svg/533c9080b4667d18d8685fbf8a3e6ec4b5b5e79e), and the DP algorithm will require ![{\displaystyle O(W10^{d})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6d1c50fe4d63b706b21c606c67ae541f7bf58e0c) space and ![{\displaystyle O(nW10^{d})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fe772579a33a6fd4a464d34e5ef63dc9138a54aa) time.

```
algorithm Meet-in-the-middle is
    input: A set of items with weights and values.
    output: The greatest combined value of a subset.

    partition the set {1...n} into two sets A and B of approximately equal size
    compute the weights and values of all subsets of each set

    for each subset of A do
        find the subset of B of greatest value such that the combined weight is less than W

    keep track of the greatest combined value seen so far

```

The algorithm takes ![O(2^{n/2})](https://wikimedia.org/api/rest_v1/media/math/render/svg/9fbb200a043616b13f8622ec89c8901f838f692f) space, and efficient implementations of step 3 (for instance, sorting the subsets of B by weight, discarding subsets of B which weigh more than other subsets of B of greater or equal value, and using binary search to find the best match) result in a runtime of ![{\displaystyle O(n2^{n/2})}](https://wikimedia.org/api/rest_v1/media/math/render/svg/805791a36032ea1befe32e2a9218d9664aa336f4). As with the [meet in the middle attack](https://en.wikipedia.org/wiki/Meet-in-the-middle_attack "Meet-in-the-middle attack") in cryptography, this improves on the ![O(n2^{n})](https://wikimedia.org/api/rest_v1/media/math/render/svg/9c02f556173ce8363d5e2b411cdb57ee5d9c9646) runtime of a naive brute force approach (examining all subsets of ![\{1...n\}](https://wikimedia.org/api/rest_v1/media/math/render/svg/9b2102a85d491c66b010463dae395439a926791c)), at the cost of using exponential rather than constant space (see also [baby-step giant-step](https://en.wikipedia.org/wiki/Baby-step_giant-step "Baby-step giant-step")).

### Approximation algorithms\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=8 "Edit section: Approximation algorithms")\]

As for most NP-complete problems, it may be enough to find workable solutions even if they are not optimal. Preferably, however, the approximation comes with a guarantee of the difference between the value of the solution found and the value of the optimal solution.

As with many useful but computationally complex algorithms, there has been substantial research on creating and analyzing algorithms that approximate a solution. The knapsack problem, though NP-Hard, is one of a collection of algorithms that can still be approximated to any specified degree. This means that the problem has a polynomial time approximation scheme. To be exact, the knapsack problem has a [fully polynomial time approximation scheme](https://en.wikipedia.org/wiki/Fully_polynomial_time_approximation_scheme "Fully polynomial time approximation scheme") (FPTAS).<sup id="cite_ref-Vazirani,_Vijay_2003_19-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-Vazirani,_Vijay_2003-19">[19]</a></sup>

#### Greedy approximation algorithm\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=9 "Edit section: Greedy approximation algorithm")\]

[George Dantzig](https://en.wikipedia.org/wiki/George_Dantzig "George Dantzig") proposed a [greedy](https://en.wikipedia.org/wiki/Greedy_algorithm "Greedy algorithm") [approximation algorithm](https://en.wikipedia.org/wiki/Approximation_algorithm "Approximation algorithm") to solve the unbounded knapsack problem.<sup id="cite_ref-dantzig1957_20-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-dantzig1957-20">[20]</a></sup> His version sorts the items in decreasing order of value per unit of weight, ![{\displaystyle v_{1}/w_{1}\geq \cdots \geq v_{n}/w_{n}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/50533bfcdf1002d51319ba2bde6f8fe31467b72f). It then proceeds to insert them into the sack, starting with as many copies as possible of the first kind of item until there is no longer space in the sack for more. Provided that there is an unlimited supply of each kind of item, if ![m](https://wikimedia.org/api/rest_v1/media/math/render/svg/0a07d98bb302f3856cbabc47b2b9016692e3f7bc) is the maximum value of items that fit into the sack, then the greedy algorithm is guaranteed to achieve at least a value of ![m/2](https://wikimedia.org/api/rest_v1/media/math/render/svg/2d6af4e80f7ef59ed62ebada0b02f1ef4b2a2016).

For the bounded problem, where the supply of each kind of item is limited, the above algorithm may be far from optimal. Nevertheless, a simple modification allows us to solve this case: Assume for simplicity that all items individually fit in the sack (![{\displaystyle w_{i}\leq W}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7b0ae396f312826064ccd1483844114eaa11a00f) for all ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)). Construct a solution ![S_{1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5bf84e7fd4fb8259a9b37f956afdf83ee2a020f9) by packing items greedily as long as possible, i.e. ![{\displaystyle S_{1}=\left\{1,\ldots ,k\right\}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fdd06388dd62aebe8fca9239d1f2cb9e023b4048) where ![{\displaystyle k=\textstyle \max _{1\leq k'\leq n}\textstyle \sum _{i=1}^{k'}w_{i}\leq W}](https://wikimedia.org/api/rest_v1/media/math/render/svg/f021d8acd04ca4c3e736a3bfb42793e8ec729f8e). Furthermore, construct a second solution ![{\displaystyle S_{2}=\left\{k+1\right\}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/2001ad20b4d59342fba29148a2e9bafdf9705aa4) containing the first item that did not fit. Since ![{\displaystyle S_{1}\cup S_{2}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/49c5d6dd06bbc5498df9695e84a95d1325ce99b7) provides an upper bound for the [LP relaxation](https://en.wikipedia.org/wiki/Linear_programming_relaxation "Linear programming relaxation") of the problem, one of the sets must have value at least ![m/2](https://wikimedia.org/api/rest_v1/media/math/render/svg/2d6af4e80f7ef59ed62ebada0b02f1ef4b2a2016); we thus return whichever of ![S_{1}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5bf84e7fd4fb8259a9b37f956afdf83ee2a020f9) and ![S_{2}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1143e284d5f25cef778ab482edf6617a523ddd9f) has better value to obtain a ![1/2](https://wikimedia.org/api/rest_v1/media/math/render/svg/e308a3a46b7fdce07cc09dcab9e8d8f73e37d935)\-approximation.

It can be shown that the average performance converges to the optimal solution in distribution at the error rate ![{\displaystyle n^{-1/2}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7bebf90fd2f3a51acbf990a52169d362e821d9f2) <sup id="cite_ref-21"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-21">[21]</a></sup>

#### Fully polynomial time approximation scheme\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=10 "Edit section: Fully polynomial time approximation scheme")\]

The [fully polynomial time approximation scheme](https://en.wikipedia.org/wiki/Fully_polynomial_time_approximation_scheme "Fully polynomial time approximation scheme") (FPTAS) for the knapsack problem takes advantage of the fact that the reason the problem has no known polynomial time solutions is because the profits associated with the items are not restricted. If one rounds off some of the least significant digits of the profit values then they will be bounded by a polynomial and 1/ε where ε is a bound on the correctness of the solution. This restriction then means that an algorithm can find a solution in polynomial time that is correct within a factor of (1-ε) of the optimal solution.<sup id="cite_ref-Vazirani,_Vijay_2003_19-1"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-Vazirani,_Vijay_2003-19">[19]</a></sup>

```
algorithm FPTAS is 
    input: ε ∈ (0,1]
           a list A of n items, specified by their values, , and weights
    output: S' the FPTAS solution

    P := max   // the highest item value
    K := ε 

    for i from 1 to n do
         := 
    end for

    return the solution, S', using the  values in the dynamic program outlined above

```

**Theorem:** The set ![S'](https://wikimedia.org/api/rest_v1/media/math/render/svg/bf9961844d1f539adee019e432dc18aa2a7ede59) computed by the algorithm above satisfies ![\mathrm {profit} (S')\geq (1-\varepsilon )\cdot \mathrm {profit} (S^{*})](https://wikimedia.org/api/rest_v1/media/math/render/svg/511f3c7c30133672cfb9f62c9e8983a0da87eeaa), where ![S^{*}](https://wikimedia.org/api/rest_v1/media/math/render/svg/b059338b0f6bc81c4edf97c51ce987e98f4bf23e) is an optimal solution.

### Dominance relations\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=11 "Edit section: Dominance relations")\]

Solving the unbounded knapsack problem can be made easier by throwing away items which will never be needed. For a given item ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20), suppose we could find a set of items ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053) such that their total weight is less than the weight of ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20), and their total value is greater than the value of ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20). Then ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) cannot appear in the optimal solution, because we could always improve any potential solution containing ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) by replacing ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) with the set ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053). Therefore, we can disregard the ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th item altogether. In such cases, ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053) is said to **dominate** ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20). (Note that this does not apply to bounded knapsack problems, since we may have already used up the items in ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053).)

Finding dominance relations allows us to significantly reduce the size of the search space. There are several different types of [dominance relations](https://en.wikipedia.org/w/index.php?title=Dominance_relations&action=edit&redlink=1 "Dominance relations (page does not exist)"),<sup id="cite_ref-poirriez_et_all_2009_11-2"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-poirriez_et_all_2009-11">[11]</a></sup> which all satisfy an inequality of the form:

![\qquad \sum _{j\in J}w_{j}\,x_{j}\ \leq \alpha \,w_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/c2cd7c4b942e77bf73b8531d019d71bcf4994568), and ![{\displaystyle \sum _{j\in J}v_{j}\,x_{j}\ \geq \alpha \,v_{i}\,}](https://wikimedia.org/api/rest_v1/media/math/render/svg/04c541084d185add19ce96fb8946d0d080ca6a4f) for some ![x\in Z_{+}^{n}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5efb76db4a440369dbeb077e65930e44a4c88dcb)

where ![{\displaystyle \alpha \in Z_{+}\,,J\subsetneq N}](https://wikimedia.org/api/rest_v1/media/math/render/svg/46549937c6c98b93ff346a2f73f8efcb09cd7f93) and ![i\not \in J](https://wikimedia.org/api/rest_v1/media/math/render/svg/ec8794a92e81fb103ef1e5cf2163986c5d69c938). The vector ![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4) denotes the number of copies of each member of ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053).

Collective dominance

The ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th item is **collectively dominated** by ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053), written as ![i\ll J](https://wikimedia.org/api/rest_v1/media/math/render/svg/f58df4fa0608a4fa93a399c9050c0ad5c0870c0b), if the total weight of some combination of items in ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053) is less than _w<sub>i</sub>_ and their total value is greater than _v<sub>i</sub>_. Formally, ![{\displaystyle \sum _{j\in J}w_{j}\,x_{j}\ \leq w_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6d256af517158bde38a4603e0123203d416a1c80) and ![{\displaystyle \sum _{j\in J}v_{j}\,x_{j}\ \geq v_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/6a8ecef93d36a8e72125449842cd3fc7f4432941) for some ![x\in Z_{+}^{n}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5efb76db4a440369dbeb077e65930e44a4c88dcb), i.e. ![\alpha =1](https://wikimedia.org/api/rest_v1/media/math/render/svg/03d67a45a44be8b8f15e99b7def2b0cf0aba1717). Verifying this dominance is computationally hard, so it can only be used with a dynamic programming approach. In fact, this is equivalent to solving a smaller knapsack decision problem where ![{\displaystyle V=v_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/78c151c780148ad29c273b2f49bd4094eb145f37), ![{\displaystyle W=w_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/7db5b660c5bb6f101dccd69721366ac801f4bfa3), and the items are restricted to ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053).

Threshold dominance

The ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th item is **threshold dominated** by ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053), written as ![i\prec \prec J](https://wikimedia.org/api/rest_v1/media/math/render/svg/c5331d5c5e05b74fb489ec4e3cbc2bba4e3d0201), if some number of copies of ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) are dominated by ![J](https://wikimedia.org/api/rest_v1/media/math/render/svg/359e4f407b49910e02c27c2f52e87a36cd74c053). Formally, ![{\displaystyle \sum _{j\in J}w_{j}\,x_{j}\ \leq \alpha \,w_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/e852c27c88d48e5bb94a2a232eaf8ec547c4b7bd), and ![{\displaystyle \sum _{j\in J}v_{j}\,x_{j}\ \geq \alpha \,v_{i}\,}](https://wikimedia.org/api/rest_v1/media/math/render/svg/04c541084d185add19ce96fb8946d0d080ca6a4f) for some ![x\in Z_{+}^{n}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5efb76db4a440369dbeb077e65930e44a4c88dcb) and ![\alpha \geq 1](https://wikimedia.org/api/rest_v1/media/math/render/svg/810dcd1d13dc817a9a6e5dc25ed3ceca99b457d4). This is a generalization of collective dominance, first introduced in<sup id="cite_ref-eduk2000_13-1"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-eduk2000-13">[13]</a></sup> and used in the EDUK algorithm. The smallest such ![\alpha ](https://wikimedia.org/api/rest_v1/media/math/render/svg/b79333175c8b3f0840bfb4ec41b8072c83ea88d3) defines the **threshold** of the item ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20), written ![t_{i}=(\alpha -1)w_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/82db9c6c69bd5f668a97f29dc1fc92e69e0ca0f1). In this case, the optimal solution could contain at most ![\alpha -1](https://wikimedia.org/api/rest_v1/media/math/render/svg/6be20746eff1825a942441a53ffb32ef178193a8) copies of ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20).

Multiple dominance

The ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th item is **multiply dominated** by a single item ![j](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f461e54f5c093e92a55547b9764291390f0b5d0), written as ![i\ll _{m}j](https://wikimedia.org/api/rest_v1/media/math/render/svg/e529970bf733a90f75a6bbf53bbaa740e4bd11e0), if ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) is dominated by some number of copies of ![j](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f461e54f5c093e92a55547b9764291390f0b5d0). Formally, ![{\displaystyle w_{j}\,x_{j}\ \leq w_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/bf0ae48f0a8011aab68a84abdd2f6244eb2abec1), and ![{\displaystyle v_{j}\,x_{j}\ \geq v_{i}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/22af7b42e0b618dd6c02ac1f8a22b60d8da79823) for some ![x_{j}\in Z_{+}](https://wikimedia.org/api/rest_v1/media/math/render/svg/d7ccf203d7dedfe6957a2c841b9e1ec16378d820) i.e. ![J=\{j\},\alpha =1,x_{j}=\lfloor {\frac {w_{i}}{w_{j}}}\rfloor ](https://wikimedia.org/api/rest_v1/media/math/render/svg/3db7a544cd0b0f4491cf6759ec85da3a74103129). This dominance could be efficiently used during preprocessing because it can be detected relatively easily.

Modular dominance

Let ![b](https://wikimedia.org/api/rest_v1/media/math/render/svg/f11423fbb2e967f986e36804a8ae4271734917c3) be the _best item_, i.e. ![{\frac {v_{b}}{w_{b}}}\geq {\frac {v_{i}}{w_{i}}}\,](https://wikimedia.org/api/rest_v1/media/math/render/svg/43732a7674af555e1b81f1ce71f65c528cc997ee) for all ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20). This is the item with the greatest density of value. The ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20)\-th item is **modularly dominated** by a single item ![j](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f461e54f5c093e92a55547b9764291390f0b5d0), written as ![i\ll _{\equiv }j](https://wikimedia.org/api/rest_v1/media/math/render/svg/bedf9947972f9349badf386c6e89ccf9e9188d15), if ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) is dominated by ![j](https://wikimedia.org/api/rest_v1/media/math/render/svg/2f461e54f5c093e92a55547b9764291390f0b5d0) plus several copies of ![b](https://wikimedia.org/api/rest_v1/media/math/render/svg/f11423fbb2e967f986e36804a8ae4271734917c3). Formally, ![w_{j}+tw_{b}\leq w_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/13e54206d9ed4cf44410e9092015b2ad45a4a039), and ![v_{j}+tv_{b}\geq v_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/fe303b8bea29a607d6b79896471057be2002c33d) i.e. ![J=\{b,j\},\alpha =1,x_{b}=t,x_{j}=1](https://wikimedia.org/api/rest_v1/media/math/render/svg/bc5f83839a1dde46cc1865b7089af3401aa548bc).

## Variations\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=12 "Edit section: Variations")\]

There are many variations of the knapsack problem that have arisen from the vast number of applications of the basic problem. The main variations occur by changing the number of some problem parameter such as the number of items, number of objectives, or even the number of knapsacks.

### Multi-objective knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=13 "Edit section: Multi-objective knapsack problem")\]

This variation changes the goal of the individual filling the knapsack. Instead of one objective, such as maximizing the monetary profit, the objective could have several dimensions. For example, there could be environmental or social concerns as well as economic goals. Problems frequently addressed include portfolio and transportation logistics optimizations.<sup id="cite_ref-22"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-22">[22]</a></sup><sup id="cite_ref-23"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-23">[23]</a></sup>

As an example, suppose you ran a cruise ship. You have to decide how many famous comedians to hire. This boat can handle no more than one ton of passengers and the entertainers must weigh less than 1000 lbs. Each comedian has a weight, brings in business based on their popularity and asks for a specific salary. In this example, you have multiple objectives. You want, of course, to maximize the popularity of your entertainers while minimizing their salaries. Also, you want to have as many entertainers as possible.

### Multi-dimensional knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=14 "Edit section: Multi-dimensional knapsack problem")\]

In this variation, the weight of knapsack item ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20) is given by a D-dimensional vector ![{\overline {w_{i}}}=(w_{i1},\ldots ,w_{iD})](https://wikimedia.org/api/rest_v1/media/math/render/svg/96d4a31f2c8766e806655a75ee18ca3f11bb847a) and the knapsack has a D-dimensional capacity vector ![(W_{1},\ldots ,W_{D})](https://wikimedia.org/api/rest_v1/media/math/render/svg/47a732a0c02c8c2add7849fc51d6d139474eadc7). The target is to maximize the sum of the values of the items in the knapsack so that the sum of weights in each dimension ![d](https://wikimedia.org/api/rest_v1/media/math/render/svg/e85ff03cbe0c7341af6b982e47e9f90d235c66ab) does not exceed ![W_{d}](https://wikimedia.org/api/rest_v1/media/math/render/svg/4276175ed1435c1c81fe805bf2e499f64ec773cd).

Multi-dimensional knapsack is computationally harder than knapsack; even for ![D=2](https://wikimedia.org/api/rest_v1/media/math/render/svg/1ac67967f48b21969a5fc88b231b13e11394278d), the problem does not have [EPTAS](https://en.wikipedia.org/wiki/EPTAS "EPTAS") unless P![=](https://wikimedia.org/api/rest_v1/media/math/render/svg/505a4ceef454c69dffd23792c84b90f488543743)NP.<sup id="cite_ref-24"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-24">[24]</a></sup> However, the algorithm in<sup id="cite_ref-CohenGrebla_25-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-CohenGrebla-25">[25]</a></sup> is shown to solve sparse instances efficiently. An instance of multi-dimensional knapsack is sparse if there is a set ![J=\{1,2,\ldots ,m\}](https://wikimedia.org/api/rest_v1/media/math/render/svg/3b3f3511f57eabb746dd4e385d52c2d04cfc0890) for ![m<D](https://wikimedia.org/api/rest_v1/media/math/render/svg/cb194618098a9bea1cb7c67291572630a714f982) such that for every knapsack item ![i](https://wikimedia.org/api/rest_v1/media/math/render/svg/add78d8608ad86e54951b8c8bd6c8d8416533d20), ![\exists z>m](https://wikimedia.org/api/rest_v1/media/math/render/svg/ca38ed867e171d564a0cb209375f85274015a534) such that ![\forall j\in J\cup \{z\},\ w_{ij}\geq 0](https://wikimedia.org/api/rest_v1/media/math/render/svg/119653a68738e1f10cf864046d8116317ef2e1d1) and ![\forall y\notin J\cup \{z\},w_{iy}=0](https://wikimedia.org/api/rest_v1/media/math/render/svg/275c260f57c4a82893051498aae9cc0ba3e6ae91). Such instances occur, for example, when scheduling packets in a wireless network with relay nodes.<sup id="cite_ref-CohenGrebla_25-1"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-CohenGrebla-25">[25]</a></sup> The algorithm from<sup id="cite_ref-CohenGrebla_25-2"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-CohenGrebla-25">[25]</a></sup> also solves sparse instances of the multiple choice variant, multiple-choice multi-dimensional knapsack.

The IHS (Increasing Height Shelf) algorithm is optimal for 2D knapsack (packing squares into a two-dimensional unit size square): when there are at most five square in an optimal packing.<sup id="cite_ref-26"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-26">[26]</a></sup>

### Multiple knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=15 "Edit section: Multiple knapsack problem")\]

This variation is similar to the [Bin Packing Problem](https://en.wikipedia.org/wiki/Bin_packing_problem "Bin packing problem"). It differs from the Bin Packing Problem in that a subset of items can be selected, whereas, in the Bin Packing Problem, all items have to be packed to certain bins. The concept is that there are multiple knapsacks. This may seem like a trivial change, but it is not equivalent to adding to the capacity of the initial knapsack. This variation is used in many loading and scheduling problems in Operations Research and has a [Polynomial-time approximation scheme](https://en.wikipedia.org/wiki/Polynomial-time_approximation_scheme "Polynomial-time approximation scheme").<sup id="cite_ref-27"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-27">[27]</a></sup>

### Quadratic knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=16 "Edit section: Quadratic knapsack problem")\]

The quadratic knapsack problem maximizes a quadratic objective function subject to binary and linear capacity constraints.<sup id="cite_ref-QKP_28-0"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-QKP-28">[28]</a></sup> The problem was introduced by Gallo, Hammer, and Simeone in 1980,<sup id="cite_ref-29"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-29">[29]</a></sup> however the first treatment of the problem dates back to Witzgall in 1975.<sup id="cite_ref-30"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-30">[30]</a></sup>

### Subset-sum problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=17 "Edit section: Subset-sum problem")\]

The [subset sum problem](https://en.wikipedia.org/wiki/Subset_sum_problem "Subset sum problem") is a special case of the decision and 0-1 problems where each kind of item, the weight equals the value: ![w_{i}=v_{i}](https://wikimedia.org/api/rest_v1/media/math/render/svg/5d3d122e68e9e655b00d819401bfde5983100797). In the field of [cryptography](https://en.wikipedia.org/wiki/Cryptography "Cryptography"), the term _knapsack problem_ is often used to refer specifically to the subset sum problem and is commonly known as one of [Karp's 21 NP-complete problems](https://en.wikipedia.org/wiki/Karp%27s_21_NP-complete_problems "Karp's 21 NP-complete problems").<sup id="cite_ref-31"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-31">[31]</a></sup>

The generalization of subset sum problem is called multiple subset-sum problem, in which multiple bins exist with the same capacity. It has been shown that the generalization does not have an [FPTAS](https://en.wikipedia.org/wiki/FPTAS "FPTAS").<sup id="cite_ref-32"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-32">[32]</a></sup>

### Geometric knapsack problem\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=18 "Edit section: Geometric knapsack problem")\]

In the **geometric knapsack problem**, there is a set of rectangles with different values, and a rectangular knapsack. The goal is to pack the largest possible value into the knapsack.<sup id="cite_ref-33"><a href="https://en.wikipedia.org/wiki/Knapsack_problem#cite_note-33">[33]</a></sup>

## See also\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=19 "Edit section: See also")\]

-   [Bin packing problem](https://en.wikipedia.org/wiki/Bin_packing_problem "Bin packing problem")
-   [Change-making problem](https://en.wikipedia.org/wiki/Change-making_problem "Change-making problem")
-   [Combinatorial auction](https://en.wikipedia.org/wiki/Combinatorial_auction "Combinatorial auction")
-   [Combinatorial optimization](https://en.wikipedia.org/wiki/Combinatorial_optimization "Combinatorial optimization")
-   [Continuous knapsack problem](https://en.wikipedia.org/wiki/Continuous_knapsack_problem "Continuous knapsack problem")
-   [Cutting stock problem](https://en.wikipedia.org/wiki/Cutting_stock_problem "Cutting stock problem")
-   [List of knapsack problems](https://en.wikipedia.org/wiki/List_of_knapsack_problems "List of knapsack problems")
-   [Packing problem](https://en.wikipedia.org/wiki/Packing_problem "Packing problem")

## Notes\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=20 "Edit section: Notes")\]

1.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-1 "Jump up")** Mathews, G. B. (25 June 1897). ["On the partition of numbers"](http://plms.oxfordjournals.org/content/s1-28/1/486.full.pdf) (PDF). _Proceedings of the London Mathematical Society_. **28**: 486–490. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1112/plms/s1-28.1.486](https://doi.org/10.1112%2Fplms%2Fs1-28.1.486).
2.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-2 "Jump up")** Dantzig, Tobias (2007). _Number : the language of science_ (The Masterpiece Science ed.). New York: Plume Book. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [9780452288119](https://en.wikipedia.org/wiki/Special:BookSources/9780452288119 "Special:BookSources/9780452288119").
3.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-3 "Jump up")** Kellerer, Hans; Pferschy, Ulrich; Pisinger, David (2004). [_Knapsack problems_](https://books.google.com/books?id=u5DB7gck08YC). Berlin: Springer. p. 449. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-540-40286-2](https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-40286-2 "Special:BookSources/978-3-540-40286-2"). Retrieved 5 May 2022.
4.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-4 "Jump up")** Kellerer, Hans; Pferschy, Ulrich; Pisinger, David (2004). [_Knapsack problems_](https://books.google.com/books?id=u5DB7gck08YC). Berlin: Springer. p. 461. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-540-40286-2](https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-40286-2 "Special:BookSources/978-3-540-40286-2"). Retrieved 5 May 2022.
5.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-5 "Jump up")** Kellerer, Hans; Pferschy, Ulrich; Pisinger, David (2004). [_Knapsack problems_](https://books.google.com/books?id=u5DB7gck08YC). Berlin: Springer. p. 465. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-540-40286-2](https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-40286-2 "Special:BookSources/978-3-540-40286-2"). Retrieved 5 May 2022.
6.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-6 "Jump up")** Kellerer, Hans; Pferschy, Ulrich; Pisinger, David (2004). [_Knapsack problems_](https://books.google.com/books?id=u5DB7gck08YC). Berlin: Springer. p. 472. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-540-40286-2](https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-40286-2 "Special:BookSources/978-3-540-40286-2"). Retrieved 5 May 2022.
7.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-7 "Jump up")** Feuerman, Martin; Weiss, Harvey (April 1973). "A Mathematical Programming Model for Test Construction and Scoring". _Management Science_. **19** (8): 961–966. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1287/mnsc.19.8.961](https://doi.org/10.1287%2Fmnsc.19.8.961). [JSTOR](https://en.wikipedia.org/wiki/JSTOR_(identifier) "JSTOR (identifier)") [2629127](https://www.jstor.org/stable/2629127).
8.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-8 "Jump up")** Skiena, S. S. (September 1999). "Who is Interested in Algorithms and Why? Lessons from the Stony Brook Algorithm Repository". _ACM SIGACT News_. **30** (3): 65–74. [CiteSeerX](https://en.wikipedia.org/wiki/CiteSeerX_(identifier) "CiteSeerX (identifier)") [10.1.1.41.8357](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.8357). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1145/333623.333627](https://doi.org/10.1145%2F333623.333627). [ISSN](https://en.wikipedia.org/wiki/ISSN_(identifier) "ISSN (identifier)") [0163-5700](https://www.worldcat.org/issn/0163-5700). [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [15619060](https://api.semanticscholar.org/CorpusID:15619060).
9.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-pisinger200308_9-0 "Jump up")** Pisinger, D. 2003. [Where are the hard knapsack problems?](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.87.7431&rep=rep1&type=pdf) Technical Report 2003/08, Department of Computer Science, University of Copenhagen, Copenhagen, Denmark.
10.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-Cacetta2001_10-0 "Jump up")** Caccetta, L.; Kulanoot, A. (2001). "Computational Aspects of Hard Knapsack Problems". _Nonlinear Analysis_. **47** (8): 5547–5558. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1016/s0362-546x(01)00658-7](https://doi.org/10.1016%2Fs0362-546x%2801%2900658-7).
11.  ^ [Jump up to: <sup><i><b>a</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-poirriez_et_all_2009_11-0) [<sup><i><b>b</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-poirriez_et_all_2009_11-1) [<sup><i><b>c</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-poirriez_et_all_2009_11-2) Poirriez, Vincent; Yanev, Nicola; Andonov, Rumen (2009). ["A hybrid algorithm for the unbounded knapsack problem"](https://doi.org/10.1016%2Fj.disopt.2008.09.004). _Discrete Optimization_. **6** (1): 110–124. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1016/j.disopt.2008.09.004](https://doi.org/10.1016%2Fj.disopt.2008.09.004). [ISSN](https://en.wikipedia.org/wiki/ISSN_(identifier) "ISSN (identifier)") [1572-5286](https://www.worldcat.org/issn/1572-5286). [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [8820628](https://api.semanticscholar.org/CorpusID:8820628).
12.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-Wojtczak18_12-0 "Jump up")** Wojtczak, Dominik (2018). "On Strong NP-Completeness of Rational Problems". _International Computer Science Symposium in Russia_. Lecture Notes in Computer Science. **10846**: 308–320. [arXiv](https://en.wikipedia.org/wiki/ArXiv_(identifier) "ArXiv (identifier)"):[1802.09465](https://arxiv.org/abs/1802.09465). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1007/978-3-319-90530-3\_26](https://doi.org/10.1007%2F978-3-319-90530-3_26). [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-319-90529-7](https://en.wikipedia.org/wiki/Special:BookSources/978-3-319-90529-7 "Special:BookSources/978-3-319-90529-7"). [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [3637366](https://api.semanticscholar.org/CorpusID:3637366).
13.  ^ [Jump up to: <sup><i><b>a</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-eduk2000_13-0) [<sup><i><b>b</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-eduk2000_13-1) Andonov, Rumen; Poirriez, Vincent; Rajopadhye, Sanjay (2000). "Unbounded Knapsack Problem : dynamic programming revisited". _European Journal of Operational Research_. **123** (2): 168–181. [CiteSeerX](https://en.wikipedia.org/wiki/CiteSeerX_(identifier) "CiteSeerX (identifier)") [10.1.1.41.2135](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.41.2135). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1016/S0377-2217(99)00265-9](https://doi.org/10.1016%2FS0377-2217%2899%2900265-9).
14.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-martellototh90_14-0 "Jump up")** S. Martello, P. Toth, Knapsack Problems: Algorithms and Computer Implementations, John Wiley and Sons, 1990
15.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-martellopisingertoth99a_15-0 "Jump up")** S. Martello, D. Pisinger, P. Toth, [Dynamic programming and strong bounds for the 0-1 knapsack problem](https://www.jstor.org/stable/pdf/2634886.pdf?casa_token=wd6Ykeu46i0AAAAA:egeiDBT-4ijIpmM5GUKA8Ywc67SyXsHHCpFR3QlIL5GGDvx546hVIjk9vbeJsgfh_1NyDcH8EMh9TkBoqRHvMHA8xtT4uUzG5x2mZJ4FzCE_wC0mEhp0Tg), _Manag. Sci._, 45:414–424, 1999.
16.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-plateau85_16-0 "Jump up")** Plateau, G.; Elkihel, M. (1985). "A hybrid algorithm for the 0-1 knapsack problem". _Methods of Oper. Res_. **49**: 277–293.
17.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-martellotothe99_17-0 "Jump up")** Martello, S.; Toth, P. (1984). "A mixture of dynamic programming and branch-and-bound for the subset-sum problem". _Manag. Sci_. **30** (6): 765–771. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1287/mnsc.30.6.765](https://doi.org/10.1287%2Fmnsc.30.6.765).
18.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-18 "Jump up")** Horowitz, Ellis; [Sahni, Sartaj](https://en.wikipedia.org/wiki/Sartaj_Sahni "Sartaj Sahni") (1974), "Computing partitions with applications to the knapsack problem", _Journal of the Association for Computing Machinery_, **21** (2): 277–292, [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1145/321812.321823](https://doi.org/10.1145%2F321812.321823), [hdl](https://en.wikipedia.org/wiki/Hdl_(identifier) "Hdl (identifier)"):[1813/5989](https://hdl.handle.net/1813%2F5989), [MR](https://en.wikipedia.org/wiki/MR_(identifier) "MR (identifier)") [0354006](https://mathscinet.ams.org/mathscinet-getitem?mr=0354006), [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [16866858](https://api.semanticscholar.org/CorpusID:16866858)
19.  ^ [Jump up to: <sup><i><b>a</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-Vazirani,_Vijay_2003_19-0) [<sup><i><b>b</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-Vazirani,_Vijay_2003_19-1) Vazirani, Vijay. Approximation Algorithms. Springer-Verlag Berlin Heidelberg, 2003.
20.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-dantzig1957_20-0 "Jump up")** [Dantzig, George B.](https://en.wikipedia.org/wiki/George_Dantzig "George Dantzig") (1957). "Discrete-Variable Extremum Problems". _Operations Research_. **5** (2): 266–288. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1287/opre.5.2.266](https://doi.org/10.1287%2Fopre.5.2.266).
21.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-21 "Jump up")** Calvin, James M.; Leung, Joseph Y. -T. (1 May 2003). "Average-case analysis of a greedy algorithm for the 0/1 knapsack problem". _Operations Research Letters_. **31** (3): 202–210. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1016/S0167-6377(02)00222-5](https://doi.org/10.1016%2FS0167-6377%2802%2900222-5).
22.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-22 "Jump up")** Chang, T. J., et al. [Heuristics for Cardinality Constrained Portfolio Optimization](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.109.6698&rep=rep1&type=pdf). Technical Report, London SW7 2AZ, England: The Management School, Imperial College, May 1998
23.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-23 "Jump up")** Chang, C. S., et al. "[Genetic Algorithm Based Bicriterion Optimization for Traction Substations in DC Railway System](https://scholarbank.nus.edu.sg/handle/10635/72660)." In Fogel \[102\], 11-16.
24.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-24 "Jump up")** Kulik, A.; Shachnai, H. (2010). ["There is no EPTAS for two dimensional knapsack"](https://www.cs.technion.ac.il/~hadas/PUB/multi_knap.pdf) (PDF). _Inf. Process. Lett_. **110** (16): 707–712. [CiteSeerX](https://en.wikipedia.org/wiki/CiteSeerX_(identifier) "CiteSeerX (identifier)") [10.1.1.161.5838](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.161.5838). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1016/j.ipl.2010.05.031](https://doi.org/10.1016%2Fj.ipl.2010.05.031).
25.  ^ [Jump up to: <sup><i><b>a</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-CohenGrebla_25-0) [<sup><i><b>b</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-CohenGrebla_25-1) [<sup><i><b>c</b></i></sup>](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-CohenGrebla_25-2) Cohen, R. and Grebla, G. 2014. ["Multi-Dimensional OFDMA Scheduling in a Wireless Network with Relay Nodes"](http://wimnet.ee.columbia.edu/wp-content/uploads/2013/03/paper_short.pdf). in _Proc. IEEE INFOCOM’14_, 2427–2435.
26.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-26 "Jump up")** Yan Lan, György Dósa, Xin Han, Chenyang Zhou, Attila Benkő [\[1\]](https://scholar.google.com/citations?user=txyI5aAAAAAJ): _2D knapsack: Packing squares_, Theoretical Computer Science Vol. 508, pp. 35–40.
27.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-27 "Jump up")** Chandra Chekuri and Sanjeev Khanna (2005). "A PTAS for the multiple knapsack problem". _SIAM Journal on Computing_. **35** (3): 713–728. [CiteSeerX](https://en.wikipedia.org/wiki/CiteSeerX_(identifier) "CiteSeerX (identifier)") [10.1.1.226.3387](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.226.3387). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1137/s0097539700382820](https://doi.org/10.1137%2Fs0097539700382820).
28.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-QKP_28-0 "Jump up")** Wu, Z. Y.; Yang, Y. J.; Bai, F. S.; Mammadov, M. (2011). "Global Optimality Conditions and Optimization Methods for Quadratic Knapsack Problems". _J Optim Theory Appl_. **151** (2): 241–259. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1007/s10957-011-9885-4](https://doi.org/10.1007%2Fs10957-011-9885-4). [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [31208118](https://api.semanticscholar.org/CorpusID:31208118).
29.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-29 "Jump up")** Gallo, G.; Hammer, P. L.; Simeone, B. (1980). _Quadratic knapsack problems_. _Mathematical Programming Studies_. Vol. 12. pp. 132–149. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1007/BFb0120892](https://doi.org/10.1007%2FBFb0120892). [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-642-00801-6](https://en.wikipedia.org/wiki/Special:BookSources/978-3-642-00801-6 "Special:BookSources/978-3-642-00801-6").
30.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-30 "Jump up")** Witzgall, C. (1975). "Mathematical methods of site selection for Electronic Message Systems (EMS)". _NASA Sti/Recon Technical Report N_. NBS Internal report. **76**: 18321. [Bibcode](https://en.wikipedia.org/wiki/Bibcode_(identifier) "Bibcode (identifier)"):[1975STIN...7618321W](https://ui.adsabs.harvard.edu/abs/1975STIN...7618321W).
31.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-31 "Jump up")** Richard M. Karp (1972). "[Reducibility Among Combinatorial Problems](https://web.archive.org/web/20190608032217/https://pdfs.semanticscholar.org/a3c3/7657822859549cd6b12b0d1f76f8ee3680a0.pdf)". In R. E. Miller and J. W. Thatcher (editors). Complexity of Computer Computations. New York: Plenum. pp. 85–103
32.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-32 "Jump up")** Caprara, Alberto; Kellerer, Hans; Pferschy, Ulrich (2000). ["The Multiple Subset Sum Problem"](http://www.or.deis.unibo.it/alberto/mssp_siam.ps). _SIAM J. Optim_. **11** (2): 308–319. [CiteSeerX](https://en.wikipedia.org/wiki/CiteSeerX_(identifier) "CiteSeerX (identifier)") [10.1.1.21.9826](https://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.21.9826). [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1137/S1052623498348481](https://doi.org/10.1137%2FS1052623498348481).
33.  **[^](https://en.wikipedia.org/wiki/Knapsack_problem#cite_ref-33 "Jump up")** Abed, Fidaa; Chalermsook, Parinya; Correa, José; Karrenbauer, Andreas; Pérez-Lantero, Pablo; Soto, José A.; Wiese, Andreas (2015). [_On Guillotine Cutting Sequences_](https://kops.uni-konstanz.de/handle/123456789/33469). pp. 1–19. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.4230/LIPIcs.APPROX-RANDOM.2015.1](https://doi.org/10.4230%2FLIPIcs.APPROX-RANDOM.2015.1). [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-939897-89-7](https://en.wikipedia.org/wiki/Special:BookSources/978-3-939897-89-7 "Special:BookSources/978-3-939897-89-7").

## References\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=21 "Edit section: References")\]

-   [Garey, Michael R.](https://en.wikipedia.org/wiki/Michael_R._Garey "Michael R. Garey"); [David S. Johnson](https://en.wikipedia.org/wiki/David_S._Johnson "David S. Johnson") (1979). [_Computers and Intractability: A Guide to the Theory of NP-Completeness_](https://en.wikipedia.org/wiki/Computers_and_Intractability:_A_Guide_to_the_Theory_of_NP-Completeness "Computers and Intractability: A Guide to the Theory of NP-Completeness"). W.H. Freeman. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-0-7167-1045-5](https://en.wikipedia.org/wiki/Special:BookSources/978-0-7167-1045-5 "Special:BookSources/978-0-7167-1045-5"). A6: MP9, pg.247.
-   Kellerer, Hans; Pferschy, Ulrich; Pisinger, David (2004). _Knapsack Problems_. Springer. [doi](https://en.wikipedia.org/wiki/Doi_(identifier) "Doi (identifier)"):[10.1007/978-3-540-24777-7](https://doi.org/10.1007%2F978-3-540-24777-7). [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-3-540-40286-2](https://en.wikipedia.org/wiki/Special:BookSources/978-3-540-40286-2 "Special:BookSources/978-3-540-40286-2"). [MR](https://en.wikipedia.org/wiki/MR_(identifier) "MR (identifier)") [2161720](https://mathscinet.ams.org/mathscinet-getitem?mr=2161720). [S2CID](https://en.wikipedia.org/wiki/S2CID_(identifier) "S2CID (identifier)") [28836720](https://api.semanticscholar.org/CorpusID:28836720).
-   Martello, Silvano; Toth, Paolo (1990). [_Knapsack problems: Algorithms and computer implementations_](https://archive.org/details/knapsackproblems0000mart). Wiley-Interscience. [ISBN](https://en.wikipedia.org/wiki/ISBN_(identifier) "ISBN (identifier)") [978-0-471-92420-3](https://en.wikipedia.org/wiki/Special:BookSources/978-0-471-92420-3 "Special:BookSources/978-0-471-92420-3"). [MR](https://en.wikipedia.org/wiki/MR_(identifier) "MR (identifier)") [1086874](https://mathscinet.ams.org/mathscinet-getitem?mr=1086874).

## External links\[[edit](https://en.wikipedia.org/w/index.php?title=Knapsack_problem&action=edit&section=22 "Edit section: External links")\]

-   [Free download of the book "Knapsack problems: Algorithms and computer implementations", by](http://www.or.deis.unibo.it/knapsack.html) [Silvano Martello](https://en.wikipedia.org/w/index.php?title=Silvano_Martello&action=edit&redlink=1 "Silvano Martello (page does not exist)") and [Paolo Toth](https://en.wikipedia.org/wiki/Paolo_Toth "Paolo Toth")[Silvano Martello](https://en.wikipedia.org/w/index.php?title=Silvano_Martello&action=edit&redlink=1 "Silvano Martello (page does not exist)")[Silvano Martello](https://en.wikipedia.org/w/index.php?title=Silvano_Martello&action=edit&redlink=1 "Silvano Martello (page does not exist)")[Silvano Martello](https://en.wikipedia.org/w/index.php?title=Silvano_Martello&action=edit&redlink=1 "Silvano Martello (page does not exist)")[Silvano Martello](https://en.wikipedia.org/w/index.php?title=Silvano_Martello&action=edit&redlink=1 "Silvano Martello (page does not exist)")
-   [Lecture slides on the knapsack problem](http://www.cse.unl.edu/~goddard/Courses/CSCE310J/Lectures/Lecture8-DynamicProgramming.pdf)
-   [PYAsUKP: Yet Another solver for the Unbounded Knapsack Problem](https://web.archive.org/web/20111006142943/http://download.gna.org/pyasukp/), with code taking advantage of the dominance relations in an hybrid algorithm, benchmarks and downloadable copies of some papers.
-   [Home page of David Pisinger](http://www.diku.dk/~pisinger/) with downloadable copies of some papers on the publication list (including "Where are the hard knapsack problems?")
-   [Knapsack Problem solutions in many languages](http://rosettacode.org/wiki/Knapsack_Problem) at [Rosetta Code](https://en.wikipedia.org/wiki/Rosetta_Code "Rosetta Code")
-   [Dynamic Programming algorithm to 0/1 Knapsack problem](http://www.personal.kent.edu/~rmuhamma/Algorithms/MyAlgorithms/Dynamic/knapsackdyn.htm)
-   [Knapsack Problem solver (online)](https://web.archive.org/web/20140223114908/http://karaffeltut.com/NEWKaraffeltutCom/Knapsack/knapsack.html)
-   [Solving 0-1-KNAPSACK with Genetic Algorithms in Ruby](http://www.nils-haldenwang.de/computer-science/computational-intelligence/genetic-algorithm-vs-0-1-knapsack) [Archived](https://web.archive.org/web/20110523210824/http://www.nils-haldenwang.de/computer-science/computational-intelligence/genetic-algorithm-vs-0-1-knapsack) 23 May 2011 at the [Wayback Machine](https://en.wikipedia.org/wiki/Wayback_Machine "Wayback Machine")
-   [Codes for Quadratic Knapsack Problem](http://www.adaptivebox.net/CILib/code/qkpcodes_link.html)

-   [Optimizing Three-Dimensional Bin Packing](https://web.archive.org/web/20190303205438/http://pdfs.semanticscholar.org/bb99/86af2f26f7726fcef1bc684eac8239c9b853.pdf)
-   [Knapsack Integer Programming Solution in Python](http://apmonitor.com/me575/index.php/Main/KnapsackOptimization) [Gekko (optimization software)](https://en.wikipedia.org/wiki/Gekko_(optimization_software) "Gekko (optimization software)")
