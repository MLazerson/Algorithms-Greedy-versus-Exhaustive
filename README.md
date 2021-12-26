# 335-project-2
Container ship weight-maximization

Group members:

Malka A Lazerson mlazerson@csu.fullerton.edu

**The Hypothesis**

This experiment will test the following hypotheses:
Exhaustive search algorithms are feasible to implement, and produce correct outputs.
Algorithms with exponential running times are extremely slow, probably too slow to be of practical use.

**The Problem**

Both algorithms will be used to solve an interesting problem. Suppose the following. 

We are a trucking company responsible for transporting goods to a seaport, to be loaded in a container ship. Our trucking company has been contracted to transport a list of goods, and we are tasked to use any of our available trucks to transport these goods to be loaded in a container ship. Each item has a weight and a volume. We are restricted by the total volume that can be loaded in the ship, while trying to maximize the total weight. One can transport any number of items and they add up as volume. For example, if we choose two different Soybean boxes of volumes +2 cubic meters and +3 cubic meters, we would be able to transport +5 cubic meters. We are limited by the total volume that can be loaded on the container ship so we cannot transport everything we want. There are two algorithms to help us pick a subset of the available goods, so that we can maximize total weight for the ship while staying within the given volume.

**A description of the problem at hand is as follows:**

Container ship weight-maximization problem

input: A positive “volume limit” budget V (floating point of TEUs); and a vector G of  n “goods” objects, containing one or more goods where each cargo item a=(w, v) has floating point weight w>0 and volume in cubic meters v>=0

output: A vector K of goods drawn from G, such that the sum of volumes of goods from K is within the prescribed volume limit Vand the sum of the goods’ weight is maximized. In other words:
(w,vt)Gv V;and the sum of all goods’ weights (w, v)w is maximized


**The Algorithms**

We must implement the following two algorithms for the Container ship weight-maximization problem. The first algorithm uses the greedy pattern. The greedy heuristic is to always choose the “best” (highest weight-per-volume) item that fits within the volume V, and keep selecting the best items until ran out of space (aka volume):

    greedy_max_time(V, goods):
        todo = goods
        result = empty vector
        result_volume = 0
        while todo is not empty:
            Find the good “a” in todo of maximum weight per its volume
            Remove “a” from todo
            Let v be a’s volume in TEUs
            if (result_volume + v) <= V:
                result.add_back(a)
                result_volume += v
        return result


The time complexity of the greedy algorithm depends on the data structures that are used to implement it. A naive approach using unsorted vectors and sequential search to find “a” takes O(n2) time. This is acceptable but not ideal. Using a heap, binary search tree, or sorting algorithm in a fairly straightforward way can speed this up to O(n n).

The second algorithm uses a proper exhaustive search.

    exhaustive_max_weight(V, goods):
        best = None
        for candidate in subsets(goods):
            if total_weight(candidate) <= V:
                if best is None or
                    total_weight(candidate) > total_weight(best):
                        best = candidate
        return best



As discussed in section 7.5.4 of ADITA, subsets(goods) can be implemented using bitwise operations.


    n = |goods|
    best = None
    for bits from 0 to (2n -1):
        candidate = empty vector
        for j from 0 to n-1:
            if ((bits >> j) & 1) == 1:
                candidate.add_back(goods[j])

    if total_cost(candidate) <= V:
        if best is None or
            total_weight(candidate) > total_weight(best):
                best = candidate
    return best


For this to work, the bits loop counter variable needs to be able to store the quantity 2n-1. A good way of ensuring that is to use the largest integer data type in C++, which is the uint64_t type that is 64 bits wide. This creates a limitation that the exhaustive search algorithm can only handle n<64. This is unlikely to be a practical problem, because the time complexity of this algorithm is O(2nn).

Our theory predicts that the O(2nn) exhaustive search algorithm will be far slower than the greedy algorithm with its O(n2) or O(n n) time complexity. Our experiment will show whether this is the case.
