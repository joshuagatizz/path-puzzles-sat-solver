# Path Puzzles SAT Repository

**Path puzzle** is a pencil-and-paper puzzle that was proven to be NP-complete. This repository contains code and data related to solving Path puzzles using a SAT-based approach using PySAT.

For more information about the puzzle, see [https://www.enigami.fun](https://www.enigami.fun/_files/ugd/e33b96_3c76845fa9dd413a9f1606ce1c040b99.pdf) or the [original book](https://books.google.co.id/books/about/Path_Puzzles.html?id=tDhaswEACAAJ&redir_esc=y) by Roderick Kimball.

## Test Cases

The `tc` directory contains a set of test cases in plain text. The directory contains two subdirectories `input` and `output` which contains the input files for the solver and the expected output of each case, respectively.

The cases are taken from [font-pathpuzzles](https://github.com/edemaine/font-pathpuzzles) repository by Erik Demaine.
There are 26 cases in each subdirectory where each case has only one solution, grid size of $6 \times 6$, and corresponds to a letter in the standard English alphabets.
In addition, we also added 5 additional cases with bigger grid sizes for further exploration. Note that these additional cases have more than one solution and the provided expected output file is just one of them

The cases from [font-pathpuzzles](https://github.com/edemaine/font-pathpuzzles) are named according to their correspoding letter (for example, `a.in` for letter a), while the additional cases are named based on their grid sizes (`7x7.in` for $7 \times 7$ grid).

## Solver

The solver code is written in Python and can be found in the `src/path_sat.py` file. It solves the given Path puzzle instance using a SAT-based approach. Here, we encode the constraints of the puzzle and use the MiniSAT module from PySAT to find the solution to the puzzle.

To install PySAT, refer to [the installation guide](https://pysathq.github.io/installation/).

### Input
The input format for the solver is as follows:
```
m n
i_a j_a
i_b j_b

cr_1 cr_2 ... cr_m
cc_1 cc_2 ... cc_n
```
where:

 - `m` and `n` are integers representing the size of the puzzle.
 - `i_a` and `j_a` are integers representing the first door cell.
 - `i_b` and `j_b` are integers representing the second door cell.
 - `cr_{i}` is an integer representing the constraint number of row `i`. If row `i` does not have a constraint number, then `cr_{i} = -1`.
 - `cc_{j}` is an integer representing the constraint number of column `j`. If column `j` does not have a constraint number, then `cc_{j} = -1`.

### Output
The output format of the solver is as follows:

 - The given input has a solution:
 ```
solution found!

p_{1,1} p_{1,2} ... p{1,n}
p_{2,1} p_{2,2} ... p{2,n}
...
p_{m,1} p_{m,2} ... p{m,n}

time taken: {running time}ms.
 ```
 - The given input has no solution:
  ```
solution not found.
 ```

where `p_{i,j}` is a character representing cell $(i,j)$ of the puzzle, where the value is either `'.'`, `d`,`l`,`r`, or `u`. `'.'` represents an empty cell while `d`,`l`,`r`, and `u` correspond to $down, left, right,$ and $up$---direction of the path passing the cell. For example if the path pass through cell $(i,j)$ and then cell  $(i+1,j)$, then `p_{i,j} = d`.  Note that the second door cell $(i_b,j_b)$ is always filled with `u`.

### Example
```
3 3
1 1
3 3

1 3 1
2 1 2
```
```
d . .
r r d
. . u
time taken: 1ms.
 ```
## Usage

To use the code in this repository, simply clone the repository and run `path_sat.py` file and type the input manually or use a file input such as the test cases.

## Experiment Results

The `data/runtime.xlsx` file contains the results of experiments conducted on the solver against the test cases. The runtime and average of three tests are recorded for each test case. Additionally, we compare the solver's runtimes with the runtimes of the backtracking method, which can be found in [path-puzzles-backtracking-solver](https://github.com/joshuagatizz/path-puzzles-backtracking-solver) repository.

The `runtime.xlsx` consists of two sheets, one for the backtracking method and the other for the SAT-based solver.
Each sheet contains two tables, one for the alphabet cases and the other is for the five additional cases. Each sheet includes two tables: one for the alphabet cases and another for the five additional cases. The runtime values for the backtracking method, specifically for the alphabet cases, are sourced from the [path-puzzles-backtracking-solver](https://github.com/joshuagatizz/path-puzzles-backtracking-solver) repository. On the other hand, the runtime values for the remaining cases are recorded during our own experiment.