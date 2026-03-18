# Rural Internet Connectivity — MST (Kruskal's Algorithm) in Java

## Project Overview
A government initiative to connect remote villages with fiber optic cable at minimum cost.
This project models villages as graph vertices and possible cable routes as weighted edges,
then finds the Minimum Spanning Tree using Kruskal's Algorithm.

---

## File Structure
```
mst_rural_internet_java/
├── GenerateVillages.java   # Generates villages.csv (100 random villages)
├── MSTKruskal.java         # Main MST implementation (Kruskal's + Union-Find)
├── villages.csv            # Auto-generated dataset (created at runtime)
├── mst_result.csv          # MST output edges (created at runtime)
└── README.md
```

---

## How to Run

### Prerequisites
- Java JDK 8 or higher
- Run `java -version` to confirm installation

### Step 1 — Generate the dataset
```bash
javac GenerateVillages.java
java GenerateVillages
```
Produces `villages.csv` with 100 villages (id, name, x_coord, y_coord).

### Step 2 — Run Kruskal's MST
```bash
javac MSTKruskal.java
java MSTKruskal
```
Reads `villages.csv`, prints:
- Step-by-step demo on a 5-village graph
- Full MST edge table for all 100 villages
- Verification (edge count, acyclic check, connectivity)

Writes `mst_result.csv` with the resulting MST edges.

---

## Algorithm: Kruskal's

**Why Kruskal's?**
With 100 villages the complete graph has 100×99/2 = 4,950 edges.
Kruskal's O(E log E) is well-suited for edge-list representations and
the union-find structure makes cycle detection simple and efficient.

**Steps:**
1. Build complete graph — all N*(N-1)/2 edges with Euclidean distances as weights
2. Sort edges by weight using manual Merge Sort (no `Arrays.sort`)
3. Use Union-Find with path compression + union by rank to detect cycles
4. Greedily add the cheapest edge that doesn't form a cycle until V-1 edges are added

---

## Data Structures Used (all arrays)
| Structure | Type | Purpose |
|---|---|---|
| `villages[]` | Village[] | Stores id, name, x, y for each village |
| `edges[]` | Edge[] | Flat edge list for complete graph |
| `mstEdges[]` | Edge[] | Stores the V-1 MST result edges |
| `parent[]` | int[] | Union-Find parent pointers |
| `rankArr[]` | int[] | Union-Find rank for union by rank |
| `tempBuf[]` | Edge[] | Scratch buffer for merge sort |

---

## Complexity Analysis

| Phase | Time | Space |
|---|---|---|
| Build edges | O(V²) | O(V²) |
| Merge sort edges | O(E log E) | O(E) |
| Union-Find (all ops) | O(E · α(V)) ≈ O(E) | O(V) |
| Overall | **O(E log E) = O(V² log V)** | O(V²) |

α(V) is the inverse Ackermann function — effectively constant for all practical V.

---

## Dataset
Self-generated: 100 villages with random (x, y) coordinates in a 1000×1000 km grid.
Euclidean distance between coordinates is used as the cable cost (in km).

**Source:** Self-generated using `java.util.Random` in `GenerateVillages.java`

---

## Sample Output
```
============================================
 Rural Internet Connectivity - MST Problem
 Algorithm: Kruskal's (with Merge Sort)
============================================

=== Step-by-Step Demo: 5-Village Graph ===
ADD   AlphaVillage -- EpsilonVillage  (dist=2.5000)
ADD   BetaVillage  -- EpsilonVillage  (dist=2.5000)
SKIP  AlphaVillage -- BetaVillage     (would form cycle)
ADD   GammaVillage -- EpsilonVillage  (dist=1.8028)
ADD   DeltaVillage -- AlphaVillage    (dist=3.0000)
Demo MST: 4 edges (V-1 = 4) ✓

=== Kruskal's MST Result ===
Villages connected : 100
MST edges          : 99 (should be 99)
Total cable length : 487.63 km

Edge   Village A            Village B            Distance(km)
----------------------------------------------------------
1      NorthPur12           GreenGanj45          4.21
2      RiverNagar7          NorthPur12           5.03
...

=== Verification ===
Edge count V-1 (99): PASS
Acyclic (no cycles): PASS
All villages connected: PASS

MST saved to mst_result.csv
```

---

## References
- Kruskal, J.B. (1956). *On the shortest spanning subtree of a graph.* Proceedings of the AMS.
- Cormen, T.H. et al. *Introduction to Algorithms*, 3rd ed. MIT Press. (Chapter 23 — MST)
- Dataset: Self-generated random coordinates (no external source required)
