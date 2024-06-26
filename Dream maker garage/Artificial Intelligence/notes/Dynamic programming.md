
##### Algorithmic paradigms
**Greedy**: Build up a solution incrementally, myopically optimising some local criterion. 
Example: Colouring graph
**Divide and conquer**: Break up the problem into independent, subproblems, solve each subproblem, and combine the solution to subproblems to form a solution to original problem.
Example: Quicksort  
**Dynamic programming**: Break up the problem into a series of overlapping subproblems, and build up solutions to larger and larger subproblems. 
Example: Dijkstra's algorithm




##### Motivation


##### Caching
Also referred as memorisation. 

##### Examples
###### Weighted interval scheduling problem (Binary choice)
![[截圖 2024-06-18 下午8.53.12.png]]
For the problem, the rules are as followed:
- Job $j$ starts at $s_j$, finishes at $f_j$, and has a weight value of $v_j$
- Two jobs are compatible if they don't overlap
- Goal is to find the maximum weight subset of mutually compatible jobs

**Earliest-finish-time algorithm**
Consider jobs in ascending order of finish time. Add the job to subset if it's compatible with previously chosen job. 
This is considered as a **time greedy** selection. It is correct if the weights $v_j$ are all 1, but fails spectacularly for weighted version. ![[截圖 2024-06-19 上午7.50.04.png]]
The notion of compatibility can be described by $p(j)$ = largest index index $i < j$ such that job $i$ is compatible with $j$. Given that the notation of this problem has to be formatted into $f_1 < f_2 < \dots < f_n$ 
From the above example, it can be seen that $p(8) = 5,\space p(7) = 3,\space p(2) = 0$

**Binary choice** 
Let $\text{OPT}(j)$ = value of optimal solution t the problem consisting of job requests $1, 2, 3, \dots, j$
**Case 1**: OPT selects job j
- Collect profits $v_j$
- Do not include the incompatible jobs {$p(j)+1, p(j)+2, \dots, j-1$}
- Must include the optimal solution to problem consisting of remaining compatible jobs {$1, 2, \dots, p(j)$}
**Case 2**: OPT does not select job j
- Must include the optimal solution to problem consisting of remaining compatible jobs {$1,2, \dots, j-1$}
![[截圖 2024-06-19 上午8.53.15.png]]


**Solution 1: Brute force**
```python
Input: n, s[1...n], f[1...n], v[1...n]
Sort jobs by finish time so that f[1] <= f[2] <= ... <= f[n]
Compute p[1], p[2], ... p[n]

# Compute-Opt(j)
if j = 0
	return 0
else
	return max(v[j]+ Compute-Opt(p[j]), Compute-Opt(p[j-1]))
```
Recursive algorithm fails spectacularly because of redundant subproblem. This becomes an exponential problem. 
![[截圖 2024-06-19 上午8.58.59.png]]
A recursion tree describe the recursive calls of the algorithm. Notice the word "redundant" because there is an **overlapping subproblem** (`OPT(p[1]), OPT(p[2]), ... etc`). 


**Solution 2: Memoisation**
```python
Input: n, s[1...n], f[1...n], v[1...n]
Sort jobs by finish time so that f[1] <= f[2] <= ... <= f[n]
Compute p[1], p[2], ... p[n]

# Initialisation
for j = 1 to n 
	M[j] <- empty
M[0] = 0

# M-Compute-Opt(j)
if M[j] is empty
	M[j] <- max(v[j]+ Compute-Opt(p[j]), Compute-Opt(p[j-1]))
return M[j]
```
In a overlapping problem, the attempt is to cache the result computed, and use look-up if solution to subproblem exists

**Finding solution**
The $\text{OPT()}$ algorithm finds the optimal solution. For all the possible solutions, it can be found by the following:
```python
# Find-solution(j)
if j = 0
	return empty_set
else if (v[j] + M(p[j]) > M[j-1])
	return {j} union Find-solution(p[j])
else 
	return Find-solution(j-1)
```
**Bottom up**
```python
M[0] <- 0

# Bottom-up(n, s, f, v)
for j = 1 to n
	M[j] <- max{v_j + M[p(j)], M[j-1]}
```
###### Segmented least square (Multiway choice)
The motivation comes from the foundational statistical problem **least squares**, where given $n$ points in the plane {$(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$}
Find a line $ax+b$ to minimise the Sum of Square Errors that is defined by
$$\text{SSE} = \sum^n_{i=1}(y_i -ax_i-b)^2$$
The solution (minimum error) is found by
$$a = \frac{n\sum_ix_iy_i}{}$$

In segmented least square problems, points lie roughly on a sequence of several line segments. 
Given $n$ points in the plane {$(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$}, with $x_1 < x_2 < \dots < x_n$. The problem is to find a sequence of lines that minimises $f(x)$
$f(x)$ is defined as a reasonable choice between accuracy (goodness of fit) and parsimony(number of lines), expressed mathematically as $E + cL$, 
where $E$ = the sum of SSE in each segment, $L$ = the number of lines, $c$ = positive constant
![[截圖 2024-06-19 上午9.43.26.png]]

Let 
- $\text{OPT}(j)$ be the minimum cost for points $p_1, p_2 \dots, p_j$
- $e(i, \space j)$ be the minimum SSE for points $p_1, p_2 \dots, p_j$
$\text{OPT}(j)$ is computed by 
- last segment uses points  $p_i, p_{i+1} \dots, p_j$ for some $j$
- Cost = $e(i, \space j) + c + \text{OPT(i-1)}$ (optimal substructure property, proved via exchange argument)


###### knapsnack problem
Given n objects and a knapsack, item i weighs $w_i > 0$ and has value $v_i > 0$. 
Knapsack has capacity of W, the goal is to fill knapsack so as to maximise total value. 
![[截圖 2024-06-19 上午9.49.31.png]]
**Greedy by weights**: Repeatedly add item with minimum $w_i$
**Greedy by values**: Repeatedly add item with maximum $v_i$
**Greedy by ratios**: Repeatedly add item with maximum ratio $v_i / w_i$
However, none of the greedy algorithms is optimal. 

**False start**:

**Adding a new variable**:
$$\text{OPT}(i,\space w)$$
###### string edit distance (alignment) (Adding a new variable)

**Case 1**: $\text{OPT}$ matchs $x_i -y_j$
Pay mismatch for $x_i -y_j$ + min cost of aligning {$x_1, x_2, \dots, x_{i-1}$} and {$y_1, y_2, \dots, y_{j-1}$}
**Case 2-1**: $\text{OPT}$ leaves $x_i$ unmatched
Pay gap for $x_i$ + min cost aligning {$x_1, x_2, \dots, x_{i-1}$} and {$y_1, y_2, \dots, y_{j}$}
**Case 2-2**: $\text{OPT}$ leaves $y_j$ unmatched
Pay gap for $y_j$ + min cost aligning {$x_1, x_2, \dots, x_{i}$} and {$y_1, y_2, \dots, y_{j-1}$}

![[截圖 2024-06-19 上午9.57.48.png]]

```python
# Sequence alignment (m, n, x, y, delta, a)
for i = 0 to m
	M[i, 0] <- i * delta
for j = 0 to n
	M[0, j] <- j * delta
for i = 0 to m 
	for j = 0 to n
		M[i, j] = min{
			a[xi, yi] + M[i-1, j-1],
			delta + M[i-1, j],
			delta + M[i, j-1]
		}
return M[m, n]
```