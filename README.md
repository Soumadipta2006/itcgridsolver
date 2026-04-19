# Zero-Error Toroidal Grid Explorer

An interactive, browser-based visualization and brute-force solver for a geometric packing problem in Information Theory and Coding (ITC).

This tool explores the optimal codeword packing for a discrete memoryless channel (the 5-symbol "Typewriter Channel") over a blocklength of $n=2$. It provides a visual and mathematical playground to understand zero-error capacity, maximum likelihood decoding overlap, and the probability of error ($P_e$).

## 🚀 Features

* **Interactive 2D Toroidal Grid**: A $5\times 5$ grid where blocks wrap around the edges (modulo 5 arithmetic), accurately representing the output space of the channel.
* **Manual Placement**: Click any cell to place a $2\times 2$ block. Overlapping blocks visually stack (increase in opacity) to highlight areas of decoder confusion.
* **Live Metrics**: Automatically calculates and updates the Unique Cells Covered ($U$) and the minimum Probability of Error ($P_e$).
* **Bitmask Brute-Force Solver**: Features an integrated Depth-First Search (DFS) algorithm that uses 32-bit integer bitmasking to instantly find the mathematically optimal configuration for any $M$ (from 5 to 10).

## 🧮 The Information Theory Behind It

Given an input alphabet $X=\{0,1,2,3,4\}$ where the output $Y$ is either $X$ or $X+1 \pmod 5$ with equal probability:

1. **The Grid ($n=2$)**: Transmitting a 2-symbol codeword generates $2 \times 2 = 4$ equiprobable outputs. This forms a $5\times 5$ toroidal grid containing 25 total cells.
2. **Zero-Error Capacity ($M=5$)**: The maximum number of mutually disjoint $2\times 2$ blocks that can fit on this grid is 5. This achieves a unique coverage of $U=20$, leaving exactly 5 empty, non-adjacent cells.
3. **Minimizing Error ($M > 5$)**: If we want to transmit more than 5 messages ($M=6$ to $10$), overlap is strictly unavoidable. To minimize the average probability of error under Maximum Likelihood (ML) decoding, we must place our blocks to maximize the total number of unique cells ($U$) covered by the codebook.
4. **Probability of Error**: $P_e = 1 - \frac{U}{4M}$

## 🛠️ How to Run

This project requires **zero dependencies**, no build steps, and no backend. 

1. Download or clone this repository.
2. Open `opt1.html` in any modern web browser (Chrome, Firefox, Safari, Edge).
3. That's it!

## 🕹️ How to Use

* **Slider**: Adjust the number of messages ($M$) you wish to transmit (between 5 and 10).
* **Manual Mode**: Click directly on the grid to drop anchors. See if you can manually find the configuration that maximizes $U$.
* **Auto-Solve (Brute Force)**: Click this button to run the JavaScript bitmask solver. It will evaluate millions of permutations in milliseconds to find and display the optimal configuration for the selected $M$.
* **Clear Grid**: Wipes the board clean to start over.

## ⚡ Technical Details: The Solver

Finding the optimal $U$ relies on combinatorial search ($\binom{25}{M}$ combinations). To run this live in the browser without freezing the UI, the grid is mapped to a 25-bit integer. 

Calculating the overlap of multiple $2\times 2$ blocks is reduced to a native CPU bitwise `OR` (`|`) operation, allowing the recursive Depth-First Search to prune invalid paths and find the absolute maximum $U$ in a fraction of a second.
