# Background
"Heuristic search refers to a search strategy that attempts to optimize a problem by iteratively improving the solution based on a given heuristic function or a cost measure." [Source](https://link.springer.com/referenceworkentry/10.1007%2F978-1-4419-9863-7_875) This repo contains the Python code that attempts to solve the 8-Puzzle by using various heuristic search methods.

# Introduction
Here we explore the methods I used to implement the Uniform Cost search, and the A* search with misplaced tile and Euclidean heuristics using Tree search. Furthermore, I
illustrate the space and time complexity of these algorithms via visualizations. This program is written in Python 3.

# Uniform Cost Search
This search is essentially a breadth-first search using a priority queue. This is because each time a state expands, its g(n) cost (the cost of getting to the current node from the initial node) increases by 1 (we did not define an expansion weight). In comparison with the other two algorithms, this is the slowest search method and takes up the largest space.

# A* Misplaced Tile Heuristic
This algorithm uses the combined values of g(n), number of misplaced tiles between the initial and current state, and h(n), the number of misplaced tiles between the current state and the goal. This algorithm performs in the middle of the other two, more efficient than the uniform cost and less so than the A* using the Euclidean heuristic. 

# A* Euclidean Distance Heuristic
This algorithm uses the combined values of g(n) and h(n). These functions represent the euclidean distance between the initial state and the current state, and the euclidean distance between the current state and the goal. This algorithm performed most efficiently when compared to the other two when the input is significantly more difficult to solve.

# Design
I used a tree search which includes a root node, and its children. A main() function call greets and prompts you to select either a default 3x3 puzzle, or enter your own custom puzzle. If you opt for the default puzzle, you are then asked to choose from a pool of premade initial states (trivial, very easy, easy, doable, oh boy, and impossible).

After you select your desired difficulty, the program asks you to select an algorithm. The following algorithms are available (only some are explored here):
<ul>
<li>Simple Uniform Cost Search</li>
<li>A* Misplaced Tile Heuristic</li>
<li>Third item</li>
<li>A* Manhattan Distance Heuristic</li>
</ul>

After you select an algorithm, the program begins to search for a solution. It subsequently displays a traceback of the solution nodes, the number of expansions, max nodes in the queue at any time, and the execution time.

The main class in this project is the Node class. Each Node has a number of useful features including its parent node (if not root), its puzzle state, the cost from start (g(n)), the cost to goal (h(n)), and a list of children. One of the most important methods in this class is the expand() method. Here, the algorithm finds 0 in its board state, and proceeds to generate a list of possible swaps. The states in this list are then used in the tree_search() function where they are pushed to a heap queue depending on whether they were previously seen (either in the heap queue, or the explored set).

The heart of this program lies in the tree_search() function. The algorithm is mostly the same as the pseudocode provided in the project manual. One of the challenges that I faced was that I needed my heap queue to be able to easily sort each node according to its f-value. Remember that the board states are wrapped inside the Node class where only g(n) and h(n) are stored. So, my heap queue had no idea how to compare these objects. You can instruct a heap queue to compare different complex objects by defining __lt__(self, other) in my class where we compare each node’s cost_from_start + cost_to_goal. This way, whenever I push a node to my heap queue, it is automatically sorted by its f-value.

# Performance and Analysis
Here, you may find a comparison of the three main algorithms when tested on the following initial state puzzles. I ran each algorithm on each puzzle state and averaged their performance over 10 iterations each:

![Game States](/images/states.png "Game States")

![Time vs. difficulty chart](/images/time_over_difficulty.png "Time vs. difficulty chart")

The uniform cost search takes the longest to finish. Moreover, the A* misplaced tile heuristic outperforms the euclidean heuristic… for now.

Let’s measure the time it takes for all algorithms to solve the “Oh Boy” puzzle difficulty:

![Oh Boy time vs. difficulty chart](/images/oh_boy_time_vs_diff.png "Oh Boy time vs. difficulty chart")

The A* Euclidean which was previously outperformed by the A* misplaced tile is now significantly more efficient. Uniform cost search takes much longer to complete and hence does not even fit on the same scale as A* euclidean!

Let’s investigate how many nodes each algorithm expands:

![Expanded nodes vs. difficulty](/images/expanded_diff.png "Expanded nodes vs. difficulty")

You may notice that the A* search outperforms the uniform cost search in terms of space requirement as well. Turns out using an admissible heuristic makes everything better.

Since the Oh Boy state is so difficult, it gets its own chart:

![Oh Boy expanded nodes vs. difficulty](/images/oh_boy_exp_vs_diff.png "Oh Boy expanded nodes vs. difficulty")

We see that again, A* search outperforms the other two by a huge margin. Uniform cost search just takes too much space that it would outscale A* euclidean if fully drawn.

Here’s another chart that shows the number of maximum nodes in our queue at any given time:


![Max nodes vs. difficulty](/images/max_nodes_vs_diff.png "Max nodes vs. difficulty")

Again, the uniform cost search requires much more space when it comes to increasingly difficult puzzles. Here’s the Oh Boy difficulty chart for max nodes in queue:


![Oh Boy max nodes vs. difficulty](/images/oh_boy_max_nodes_vs_diff.png "Oh Boy max nodes vs. difficulty")

# Conclusion
Based on my empirical results concerning the three search algorithms of uniform cost, A* misplaced tile, and A* euclidean heuristics, it can be said that:

<ol>
<li>Using a good heuristic for the job saves us a lot of space and time. For example, uniform cost takes a substantial amount of time to complete the same job compared to A*. In our case, for the uniform cost search, since h(n) was 0, and g(n) was always equal to the depth of a node in the tree, it degenerated into a breadth-first-search, one that uses a priority queue. The time and space complexity for a breadth-first-search is O(bd) where b is the branching factor and d is the depth.
</li>
<li>Using the A* search significantly improved the time and complexity compared to the uniform cost. Between the two heuristics that we used to A*, euclidean distance outperformed missing tiles in both time and space when the puzzle was more difficult. The missing tiles heuristic slightly outperforms euclidean distance in terms of time when our puzzle is simple while it occupies the same space.
</li>
</ol>

# Program Output
Welcome to Amir's 8-puzzle solver.
Select an option:

[1] Default 3x3 puzzle
 2  Custom puzzle
(Press enter to default to [1])
 
Default puzzle: enter the difficulty (1 to 7):

[1] demo
 2  trivial
 3  very easy
 4  easy
 5  doable
 6  oh boy
 7  impossible
 5
Selected doable

Display state traceback at the end? (may crash your notebook if puzzle is too difficult) [y]/n
 
Select algorithm:
 1  Uniform Cost Search
 2  A* Misplaced Tile Heuristic
 3  A* Manhattan Distance Heuristic
[4] A* Euclidean Distance Heuristic (fastest)

 
Selected A* Euclidean Distance Heuristic

The best state to expand with g(n) = 0 and h(n) = 0 is...<br />
[0, 1, 2]<br />
[4, 5, 3]<br />
[7, 8, 6]<br />

The best state to expand with g(n) = 1 and h(n) = 3 is...<br />
[1, 0, 2]<br />
[4, 5, 3]<br />
[7, 8, 6]<br />

The best state to expand with g(n) = 2 and h(n) = 2 is...<br />
[1, 2, 0]<br />
[4, 5, 3]<br />
[7, 8, 6]<br />

The best state to expand with g(n) = 3 and h(n) = 1 is...<br />
[1, 2, 3]<br />
[4, 5, 0]<br />
[7, 8, 6]<br />

The best state to expand with g(n) = 4 and h(n) = 0 is...<br />
[1, 2, 3]<br />
[4, 5, 6]<br />
[7, 8, 0]<br />

Found a solution!<br />
Traceback:<br />
[[1, 2, 3], [4, 5, 6], [7, 8, 0]]<br />
[[1, 2, 3], [4, 5, 0], [7, 8, 6]]<br />
[[1, 2, 0], [4, 5, 3], [7, 8, 6]]<br />
[[1, 0, 2], [4, 5, 3], [7, 8, 6]]<br />
[[0, 1, 2], [4, 5, 3], [7, 8, 6]]<br />
5 nodes traced.

To solve this problem the search algorithm expanded a total of 4 nodes.<br />
The maximum number of nodes in the queue at any one time: 4<br />
--- 0.0067656999999599066 milliseconds ---
