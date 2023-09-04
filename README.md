# puzzles-search-algorithms
Implementing the A* Algorithm on 8-puzzle, and Evolutionary Algorithm on Sudoku puzzle using Python.

## Question 1: 8-Puzzle Game
## The 8-Puzzle Problem Overview:
The 8-puzzle game consists of a 3x3 grid with 9 squares and 8 numbered tiles. The game starts with the 8 numbered tiles placed in their 'Initial State'- the player shifts one tile at a time in an effort to match the tile placement to the grid's 'Goal State'. The game rules are as follows:

Goal: reach the tile configuration in the 'Goal State' from the 'Initial State'
Legal Actions: up, down, left, right.
One tile at a time.
Only tiles adjacent to the blank space in the current state.
This game can be viewed as a search problem in the sense that the player is searching for a path that would lead from the 'Initial State' to the 'Goal State' by rearranging the tiles.

For example: The paths would be considered short if few tiles are moved and few moves are made in the process of reaching the goal state - and vice versa.
An example of a task that applies the same logic is mapping directions to a location (ex. GPS).
The A* algorithm is implemented using Python to solve the given 8-Puzzle problem.

## A* Algorithm Overview:
Heuristics are informed problem-solving techniques, meaning that they use a heuristic / estimation of the fastest path to the solution. This can provide a time-efficient solution to given problems. The solution / path is based on a simple set of rules (depending on the problem), however, using a heuristic function does not guarantee that the solution is optimal, just that it is complete. The A* algorithm is a computer algorithm that employs heuristic functions to find the path in a tree with the lowest cost from the start node to the goal node. With an admissible heuristic function, A* provides a complete and optimal solution. A heuristic function is considered admissible if it uses an optimistic estimate- that is that it does not overestimate the cost. The algorithm keeps track of each node visited and doesn't waste time exploring paths with relatively higher cost. The A* algorithm uses the following function f(n) to calculate the cost of a move at every node: f(n) = g(n) + h(n)

f(n) = total estimated cost of path through node n
g(n) = sum of costs so far that made us reach node n
h(n) = estimated cost from node n to goal node (heuristic)
Can be calculated using Manhattan Distance from node n to the goal node. Manhattan Distance is the sum of vertical and horizontal steps / distance between two points.
Ideally h(n) = h x (n)
When h(n) = 0, the algorithm prioritizes finding the shortest path to the goal node (aka Dijkstra's algorithm)
The goal is to choose the path with the smallest cost (f(n) ) possible.

### Admissible heuristic functions:
h1(n): number of misplaced tiles
h2(n): sum of Manhattan distance for every tile from current to goal state
The reason these two heuristics are admissible is because they do not overestimate the cost of moves made to reach the goal state, and help us find an optimal path from the start to the goal state. They help guide the algorithm from the start state to the goal state efficiently. The reason H1 was chosen is because every tile in the start state is misplaced and will therefore need to be moved in order to reach the goal state. As for H2, it is used since with every move each tile can only get 1 step closer to the goal position. Our goal is to minimize both functions; the smaller the sum of misplaced tiles and the smaller the sum of distances, the closer we are to reaching the goal state.


## Question 2: Sudoko Puzzle
### Evolutionary Algorithm Overview:
Evolutionary algorithms are algorithms inspired by Darwin’s Theory of Evolution that use mechanisms similar to those that occur in nature. The algorithms aim to mimic and apply behaviors of organisms computationally in an effort to create effective solutions to different problems.
Much like living biological creatures, Evolutionary Algorithms revolve around the concept of ‘survival of the fittest’ and include functions like selection, reproduction, mutation and recombination.

The subset of the Evolutionary Algorithm we will be using to solve the Sudoku puzzle is the Genetic Algorithm - a heuristic search algorithm based on genetics and natural selection.

### Genetic Algorithm Overview:
The Genetic Algorithm mimics the process of natural selection and 'survival of the fittest', where the best individual is selected for reproduction of the next generation. Given a population, the most fit 'parents' are selected to reproduce. The 'children' then inherit the 'genes' (characteristics) of the parent, and the process repeats until we are left with the most fit population of individuals.

This could be implemented to search problems by eliminating the least fit solutions and selecting the best one. The five phases we consider are:

Initial population: This is the set of solutions. Each individual is characterized by a set of 'genes' (parameters) that form a 'chromosome' (solution).
Fitness function: This computes the probability of an 'individual' (solution) being more fit relative to the others in a population.
Selection: The most fit 'individuals' (solutions) are selected and paired to be 'parents' and are responsible for creating offspring and passing on their 'genes' (parameters).
Crossover: This is the most important phase of the algorithm, where a crossover point is selected at random for both 'parents genes', and the 'genes' are exchanged. This creates an offspring with the characteristics of both 'parents', and the offspring is added to the population (as a new, fit soltion).
Mutation: The purpose of this phase is to increase diversity. In this phase, some of the 'genes' positions are randomly swapped.
When does it end?
The process terminates when the offspring produced doesn't show significant change / imporvement in fitness. This means that we have reached optimal fitness (aka the best solution).

### Sudoku Puzzle Overview & Design:
The Sudoku puzzle is a popular game consisting of a 9x9 grid made up of 9 smaller 3x3 squares. The goal of the game is to fill out the grid with numbers 1-9 without repeating numbers in each column, row, or square. The numbers in each column, row, and square should sum up to 45.

We implement the Genetic Algorithm to create a program that can solve Sudoku puzzles in a computationally efficient manner. We use the five phases and design the algorithm as follows:

Initial population: Generate a random permutation of numbers between 1-9 to fill empty spaces in the grid.

Fitness function:: We use a constraint satisfaction fitness function to evaluate the fitness of each generated solution. The solution with the least duplicates that best satisfies the constraints is selected for reproduction. The function measures the number of duplicates in each row, column, and square. Fewer duplicates = better fitness The constraints of the Sudoku puzzle are:

Each row should not have duplicates
Each column should not have duplicates
Each square should not have duplicates
The simplest way to represent a fitness function for the Sudoku puzzle is as follows:

fitness function = ∑9𝑖=1𝑟𝑖+𝑐𝑖+𝑠𝑖
where:

𝑟𝑖
 = duplicate numbers in row i
𝑐𝑖
 = duplicate numbers in column i
𝑐𝑖
 = duplicate numbers in subgrid i
However, in our program, we use a linear combination of the partial costs to create the constraint satisfaction fitness function used by the algorithm. This helps us ensure that the solution satisfies the requirements of the game and allows us to effectively measure its fitness.

Selection: The selection process is done using ranking and tournament operators.

Ranking: ranks all the solutions in a population based on fitness (1 being the most fit, etc...)
Tournament: randomly selects solutions from a population and compares their fitness. The solutions with higher fitness scores are reserved, while the solutions with lower fitness scores 'lose' the tournament and are eliminated.
Crossover: In 9x9 Sudoku puzzles, there are 8 possible crossover points. The crossover operators we used are:

One-point crossover: one point is selected for both 'parents', and the position of the numbers in the offpspring is flipped accordingly.
Uniform genes crossover: randomly selects 'gene' from the 'parents' for the offspring to inherit.
Mutation: We use swap as a mutation operator, which causes two numbers in a subgrid to swich positions. We set a small value for the mutation rate (0.01) for all population sizes, as a high value will lead to increased randomness. We also use reset mutation, which randomly changes one of the gene's values to a different legal value (range 1-9).

Termination: The process will terminate once the constraints are satisified and the puzzle is solved. Otherwise, if the offspring shows no improvement in fitness, the puzzle is ruled to have no solution.
