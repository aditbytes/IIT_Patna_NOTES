# MASTER PROMPT — BO CDA 203: Design of Algorithms → Complete Exam-Prep DOCX

> Paste this whole file into a fresh Claude Code session whose working directory is
> `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3`.

---

## 0. ROLE AND GOAL

You are an expert professor of algorithms (at the level of Kleinberg–Tardos, CLRS and Roughgarden) and an exam coach for Indian CBT (computer-based, MCQ-only) exams, in the style of GATE-CS algorithm questions.
Your student is **Aditya**, a 3rd-semester **B.S. Computer Science & Data Analytics (CSDA)** student at **IIT Patna**.

Produce **one complete, self-sufficient Word document** (`CDA203_Algorithms_Exam_Guide.docx`) for **BO CDA 203 — Design of Algorithms (L-T-P-C 3-1-2-5)**.
After studying this guide, Aditya should not need to open any other book or note. The exam is **CBT, MCQ-only**, so the guide must teach
every algorithm from scratch (intuition, pseudocode, dry run, correctness proof, complexity) **and** drill it with a very large bank of MCQs
(trace outputs, complexity identification, recurrence solving, "which algorithm/data structure", true/false properties).

**Non-negotiables**
1. **Follow the official syllabus order exactly (Section 2)**; every syllabus phrase must appear as a heading. Tag each section with 🏫 *Taught in class (date)* or 📚 *From textbook*.
2. **Be correct.** Every trace, every output and every complexity claim must be checked by actually running Python code (Section 8).
3. **Use the professor's notation, examples and proof style first** (Section 3). His style closely follows **Tim Roughgarden's *Algorithms Illuminated*** (e.g., "greedy diff vs. greedy ratio", "consecutive inversion" exchange argument, Σ-trees for Huffman, Master Method "good force b^d vs. bad force a"). Keep that vocabulary.
4. **Correct, don't copy, the errors** listed in Section 4.
5. **Read the source files yourself** (Section 1) first; if you find something the digest missed, include it.
6. **Build in stages:** one syllabus unit per turn, appended to the same .docx. After each unit, report its pages and MCQs, then wait for "continue".

---

## 1. SOURCE FILES (read these first)

**Official syllabus:** `203/CDA 203.pdf` and `s-2.png` (right half).

**Professor's lecture notes (handwritten tablet notes, no text layer — render every page and read it):** `203/Lecture Notes-20260921 (1)/`
`Lec 1-2.pdf` (17/8), `Lec 3 Tute 1.pdf` (17/8), `Lec 4-5.pdf` (24/8), `Lec 6 Tute 2.pdf` (25/8), `Lec 7-8.pdf` (31/8), `Lec 9 Tute 3.pdf` (1/9), `Lec 10-11.pdf` (7/9), `Lec 12 Tute 4.pdf` (8/9), `Lec 13-14.pdf` (14/9), `Lec 15 Tute 5.pdf` (15/9)

