# 🎲 Random Graphs: Phase Transitions via DFS & Monte Carlo Simulation

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-required-013243?logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-required-11557c)
![Course](https://img.shields.io/badge/Course%20Project-CS648-green)

An empirical study of the **Erdős–Rényi random graph model G(n, p)**. The project uses a **DFS-based connectivity algorithm** and **Monte Carlo simulation** to validate two classic phase-transition thresholds from probabilistic analysis:

1. 🌐 **Emergence of the giant connected component** at `p = 1/n`
2. 🔗 **Graph connectivity** at `p = ln(n)/n`


---

## 📌 Table of Contents
- [Background](#-background)
- [What This Project Does](#-what-this-project-does)
- [Results](#-results)
- [How It Works](#-how-it-works)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Customization](#-customization)
- [Key Takeaways](#-key-takeaways)

---

## 📖 Background

In the **G(n, p)** model, a graph has `n` vertices and each of the `n(n-1)/2` possible edges is included **independently with probability `p`**.

Remarkably, many graph properties switch from "almost never true" to "almost always true" within a very narrow range of `p`. This sudden change is called a **phase transition**.

| Property | Threshold | Below threshold | Above threshold |
|---|---|---|---|
| Giant component | `p = c/n`, `c = 1` | All components are small (`O(log n)`) | One component of size `Θ(n)` appears |
| Connectivity | `p = c·ln(n)/n`, `c = 1` | Isolated vertices exist, graph is disconnected | Graph is connected with high probability |

**Why `ln(n)/n` for connectivity?** Below this threshold, some vertices end up with no edges at all (isolated), so the graph cannot be connected. Right at the threshold, the expected number of isolated vertices drops to a constant, and above it, it vanishes.

---

## 🚀 What This Project Does

- Generates random graphs `G(n, p)` for many values of `n` and `p`
- Uses an **iterative DFS** to find all connected components
- Runs **Monte Carlo simulations** (many independent trials per setting) to estimate:
  - the average **fraction of vertices in the largest component**
  - the **probability that the graph is connected**
- Plots results across multiple values of `n` and overlays the **theoretical threshold**
- Shows that the transition gets **sharper as `n` grows**

---

## 📊 Results

### Giant Component Emergence
Using `p = c/n`, the largest component jumps from tiny to a constant fraction of the graph around **c = 1**.

![Giant component](giant_component.png)

### Connectivity Threshold
Using `p = c·ln(n)/n`, the probability of being connected rises from 0 to 1 around **c = 1**, and the curve becomes steeper for larger `n`.

![Connectivity](connectivity.png)

---

## 🧠 How It Works

### 1. Graph generation
Random numbers are drawn for every possible edge (upper triangle of the adjacency matrix with NumPy), and an edge is kept if its random value is below `p`. The result is stored as an adjacency list.

### 2. Connected components with DFS
An **iterative DFS** (explicit stack, no recursion limit issues) visits every vertex once. Each fresh DFS start marks a new component, and its size is recorded.

**Complexity:** `O(n + m)` per graph.

### 3. Monte Carlo estimation
For each parameter value, the experiment is repeated `trials` times on independent random graphs and the results are averaged. By the law of large numbers, the average approaches the true expected value.

```
for each n:
    for each c:
        repeat `trials` times:
            G = random_graph(n, p(c, n))
            record largest component size / connectivity
        average over trials
```

---

## ⚙️ Getting Started

### Prerequisites
- Python 3.8 or higher
- `numpy` and `matplotlib`

### Installation

```bash
git clone https://github.com/<your-username>/random-graphs-phase-transition.git
cd random-graphs-phase-transition
pip install numpy matplotlib
```

### Run

```bash
python random_graphs.py
```

Two images will be generated in the project folder:
- `giant_component.png`
- `connectivity.png`

### Run without installing anything
Open [Google Colab](https://colab.research.google.com), paste the contents of `random_graphs.py` into a cell, and press ▶.

---

## 🗂 Project Structure

```
random-graphs-phase-transition/
├── random_graphs.py        # Graph generation, DFS, experiments, plotting
├── giant_component.png     # Output: giant component phase transition
├── connectivity.png        # Output: connectivity phase transition
└── README.md
```

### Main functions

| Function | Purpose |
|---|---|
| `random_graph(n, p)` | Builds a G(n, p) graph as an adjacency list |
| `component_sizes(adj)` | Iterative DFS returning all component sizes |
| `giant_fraction(n, p)` | Largest component size divided by `n` |
| `is_connected(n, p)` | True if the graph has exactly one component |
| `experiment_giant()` | Monte Carlo sweep for the giant component threshold |
| `experiment_connectivity()` | Monte Carlo sweep for the connectivity threshold |

---

## 🔧 Customization

Change graph sizes and number of trials directly in the function calls at the bottom of `random_graphs.py`:

```python
experiment_giant(ns=(100, 200, 400), trials=100)
experiment_connectivity(ns=(50, 100, 200), trials=200)
```

- More **trials** gives smoother curves (but takes longer)
- Larger **n** gives a sharper transition (but takes longer)

---

## 💡 Key Takeaways

- Simple random processes can produce **sudden, sharp changes in global structure**.
- The empirical curves from simulation **agree with the theoretical thresholds** (`1/n` and `ln(n)/n`).
- As `n` increases, the transition becomes **steeper**, consistent with the theory that thresholds are sharp in the limit `n → ∞`.

---

## 📚 References

- P. Erdős and A. Rényi, *On the evolution of random graphs* (1960)
- M. Mitzenmacher and E. Upfal, *Probability and Computing*
  

---

## 👤 Author

**Gagan Piplani**
Feel free to ⭐ the repo if you found it useful!
