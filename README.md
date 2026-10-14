*This project has been created as part of the 42 curriculum by ebansse.*

# push_swap

Sort a list of integers using two stacks and a limited set of operations, in **as few moves as possible**. The program prints the sequence of operations that sorts the numbers.

## The rules

You start with all the numbers on stack **a** (the first argument on top) and an empty stack **b**. Only these operations are allowed:

| Op | Effect |
|---|---|
| `sa` / `sb` / `ss` | swap the top two elements of a / b / both |
| `pa` / `pb` | push the top of b onto a / the top of a onto b |
| `ra` / `rb` / `rr` | rotate a / b / both up (the top element goes to the bottom) |
| `rra` / `rrb` / `rrr` | reverse rotate a / b / both (the bottom element goes to the top) |

## Algorithm

- **2 to 3 numbers:** a hard-coded sort (`sort_3`) that needs at most 2 moves
- **Up to 5 numbers:** push the smallest values to b, sort the remaining 3, then push them back (`sort_5`)
- **Larger inputs:** a cost-based insertion sort, often called the *Turk algorithm*:
  1. Push two numbers to b.
  2. For every number still in a, compute how many moves it would take to bring it to the top of a **and** to rotate b so that the number lands right above its closest smaller value (b is kept in descending order).
  3. Push the cheapest number, combining `rr` and `rrr` whenever both stacks need to rotate the same way.
  4. Once a is empty, rotate the largest value of b to the top and push everything back to a.

### Measured results (random inputs)

| Input size | Max operations | 42 target for full marks |
|---|---|---|
| 3 | 2 | ≤ 3 |
| 5 | 10 | ≤ 12 |
| 100 | ~650 | < 700 |
| 500 | ~5,400 | ≤ 5,500 |

## Build & usage

```bash
make
./push_swap 3 2 5 1 4
./push_swap "3 2 5 1 4"        # a single quoted string also works
```

Check the result with the provided checker:

```bash
ARG=$(shuf -i 1-1000 -n 100 | tr '\n' ' ')
./push_swap $ARG | ./checker_linux $ARG     # prints OK
./push_swap $ARG | wc -l                      # number of operations
```

Invalid input (non-numeric values, duplicates, values outside the int range) prints `Error`. Already sorted input prints nothing.

## Project structure

| Path | Role |
|---|---|
| `main.c` | argument parsing and program entry |
| `movements/` | the 11 stack operations |
| `tiny_sort.c` | sorting for 3 and 5 numbers |
| `turkey_sort.c`, `move.c` | cost computation and the main algorithm |
| `tools/` | parsing, list helpers, memory cleanup |
| `libft/` | my Libft |