**Textbooks in the folder (and what they really are):**
1. `203/M. A. Weiss, Data Structures and Algorithms in C++, 4th Ed.pdf`: Ch 2 Algorithm Analysis · Ch 4 Trees (4.3 BST, 4.4 AVL, 4.7 B-trees) · Ch 5 Hashing · Ch 6 Priority Queues/Heaps · Ch 7 Sorting (7.8 lower bound, 7.6 heapsort, 7.7 quicksort, 7.11 linear-time sorts) · Ch 8 Disjoint Sets · Ch 9 Graph Algorithms (9.3 shortest paths, 9.4 network flow, 9.5 MST, 9.6 DFS, 9.7 NP-completeness) · Ch 10 Design Techniques (10.1 greedy incl. Huffman, 10.2 D&C, 10.3 DP, 10.4 randomized) · Ch 12.2 Red-black trees
2. `203/Algorithm Design - John Kleinberg - Éva Tardos.pdf` (scanned, with OCR): Ch 2 Basics of analysis · Ch 3 Graphs (BFS, DFS, bipartite, DAG/topological order) · Ch 4 Greedy (interval scheduling, lateness exchange argument, Dijkstra 4.4, MST 4.5–4.6 with union-find, clustering, **Huffman 4.8**) · Ch 5 D&C (mergesort recurrence, counting inversions, closest pair, **integer multiplication 5.5**) · Ch 6 DP (weighted interval scheduling, knapsack, sequence alignment, **Bellman–Ford 6.8**, negative cycles) · Ch 7 Network flow (Ford–Fulkerson) · Ch 8 NP & intractability · Ch 11.3 **Set cover greedy**
3. `203/T. H. Cormen … Introduction to Algorithms, MIT Press, 2009.pdf` → **this is really the CLRS 3e *Instructor's Manual*** (lecture notes and solutions for Ch 2–17, 21–27; **no Ch 18 B-trees and no Ch 34 NP-completeness**). Use it for solved exercises and for the CLRS chapter mapping: Ch 2, 3, 4 (substitution, recursion tree, master), 5, 6 heapsort, 7 quicksort, 8 linear sorting and the lower bound, 11 hashing, 12 BST, 13 RB-trees, 15 DP, 16 greedy, 22 BFS/DFS, 23 MST, 24 SSSP (Bellman–Ford, Dijkstra), 25 all-pairs (**Johnson 25.3**), 26 max-flow. For B-trees (CLRS 18) and NP-completeness (CLRS 34), use Weiss 4.7 and 9.7, Kleinberg–Tardos Ch 8, and your own expert knowledge.
4. `203/A. Aho, J. E. Hopcroft and J. D. Ullman … 1974.pdf` → **mislabeled: it is only a 25-page excerpt of Dexter Kozen's *The Design and Analysis of Algorithms*** (early lectures, incl. matroids and greedy). Use it only for the matroid/greedy-optimality material, and cover RAM-model/cost-model content (the true Aho–Hopcroft–Ullman Ch 1 topic) from your own expert knowledge.

**Practical component:** labs in C/Python. Every algorithm gets a short, runnable Python implementation in the guide.

---

## 2. OFFICIAL SYLLABUS — THE ORDER OF THE DOCUMENT (do not change)

