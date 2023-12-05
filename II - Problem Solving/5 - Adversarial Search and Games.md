If two or more agents have conflicting goals, an adversarial search problem is applied on the competitive environment.
# Game theory
Playing games in adversarial scenarios requires the definition of an optimal move and an algorithm for finding it. Since it is usually not possible to examine all possibile sequences of moves, we often prune branches of the search tree that can be ignored in order to find the best move.<br>
In complex games we cannot be sure of finding the optimal action to play because of computational limits, thus we can apply an evaluation function to estimate the winner based on the state we're in, or we can average the outcomes of many game simulations.<br>
The games that are usually taken in consideration are
- deterministic,
- two-player,
- turn-taking,
- with perfect information (fully observable),
- zero-sum (or constant-sum) such that what's good for one is just as bad for the other.

In the terms of search algorithms, a player making a move is the same as selecting an action, and a position in the game is a synonym for state.<br>
In the definition of the algorithms the two players are named MAX and MIN (where MAX moves first); these names comes from the fact that, from MAX point of view, MAX wants to maximize its outcome, whilst MIN wants to minimize it.<br>
Any given game (with initial state $S_0$ provided) can be formalized by defining a set of applicable functions.
- `toMove(s)` returns the player whose turn it is in state *s*.
- `actions(s)` returns a set of legal moves.
- `result(s, a)` is the transition model, which defines the state resulting from taking action *a* in state *s*.
- `isTerminal(s)` is a terminal test, such that it's true if game's over.
- `utility(s, p)` defines the final numeric value of player *p* in terminal state *s* (for example, 1 for win and 0 for loss).

In the game search tree, vertices represent positions and edges moves.
The value on each leaf node indicates the utility of that terminal state for MAX.<br>
It is possibile to recognize two types of strategies that can be adopted to play games:
- a Type A strategy considers all moves to a given depth, therefore the tree will be wide but shallow;
- a Type B strategy follows promising branches as far as possibile, therefore the tree will be deep but narrow.
## Minimax search
- The minimax algorithm performs a complete [depth-first](../II%20-%20Problem%20Solving/3%20-%20Solving%20Problems%20by%20Searching.md#depth-first-search) exploration of the game tree
- The optimal decision is determined by the outcome a minimax function (used as evaluation function), in order to choose the optimal path (the one that leads to the state with highest minimax value)
- The simulated game tree always assumes that MAX takes the maximum value while MIN takes the minimum (both players play optimally). If a state is terminal, `minimax(s)` equals `utility(s, MAX)`
- Given $m$ the maximum depth of the tree and $b$ the number of legal moves at each point, time complexity is $O(b^m)$. Space complexity is $O(bm)$, or $O(m)$ if we use backtracking
## Alpha-beta pruning
- Sometimes it is possible to prune (eliminate) large parts of the tree that are useless for taking a decision. In particular, a minimax decision can be independent of the values of some leaves, and therefore they can be pruned
- Instead of `minimax(s)` we use `maxValue(s, alpha, beta)`, where $\alpha$ is the best value for MAX and $\beta$ the best for MIN (thus the highest and the lowest values). Values $\alpha$ and $\beta$ are updated during the search and nodes with worse values are pruned
- Complexity depends on the ordering of the provided moves. Perfect move ordering (better moves are returned first) needs $O(b^{m/2})$, whereas random ordering $O(b^{3m/4})$
- Optimizations can be made if a transposition table is addressed to catch permutations of move sequences that end up in the same position
## Heuristic alpha-beta pruning
- If constructing the entire game tree proves to be unfeasible, we can apply a heuristic evaluation function that treats non-terminal nodes as they were, by evaluating an expected value for the final outcome
- The `utility(s, p)` function is replaced with `eval(s, p)`, which estimates a state's utility; the terminal test `isTerminal(s)` is replaced by a cutoff test `isCutoff(s, d)`, that decides when to stop the search. `minimax(s)` is replaced by `h-minimax(s, d)`, which takes also the search depth *d* into account (*d* can be updated with iterative deepening)
- The evaluation function can be defined by looking for recurrent patterns in the game state that are known to lead to good decisions. The formula $$EVAL(s)=\sum_{i=1}^nw_if_i(s)$$ where each $f_i$ is a feature of the position and $w_i$ is the respective weight, is a linear example of what an evaluation can be. It is possible to use non linear combinations
### Problems and optimizations
1. The evaluation function should be applied only on quiescent positions. In fact, evaluating the game in proximity of killer moves will result in untrustworthy values, since they will widely fluctuate move after move. It is preferred to expand those branches further before evaluating
2. Another problem to face is the horizon effect: the opponent may be able to cause damage with an unavoidable move, but the program prefers the use of delaying tactics that will prove useless in the end. A solution is to allow singular extensions: the branch is expanded further the cutoff point if a move is drastically better than the others
3. Variants of the algorithm may make use of forward pruning:
	- the **beam search** considers only the $n$ best move according to the evaluation function, ignoring other ones;
	- the **probabilistic cut** algorithm uses statistics from prior experience to avoid pruning the best move;
	- a **late move reduction** assumes that move ordering is good, thus it reduces the depth to which we search moves that appears later.
4. There are some occasions, often the opening and ending of games, where some move patterns are broadly recurrent. Therefore it's useless wasting computation power when we can exploit lookup tables that store these remarkable good moves
## Monte Carlo search
- Monte Carlo Search tree (MCST) does not use a heuristic evaluation function. Instead, it estimates the average utility of a state by simulating many full playouts of the game. The value of a node is thus defined as the percentage of wins over the total number of playouts going through that state 
- In a simulation (that uses a playout policy to decide which moves to play), when a terminal state is reached, we apply a back-propagation on the path of the result: winning nodes are updated incrementing both the playouts number and the number of wins; loosing nodes updates only the playouts one.
- A selection policy balances two factors:
	- exploitation of good states from past playouts (moves with highest win percentage);
	- exploration of states that had few playouts (moves with high uncertainty).
- A very effective selection policy is the "upper confidence bounds", that ranks moves based on the formula $$UCB\textit{1}(n)=\frac{U(n)}{N(n)}+C\times\sqrt{\frac{logN({\small PARENT}(n))}{N(n)}}$$ where $U(n)$ is the total utility of the $N(n)$ playouts that went through $n$, and where $C$ is a constant that balances exploitation and exploration
- MCST is advantageous over $\alpha$-$\beta$ pruning when the branching factor is high and when the evaluation function is inaccurate
## Expectiminimax
- In stochastic games, where the available moves for a player are dictated by probability (such as rolling dices), we need to include chance nodes alongside MAX and MIN
- `expectiminimax(s)` is used as evaluation function. It returns values that are linear transformations of the probability of winning. It works the same as before for MAX and MIN nodes
- For chance nodes making decisions implies the calculation of an expected value that averages all possible outcomes. The expectiminimax value for a chance node is the sum of the values over all outcomes weighted over their probabilities: $$\sum_{r}P(r){\small EXPECTIMINIMAX}({\small RESULT}(s, r))$$ where $r$ represent all possible chance events
- Complexity will be $O(b^mn^m)$, with $n$ the number of distinct chance events
- Optimization is obtainable by concentrating on more likely occurrences by applying forward pruning