**Unit 0: Introduction** (🏫 L1–2): what an algorithm is; computational problem (input → output); input size (#bits, e.g., Σ log₂ aᵢ); analysis = correctness + efficiency (time/space); the three design techniques; preview of P/NP/NPC. Insertion sort and selection sort (pseudocode, cost table c₁…c₇, tᵢ, best/worst case, correctness by induction / loop invariant).

**Unit 1: Model of Computation**: 1.1 RAM model (unit-time primitive ops, memory model) · 1.2 **Uniform cost model** · 1.3 **Logarithmic cost model** (cost ∝ bit length; when each is appropriate; example: computing 2^(2^n) by repeated squaring under both models) · 1.4 Counting primitive operations (🏫 L1–2)

**Unit 2: Complexity Analysis**: 2.1 **Big-O, Big-Ω, Big-Θ** (formal c, n₀ definitions, 🏫 L3) plus little-o/ω, limit rules, and common-function ordering · 2.2 Best/worst/average case · 2.3 **Solving recurrence relations**: iteration/unrolling (🏫 merge sort), recursion tree (🏫 3-way, n/3 : 2n/3, 1:99 splits), substitution method, **Master theorem** (🏫 L7–8: a vs. b^d form, all three cases, the proof via level-j work c·n^d(a/b^d)^j), plus a note on the CLRS (f(n) vs. n^{log_b a}) form, how the two forms map to each other, and cases where the Master theorem does not apply

**Unit 3: Data Structures**: 3.1 **Binary search trees** (search/insert/delete, successor, height, traversals) · 3.2 **AVL trees** (balance factor, LL/RR/LR/RL rotations with worked insert/delete sequences) · 3.3 **Red-black trees** (5 properties, black-height, height ≤ 2 log₂(n+1), insert fix-up cases with recolour/rotate, deletion overview) · 3.4 **B-trees** (order/min-degree t, properties, search, insert with split, delete overview, height bound, disk-access motivation, B+ trees for awareness) · 3.5 **Hashing** (hash functions: division, multiplication, universal hashing; chaining; open addressing: linear/quadratic probing, double hashing; load factor α; expected costs 1+α and 1/(1−α); clustering; rehashing) · 3.6 **Priority queues** (ADT) · 3.7 **Heaps** (binary heap array indexing, heapify, build-heap in O(n), insert, extract-max/min, decrease-key; d-ary heaps for awareness)

**Unit 4: Sorting Algorithms**: 4.1 **Merge sort** (🏫 L3: merge procedure, recurrence, 3-way variant) · 4.2 **Quick sort** (🏫 L4–5: first-element pivot, partition, cases I–IV, constant-fraction vs. constant-number splits, 1:99 argument) plus Lomuto/Hoare partition · 4.3 **Heap sort** · 4.4 **Randomized quick sort** (🏫 expected O(n log n); full expected-comparisons proof E = Σ 2/(j−i+1) ≈ 2n ln n) · 4.5 **Lower bound for comparison-based sorting** (🏫 L6: decision tree, n! leaves, h ≥ log₂ n! = Ω(n log n)) · 4.6 **Counting sort** (🏫 queue version plus the CLRS stable prefix-sum version, O(n+k)) · 4.7 **Radix sort** (🏫 LSD example, stability, O(d(n+k)); the MSD exercise) · 4.8 **Bucket sort** (🏫 L7: uniform [0,1), expected O(n)) · 4.9 Comparison table (time best/avg/worst, space, stable?, in-place?, adaptive?)

**Unit 5: Algorithm Design Techniques** (keep this order: Greedy → Divide & Conquer → Dynamic Programming)
- 5.1 **Greedy**: paradigm and "greedy stays ahead" vs. exchange argument; 🏫 **Scheduling to minimize the weighted sum of completion times** (L10–12: diff vs. ratio, counter-example, full exchange-argument proof with and without ties); 🏫 **Set cover** greedy plus the O(log n) (Hₙ ≈ ln n) approximation (L12); 🏫 **Huffman coding** (L13–15: prefix-free codes, Σ-trees, average leaf depth, algorithm, O(n log n), full optimality proof with Claims 1 and 2); plus activity/interval selection, fractional knapsack, the matroid view (Kozen excerpt)
- 5.2 **Divide & Conquer**: paradigm; merge sort; 🏫 **integer multiplication** (L9: grade-school O(n²), 4-subproblem D&C O(n²), **Karatsuba** O(n^{log₂3}) ≈ O(n^{1.585})); 🏫 **matrix multiplication** (naive O(n³), block D&C 8T(n/2) + O(n²) = O(n³), **Strassen** 7T(n/2) + O(n²) = O(n^{log₂7}) ≈ O(n^{2.81})); 🏫 **searching lower bounds** (unsorted Ω(n), sorted Ω(log n) via the 3-way decision tree 3^h ≥ n); binary search; counting inversions; closest pair of points; maximum subarray; quickselect/median-of-medians
- 5.3 **Dynamic Programming**: principles (optimal substructure, overlapping subproblems, memoization vs. tabulation); weighted interval scheduling; 0/1 knapsack; LCS; edit distance/sequence alignment; matrix-chain multiplication; rod cutting; coin change; Bellman–Ford as DP; Floyd–Warshall; for each, give the table-filling trace and the reconstruction

**Unit 6: Graph Algorithms**: 6.0 Representations (adjacency list/matrix, costs) · 6.1 **BFS and DFS** (colouring/discovery–finish times, edge classification, BFS shortest paths in unweighted graphs, bipartiteness, topological sort, SCC (Kosaraju) for awareness) · 6.2 **Minimum spanning trees**: cut & cycle properties, **Kruskal** (with union-find, union by rank and path compression) and **Prim** (heap-based); O(E log V) · 6.3 **Shortest paths**: **Dijkstra** (why it fails with negative edges), **Bellman–Ford** (V−1 relaxations, negative-cycle detection), **Johnson's algorithm** (reweighting h via Bellman–Ford from a new vertex s, w′ = w + h(u) − h(v), then V × Dijkstra; O(VE log V)) · 6.4 **Network flow**: flow networks, residual graph, augmenting paths, **Ford–Fulkerson**, max-flow min-cut theorem, Edmonds–Karp O(VE²), integrality, bipartite matching application

**Unit 7: NP-Completeness**: 7.1 Decision vs. optimisation problems; polynomial time · 7.2 **Class P**, **Class NP** (certificates/verifiers), co-NP (awareness) · 7.3 Polynomial-time reductions (≤ₚ) · 7.4 **NP-hard** and **NP-complete**; Cook–Levin (SAT) · 7.5 **Examples of NP-complete problems** with their reduction chain: SAT → 3-SAT → Independent Set ↔ Vertex Cover ↔ Clique; 3-SAT → Hamiltonian Cycle → TSP; Subset Sum/Knapsack; 3-Colouring; Set Cover (tie back to the greedy approximation) · 7.6 Coping with hardness (approximation, exact exponential algorithms, special cases) · 7.7 Venn diagram under P ≠ NP and the "if P = NP" consequences

---

## 3. WHAT THE PROFESSOR ACTUALLY TAUGHT (reproduce these as "Professor's Example" boxes)

- **L1–2 (17/8):** techniques D&C, Greedy, DP; correctness + efficiency; P/NP/NPC. Insertion sort pseudocode: `for i=1 to n−1: key=A[i]; j=i−1; while (A[j]>key and j≥0): A[j+1]=A[j]; j=j−1; A[j+1]=key` with costs c₁n, c₂(n−1), c₃(n−1), c₄Σtᵢ, c₅Σ(tᵢ−1), c₆Σ(tᵢ−1), c₇(n−1); tᵢ = number of while-condition checks. Best case (sorted) tᵢ = 1 → O(n); worst (reverse) tᵢ = i → O(n²). Array example 3 9 11 21 | 12 16 15. Correctness by induction (base t = 1; IH: first t−1 elements are sorted; step: insert x at the unique position a ≤ x < b, assuming distinct elements). Machine-independent analysis: count primitive ops (arithmetic, logical, assignment); drop lower-order terms and constants.
- **L3 + Tute 1 (17/8):** Big-O graph with n₀; 5n+10 ≤ 6n for n ≥ 10 (c = 6, n₀ = 10); polynomial Σaᵢnⁱ = O(nᵏ) with c = Σ|aᵢ|, n₀ = 1; nᵏ ≠ O(nᵏ⁻¹) (contradiction n ≤ c); exercises: is 2^{n+10} = O(2ⁿ)? (Yes, c = 2¹⁰.) Is 2^{10n} = O(2ⁿ)? (No.) Big-Ω and Big-Θ; polynomial = Θ(nᵏ). Merge sort steps with costs O(1) + 2T(n/2) + O(n); merge of C = [2, 9, 13, 16, 21] and D = [6, 8, 18] (a small tail trace glitch in the notes; redo the trace correctly); recursion tree; unrolling 2ᵏT(n/2ᵏ) + kcn with k = log₂n → c₁n + cn log₂n = O(n log n). Exercises: (a) 3 equal parts, (b) n/3 and 2n/3 split: analyse them.
- **L4–5 (24/8):** recursion trees for (a) 3-way: cn per level × log₃n levels; (b) n/3 : 2n/3: the leftmost branch dies at log₃n and the rightmost at log_{3/2}n, so O(n log n); a 1:99 split gives cn·log_{100/99}n = O(n log n). Quick sort: pivot = first element; partition into <p | p | >p; example A = [10, 3, 9, 16, 21, 5, 7, 22], p = 10 → [3, 9, 5, 7, 10, 22, 21, 16]; partition O(n) returns the pivot index j; QuickSort(A, l, r) pseudocode; T(n) = T(k) + T(n−k−1) + O(n). Case I (median pivot) O(n log n); Case II (1:c ratio) O(n log n); Case III (min pivot) T(n−1) + O(n) = O(n²); Case IV (constant c elements on one side) O(n²). A constant fraction on one side → n log n; a constant number → n². 98n/100 of the pivots give a 1:99-or-better split (9998n/10000 for 1:9999). Randomized QS → expected O(n log n).
- **L6 + Tute 2 (25/8):** decision tree for sorting {a₁, a₂, a₃} (root a₁ ≤ a₂, 6 leaves); correspondence internal node ↔ comparison, leaf ↔ answer, root-to-leaf path ↔ one execution, height ↔ worst-case time; n! leaves ≤ 2ʰ ⇒ h ≥ log₂n!; n! > (n/2)^{n/2} ⇒ log n! > (n/2) log(n/2) = Ω(n log n). Linear-time integer sorting (non-comparison). Counting sort for integers in {1..k} (example: marks 0–100 of 10⁸ students), queue per value, O(n+k), linear if k = O(n). Radix sort example 9215, 2135, 2392, 2531, 7212 sorted LSD → 2135, 2392, 2531, 7212, 9215 (show every pass); stable property; O(d(n+k)); linear if d = O(1), k = O(n). Exercise: do it from the thousands place down to units (MSD) and explain why naive MSD needs recursion per bucket.
- **L7–8 (31/8):** bucket sort (n buckets [i/n, (i+1)/n); insertion-sort each bucket; expected O(n) + ΣO(pᵢ²) + O(n) = O(n)). Master theorem T(n) ≤ aT(n/b) + cnᵈ: O(nᵈ log n) if a = bᵈ; O(nᵈ) if a < bᵈ; O(n^{log_b a}) if a > bᵈ. Proof by recursion tree: level j has aʲ subproblems of size n/bʲ and total work cnᵈ(a/bᵈ)ʲ; a = rate of subproblem proliferation ("bad force"), bᵈ = rate of work shrinkage ("good force"); geometric series; (bᵈ)^{log_b n} = nᵈ and a^{log_b n} = n^{log_b a}.
- **L9 + Tute 3 (1/9):** integer multiplication (x = 7239 = 72·100 + 39); grade-school O(n²); x = a·10^{n/2} + b, y = c·10^{n/2} + d, xy = ac·10ⁿ + (ad+bc)·10^{n/2} + bd → T = 4T(n/2) + O(n) = O(n²) (a = 4 > b^d = 2); Karatsuba ad+bc = (a+b)(c+d) − ac − bd → 3 subproblems → O(n^{log₂3}) = O(n^{1.58}). Matrix multiplication naive O(n³); matrix-vector O(n²); block D&C 8 subproblems → O(n³); Strassen's 7 products → O(n^{log₂7}) = O(n^{2.8}). Searching: unsorted cannot beat n; sorted cannot beat log n; 3-way decision tree (=, <, >) with 3ʰ ≥ n ⇒ h ≥ log₃n (exercise: prove by decision tree).
- **L10–11 (7/9):** greedy = iterative, myopic choice. Weighted completion-time scheduling: C_j(σ) = sum of lengths up to and including j; C(σ) = Σwⱼ Cⱼ(σ). Example lengths (1, 2, 3), weights (3, 2, 1): (J₃, J₂, J₁) = 31, (J₂, J₃, J₁) = 27, (J₁, J₂, J₃) = 15. Equal weights → shortest first; equal lengths → heaviest first. Scores: diff wⱼ − lⱼ vs. ratio wⱼ/lⱼ. Counter-example l = (5, 2), w = (3, 1): diff → (J₂, J₁) cost 23; ratio → (J₁, J₂) cost 22 ⇒ diff fails. The ratio rule is always correct; algorithm O(n log n); "weight per unit time".
- **L12 + Tute 4 (8/9):** exchange-argument proof (no ties; consecutive inversion (i, j) with i > j, i immediately before j; every non-greedy schedule has one; swapping changes the cost by wᵢlⱼ − wⱼlᵢ < 0 ⇒ contradiction); ties: the swap doesn't increase the cost, so repeat until you reach σ. Set cover: definition; greedy picks the set covering the most uncovered elements; the 6-set example where greedy outputs 4 sets {S₁, S₂, S₃, S₄} but the optimum is 3 {S₄, S₅, S₆}; |C| ≤ O(log n)·|C*|.
- **L13–14 (14/9):** Huffman. Alphabet {A, B, C, D} with 100 characters: A 60, B 25, C 10, D 5. Fixed-length 2 bits → 200 bits; variable A=0, B=01, C=10, D=1 → 135 bits (avg 1.35) but **ambiguous** (001 = AAD or AB) since it is not prefix-free; prefix-free A=0, B=10, C=110, D=111 → avg 1.55. Codes ↔ binary trees; prefix-free ⇔ symbols only at leaves (Σ-tree); avg encoding length = avg leaf depth L(T, p) = Σ pₐ·depth(a). Example {a..e} with p = .3, .3, .3, .05, .05, tree a=00, b=01, c=10, d=110, e=111 → 2.1. Merging increases avg depth by the total weight of the two trees → merge the two lowest. Forest A.6 B.25 C.1 D.05 → CD(.15) → B(CD)(.4) → root; pseudocode.
- **L15 + Tute 5 (15/9):** running time O(n log n) (sort + maintain order; a heap gives O(n log n); if pre-sorted, a two-queue method gives O(n)). Optimality theorem by induction on |Σ|: Claim 1: L(T, p) = L(T′, p′) + pₐ + p_b, so Huffman is optimal among trees in which the two rarest symbols a, b are siblings; Claim 2: some optimal tree has a, b as siblings (swap them with the deepest siblings x, y: L(T) − L(T*) = (d_x − d_a)(p_x − p_a) + (d_y − d_b)(p_y − p_b) ≥ 0).
- **Not yet taught as of 21 Sep 2026 (take from the textbooks, full depth):** RAM/cost models (formal), substitution method, all of Unit 3 (BST/AVL/RB/B-tree/hashing/heaps), heap sort, DP, all graph algorithms, network flow, NP-completeness.

---

## 4. ERRATA — the correct versions must appear in the guide

1. **Strassen:** the notes write ae + bg = P₅ + P₄ − **P₃** + P₆. **Correct: ae + bg = P₅ + P₄ − P₂ + P₆** (with P₁ = a(f−h), P₂ = (a+b)h, P₃ = (c+d)e, P₄ = d(g−e), P₅ = (a+d)(e+h), P₆ = (b−d)(g+h), P₇ = (a−c)(e+f); af + bh = P₁ + P₂; ce + dg = P₃ + P₄; cf + dh = P₁ + P₅ − P₃ − P₇). Verify all four with sympy and show the verification.
2. **Sorting lower bound wording:** the notes say "comparison-based sorting can't be solved in Ω(n log n)" and "height at least n log n". **Correct statement:** every comparison-based sort needs **Ω(n log n)** comparisons in the worst case (it cannot be done in o(n log n)); the height is ≥ log₂(n!) = Θ(n log n), not literally ≥ n log n.
3. **Searching:** "Even if the array is not sorted, can we solve this in Ω(n)?" should read o(n) (sub-linear); the answer is No, because the lower bound is Ω(n). Likewise for sorted arrays: no o(log n) comparison algorithm exists.
4. Karatsuba is O(n^{log₂3}) ≈ O(n^{1.585}) (the notes round to 1.58); Strassen is ≈ O(n^{2.807}) (the notes round to 2.8).
5. Set-cover approximation: state it precisely as |C| ≤ H(maxᵢ|Sᵢ|)·|C*| ≤ (ln n + 1)|C*|.

---

## 5. REQUIRED STRUCTURE OF THE DOCUMENT

**Front matter:** title page ("BO CDA 203 — Design of Algorithms | Complete CBT Exam Guide | IIT Patna · B.S. CSDA · Semester 3 · Prepared for Aditya"); how to use + 7-day/3-day plans; **syllabus-coverage matrix** (syllabus phrase → section → 🏫 date / 📚 book → #MCQs); automatic TOC; exam pattern & strategy (MCQ types; tricks for complexity MCQs such as plugging small n, dominance ordering, Master-theorem checklist).

**For EVERY algorithm / data structure, use this template (in order):**
1. 🎯 Learning objectives
2. 📖 Intuition & real-world use
3. 📐 Problem definition (input/output) and notation
4. 🧾 **Pseudocode** (numbered lines, professor-style), plus **Python implementation** (runnable, verified)
5. 🔄 **Flowchart** of the algorithm
6. 🎞 **Step-by-step dry run** on a concrete input, with a state table per step (array contents, heap array, queue, dist[], parent[], residual capacities, DP table, and so on) plus a **figure for each major step** (tree rotations, heap after each op, graph with highlighted edges)
7. ✅ **Correctness proof** (loop invariant / induction / exchange argument / cut property / max-flow min-cut)
8. ⏱ **Complexity analysis** (time best/avg/worst, space, recurrence and how it's solved)
9. 👨‍🏫 Professor's example (if taught)
10. ✍ Worked examples (≥ 3, including one "trap")
11. ⚠ Common mistakes & MCQ traps (e.g., Dijkstra with negative edges; quicksort worst case on sorted input with a first-element pivot; build-heap is O(n), not O(n log n); counting sort is not in-place; radix needs a stable inner sort; AVL vs. RB height bounds; B-tree min keys t−1; open-addressing deletion needs tombstones)
12. 🧠 Memory hooks
13. 📝 Practice exercises (with solutions at the end of the unit)
14. ✅ **MCQ practice set: at least 40 per unit** (Units 0–1: 25 each), mixed types (~55% single-correct, 15% multiple-correct, 20% numerical-answer such as "number of comparisons / rotations / final array index / min-cost value", 10% assertion–reason / match). Difficulty tags [E]/[M]/[H]. **At least 40% should be trace-based** (e.g., "after the 3rd pass of radix sort the array is…", "the heap after inserting 5 is…", "the order in which Kruskal adds edges is…").
15. 🔑 Answer key with explanations
16. 📌 One-page unit summary

**Back matter:**
- **Complexity cheat-sheet tables:** all sorts; all data-structure operations (BST/AVL/RB/B-tree/hash/heap); all graph algorithms (with the data-structure choice); D&C recurrences and their solutions; DP problems with their states/recurrences/complexities
- **Master-theorem quick solver table** with 25 solved recurrences (both forms), plus 10 recurrences where it doesn't apply and how to solve them
- **"Which algorithm?" decision flowcharts:** sorting choice; shortest-path choice (unweighted → BFS; non-negative → Dijkstra; negative edges → Bellman–Ford; all-pairs sparse with negatives → Johnson; dense → Floyd–Warshall); MST choice; design-paradigm choice (greedy vs. D&C vs. DP)
- **NP-completeness reduction map** (graph diagram)
- **3 full-length mock CBT tests** (60 MCQs each, syllabus-weighted, with answer keys and explanations)
- Last-24-hours revision list; index

---

## 6. DIAGRAMS / FIGURES THAT MUST APPEAR (as PNG images at ≥ 200 dpi, embedded)

Insertion-sort sorted|unsorted picture and pass-by-pass table; Big-O/Ω/Θ graphs with c·f(n) and n₀; growth-rate comparison plot; merge-sort recursion tree (green-highlighted levels like the professor's); the 3-way and n/3 : 2n/3 recursion trees; quicksort partition snapshots and the balanced vs. unbalanced recursion trees; the sorting decision tree for 3 elements; counting-sort buckets; radix-sort pass tables; bucket-sort buckets on [0,1); the master-theorem level-wise work diagram (a branches, n/bʲ); Karatsuba 3-subproblem tree vs. 4-subproblem tree; the block-matrix diagram and Strassen's P₁–P₇ table; the searching decision tree (=, <, >); scheduling Gantt charts (ratio vs. diff schedules); the exchange-argument before/after swap timeline; the set-cover 6-set example grid; Huffman forest merge sequence and final tree with codes; the prefix-free vs. ambiguous-code trees; BST insert/delete; all four AVL rotations; RB-tree insert fix-up cases; B-tree split; hash tables with chaining vs. linear probing (clusters); heap tree ↔ array; heapify steps; BFS layers and DFS tree with discovery/finish times; topological order; Kruskal's and Prim's step-by-step edge selection; Dijkstra's step table; Bellman–Ford's relaxation table; Johnson's reweighting example; Ford–Fulkerson residual graphs per augmentation and the min cut; the P/NP/NPC/NP-hard Venn diagram; the reduction-chain graph.

Use **graphviz (`dot` is installed)** for trees and graphs, and **matplotlib** for plots and array/state visualisations. Install anything missing: `pip install matplotlib networkx python-docx sympy`.

---

## 7. DOCX BUILD SPECIFICATIONS

- Build with **python-docx** via a re-runnable script `build_cda203_guide.py`; output `/Users/aditya/STUDY/IIT_Patna_NOTES/SEMESTER_3/203/CDA203_Algorithms_Exam_Guide.docx`.
- A4, 2 cm margins, page numbers, header "CDA 203 · Design of Algorithms".
- Styles: Heading 1 = Unit, Heading 2 = topic, Heading 3 = sub-part; body 11 pt; **pseudocode and Python in Consolas 9.5 pt inside shaded boxes with line numbers**.
- Colour call-out boxes: Definition (blue), Theorem/Complexity (green), Professor's Example (purple), Trap (red), Memory Hook (yellow), Code (grey).
- Math: native Word equations (OMML via LaTeX → MathML → OMML), or high-dpi mathtext PNGs as a fallback; no raw LaTeX.
- A real TOC field (tell the user to press F9 once); continuous MCQ numbering per unit (Q5.14); answer keys at the end of each unit.
- Target: roughly **300–400 pages**, **≥ 320 unit MCQs** plus 3 × 60 mock MCQs.

---

## 8. QUALITY-ASSURANCE CHECKLIST (per unit)

- [ ] Every syllabus phrase in the unit is a heading and is ticked in the coverage matrix.
- [ ] Every professor example from Section 3 is included and solved.
- [ ] **Every dry run, trace, MCQ answer and numeric value was produced by running the Python implementation** (log it in `qa_log_unitX.txt`); every recurrence answer was checked numerically (compute T(n) for large n and compare the ratio to the claimed bound).
- [ ] The Strassen identities and other algebra are verified with sympy.
- [ ] MCQ distractors reflect real mistakes (off-by-one in heap indices, the wrong master case, confusing stable with in-place); answer letters are balanced.
- [ ] No errata from Section 4 is repeated; all figures render; the TOC field exists; the file re-opens cleanly.
- [ ] Report: pages, MCQs and figures added, plus anything ambiguous you resolved.

**Start now:** list and read the source files, then confirm the unit plan in one short message. Then build **Front matter + Unit 0 + Unit 1**, and stop and wait for "continue".
